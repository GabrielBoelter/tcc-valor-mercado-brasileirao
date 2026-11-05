# Registro de decisões metodológicas

Este documento registra decisões tomadas ao longo do trabalho. **Não é um pré-registro**: o
histórico público do repositório começa em 27/10/2026, e não há evidência datada e verificável
de que as regras abaixo tenham sido fixadas antes das análises. A cronologia separa regras
definidas antes de examinar o erro por grupo, mudanças exploratórias e escolhas posteriores à
observação de resultados.

## Critérios de trajetória de carreira (Seção 6)

Indicadores calculados apenas com valores publicados nos **3 anos anteriores à data de corte**
da temporada (`CORTE[t]`), com no mínimo **4 registros**. O histórico só classifica as
observações; não é entrada dos modelos.

| Indicador | Definição |
|---|---|
| inclinação | coeficiente angular de log(valor) contra o tempo, em anos |
| volatilidade | desvio-padrão das diferenças sucessivas de log(valor) |
| queda máxima | maior queda proporcional em relação ao valor máximo anterior |

| Ordem | Grupo | Versão 1 | Versão 2 |
|---|---|---|---|
| 1 | Sem histórico | menos de 4 registros | igual |
| 2 | Volátil | volatilidade ≥ 0,40 **ou** queda ≥ 0,35 | volatilidade ≥ 0,40 **ou** queda ≥ 0,50 |
| 3 | Ascensão | inclinação ≥ 0,15 | igual |
| 4 | Estagnação ou declínio | inclinação < 0,15 | igual |

## Cronologia

| Momento | Decisão | Natureza |
|---|---|---|
| antes de examinar o erro por grupo | indicadores, janela de 3 anos, mínimo de 4 registros e versão 1 dos limiares | regra definida antes da análise por grupo (sem registro verificável) |
| após ver a distribuição da versão 1 | fusão das ascensões (o grupo de ascensão rápida tinha 25 casos) | mudança exploratória, pela distribuição |
| após ver a distribuição dos indicadores | versão 2: limiar de queda no P75 observado (0,50) | ajuste posterior à exploração dos dados |
| sempre | as duas versões são reportadas lado a lado | — |
| 05/11/2026 | revisão documentada de 80 pares (40 aceitos, 40 incertos): 0/40 e 31/40 errados; análise principal mantida com todos os pares, sensibilidade sem os incertos reportada | decisão posterior à observação dos resultados da revisão |
| 03/11/2026 (revisão do orientador) | corte por temporada, validação interna temporal, estimadores clonados, base única de predições, bootstrap por jogador como inferência principal, referência do sentido aprendida no treino, modelos sem os exportados, conformal por jogador e tempo | correções metodológicas; resultados regenerados na v23 |

## Outras decisões mantidas

- Janela do alvo: 182 dias após o corte, com o jogador ainda no BRA1.
- Especificação principal: D1 (variação), com idade, altura, posição, pé, jogos, pontuação,
  gols, assistências, desarmes, faltas sofridas e cometidas.
- Limiar de revisão do valor: |Δ log| > 0,01.
- Preço do Cartola fora da especificação principal (só em sensibilidade).
