# Valor de mercado de jogadores do Brasileirão por aprendizado de máquina

Pipeline do Trabalho de Conclusão de Curso de **Gabriel C. Boelter** (Centro Universitário UniSATC, 2026).

**Pergunta:** em que condições um modelo de aprendizado de máquina consegue estimar com
confiabilidade o valor de mercado de jogadores do Campeonato Brasileiro, e o que o confronto
entre essas estimativas e as taxas efetivamente pagas revela sobre a precificação das
transferências para a Europa?

## Como reproduzir

1. Abra `notebooks/pipeline_valor_mercado_brasileirao.ipynb` no Google Colab.
2. Coloque os arquivos do Transfermarkt na pasta definida em `DADOS` (ver `data/README.md`).
3. Execute as células em ordem. Os dados do Cartola FC são baixados automaticamente, e os
   resultados intermediários são salvos em checkpoints validados (pasta `CKPT`).

A semente aleatória é fixa (`RANDOM_STATE = 42`). O notebook está salvo **com as saídas** da
execução de referência, para que os resultados possam ser lidos sem reexecutar.

## Desenho

| Etapa | Seção do notebook | O que faz |
|---|---|---|
| Base de desempenho | 1 | lê o último arquivo de cada temporada do Cartola FC (2020–2025) |
| Pareamento | 2 | casa Cartola e Transfermarkt por nome e apelido, restrito aos jogadores do Brasileirão no ano; descarta nomes ambíguos e colisões |
| Alvo | 3 | primeiro valor de mercado após 31/12, em até 182 dias, ainda em clube do Brasileirão; auditoria de ordem temporal |
| Modelos de nível | 4 | média, Ridge, Random Forest e XGBoost estimam o valor sem o valor anterior |
| Modelo por variação | 4B | Ridge prevê a variação do log do valor a partir do último valor publicado; referência: persistência |
| RMSE e outliers | 4C | painel completo de métricas e RMSE com e sem os jogadores de maior valor |
| Exportações | 5 | aplica o modelo às vendas Brasileirão → Europa e compara com as taxas pagas |
| Estratificação | 6 | erro por trajetória de carreira, posição e faixa de valor |
| Interpretação | 7 | valores SHAP, global e por posição |
| Incerteza | 8 | predição conforme (*split conformal*), 90% |
| Casos individuais | 9 | o modelo aplicado a jogadores reais, selecionados por regra fixa |

**Validação:** partições por temporada (treino nos anos anteriores, teste no seguinte; testes
de 2022 a 2025, 882 observações), sem o mesmo jogador no treino e no teste. Todo o
pré-processamento fica dentro de `Pipeline` do scikit-learn.

## Amostra

| Etapa | n |
|---|---|
| Jogadores-temporada no Cartola FC (3+ jogos) | 3.424 |
| Pareados sem ambiguidade com o Transfermarkt | 1.425 |
| Com valor-alvo válido (amostra de modelagem) | 1.195 (517 jogadores) |
| Exportações Brasileirão → Europa avaliadas | 50 de 149 |

## Principais resultados (882 observações de teste)

| Estratégia | R² (log) | RMSE (€ mi) | RMSE (log) | MAE (€ mi) | MdAPE |
|---|---|---|---|---|---|
| Persistência (repetir o último valor) | 0,942 | 0,95 | 0,270 | 0,48 | 12,0% |
| Nível – Ridge (só desempenho) | 0,347 | 3,87 | 0,907 | 2,19 | 54,4% |
| Nível – Random Forest | 0,317 | 4,27 | 0,927 | 2,33 | 56,9% |
| Nível – XGBoost | 0,263 | 4,45 | 0,964 | 2,42 | 56,4% |
| **Variação D1 (modelo principal)** | **0,948** | **1,10** | **0,256** | **0,56** | **10,4%** |

- **Sem o valor anterior**, o desempenho explica cerca de um terço da variação do valor.
- **Com o valor anterior**, o modelo reduz em 10,1% o erro quadrático da persistência em escala
  logarítmica (IC 95% por bootstrap por jogador: 6,2% a 13,9%; Diebold-Mariano p < 0,001) e
  acerta o sentido da revisão do valor em 72,0% dos casos (referência: 61,0%).
- **Em euros**, o RMSE do modelo é um pouco maior que o da persistência: poucos jogadores caros
  concentram o erro (1% das observações responde por cerca de 45% do erro quadrático) e o
  modelo exagera as valorizações dos jovens mais valiosos. Ver `figures/rmse_outliers.png`.
- **Incerteza:** intervalo de 90% equivalente a multiplicar e dividir a estimativa por 1,67.
- **Taxas reais:** a estimativa reproduz o nível mediano das taxas pagas (razão 1,03), mas não
  fica mais próxima de cada taxa do que o valor anterior.

## Decisões metodológicas

As decisões e os critérios de trajetória estão registrados em `docs/pre_registro_trajetorias.md`
e no cabeçalho do notebook, que lista todas as correções feitas ao longo das versões.

## Limitações

O alvo é a estimativa coletiva do Transfermarkt, e não um preço de transação; o pareamento por
nome deixa parte dos atletas de fora; a base de transferências não distingue empréstimos e
registra a data de execução, não a do acordo; goleiros, atletas sem histórico e exportações
formam grupos pequenos.

## Referência principal

MÜLLER, O.; SIMONS, A.; WEINMANN, M. Beyond crowd judgments: data-driven estimation of market
value in association football. *European Journal of Operational Research*, v. 263, n. 2,
p. 611-624, 2017.
