# Dados

Os dados não são versionados neste repositório. São duas fontes abertas.

## 1. Transfermarkt (valores de mercado, transferências e atributos)

Conjunto **transfermarkt-datasets** (dcaribou), versão com registros até junho de 2026:
<https://github.com/dcaribou/transfermarkt-datasets>

Arquivos usados pelo notebook (formato CSV):

| Arquivo | Uso |
|---|---|
| `players.csv` | data de nascimento, posição, pé, altura, nome |
| `player_valuations.csv` | histórico de valores de mercado (alvo, valor anterior, trajetória) |
| `transfers.csv` | transferências e taxas pagas (amostra de aplicação) |
| `clubs.csv` | liga de cada clube |
| `competitions.csv` | confederação de cada liga |

Coloque esses cinco arquivos na pasta indicada pela variável `DADOS` na Seção 0 do notebook
(no Colab, por padrão, `/content/drive/MyDrive/TCC`).

## 2. Cartola FC (desempenho no Campeonato Brasileiro)

Repositório **caRtola** (Henrique Gomide): <https://github.com/henriquepgomide/caRtola>

O notebook baixa o repositório automaticamente na Seção 1. Temporadas usadas: 2020 a 2025.
O arquivo de 2021 está em JSON (latin-1) e é convertido para o mesmo formato das demais.
