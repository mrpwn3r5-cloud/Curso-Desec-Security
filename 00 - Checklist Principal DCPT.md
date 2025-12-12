Referências:
https://www.emmanuelsolis.com/oscp.html
## Visão Geral
Este checklist principal integra todos os outros checklists específicos para a certificação DCPT, fornecendo um guia completo e sistemático para testes de penetração.

## Fases do Pentest

### 1. Reconhecimento e Varredura
- [ ] **Varredura de Portas**
  - [ ] Varredura TCP completa
  - [ ] Varredura UDP das portas importantes (top 1000)
  - [ ] Detecção de serviços e versões
  - [ ] Detecção de sistema operacional
  - [ ] Tentar com Nmap com decoy ou g53

**Referência:** [[01.01 - Varredura Completa]]

### 2. Enumeração
- [ ] **Enumeração Web**
  - [ ] Identificação de tecnologias
  - [ ] Enumeração de domínios
  - [ ] Enumeração de subdomínios
  - [ ] Enumeração de diretórios e arquivos
  - [ ] Enumeração de parâmetros
  - [ ] Enumeração de endpoints API
  - [ ] Verificar - Possibilidade de Brute Force

- [ ] **Enumeração de Serviços**
  - [ ] SMB/NetBIOS
  - [ ] SSH
  - [ ] FTP
  - [ ] HTTP/HTTPS
  - [ ] DNS
  - [ ] SMTP/POP3/IMAP
  - [ ] SNMP
  - [ ] MySQL/PostgreSQL
  - [ ] RDP
  - [ ] LDAP
  - [ ] NFS

**Referências:** [[02.01 - Enumeração Web Completa]] | [[02.02 - Enumeração de Serviços]]

### 3. Exploração
- [ ] **Exploração com Metasploit**
  - [ ] Configuração inicial
  - [ ] Importação de dados
  - [ ] Busca de exploits
  - [ ] Configuração de payloads
  - [ ] Estabelecimento de sessões

- [ ] **Exploração Manual**
  - [ ] Análise de vulnerabilidades
  - [ ] Desenvolvimento de exploits
  - [ ] Teste de exploits customizados

**Referência:** [[03.02 - Exploração com Metasploit]]

### 4. Pós-Exploração
- [ ] **Estabelecimento de Shell Estável**
  - [ ] Melhoria de shell básica
  - [ ] Configuração de TTY
  - [ ] Shell reversa com Netcat

- [ ] **Enumeração do Sistema**
  - [ ] Informações básicas do sistema
  - [ ] Informações de rede
  - [ ] Processos e serviços
  - [ ] Usuários e grupos
  - [ ] Arquivos e permissões

- [ ] **Transferência de Arquivos**
  - [ ] Servidor HTTP Python
  - [ ] Transferência com Netcat
  - [ ] Transferência com Base64

**Referências:** [[04.01 - Pós-Exploração Linux]] | [[04.02 - Pós-Exploração Windows]]

### 5. Escalação de Privilégios
- [ ] **Linux**
  - [ ] Enumeração de arquivos SUID/SGID
  - [ ] Exploração de sudo
  - [ ] Exploração de capabilities
  - [ ] Exploração de cron jobs
  - [ ] Exploração de serviços
  - [ ] Exploração de kernel

- [ ] **Windows**
  - [ ] Verificação de UAC
  - [ ] Exploração de serviços
  - [ ] Exploração de drivers
  - [ ] Exploração de registry
  - [ ] Exploração de scheduled tasks

**Referência:** [[04.01 - Pós-Exploração Linux]]

### 6. Vulnerabilidades Web
- [ ] **SQL Injection**
  - [ ] Detecção básica
  - [ ] Blind SQL injection
  - [ ] Union-based SQL injection
  - [ ] Time-based SQL injection

- [ ] **Cross-Site Scripting (XSS)**
  - [ ] XSS refletido
  - [ ] XSS armazenado
  - [ ] XSS DOM
  - [ ] Bypass de filtros

- [ ] **File Inclusion**
  - [ ] Local File Inclusion (LFI)
  - [ ] Remote File Inclusion (RFI)
  - [ ] Bypass de filtros

- [ ] **Outras Vulnerabilidades**
  - [ ] XML External Entity (XXE)
  - [ ] Command Injection
  - [ ] File Upload
  - [ ] Directory Traversal
  - [ ] Server-Side Request Forgery (SSRF)
  - [ ] Insecure Direct Object References (IDOR)

**Referência:** [[03.01 - Exploração Web]]

### 7. Brute Force e Quebra de Senhas
- [ ] **Geração de Wordlists**
  - [ ] Wordlists padrão
  - [ ] Geração com Crunch
  - [ ] Geração com Cewl
  - [ ] Geração com John the Ripper

- [ ] **Brute Force de Serviços**
  - [ ] SSH
  - [ ] HTTP
  - [ ] FTP
  - [ ] SMB
  - [ ] RDP
  - [ ] MySQL/PostgreSQL

- [ ] **Quebra de Hashes**
  - [ ] John the Ripper
  - [ ] Hashcat
  - [ ] Hydra para hashes

**Referência:** [[03.03 - Brute Force e Quebra de Senhas]]

## Ferramentas Essenciais

### Varredura e Enumeração
- **Nmap** - Varredura de portas e serviços
- **Masscan** - Varredura rápida de portas
- **Zmap** - Varredura de internet
- **Unicornscan** - Varredura UDP

### Enumeração Web
- **Gobuster** - Enumeração de diretórios
- **FFUF** - Fuzzing web
- **Dirb** - Enumeração de diretórios
- **Nikto** - Scanner de vulnerabilidades web
- **OWASP ZAP** - Proxy de segurança

### Enumeração de Serviços
- **Enum4linux** - Enumeração SMB
- **RPCClient** - Cliente RPC
- **CrackMapExec** - Ferramenta de rede
- **SNMPWalk** - Enumeração SNMP

### Exploração
- **Metasploit** - Framework de exploração
- **MSFVenom** - Geração de payloads
- **Burp Suite** - Proxy de segurança
- **SQLMap** - Exploração SQL injection

### Brute Force
- **Hydra** - Brute force de serviços
- **Medusa** - Brute force de serviços
- **Ncrack** - Brute force de serviços
- **John the Ripper** - Quebra de senhas
- **Hashcat** - Quebra de hashes

### Pós-Exploração
- **LinPEAS** - Enumeração Linux
- **WinPEAS** - Enumeração Windows
- **PowerUp** - Escalação Windows
- **PrivescCheck** - Escalação Windows

## Metodologia Recomendada

### 1. Planejamento
- [ ] Definir escopo do teste
- [ ] Obter autorização por escrito
- [ ] Configurar ambiente de teste
- [ ] Preparar ferramentas necessárias

### 2. Reconhecimento
- [ ] Coleta de informações públicas
- [ ] Identificação de alvos
- [ ] Mapeamento de rede
- [ ] Identificação de tecnologias

### 3. Varredura
- [ ] Varredura de hosts ativos
- [ ] Varredura de portas
- [ ] Detecção de serviços
- [ ] Identificação de vulnerabilidades

### 4. Enumeração
- [ ] Enumeração detalhada de serviços
- [ ] Enumeração de aplicações web
- [ ] Identificação de usuários
- [ ] Mapeamento de estrutura

### 5. Exploração
- [ ] Exploração de vulnerabilidades
- [ ] Estabelecimento de acesso
- [ ] Escalação de privilégios
- [ ] Manutenção de acesso

### 6. Pós-Exploração
- [ ] Enumeração do sistema comprometido
- [ ] Coleta de informações sensíveis
- [ ] Pivoting para outros sistemas
- [ ] Estabelecimento de persistência

### 7. Relatório
- [ ] Documentação de descobertas
- [ ] Análise de impacto
- [ ] Recomendações de correção
- [ ] Priorização de vulnerabilidades

## Checklist de Validação Final

### Técnico
- [ ] Todos os alvos foram testados
- [ ] Todas as portas foram verificadas
- [ ] Todos os serviços foram enumerados
- [ ] Todas as vulnerabilidades foram exploradas
- [ ] Escalação de privilégios foi alcançada
- [ ] Persistência foi estabelecida

### Documentação
- [ ] Todas as descobertas foram documentadas
- [ ] Evidências foram coletadas
- [ ] Screenshots foram capturados
- [ ] Logs foram salvos
- [ ] Comandos foram registrados

### Relatório
- [ ] Resumo executivo foi criado
- [ ] Vulnerabilidades foram classificadas
- [ ] Impacto foi avaliado
- [ ] Recomendações foram fornecidas
- [ ] Cronograma de correção foi sugerido

## Dicas Importantes para DCPT

### Preparação
1. **Pratique regularmente** com laboratórios
2. **Familiarize-se** com todas as ferramentas
3. **Estude metodologias** de pentest
4. **Mantenha-se atualizado** com novas vulnerabilidades

### Durante o Teste
1. **Siga a metodologia** sistematicamente
2. **Documente tudo** desde o início
3. **Use múltiplas ferramentas** para validação
4. **Teste manualmente** antes de usar ferramentas automatizadas

### Após o Teste
1. **Revise todas as descobertas**
2. **Valide evidências** coletadas
3. **Priorize vulnerabilidades** por impacto
4. **Prepare recomendações** específicas

## Recursos Adicionais

### Laboratórios Práticos
- **TryHackMe** - Laboratórios interativos
- **HackTheBox** - Máquinas vulneráveis
- **VulnHub** - VMs vulneráveis
- **PentesterLab** - Exercícios práticos

### Ferramentas Complementares
- **Recon-ng** - Reconhecimento automatizado
- **TheHarvester** - Coleta de informações
- **Shodan** - Motor de busca para dispositivos
- **Censys** - Motor de busca para dispositivos

### Referências Técnicas
- **OWASP Top 10** - Vulnerabilidades web mais comuns
- **NIST Cybersecurity Framework** - Estrutura de segurança
- **PTES** - Metodologia de teste de penetração
- **OSSTMM** - Metodologia de teste de segurança

## Conclusão

Este checklist principal serve como um guia abrangente para a certificação DCPT, integrando todos os aspectos técnicos e metodológicos necessários para realizar testes de penetração eficazes. Use-o como referência durante seus estudos e prática, sempre adaptando às necessidades específicas de cada teste.

**Lembre-se:** A prática constante e a experiência são fundamentais para o sucesso na certificação DCPT. Use este checklist como base, mas desenvolva sua própria metodologia baseada na experiência prática.
