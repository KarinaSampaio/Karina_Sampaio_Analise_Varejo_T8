# Análise Exploratória de Dados de Varejo com Python

**Simulado didático do Mini-Projeto — Módulo 1 — Turma T8**  
**Autoria do material:** Karina Sampaio

Projeto educacional de análise exploratória de dados de varejo com Python e pandas. O trabalho reúne leitura de CSV, diagnóstico de qualidade, limpeza justificada, funções reutilizáveis, estatísticas, filtros, agrupamentos e exportação dos dados tratados.

O objetivo é transformar dados brutos em informações compreensíveis e demonstrar como cada decisão de tratamento influencia a análise.

Os resultados apresentados neste README correspondem à aplicação das regras descritas à base de treinamento fornecida em aula.

## Objetivos

- Compreender o significado das linhas e das colunas da base.
- Identificar valores ausentes, textos em branco, duplicatas e diferenças de escrita.
- Aplicar regras de limpeza com justificativas explícitas.
- Criar e utilizar funções com parâmetros e retorno.
- Calcular estatísticas e responder a perguntas usando filtros e agrupamentos.
- Comunicar resultados, recomendações e limitações com base em evidências.
- Organizar uma análise que possa ser executada novamente a partir do CSV original.

## Tecnologias utilizadas

| Tecnologia | Aplicação |
|---|---|
| Python 3 | Programação e organização das etapas |
| pandas | Leitura, tratamento e análise dos dados |
| Google Colab | Execução do notebook e interação com os arquivos |
| GitHub e Markdown | Organização do projeto e documentação |

## Origem da base e unidade de análise

- **Fonte:** material de treinamento fornecido para a Aula 23 — Simulado do Mini-Projeto.
- **Arquivo de entrada:** `varejo_simulado_avaliacao.csv`.
- **Versão de referência:** arquivo de treinamento com 50 linhas e 8 colunas utilizado nesta atividade.
- **Finalidade:** prática educacional de análise de dados de varejo.
- **Arquivo de saída:** `varejo_simulado_limpo.csv`.

Cada linha descreve um registro comercial identificado por `registro_id`, associado a um cliente, categoria, valor, quantidade, cidade e canal.

O mesmo `cliente_id` pode aparecer em registros diferentes. Por isso, a contagem de registros e a contagem de identificadores de clientes são medidas distintas.

Os resultados se referem a esta base de treinamento. Eles não descrevem o desempenho de uma empresa real.

### Dicionário das colunas

| Coluna | Significado no exercício | Uso na análise |
|---|---|---|
| `registro_id` | Identificador do registro comercial | Conferência da unidade de observação e das repetições |
| `cliente_id` | Identificador do cliente associado ao registro | Contagem de identificadores distintos e conferência de consistência |
| `genero` | Rótulo de gênero preenchido no registro | Diagnóstico de ausências e diferenças entre registros do mesmo cliente |
| `categoria` | Categoria associada ao registro | Contagem de registros por categoria |
| `valor_compra` | Valor de compra registrado, apresentado em reais no exercício | Estatísticas, filtro e soma dos valores conhecidos |
| `quantidade` | Quantidade associada ao registro | Informação preservada na base; sua unidade deve ser confirmada antes de cálculos adicionais |
| `cidade` | Cidade informada no registro | Padronização de escrita |
| `canal` | Canal do registro: App, Loja ou Site | Agrupamento dos valores conhecidos |

O projeto utiliza `valor_compra` conforme registrado. Não foi aplicada multiplicação por `quantidade`, pois esse cálculo exigiria confirmar se o valor representa um preço unitário ou o total do registro.

## Organização em sprints

| Sprint | Etapa | Entrega |
|---|---|---|
| 1 | Leitura e diagnóstico | Estrutura da base e registro dos problemas encontrados |
| 2 | Limpeza e padronização | Decisões de tratamento e comparação antes/depois |
| 3 | Funções e organização | Funções de tratamento e resumo, com parâmetro e retorno |
| 4 | Análise | Quatro estatísticas, um filtro e dois agrupamentos interpretados |
| 5 | Exportação e comunicação | CSV tratado, conclusões, recomendação e limitação |
| 6 | Revisão e entrega | Execução completa, arquivos organizados e documentação |

## Diagnóstico inicial

| Verificação | Resultado |
|---|---|
| Quantidade de linhas | 50 |
| Quantidade de colunas | 8 |
| Identificadores de registros distintos | 48 |
| Identificadores de clientes distintos | 21 |
| Duplicatas completas excedentes | 2 |
| Registros com cópias completas | `R0003` e `R0012` |
| Valores ausentes em `valor_compra` | 2, nos registros `R0010` e `R0032` |
| Ausências reconhecidas por `isna()` nas demais colunas | 0 |
| Textos preenchidos apenas com espaços | 1, na coluna `genero` do registro `R0017` |
| Textos vazios ou compostos apenas por espaços nas demais colunas textuais | 0 |

Também foram identificadas diferenças de escrita, como:

- `moda `
- `ELETRÔNICOS`
- `rio de janeiro`
- `SÃO PAULO `

As duas duplicatas excedentes correspondem a quatro linhas envolvidas nas repetições: uma ocorrência original e uma cópia para cada um dos dois registros.

A remoção preserva uma ocorrência de cada registro.

### Consistência das informações por cliente

O cliente `C017` aparece em sete registros distintos:

| Gênero registrado | Quantidade de registros |
|---|---:|
| Feminino | 3 |
| Masculino | 2 |
| Não informado | 2 |
| **Total** | **7** |

Essa variação precisa ser esclarecida junto à origem dos dados antes de uma análise de perfil por cliente.

A repetição do identificador do cliente, isoladamente, não comprova duplicidade. Também não há evidência suficiente para substituir os rótulos pelo mais frequente.

Os registros foram preservados e a questão foi documentada como uma limitação.

## Decisões de limpeza

| Problema | Tratamento | Justificativa |
|---|---|---|
| Espaços no início e no fim dos textos | Remoção com `str.strip()` | Evitar rótulos diferentes apenas por espaços |
| Variações de maiúsculas e minúsculas | Padronização nas colunas `genero`, `categoria`, `cidade` e `canal` | Reunir escritas equivalentes nos mesmos grupos |
| Rótulos como `Rio De Janeiro` e `Não Informado` após a capitalização | Ajuste para `Rio de Janeiro` e `Não informado` | Manter a forma de escrita escolhida |
| Gênero preenchido apenas com espaço | Conversão para ausência e preenchimento com `Não informado` | Explicitar a falta da informação sem inferir um gênero |
| Dois valores de compra ausentes | Preservação das ausências | Não há evidência para recuperar os valores; zero representaria uma informação diferente |
| Duas cópias completas confirmadas | Remoção das ocorrências excedentes | Evitar contagem e soma repetidas |
| Rótulos de gênero diferentes para o mesmo cliente | Preservação e documentação | A escolha de um preenchimento exige esclarecimento da fonte |

A tabela original é preservada em `df_original`. A função de tratamento trabalha sobre uma cópia e devolve `df_limpo`.

### Comparação antes e depois

| Indicador | Antes | Depois |
|---|---:|---:|
| Linhas | 50 | 48 |
| Colunas | 8 | 8 |
| Duplicatas completas excedentes | 2 | 0 |
| Identificadores de clientes distintos | 21 | 21 |
| Valores de compra ausentes | 2 | 2 |
| Gênero preenchido apenas com espaço | 1 | 0 |
| Soma dos valores de compra conhecidos | R$ 34.178,11 | R$ 32.956,85 |

A remoção das cópias retirou **R$ 1.221,26** que estavam sendo somados duas vezes.

As ausências monetárias foram mantidas conforme a decisão documentada.

Após o tratamento, **13 registros** apresentam o rótulo `Não informado` em gênero.

## Funções utilizadas

| Função | Entrada | Responsabilidade | Retorno |
|---|---|---|---|
| `tratar_dados(tabela)` | DataFrame original | Padronizar textos, explicitar a ausência de gênero e remover as cópias completas confirmadas | Novo DataFrame tratado |
| `gerar_resumo(tabela)` | DataFrame tratado | Calcular contagem válida, média, mínimo e máximo de `valor_compra` | Dicionário com as quatro medidas |

As funções precisam ser definidas e chamadas no notebook.

Após a importação, o envio do CSV e a execução das células de definição das funções, o fluxo principal é:

```python
df_original = pd.read_csv(nome_arquivo)

df_limpo = tratar_dados(df_original)

resumo = gerar_resumo(df_limpo)

df_limpo.to_csv(
    "varejo_simulado_limpo.csv",
    index=False
)
```

## Resultados de referência

Os números abaixo correspondem à base indicada e às decisões de tratamento descritas neste README.

Eles servem para conferir a implementação completa do simulado.

### Estatísticas de valor de compra

| Medida | Resultado |
|---|---:|
| Contagem de valores válidos | 46 |
| Média | R$ 716,45 |
| Mínimo | R$ 88,90 |
| Máximo | R$ 1.479,64 |

Dos 48 registros tratados, 46 possuem valor de compra preenchido.

A média de **R$ 716,45** foi calculada com esses valores conhecidos. Os dois valores ausentes não foram substituídos por zero.

O menor valor conhecido é **R$ 88,90** e o maior é **R$ 1.479,64**. Essas medidas descrevem os registros presentes na base de treinamento.

### Filtro: valor de compra superior a R$ 500

O filtro aplicado foi:

```python
registros_acima_500 = df_limpo[
    df_limpo["valor_compra"] > 500
]
```

Foram encontrados **28 registros**, equivalentes a aproximadamente **60,87% dos 46 registros com valor conhecido**.

O limite de R$ 500 foi escolhido para a atividade didática.

Os registros com valor ausente não permitem verificar se a condição é atendida e não participam desse percentual.

### Agrupamento por categoria

| Categoria | Quantidade de registros |
|---|---:|
| Eletrônicos | 11 |
| Moda | 11 |
| Beleza | 10 |
| Alimentos | 9 |
| Casa | 7 |
| **Total** | **48** |

Eletrônicos e Moda apresentam as maiores contagens, com **11 registros cada**.

A operação utiliza `groupby()` com `size()` e conta linhas por categoria. A coluna `quantidade` não foi somada neste resumo.

### Agrupamento por canal

| Canal | Registros | Valores preenchidos | Soma dos valores conhecidos |
|---|---:|---:|---:|
| App | 18 | 16 | R$ 10.325,54 |
| Loja | 14 | 14 | R$ 10.971,23 |
| Site | 16 | 16 | R$ 11.660,08 |
| **Total** | **48** | **46** | **R$ 32.956,85** |

O App possui a maior quantidade de registros.

O Site apresenta a maior soma dos valores conhecidos.

A comparação das somas deve considerar que o App concentra os dois valores de compra ausentes.

No agrupamento:

- `size()` conta os registros.
- `count()` conta os valores preenchidos.
- `sum(min_count=1)` soma os valores disponíveis, mantendo a soma ausente caso um grupo não tenha nenhum valor válido.

## Principais conclusões

1. A remoção de duas cópias completas reduziu a base de **50 para 48 registros**, preservando **21 identificadores de clientes distintos**.
2. Existem **46 valores de compra preenchidos**, cuja média é de aproximadamente **R$ 716,45**.
3. Os valores conhecidos variam de **R$ 88,90 a R$ 1.479,64**.
4. Foram identificados **28 registros** com valor superior a **R$ 500**, correspondentes a **60,87% dos registros com valor conhecido**.
5. **Eletrônicos e Moda** apresentam as maiores contagens por categoria, com **11 registros cada**.
6. O **Site** apresenta a maior soma dos valores conhecidos, de **R$ 11.660,08**. O **App** possui **18 registros**, dos quais **16 têm valor preenchido**, o que limita a comparação entre canais.

## Recomendação

Conferir na origem os dois valores de compra ausentes do canal App antes de utilizar as somas para orientar comparações de desempenho.

Essa verificação deve buscar os valores efetivos dos registros, preservando a rastreabilidade de qualquer correção.

## Limitações

- As estatísticas monetárias utilizam somente os valores preenchidos. Os dois valores ausentes impedem conhecer a soma completa dos registros.
- Há rótulos de gênero diferentes em registros associados ao mesmo identificador de cliente. Essa variação precisa ser esclarecida antes de uma análise de perfil por cliente.
- A base é pequena e destinada ao treinamento. As conclusões descrevem esse conjunto e não devem ser generalizadas para uma população de consumidores.
- A definição de `valor_compra` e da unidade de `quantidade` precisa ser confirmada antes de criar métricas adicionais que combinem essas colunas.

## Leitura, transformação e gravação

A leitura do CSV é realizada com `pd.read_csv()`.

A transformação ocorre na função `tratar_dados()`, que trabalha sobre uma cópia da tabela original, padroniza os textos, explicita a ausência de gênero e remove as cópias completas confirmadas.

A gravação é feita com `to_csv()`, usando `index=False`.

A remoção das duas cópias reduziu a soma dos valores conhecidos de **R$ 34.178,11** para **R$ 32.956,85**, evitando a contagem repetida de **R$ 1.221,26**.

## Arquivos do projeto

Organize os arquivos abaixo na pasta principal do repositório:

| Arquivo | Conteúdo |
|---|---|
| `README.md` | Apresentação, decisões, resultados e instruções de execução |
| `Aula_23_Aluno_Simulado_MiniProjeto.ipynb` | Notebook com os seis sprints preenchidos, incluindo as definições e chamadas das funções |
| `varejo_simulado_avaliacao.csv` | Base original de treinamento |
| `varejo_simulado_limpo.csv` | Base tratada gerada pela execução do notebook |

O CSV original é o ponto de partida para reproduzir a análise.

O arquivo tratado deve ser gerado novamente pelo código, sem depender de alterações manuais.

## Como executar no Google Colab

1. Obtenha os arquivos do repositório, incluindo o notebook e o CSV original.
2. Abra no Google Colab a versão preenchida de `Aula_23_Aluno_Simulado_MiniProjeto.ipynb`.
3. Execute as células em ordem, começando pelas importações.
4. Quando a célula com `files.upload()` solicitar um arquivo, envie somente `varejo_simulado_avaliacao.csv`.
5. Execute o diagnóstico, as definições das funções e suas chamadas.
6. Confira a tabela tratada e execute as células de estatísticas, filtro e agrupamentos.
7. Execute a exportação para gerar `varejo_simulado_limpo.csv` com `index=False`.
8. Releia o CSV gerado e confira as dimensões esperadas: **48 linhas e 8 colunas**.
9. Execute a célula de download, se desejar guardar o arquivo tratado no computador.

O envio de apenas um CSV evita selecionar o arquivo errado quando a leitura utiliza `next(iter(uploaded))`.

Ao reiniciar a sessão, envie novamente o CSV original quando solicitado.

### Conferência da execução

- O notebook executa desde a leitura do arquivo original.
- As duas funções têm parâmetro e retorno e são efetivamente chamadas.
- As análises utilizam `df_limpo`.
- A base tratada apresenta 48 linhas, 8 colunas e nenhuma duplicata completa excedente.
- Permanecem dois valores de compra ausentes, conforme a decisão documentada.
- O CSV exportado não contém uma coluna adicional de índice.
- Os resultados do notebook correspondem às conclusões registradas neste README.

## Referências

- Material da Aula 23 — Simulado do Mini-Projeto, Turma T8.
- Roteiro didático de limpeza, funções, análise e comunicação desenvolvido para a atividade.
- [Documentação do pandas](https://pandas.pydata.org/docs/).

## Autoria

**Karina Sampaio** — material didático para a turma T8.

[Perfil no GitHub](https://github.com/KarinaSampaio)
