# Aplicar filtros a consultas SQL

> **Contexto:** Google Cybersecurity Professional Certificate (módulo de SQL)
> **Habilidades:** `SELECT`, `WHERE`, `AND`, `OR`, `NOT`, `LIKE` com `%`, investigação de logs de autenticação

## Descrição do projeto

Neste projeto, atuei como analista de segurança em uma grande organização, investigando possíveis problemas de segurança ligados a tentativas de login e às máquinas dos funcionários. Usei SQL para consultar as tabelas `log_in_attempts` e `employees` e filtrei os dados com os operadores `AND`, `OR`, `NOT` e `LIKE` para encontrar só os registros que importavam em cada caso. Cada consulta responde a uma pergunta concreta da equipe de segurança, como quem tentou entrar fora do horário ou quais máquinas precisavam de atualização.

## Recuperar tentativas de login malsucedidas fora do horário comercial

Meu primeiro passo foi investigar um possível incidente fora do horário comercial. O expediente termina às 18:00, então eu queria ver todas as tentativas de login que falharam depois desse horário. Para isso, consultei a tabela `log_in_attempts`:

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00' AND success = 0;
```

A consulta combina duas condições com o `AND`, e uma linha só aparece no resultado se as duas forem verdadeiras ao mesmo tempo:

- `login_time > '18:00'` filtra pelo horário. Como `login_time` guarda a hora da tentativa, o operador `>` traz só as que aconteceram depois das 18:00. O horário vai entre aspas simples porque é tratado como texto.
- `success = 0` filtra pelas tentativas que falharam. No MySQL, o valor booleano falso é guardado como `0` (e verdadeiro como `1`), então `0` representa um login malsucedido. Também daria para escrever `success = FALSE`. Aqui não uso aspas, porque é um valor booleano e não um texto.

O resultado é a lista de tentativas de login malsucedidas feitas fora do expediente, que é o ponto de partida para investigar uma atividade suspeita.

## Recuperar tentativas de login em datas específicas

Depois, a equipe identificou um evento suspeito em 2022-05-09 e eu precisava ver tudo o que aconteceu nesse dia e no dia anterior. Usei a coluna `login_date`:

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```

Aqui usei o `OR`, porque queria as tentativas de qualquer um dos dois dias. Com o `OR`, basta que uma das condições seja verdadeira para a linha aparecer. Se eu tivesse usado `AND`, o resultado ficaria vazio, já que uma tentativa não pode ter acontecido nos dois dias ao mesmo tempo. As datas seguem o formato `AAAA-MM-DD` e ficam entre aspas simples, e preciso repetir a coluna `login_date` nas duas condições.

O resultado mostra todas as tentativas de login desses dois dias, bem-sucedidas ou não, para que a equipe possa comparar o comportamento do dia do evento com o do dia anterior.

## Recuperar tentativas de login fora do México

Em seguida, a equipe determinou que a atividade suspeita não veio do México, então eu precisava ver as tentativas de login originadas em qualquer outro país. O detalhe é que a coluna `country` guarda o México de duas formas, como `MEX` e como `MEXICO`. Por isso usei o `LIKE` com o curinga `%`:

```sql
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```

O `LIKE` faz uma busca por padrão em texto. O símbolo `%` representa qualquer quantidade de caracteres, inclusive nenhum, então `'MEX%'` pega todo valor que começa com `MEX`, o que cobre `MEX` e `MEXICO` de uma vez. Sem o `LIKE`, eu teria que escrever as duas variações manualmente e correria o risco de esquecer alguma.

Já o `NOT` inverte o filtro. Em vez de trazer as linhas que combinam com o padrão, ele traz todas as que não combinam. O resultado são as tentativas de login que vieram de países diferentes do México.

## Recuperar funcionários do departamento de Marketing

Depois, a equipe precisou fazer atualizações de segurança nas máquinas do departamento de Marketing que ficam no prédio Leste. Para isso, consultei a tabela `employees`:

```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```

A consulta usa duas condições ligadas por `AND`:

- `department = 'Marketing'` limita o resultado ao departamento de Marketing.
- `office LIKE 'East%'` pega todos os escritórios cujo nome começa com `East`, como `East-170` ou `East-320`. Os escritórios seguem o padrão prédio, hífen e número, então o `%` cobre qualquer número que venha depois de `East-`.

Como uso o `AND`, só aparecem os funcionários que estão em Marketing e também em um escritório do prédio Leste. Alguém de Marketing em outro prédio, ou de outro departamento no prédio Leste, fica de fora.

## Recuperar funcionários de Finanças ou Vendas

A próxima atualização de segurança era para os departamentos de Finanças e de Vendas, e eu precisava localizar esses funcionários:

```sql
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';
```

Aqui o operador certo é o `OR`, porque cada funcionário pertence a um único departamento. Se eu usasse `AND`, ninguém passaria pelo filtro, já que uma pessoa não está em Finanças e em Vendas ao mesmo tempo. Uma coisa que aprendi é que, mesmo comparando a mesma coluna nas duas condições, preciso escrever `department =` nas duas, e não só `department = 'Finance' OR 'Sales'`. O resultado traz todos os funcionários que estão em um desses dois departamentos.

## Recuperar todos os funcionários que não estão em TI

Por fim, a equipe tinha mais uma atualização para fazer. Os funcionários de TI já a tinham recebido, então eu precisava de todos os outros:

```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```

O `NOT` exclui os registros que atendem à condição. Em vez de listar todos os outros departamentos (Marketing, Finanças, Vendas e assim por diante), é mais simples e seguro dizer o que eu não quero, e qualquer departamento novo que surgir já entra no resultado automaticamente. O texto precisa estar escrito exatamente como está nos dados, `'Information Technology'`. O resultado é a lista dos funcionários que ainda precisam da atualização.

## Resumo

| Operador | Uso no projeto |
|---|---|
| `AND` | Exigir várias condições ao mesmo tempo (horário após 18:00 **e** login falho) |
| `OR` | Aceitar mais de um valor (duas datas, dois departamentos) |
| `NOT` | Excluir resultados (México, departamento de TI) |
| `LIKE` + `%` | Encontrar padrões de texto (`MEX%`, `East%`) |

Neste projeto, usei filtros em SQL para investigar tentativas de login suspeitas e para identificar as máquinas de funcionários que precisavam de atualização. Com isso, consegui transformar tabelas grandes em listas pequenas e focadas, que a equipe de segurança consegue usar para agir.
