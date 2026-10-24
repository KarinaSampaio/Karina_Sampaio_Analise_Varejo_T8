# Mini-Projeto de Análise de Dados de Varejo

**Simulado guiado — Módulo 1 — Turma T8**  
**Autoria do material:** Karina Sampaio

Este projeto apresenta uma análise exploratória de uma base de treinamento de varejo utilizando Python e pandas. O desenvolvimento foi realizado no Google Colab, com leitura do CSV, diagnóstico, limpeza, funções reutilizáveis, estatísticas, filtros, agrupamentos e exportação.

Este README documenta o notebook `Aula_23_Aluno_Simulado_MiniProjeto (1).ipynb` e os resultados calculados em suas células. O objetivo é explicar o processo e as decisões de tratamento, permitindo reproduzir e compreender a análise.

## Objetivos

- Identificar problemas de qualidade e justificar o tratamento dos dados.
- Distinguir registros comerciais de identificadores de clientes.
- Organizar o tratamento e o resumo numérico em funções reutilizáveis.
- Investigar os valores de compra e sua distribuição entre categorias e canais.
- Comunicar conclusões, uma recomendação e as limitações da análise.
- Gerar um CSV tratado a partir da base original.

## Tecnologias utilizadas

- **Python 3:** programação e definição das funções.
- **pandas:** leitura, limpeza, filtros, estatísticas e agrupamentos.
- **Google Colab:** execução do notebook e envio do CSV.
- **Markdown e GitHub:** documentação e organização da entrega.

## Origem e significado dos dados

A base `varejo_simulado_avaliacao.csv` foi fornecida como material de treinamento da Aula 23. A versão utilizada possui **50 linhas e 8 colunas** e é destinada à prática educacional.

Cada linha descreve um registro comercial identificado por `registro_id`, associado a um cliente, categoria, valor de compra, quantidade, cidade e canal. Um mesmo cliente pode aparecer em vários registros.

| Coluna | Significado no exercício |
|---|---|
| `registro_id` | Identificador do registro comercial |
| `cliente_id` | Identificador do cliente associado ao registro |
| `genero` | Rótulo de gênero informado no registro |
| `categoria` | Categoria associada ao registro |
| `valor_compra` | Valor de compra registrado, apresentado em reais no exercício |
| `quantidade` | Quantidade associada ao registro, preservada na base |
| `cidade` | Cidade informada |
| `canal` | Canal do registro: App, Loja ou Site |

Os cálculos monetários utilizam `valor_compra` conforme registrado. A multiplicação por `quantidade` não foi aplicada, pois exigiria confirmar se o valor é unitário ou corresponde ao total do registro.

## Etapas desenvolvidas

| Sprint | Atividade |
|---|---|
| 1 — Leitura e diagnóstico | Importação, estrutura, tipos, ausências, espaços, duplicatas e diferenças de escrita |
| 2 — Limpeza e padronização | Definição das decisões de tratamento e suas justificativas |
| 3 — Funções | Implementação e chamada das funções de tratamento e resumo |
| 4 — Análise | Estatísticas, dois filtros e agrupamentos por categoria e canal |
| 5 — Exportação e conclusões | Geração e releitura do CSV tratado, interpretação e comunicação |
| 6 — Documentação | Organização do README e preparação dos arquivos para o GitHub |

## Diagnóstico inicial

| Verificação | Resultado |
|---|---|
| Linhas | 50 |
| Colunas | 8 |
| Identificadores de registros distintos | 48 |
| Identificadores de clientes distintos | 21 |
| Duplicatas completas excedentes | 2, referentes a `R0003` e `R0012` |
| Ausências em `valor_compra` | 2, nos registros `R0010` e `R0032` |
| Ausências reconhecidas por `isna()` nas demais colunas | 0 |
| Texto preenchido apenas com espaço | 1, em `genero`, no registro `R0017` |
| Textos vazios ou apenas com espaços nas demais colunas textuais | 0 |

Foram encontradas variações como `moda `, `ELETRÔNICOS`, `rio de janeiro` e `SÃO PAULO `.

### Consistência das informações do cliente

A análise do cliente `C017` identificou sete registros distintos: três com gênero Feminino, dois com Masculino e dois com Não informado.

Como a origem dessa variação não foi esclarecida, os valores foram preservados. O projeto utiliza categoria e canal nos agrupamentos, mantendo a divergência de gênero documentada como limitação para uma análise de perfil por cliente.

A repetição de `cliente_id`, por si só, não comprova duplicidade. As remoções consideram as repetições completas confirmadas.

## Decisões de tratamento

| Problema | Decisão | Justificativa |
|---|---|---|
| Espaços nas extremidades dos textos | Aplicar `str.strip()` | Evitar diferenças causadas apenas por espaços |
| Maiúsculas e minúsculas diferentes | Aplicar `str.title()` nas colunas textuais selecionadas | Padronizar a capitalização dos rótulos |
| Gênero preenchido apenas com espaço | Converter para ausência e preencher com `Não informado` | Explicitar a falta da informação sem inferir um gênero |
| Rótulo `Não Informado` após a capitalização | Ajustar para `Não informado` | Manter a forma de escrita escolhida |
| Valores de compra ausentes | Manter as ausências | Não há evidência para recuperar os valores; preencher com zero alteraria seu significado |
| Duas cópias completas | Remover as ocorrências excedentes | Evitar contagem e soma repetidas |
| Gênero variando entre registros do mesmo cliente | Preservar e documentar | A definição de um preenchimento exige consulta à origem |

O DataFrame `df_original` é preservado. O tratamento utiliza uma cópia e produz `df_limpo`.

## Funções implementadas

| Função | Entrada | Processamento | Retorno |
|---|---|---|---|
| `tratar_dados(tabela)` | DataFrame original | Padronização textual, tratamento da ausência de gênero e remoção de duplicatas completas | DataFrame tratado |
| `gerar_resumo(tabela)` | DataFrame tratado | Contagem válida, média, mínimo e máximo de `valor_compra` | Dicionário com as quatro medidas |

Após definir as funções, o notebook executa:

```python
df_limpo = tratar_dados(df_original)
resumo = gerar_resumo(df_limpo)
```

## Comparação antes e depois

| Indicador | Antes | Depois |
|---|---:|---:|
| Linhas | 50 | 48 |
| Colunas | 8 | 8 |
| Duplicatas completas excedentes | 2 | 0 |
| Identificadores de clientes distintos | 21 | 21 |
| Valores de compra ausentes | 2 | 2 |
| Soma dos valores de compra conhecidos | R$ 34.178,11 | R$ 32.956,85 |

A remoção das duas cópias retirou **R$ 1.221,26** que estavam sendo somados duas vezes. As duas ausências em `valor_compra` foram mantidas conforme a decisão documentada.

## Análises e resultados

### Estatísticas de valor de compra

| Medida | Resultado |
|---|---:|
| Contagem de valores válidos | 46 |
| Média | R$ 716,45 |
| Mínimo | R$ 88,90 |
| Máximo | R$ 1.479,64 |

Dos 48 registros tratados, 46 possuem valor de compra preenchido. A média de R$ 716,45 considera esses valores conhecidos.

O menor valor conhecido é R$ 88,90 e o maior é R$ 1.479,64. Os identificadores dos registros e dos clientes não foram utilizados como medidas numéricas.

### Filtro 1 — Valores de compra acima da média

O notebook seleciona os registros cujo valor supera a média calculada e exibe seus identificadores e valores:

```python
registros_acima_media = df_limpo.loc[
    df_limpo["valor_compra"] > resumo["media"],
    ["registro_id", "cliente_id", "valor_compra"]
]
```

**Resultado: 22 registros acima da média.**

O filtro utiliza a média completa armazenada em `resumo["media"]`. O arredondamento para R$ 716,45 é usado na apresentação do resultado.

### Filtro 2 — Valores de compra acima de R$ 600

```python
registros_acima_600 = df_limpo[
    df_limpo["valor_compra"] > 600
]
```

**Resultado: 26 registros com valor de compra superior a R$ 600.**

Nos dois filtros, os registros sem valor de compra conhecido não atendem à seleção. As contagens representam registros, não necessariamente clientes distintos.

### Agrupamentos por categoria

O notebook calcula a quantidade de registros e a soma dos valores conhecidos por categoria. A tabela abaixo reúne os dois resultados:

| Categoria | Registros | Soma dos valores conhecidos |
|---|---:|---:|
| Moda | 11 | R$ 7.620,55 |
| Beleza | 10 | R$ 7.198,61 |
| Eletrônicos | 11 | R$ 6.709,92 |
| Alimentos | 9 | R$ 6.373,60 |
| Casa | 7 | R$ 5.054,17 |
| **Total** | **48** | **R$ 32.956,85** |

Moda e Eletrônicos estão empatadas na maior quantidade de registros, com **11 cada**. Moda apresenta a maior soma dos valores conhecidos, de **R$ 7.620,55**.

A contagem utiliza `size()` e mede linhas por categoria. A coluna `quantidade` não foi somada nesse resumo.

### Agrupamentos por canal

O notebook apresenta a contagem de registros por canal e um resumo com registros, valores preenchidos e soma dos valores conhecidos:

| Canal | Registros | Valores preenchidos | Soma dos valores conhecidos |
|---|---:|---:|---:|
| App | 18 | 16 | R$ 10.325,54 |
| Loja | 14 | 14 | R$ 10.971,23 |
| Site | 16 | 16 | R$ 11.660,08 |
| **Total** | **48** | **46** | **R$ 32.956,85** |

O App apresenta a maior quantidade de registros, com **18**. O Site apresenta a maior soma dos valores conhecidos, de **R$ 11.660,08**.

Os dois valores de compra ausentes estão no App. Por isso, a comparação das somas deve considerar que esse canal possui 18 registros, mas somente 16 valores preenchidos.

## Principais conclusões

1. A remoção de duas cópias completas reduziu a base de **50 para 48 registros**, mantendo **21 identificadores de clientes distintos**.
2. Existem **46 valores de compra preenchidos**, com média de **R$ 716,45**, mínimo de **R$ 88,90** e máximo de **R$ 1.479,64**.
3. Foram identificados **22 registros com valor de compra acima da média**.
4. Foram encontrados **26 registros com valor de compra superior a R$ 600**.
5. **Moda e Eletrônicos** apresentam as maiores contagens por categoria, com **11 registros cada**. **Moda** possui a maior soma dos valores conhecidos, de **R$ 7.620,55**.
6. O **App** possui a maior contagem por canal, com **18 registros**. O **Site** apresenta a maior soma dos valores conhecidos, de **R$ 11.660,08**. A soma do App considera somente **16 valores preenchidos**.

## Recomendação

Conferir na origem os dois valores de compra ausentes do canal App antes de utilizar as somas para orientar uma comparação de desempenho. As correções devem utilizar informações verificadas dos registros originais.

## Limitações

- As estatísticas monetárias utilizam apenas os valores preenchidos. As duas ausências impedem conhecer a soma completa dos registros.
- Há rótulos de gênero diferentes em registros associados ao mesmo cliente, exigindo esclarecimento antes de uma análise de perfil por cliente.
- A base é destinada ao treinamento. As conclusões descrevem esse conjunto e não devem ser generalizadas para uma população de consumidores.
- A definição de `valor_compra` e da unidade de `quantidade` precisa ser confirmada antes de criar métricas adicionais que combinem essas colunas.

## Explicação do processo

A leitura do CSV foi realizada com `pd.read_csv()`. A transformação ocorreu na função `tratar_dados()`, que padronizou textos, explicitou a ausência de gênero e removeu duas cópias completas. A gravação foi realizada com `to_csv()`, usando `index=False`. A remoção das cópias reduziu a base de 50 para 48 registros e a soma dos valores conhecidos de R$ 34.178,11 para R$ 32.956,85, evitando a soma repetida de R$ 1.221,26.

## Exportação e conferência

A base tratada é salva com:

```python
df_limpo.to_csv(
    "varejo_simulado_limpo.csv",
    index=False
)
```

O notebook relê o arquivo exportado e confere suas dimensões e colunas. O resultado obtido é **48 linhas e 8 colunas**, sem uma coluna adicional de índice.

## Arquivos do projeto

| Arquivo | Finalidade |
|---|---|
| `README.md` | Documentação do projeto |
| `Aula_23_Aluno_Simulado_MiniProjeto (1).ipynb` | Notebook com os códigos, resultados e interpretações |
| `varejo_simulado_avaliacao.csv` | Base original de treinamento |
| `varejo_simulado_limpo.csv` | Base tratada gerada pelo notebook |

O CSV original deve acompanhar o projeto para permitir a reprodução dos resultados. O CSV tratado pode ser gerado novamente pela execução do código.

## Como executar no Google Colab

1. Obtenha o notebook e o CSV original disponibilizados no repositório.
2. Abra `Aula_23_Aluno_Simulado_MiniProjeto (1).ipynb` no Google Colab.
3. Execute a primeira célula e envie somente `varejo_simulado_avaliacao.csv` quando solicitado.
4. Execute as demais células de código em ordem, incluindo as definições e as chamadas das funções.
5. Confira o diagnóstico, a comparação antes/depois, os filtros e os agrupamentos.
6. Execute a célula de exportação para gerar `varejo_simulado_limpo.csv`.
7. Execute a releitura do CSV e confirme as dimensões de **48 linhas e 8 colunas**.
8. Baixe o CSV gerado pelo painel de arquivos do Colab e inclua-o na entrega.

O envio de apenas um CSV evita selecionar o arquivo errado quando a leitura utiliza `next(iter(uploaded))`. Em uma nova sessão, envie novamente a base original quando solicitado.

## Referências

- Notebook da Aula 23 — Simulado Guiado do Mini-Projeto, Turma T8.
- Base de treinamento fornecida em aula: `varejo_simulado_avaliacao.csv`.
- [Documentação do pandas](https://pandas.pydata.org/docs/).

## Autoria

**Karina Sampaio** — material didático para a turma T8.  
[Perfil no GitHub](https://github.com/KarinaSampaio)
