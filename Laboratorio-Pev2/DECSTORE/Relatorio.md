

- Iniciei realizando  uma varreda de servicos/portas expostas  com o `nmap` e encontrei as portas `80,8080,53,2121,22552.

```bash
	nmap -v -sS --open -p- -Pn 172.23.1.45
```

![[nmap.png]]

- Digitando o ip no navegador não retornava nada,
![[2025-12-15_11-35.png]]
