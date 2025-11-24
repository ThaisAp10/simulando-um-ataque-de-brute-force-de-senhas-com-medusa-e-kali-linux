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



