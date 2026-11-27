# Análise de vazamento de dados com o princípio do menor privilégio

> **Contexto:** Google Cybersecurity Professional Certificate (curso Assets, Threats, and Vulnerabilities)
> **Habilidades:** análise de incidente, princípio do menor privilégio, NIST CSF, NIST SP 800-53 (AC-6)

## Descrição do projeto

Nesta atividade, trabalhei como analista em uma empresa de tecnologia educacional que tem um aplicativo para ajudar professores a corrigir tarefas. Planos internos de negócio vazaram nas redes sociais depois que um funcionário compartilhou, sem querer, uma pasta confidencial com um parceiro externo.

Meu trabalho foi entender o que levou ao vazamento, estudar o controle de menor privilégio da NIST SP 800-53 (AC-6) e recomendar melhorias para que isso não aconteça de novo.

O documento preenchido está em [`vazamento-de-dados.pdf`](vazamento-de-dados.pdf). Ele segue o modelo original do curso, em inglês.

## O incidente

1. Em uma reunião, um gerente de vendas deu à equipe acesso a uma pasta de documentos internos. A pasta tinha arquivos de um produto ainda não anunciado, análises de clientes e materiais de divulgação.
2. Depois da reunião, o gerente não tirou o acesso. Só avisou a equipe para esperar aprovação antes de compartilhar os materiais de divulgação.
3. Em uma videochamada com um parceiro de negócios, um vendedor esqueceu o aviso. Ele queria mandar o link dos materiais de divulgação, mas mandou o link da pasta interna inteira.
4. O parceiro achou que era o material de divulgação e publicou o link na página da empresa dele nas redes sociais.

## Onde isso se encaixa no plano de segurança

| Função | Categoria | Subcategoria | Referência |
|---|---|---|---|
| Proteger (Protect) | PR.DS: Segurança de dados | PR.DS-5: Proteção contra vazamento de dados | NIST SP 800-53: AC-6 |

## Análise

**Problemas.** O gerente deu à equipe inteira acesso a arquivos internos, quando ela só precisava dos materiais de divulgação, e não removeu esse acesso depois da reunião. A única proteção era um aviso verbal, então nada impediu o vendedor de mandar o link errado para alguém de fora.

**O que a NIST SP 800-53: AC-6 diz.** O controle AC-6 trata do princípio do menor privilégio: cada usuário deve ter só o acesso mínimo de que precisa para fazer uma tarefa. Ele também lista melhorias que deixam o controle mais forte, como restringir o acesso por função, revogar o acesso automaticamente, manter logs de atividade e auditar os privilégios com frequência.

**Recomendações.**
1. Restringir o acesso a recursos sensíveis com base na função do usuário.
2. Revogar automaticamente o acesso às informações depois de um período de tempo.

**Justificativa.** Com o acesso por função, os arquivos internos não teriam chegado à equipe de vendas, que só precisava dos materiais de divulgação. Com a revogação automática, a pasta teria sido fechada depois da reunião, e mesmo que o link errado fosse enviado, o parceiro não conseguiria abri-la.

## O que aprendi

- O vazamento não foi culpa de uma pessoa só. Foi uma sequência de pequenas falhas: acesso demais, acesso que não foi retirado e uma regra que dependia só da memória do funcionário.
- Um aviso verbal não é um controle de segurança. Se a regra é importante, ela precisa ser garantida pelo sistema.
- O menor privilégio funciona melhor quando o acesso é pensado por função e tem prazo para acabar, e não dado "por enquanto" e esquecido.
- Escolhi duas melhorias que atacam cada uma um problema diferente: uma evita o acesso em excesso e a outra evita o acesso esquecido. Logs e auditorias também ajudariam, mas mais para descobrir o problema depois do que para evitá-lo.
