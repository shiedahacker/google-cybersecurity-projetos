**Português** · [English](README.en.md)

# Avaliação de vulnerabilidade de um banco de dados exposto ao público

> **Contexto:** Google Cybersecurity Professional Certificate (curso Assets, Threats, and Vulnerabilities)
> **Habilidades:** avaliação de vulnerabilidade, análise de risco qualitativa, NIST SP 800-30 Rev. 1, recomendação de controles

## Descrição do projeto

Nesta atividade, eu era um analista recém-contratado em uma empresa de e-commerce. A empresa guarda seus dados em um servidor de banco de dados remoto, porque muitos funcionários trabalham de casa em vários países e usam esses dados para encontrar novos clientes.

O problema é que esse banco de dados está aberto ao público desde que a empresa começou, há três anos. Meu trabalho foi fazer um relatório de avaliação de vulnerabilidade explicando os riscos disso para os gestores e o que pode ser feito para proteger o servidor.

O relatório preenchido está em [`avaliacao-de-vulnerabilidade.pdf`](avaliacao-de-vulnerabilidade.pdf). Também tem a versão em inglês em [`vulnerability-assessment-en.pdf`](vulnerability-assessment-en.pdf).

## O sistema avaliado

| Item | Detalhe |
|---|---|
| Hardware | CPU potente e 128 GB de memória |
| Sistema operacional | Linux (versão mais recente) |
| Banco de dados | MySQL |
| Rede | IPv4, conectado a outros servidores |
| Segurança atual | Conexões criptografadas com SSL/TLS |
| Escopo | Controles de acesso do sistema, de junho a agosto |

## Avaliação de risco

Usei a NIST SP 800-30 Rev. 1 para escolher as fontes e os eventos de ameaça. A nota de risco é a probabilidade vezes a gravidade, cada uma de 1 a 3.

| Fonte de ameaça | Evento de ameaça | Probabilidade | Gravidade | Risco |
|---|---|---|---|---|
| Hacker | Obter informações confidenciais por exfiltração | 3 | 3 | 9 |
| Funcionário | Alterar ou excluir informações críticas | 2 | 3 | 6 |
| Concorrente | Realizar ataques de negação de serviço (DoS) | 2 | 2 | 4 |

**Por que essas notas.** O roubo de dados por um hacker ficou com a nota mais alta porque o banco está aberto na internet, então é bem provável que alguém tente, e o estrago seria grande. A alteração de dados por um funcionário é menos provável, mas também seria grave, já que hoje não há controle de quem pode mudar ou apagar informações. O DoS de um concorrente deixaria o servidor fora do ar por um tempo, mas os dados não seriam perdidos, por isso dei uma gravidade menor.

## Estratégia de correção

1. Tirar o banco de dados do acesso público e liberar só conexões autorizadas, por exemplo por VPN.
2. Usar autenticação multifator (MFA) para todos os usuários.
3. Aplicar o princípio do privilégio mínimo, para que só quem precisa possa alterar ou apagar dados.
4. Manter o SSL/TLS com certificados válidos e usar PKI.
5. Aplicar defesa em profundidade com firewall, IDS e registro de acessos (AAA).

## O que aprendi

- Um único erro de configuração, como deixar o banco aberto, pode abrir caminho para vários tipos de ataque diferentes.
- A avaliação qualitativa depende muito do julgamento de quem faz. Por isso é importante explicar no relatório como cheguei em cada nota.
- Nem toda ameaça vem de fora. O risco de um funcionário apagar ou mudar dados também precisa entrar na análise.
- Para o relatório ser útil para os gestores, as recomendações precisam ser práticas e ligadas aos riscos que encontrei.
