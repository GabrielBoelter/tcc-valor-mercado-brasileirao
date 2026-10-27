# Pré-registro dos critérios de trajetória de carreira

Indicadores calculados apenas com o histórico de valor de mercado dos **3 anos anteriores à data
de corte** (31/12 da temporada), com no mínimo **4 registros**. O histórico serve só para
classificar as observações; não é entrada dos modelos.

| Indicador | Definição |
|---|---|
| inclinação | coeficiente angular de log(valor) contra o tempo, em anos |
| volatilidade | desvio-padrão das diferenças sucessivas de log(valor) |
| queda máxima | maior queda proporcional em relação ao valor máximo anterior |

## Regras (aplicadas nesta ordem)

| Ordem | Grupo | Versão 1 (original) | Versão 2 (13/10/2026) |
|---|---|---|---|
| 1 | Sem histórico | menos de 4 registros | igual |
| 2 | Volátil | volatilidade ≥ 0,40 **ou** queda ≥ 0,35 | volatilidade ≥ 0,40 **ou** queda ≥ 0,50 |
| 3 | Ascensão | inclinação ≥ 0,15 | igual |
| 4 | Estagnação ou declínio | inclinação < 0,15 | igual |

## Histórico das versões

- **Versão 1:** metade da amostra caía no grupo volátil, e o grupo de ascensão rápida
  (inclinação ≥ 0,60 e idade < 23) tinha 25 casos. Aplicou-se o plano B previsto no próprio
  documento: as ascensões foram fundidas em um único grupo.
- **Versão 2:** o limiar de queda passou ao percentil 75 observado (0,50), escolhido pela
  distribuição dos grupos. Como a alteração ocorreu após rodadas exploratórias, **as duas
  versões são reportadas** (Seção 6 do notebook); a conclusão sobre o viés por grupo é a mesma
  nas duas.
