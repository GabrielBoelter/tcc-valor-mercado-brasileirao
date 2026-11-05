# Dados

Os dados brutos não são versionados neste repositório. São duas fontes abertas.

## 1. Transfermarkt (valores de mercado, transferências e atributos)

Conjunto **transfermarkt-datasets** (dcaribou): <https://github.com/dcaribou/transfermarkt-datasets>.
Versão usada: registros de valor até **12/06/2026** (acesso em 25/08/2026). Identidade dos
arquivos (o notebook compara automaticamente):

| Arquivo | SHA-256 | Uso |
|---|---|---|
| `players.csv` | `b94216cf2ff8777b5eddd4fd91402c19e757b366fbdeb4f4437580c302ff2d66` | nascimento, posição, pé, altura, nome |
| `player_valuations.csv` | `9caf4ff707905f71463a04253c5ac640b3ff9575e07912fd7c50365697efa47d` | valores (alvo, valor anterior, trajetória) |
| `transfers.csv` | `5331311c6ec68c2c7a9c22979aff7d6f6fd2cefd96201d1bcfb35998a9d244ae` | transferências e taxas |
| `clubs.csv` | `26dc6113d8206246145add2c7a318a657b3995fccdb0c9004d65ca6ae5e5968b` | liga e nome de cada clube |
| `competitions.csv` | `fadaf2992e01f8450d8d419006b0f533bbd6742163f4a0c5ee6a157fae7b791e` | confederação de cada liga |

Observações sobre a temporalidade: os atributos de `players.csv` são do cadastro no momento da
extração (2026). Em `player_valuations.csv`, `current_club_id` e
`player_club_domestic_competition_id` às vezes refletem o clube atual do atleta, e não o da
data do registro (o id coincide com o nome histórico do clube em 84,8% dos registros BRA1 de
2020 e em mais de 99% a partir de 2024; ver `results/coerencia_clube_valuations.csv`). Clubes
fora do BRA1 em 2026 (América-MG, Atlético-GO, Avaí, Criciúma, Cuiabá, Goiás) não constam de
`clubs.csv`.

## 2. Cartola FC (desempenho no Campeonato Brasileiro)

Repositório **caRtola** (Henrique Gomide), commit fixo
`d3fd24db8ae73e3277d2f8b673068efece3decd4` (09/10/2026), baixado pelo notebook em
`https://codeload.github.com/henriquepgomide/caRtola/tar.gz/<commit>`.

| Temporada | Arquivo | Disponível em | SHA-256 (início) |
|---|---|---|---|
| 2020 | `data/01_raw/2020/rodada-38.csv` | 25/02/2021 | ver `results/manifesto_execucao.json` |
| 2021 | `data/01_raw/2021/Mercado_39.txt` | 09/12/2021 | idem |
| 2022 | `data/01_raw/2022/rodada-38.csv` | 13/11/2022 | idem |
| 2023 | `data/01_raw/2023/rodada-38.csv` | 06/12/2023 | idem |
| 2024 | `data/01_raw/2024/rodada-38.csv` | 08/12/2024 | idem |
| 2025 | `data/01_raw/2025/rodada-38.csv` | 07/12/2025 | idem |
