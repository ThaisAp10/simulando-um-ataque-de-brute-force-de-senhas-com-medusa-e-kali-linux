# Simulando um ataque de Brute Force de senhas com Medusa e Kali Linux
Este repositório contém um passo a passo seguindo o curso de cibersegurança da DIO em parceria com o banco Santander.

Projeto realizado no curso de Cibersegurança da DIO em parceria com o Santander. O laboratório foi executado exclusivamente em um ambiente controlado (máquinas virtuais isoladas), com fins didáticos e legais.

#### Contexto e objetivo
Este projeto tem como objetivo demonstrar, de forma segura e controlada, como ataques de autenticação por força bruta podem ser executados contra serviços vulneráveis e quais contramedidas reduzirão esses riscos. Ferramentas utilizadas: Kali Linux (ambiente atacante), Medusa (ferramenta de força bruta), e ambientes vulneráveis de teste (ex.: Metasploitable 2).

#### A importância da autenticação
A autenticação é fundamental para a segurança digital, pois verifica a identidade de um usuário ou sistema antes de conceder acesso a informações e recursos confidenciais.

#### Principais tipos de ataque
- Ataque de força bruta (brute force): tenta todas as combinações possíveis de senhas até encontrar a correta.
- Ataque de dicionário: utiliza uma lista pré compilada de senhas prováveis (um “dicionário”) para testar autenticações.
- Ataque de permutação (variante de brute force): gera combinações a partir de padrões e variações (por exemplo, adicionar números ou símbolos a palavras comuns).
- Ataque híbrido: combina ataques de dicionário com variações (permutação) para aumentar a eficácia.
- Password spraying: tenta um pequeno conjunto de senhas comuns contra muitos usuários, evitando bloqueios por tentativas excessivas em uma única conta.
- Credential stuffing: utiliza credenciais vazadas de outros serviços para tentar acesso em massa a diferentes sistemas, explorando a reutilização de senhas.

#### Preparando o ambiente
Foi instalado o Oracle VM VirtualBox para execução das máquinas virtuais.
Para o projeto foi utilizado o Kali Linux, que inclui diversas ferramentas de segurança pré instaladas. A ferramenta Hydra é uma das mais completas para ataques de força bruta.

#### Ambiente do projeto
Para este projeto foram utilizadas duas máquinas virtuais: Kali Linux e Metasploitable 2.
O Metasploitable 2 é uma VM propositalmente vulnerável, utilizada em ambiente controlado para fins de estudo.
Após instalar as máquinas virtuais, o próximo passo é configurar a rede para que elas possam se comunicar.
![Kali Linux Settings](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela1.png)
Antes de iniciar qualquer procedimento no Metasploitable 2, recomenda se criar um snapshot; ele permite restaurar a VM rapidamente ao estado original caso ocorram mudanças indesejadas ou comprometimento.

![Criar Snapshot](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela2.png)

#### Exercício: Alcançado Máquina Vulnerável
Neste teste, o Kali Linux será o sistema atacante e o Metasploitable será a máquina alvo.
Com ambas as VMs iniciadas, acesse a interface do Metasploitable e faça login no terminal. Em seguida, descubra o IP da máquina com o comando ip a, que exibirá, por exemplo, o endereço **192.168.56.102**.

![Metasploitable](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela3.png)

![Comandos Metasploitable](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela4.png)

Após descobrir o IP da máquina alvo, volte para o Kali Linux e abra um terminal para contatar a máquina vulnerável Metasploitable. O primeiro comando usado é **ping -c 3 192.168.56.102** para verificar a conectividade entre as máquinas.

![Terminal Kali Linux](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela5.png)

Se ao utilizar o comando e não apresentar nenhum erro significa que foi possível conectar à máquina.

![Terminal Kali Linux 2](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela6.png)

Agora o objetivo é identificar se o sistema está exposto e vulnerável, para isto, será utilizado uma ferramenta chamada Nmap. No terminal do Kali Linux foi digitado o seguinte comando:

**nmap -sV -p 21,22,80,445,139 192.168.56.102**

Este comando irá escanear as portas 21, 22, 80, 445 e 139, que são portas muito comuns como FTP, SSH, HTTP e SMB. Ao executar o comando ele apresentará se possui alguma porta aberta e a versão. No teste consta que todas as portas estão abertas conforme a imagem:
![Terminal Kali Linux 3](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela7.png)

Para conectar ao FTP, é digitado o seguinte comando: **ftp 192.168.56.102**

Ao exibir o resultado como 'connected', isso significa que a conexão foi estabelecida, e o sistema solicita alguns dados:

![Terminal Kali Linux 4](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela8.png)

Para descobrir os dados, será utilizado o Medusa para ataques de força bruta.

#### Criando usuários e senhas

Em seguida criamos alguns usuários e senhas comuns. Use o comando abaixo para criar o arquivo com os nomes de usuário:

**echo -e "user\nmsfadmin\nadmin\nroot" > users.txt**

E o comando abaixo para criar o arquivo com as senhas:

**echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt**

Após a execução, serão gerados os arquivos users.txt e pass.txt, contendo respectivamente os usuários e as senhas.

![Terminal Kali Linux 5](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela9.png)

#### Começando o ataque

Para iniciar o ataque ao FTP será digitado o seguinte comando utilizando o Medusa:

**medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6**

Ao executar o comando, o Medusa irá combinar os usuários e senhas, gerando uma lista de respostas. Quando aparecer a opção [SUCCESS], significa que a combinação está correta.

Resultado:

**User: msfadmin**

**Password: msfadmin**

Com estas credenciais será possível acessar o ftp da máquina alvo.

![Terminal Kali Linux 6](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela10.png)

Teste de acesso ao FTP da máquina alvo:

![Terminal Kali Linux 7](https://github.com/ThaisAp10/simulando-um-ataque-de-brute-force-de-senhas-com-medusa-e-kali-linux/blob/main/img/tela11.png)

Percebe-se que, em poucos minutos e com poucos comandos, foi possível acessar um serviço exposto. Dessa forma, fica evidente a importância de manter os sistemas atualizados, aplicar mecanismos de autenticação adequados, utilizar senhas fortes e adotar proteções apropriadas para que não sofram esse tipo de ataque.

#### Fim do exercício





