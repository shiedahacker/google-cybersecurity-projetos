# Investigação de pagamento indevido com análise de controles de acesso

> **Contexto:** Google Cybersecurity Professional Certificate (curso Assets, Threats, and Vulnerabilities)
> **Habilidades:** análise de logs, correlação de evidências, autenticação e autorização, contabilização (AAA), princípio do menor privilégio

## Descrição do projeto

Nesta atividade, fui o primeiro profissional de segurança contratado por uma empresa em crescimento. Um pagamento da empresa foi enviado para uma conta bancária desconhecida. O gerente financeiro disse que não cometeu nenhum erro, e a empresa conseguiu interromper o pagamento a tempo.

Meu trabalho foi fazer a contabilização do incidente: analisar o registro de eventos, cruzar com o diretório de funcionários para identificar quem fez a alteração, apontar as falhas de controle de acesso que permitiram isso e recomendar melhorias.

A planilha preenchida está em [`controle-de-acesso.docx`](controle-de-acesso.docx). O registro de eventos e o diretório de funcionários usados na análise estão em [`exercicio-de-contabilizacao.xlsx`](exercicio-de-contabilizacao.xlsx).

## As evidências

**Registro de eventos**

| Campo | Valor |
|---|---|
| Event ID | 1227 |
| Origem | AdsmEmployeeService |
| Data e hora | 10/03/2023, 8:29:57 AM |
| Usuário | `Legal\Administrator` |
| Computador | `Up2-NoGud` |
| IP | `152.207.255.255` |
| Descrição | Payroll event added. `FAUX_BANK` |

**Registro correspondente no diretório de funcionários**

| Nome | Função | Status | Autorização | IP | Último acesso | Fim do contrato |
|---|---|---|---|---|---|---|
| Robert Taylor Jr. | Legal attorney | Contractor | Admin | 152.207.255.255 | 8:29:57 am (5 dias atrás) | 27/12/2019 |

## Análise

**Observações.**
- Em 10/03/2023, às 8:29:57, alguém adicionou à folha de pagamento um destino desconhecido (`FAUX_BANK`).
- A ação partiu da conta `Legal\Administrator`, no computador `Up2-NoGud`, com o IP 152.207.255.255. Esse é o mesmo IP de Robert Taylor Jr., advogado terceirizado, e o último acesso dele no diretório bate exatamente com o horário do evento.

**Problemas.**
1. **Conta de ex-contratado ativa.** O contrato de Robert Taylor Jr. terminou em 27/12/2019, mas a conta dele continuava ativa quase quatro anos depois e foi usada para alterar a folha de pagamento.
2. **Privilégios em excesso.** Todos os funcionários, inclusive designer, vendedores, terceirizados e ex-funcionários, têm autorização de **Admin** na unidade de nuvem compartilhada. Além disso, a conta usada era genérica (`Administrator`), o que dificulta saber quem fez o quê.

**Recomendações.**
1. **Menor privilégio com controle por função (RBAC).** Só o setor financeiro deve poder alterar a folha de pagamento, e as contas administrativas devem ser individuais e restritas.
2. **Processo de desligamento (offboarding).** Revogar o acesso automaticamente na data de término do contrato e revisar as contas periodicamente para desativar usuários inativos ou desligados.
3. **MFA e separação de funções.** Mudanças de dados bancários na folha precisam da aprovação de uma segunda pessoa, com alertas em tempo real nos logs para esse tipo de evento.

## O que aprendi

- Um log sozinho mostra o que aconteceu, mas cruzar com outra fonte (aqui, o diretório de funcionários) é o que mostra quem fez. O IP e o horário do último acesso foram as pistas que ligaram a conta genérica a uma pessoa.
- Os três "A" do AAA falharam juntos: a autenticação aceitou uma conta que não deveria existir, a autorização dava Admin para todo mundo e a contabilização dependia de uma conta compartilhada.
- Esquecer de desativar uma conta é tão perigoso quanto dar acesso demais. O offboarding precisa ser um processo automático, e não algo que depende de alguém lembrar.
- Para ações de alto impacto, como mexer em pagamentos, um único controle não basta. Exigir uma segunda aprovação faz com que uma conta comprometida não seja suficiente para causar o dano.
