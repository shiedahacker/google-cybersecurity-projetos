# Inventário de ativos de uma rede doméstica

> **Contexto:** Google Cybersecurity Professional Certificate (curso Assets, Threats, and Vulnerabilities)
> **Habilidades:** inventário de ativos, classificação de dados, tríade CIA (confidencialidade, integridade e disponibilidade)

## Descrição do projeto

Nesta atividade, o cenário era o de uma pequena empresa que funciona dentro de casa. Meu trabalho foi montar um inventário dos dispositivos ligados à rede doméstica e classificar cada um pelo nível de sensibilidade, para saber quais precisam de mais proteção.

O modelo da atividade já vinha com três dispositivos preenchidos (roteador, desktop e smartphone de visitante). Eu acrescentei outros três: o notebook de trabalho, a impressora de rede e a smart TV.

A planilha preenchida está em [`inventario-de-ativos-domesticos.xlsx`](inventario-de-ativos-domesticos.xlsx). Ela segue o modelo original do curso, em inglês.

## Inventário

| # | Ativo | Acesso à rede | Proprietário | Localização | Observações | Sensibilidade |
|---|---|---|---|---|---|---|
| 1 | Roteador de rede | Contínuo | Provedor de internet | No local | Tem conexões de 2,4 GHz e 5 GHz. Todos os dispositivos da casa usam a de 5 GHz. | Confidencial |
| 2 | Desktop | Ocasional | Dono da casa | No local | Guarda informações privadas, como fotos. | Restrito |
| 3 | Smartphone de visitante | Ocasional | Amigo | Dentro e fora do local | Conecta-se à rede da casa. | Somente interno |
| 4 | **Notebook de trabalho** | Ocasional | Dono da casa | Dentro e fora do local | Usado para tocar a empresa. Guarda arquivos de clientes, notas fiscais e senhas salvas. Também se conecta a Wi-Fi público fora de casa. | Restrito |
| 5 | **Impressora de rede** | Ocasional | Dono da casa | No local | Conecta-se por Wi-Fi. Imprime documentos da empresa e mantém os últimos trabalhos na memória. Ainda usa a senha de administrador padrão. | Confidencial |
| 6 | **Smart TV** | Contínuo | Dono da casa | No local | Tem contas de streaming logadas. É usada pela família e por visitas. Quase nunca recebe atualização. | Somente interno |

As linhas em negrito são as que eu adicionei.

## Categorias de sensibilidade

| Categoria | Quem pode acessar |
|---|---|
| Restrito | Só quem realmente precisa (need-to-know) |
| Confidencial | Usuários específicos |
| Somente interno | Quem está no local |
| Público | Qualquer pessoa |

## Por que classifiquei assim

Para cada dispositivo, me fiz as perguntas da atividade: que informação seria roubada, se um ataque poderia alterar os dados e o que aconteceria com a empresa se eles fossem destruídos.

- **Notebook de trabalho: Restrito.** É onde ficam os dados dos clientes e as senhas. Se ele for comprometido, a empresa perde informação que não é só dela, e pode até parar de funcionar. Ele também sai de casa e entra em redes que eu não controlo, então é o que corre mais risco. Só o dono deveria ter acesso.
- **Impressora de rede: Confidencial.** Eu não tinha pensado em impressora como um risco, mas os documentos passam por ela e ficam na memória. Não é tão crítica quanto o notebook, mas só as pessoas da empresa deveriam usá-la. A senha padrão é um problema que anotei para corrigir.
- **Smart TV: Somente interno.** Não guarda dados da empresa, e quem está em casa pode usar. Mesmo assim ela fica ligada na rede o tempo todo e quase não é atualizada, então pode servir de porta de entrada para os outros dispositivos.

## O que aprendi

- Não dá para proteger o que eu não sei que existe. O inventário é o primeiro passo de qualquer plano de segurança.
- A classificação mostra onde concentrar o esforço: nem tudo precisa do mesmo nível de proteção.
- Um dispositivo com pouca informação, como a TV, ainda importa, porque cada aparelho na rede é um possível ponto de entrada para os outros.
- Só de preencher a coluna de observações já apareceram melhorias: trocar a senha padrão da impressora, colocar a TV e os visitantes em uma rede separada e criptografar o disco do notebook.
