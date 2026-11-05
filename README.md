# Valor de mercado de jogadores do Brasileirão por aprendizado de máquina

Pipeline do Trabalho de Conclusão de Curso de **Gabriel C. Boelter** (Centro Universitário UniSATC, 2026).

**Pergunta:** em que condições um modelo de aprendizado de máquina consegue estimar com
confiabilidade o valor de mercado de jogadores do Campeonato Brasileiro, e o que o confronto
entre essas estimativas e as taxas efetivamente pagas revela sobre a precificação das
transferências para a Europa?

Versão **v23.2** (05/11/2026), que responde à revisão do orientador de 03/11/2026 (itens 01 a 15).
Ambiente de referência: Google Colab, Python 3.13.18 (versões em `requirements.txt`; o notebook avisa se o ambiente for diferente).

## Como reproduzir

1. Obtenha os cinco CSVs do Transfermarkt e confira os hashes SHA-256 em `data/README.md`.
2. **Colab:** coloque os CSVs em `/content/drive/MyDrive/TCC` e execute o notebook do início.
   **Local:** defina `TCC_DADOS` (pasta dos CSVs), `TCC_CKPT` (pasta de checkpoints) e, se já
   tiver um clone, `TCC_CARTOLA`; instale `requirements.txt`.
3. O repositório do Cartola é baixado no commit fixo `d3fd24db8ae73e3277d2f8b673068efece3decd4`.
4. Cada checkpoint tem um manifesto (versão do código, cortes, hashes dos dados e do Cartola);
   se não coincidir com a execução atual, o checkpoint é descartado e recalculado. A pasta `saidas`
   é limpa no início de cada execução e sempre regenerada.

O notebook está salvo **com as saídas** da execução de referência, feita do zero em ambiente
limpo. Todas as saídas tabulares estão em `results/`, incluindo `predicoes_externas.csv`
(todas as predições de teste por configuração, jogador e temporada), a partir das quais as
tabelas do artigo podem ser regeneradas sem retreinar. O manifesto da execução está em
`results/manifesto_execucao.json`.

## Desenho

| Etapa | Seção | O que faz |
|---|---|---|
| Ambiente e manifesto | 0 | hashes dos dados, versões, regras de corte, validação de checkpoints |
| Cartola | 1 | último arquivo de cada temporada (2020–2025), commit fixo |
| Pareamento | 2 | nome e apelido, restrito aos jogadores do BRA1 no ano; ambíguos e colisões excluídos e registrados |
| Corte, alvo e variáveis | 3 | corte = max(31/12, dia seguinte à última rodada); 2020 → 26/02/2021; verificação entrada ≤ corte < alvo |
| Motor de modelagem | 4 | todas as configurações com as mesmas partições externas e internas, grade e critério; estimadores clonados |
| Resultados | 4B | Tab. 2–5 a partir de uma única base de predições; bootstrap por jogador; ganho por ano; sentido da revisão; AUC |
| Outliers / treino × teste | 4C, 4D | Tab. 6 e 9, Fig. 1, 2, 3 e 5 |
| Exportações | 5 | temporada de referência pelo corte; modelos sem os atletas exportados; funil com causas |
| Estratificação | 6 | trajetória (limiares v1 e v2), posição e faixa de valor |
| SHAP | 7 | modelo principal de 2025 (Fig. 4) |
| Predição conforme | 8 | ajuste < T−1, calibração T−1, teste T, para T = 2022–2025; D1 e persistência |
| Auditorias | 10 | pareamento (clube nas duas fontes), sensibilidade sem pares incertos, seleção |
| Manifesto | 11 | grava o manifesto da execução |

**Validação externa:** treino nas temporadas anteriores, teste na seguinte (2022 a 2025; 882
observações de 460 atletas), sem atletas do teste no treino. **Validação interna:** dentro do
treino, cada temporada (exceto a primeira) valida um modelo treinado com as anteriores, sem
atletas comuns; sem partição válida (mínimo de 30 observações), regra fixa.

## Amostra

| Etapa | n |
|---|---|
| Jogadores-temporada no Cartola FC (3+ jogos) | 3.424 |
| Pareados sem ambiguidade detectada | 1.425 |
| Com valor-alvo válido (modelagem) | 1.185 (513 jogadores) |
| Exportações Brasileirão → Europa avaliadas | 50 de 110 no período elegível (149 no total) |

## Principais resultados (882 observações de teste)

| Estratégia | R² (log) | RMSE log | RMSE (€ mi) | MAE (€ mi) | MdAPE | Ganho de MSE log (IC 95%) |
|---|---|---|---|---|---|---|
| Persistência | 0,942 | 0,270 | 0,95 | 0,48 | 12,0% | — |
| Nível – Ridge | 0,322 | 0,924 | 3,93 | 2,24 | 54,8% | — |
| **Variação D1** | **0,950** | **0,251** | **0,95** | **0,51** | **10,0%** | **13,5% (9,7 a 17,3)** |

- Ganho por ano de teste: 8,5% (2022), −0,3% (2023), 15,4% (2024), 17,8% (2025); sem 2020 no
  treino, 8,8% (3,7 a 13,4).
- O D1 erra menos que a persistência em 44,4% das observações; em euros, empata no RMSE e tem
  MAE maior.
- Entre os casos com revisão superior ao limiar (|Δ log| > 0,01), acerta o sentido em 76,8%,
  contra 55,0% de uma regra aprendida no treino (diferença +21,8 p.p., IC 17,1 a 26,8).
- Classificador de revisão: AUC 0,555 (0,519 a 0,592) — baixa capacidade discriminatória.
- Intervalos de 90%: cobertura de 91,6% (84,4% a 96,8% entre os anos).
- Exportações (modelos sem os exportados): taxa / estimativa = 1,08 (mediana).

## Revisão do pareamento (05/11/2026)

Amostra de 80 pares (40 aceitos e 40 incertos pela auditoria de clubes), revisada com fontes
públicas (data de nascimento e histórico de clubes): `results/revisao_pareamento_amostra80.csv`.

| Grupo | Corretos | Errados | Taxa de erro (IC 95%) |
|---|---|---|---|
| Aceitos (clube coincide) | 40 | 0 | 0% (0 a 8,8%) |
| Incertos (clube diverge) | 9 | 31 | 77,5% (61,5% a 89,2%) |

Os erros são homônimos que o Transfermarkt associa a clubes brasileiros pelo clube atual. A
análise sem os 41 pares incertos mantém o ganho do D1 (12,2%, IC 8,2 a 16,5).

## Decisões metodológicas

Ver `docs/registro_decisoes_metodologicas.md`. Os limiares de trajetória **não** foram
pré-registrados de forma verificável; a versão 2 foi ajustada após exploração.

## Limitações

Alvo é a estimativa do Transfermarkt; pareamento nominal sem confirmação independente (41 pares
com clube divergente marcados como incertos); seleção favorece atletas com mais jogos e de
clubes do BRA1 no retrato de 2026; em parte dos registros antigos de `player_valuations`, o id
do clube é o do clube atual; atributos cadastrais de 2026; a base de transferências não
distingue empréstimos e registra a data de execução.
