# Registro de riscos de um banco comercial

> **Contexto:** Google Cybersecurity Professional Certificate (curso Assets, Threats, and Vulnerabilities)
> **Habilidades:** avaliação de riscos, registro de riscos, matriz de probabilidade x impacto, NIST CSF

## Descrição do projeto

Nesta atividade, o cenário era o de uma equipe de segurança de um banco comercial fazendo uma avaliação de risco. A equipe já tinha listado cinco riscos para os fundos do banco em um registro de riscos. Meu trabalho foi estimar a probabilidade e a gravidade de cada um, calcular a pontuação e ordenar os riscos por prioridade.

O documento preenchido está em [`registro-de-riscos.docx`](registro-de-riscos.docx). Ele segue o modelo original do curso, em inglês.

## Ambiente operacional

- O banco fica em uma área costeira com baixo índice de criminalidade.
- 100 funcionários trabalham no local e 20 trabalham remotamente.
- A base de clientes tem 2.000 contas individuais e 200 contas comerciais.
- Os serviços são divulgados por um time esportivo profissional e por dez empresas locais.
- Há regras financeiras rígidas, como ter dinheiro suficiente disponível todos os dias para cumprir as exigências do Federal Reserve.

## Como a pontuação funciona

**Probabilidade x Gravidade = Risco (prioridade)**

- **Probabilidade (1 a 3):** a chance de o risco virar um evento de segurança.
- **Gravidade (1 a 3):** o tamanho do dano para o banco se ele acontecer.
- **Prioridade (1 a 9):** quanto maior, mais cedo o risco deve ser tratado.

## Registro de riscos

| Ativo | Risco | Descrição | Probabilidade | Gravidade | Prioridade |
|---|---|---|:-:|:-:|:-:|
| Fundos | Comprometimento de e-mail comercial | Um funcionário é enganado e compartilha informação confidencial. | 2 | 2 | 4 |
| Fundos | Comprometimento do banco de dados de usuários | Os dados dos clientes estão mal criptografados. | 2 | 3 | 6 |
| Fundos | Vazamento de registros financeiros | Um servidor com backups está acessível publicamente. | 3 | 3 | **9** |
| Fundos | Roubo | O cofre do banco é deixado destrancado. | 1 | 3 | 3 |
| Fundos | Interrupção na cadeia de suprimentos | Atrasos nas entregas por causa de desastres naturais. | 1 | 2 | 2 |

**Notas:** eventos de segurança são possíveis porque o banco trabalha com muitas pessoas e parceiros. Os 120 funcionários, o time esportivo e as dez empresas locais criam oportunidades para phishing e exposição de dados, e os funcionários remotos aumentam os pontos de entrada. As regras rígidas também fazem com que qualquer vazamento ou perda de fundos traga multas e prejudique a reputação do banco.

## Por que pontuei assim

- **Vazamento de registros financeiros (3 x 3 = 9).** Um servidor aberto na internet pode ser encontrado a qualquer momento por quem faz varreduras automáticas, então a probabilidade é alta. O impacto também é o maior: perda de clientes, multas e dano à reputação.
- **Banco de dados de usuários (2 x 3 = 6).** A criptografia fraca só vira problema se alguém conseguir chegar ao banco de dados, por isso a probabilidade é moderada. Mas, se acontecer, expõe os dados das 2.200 contas.
- **E-mail comercial (2 x 2 = 4).** Com 120 funcionários, é provável que alguém caia em um golpe por e-mail em algum momento. O dano costuma ficar limitado ao que aquele funcionário sabia ou podia acessar, então dei gravidade moderada.
- **Roubo (1 x 3 = 3).** A região tem pouca criminalidade e o cofre fica dentro do banco, então a chance é baixa. Só que perder o dinheiro do cofre impediria o banco de cumprir a exigência diária de caixa.
- **Cadeia de suprimentos (1 x 2 = 2).** Um furacão forte acontece só a cada alguns anos, e o atraso nas entregas é temporário.

## Ordem de prioridade

1. Vazamento de registros financeiros (9)
2. Comprometimento do banco de dados de usuários (6)
3. Comprometimento de e-mail comercial (4)
4. Roubo (3)
5. Interrupção na cadeia de suprimentos (2)

## O que aprendi

- A pontuação transforma uma lista de preocupações em uma ordem de trabalho. Sem ela, tudo parece urgente.
- Probabilidade e gravidade são coisas diferentes. O roubo do cofre seria grave, mas é improvável, e por isso ficou abaixo do golpe por e-mail.
- As notas são estimativas. Outra pessoa poderia dar um ponto a mais ou a menos, e o importante é conseguir justificar cada número com base no ambiente do banco.
- O risco mais alto era também o mais simples de corrigir: bastava tirar o servidor de backup da internet.
