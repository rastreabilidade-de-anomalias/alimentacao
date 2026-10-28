# SIREA — Sistema Inteligente de Rastreabilidade e Estabilização de Anomalias

Aplicativo web (arquivo único, sem servidor) para a coordenação de alimentação: registra anomalias, investiga causas, rastreia pontos de falha, acompanha ações e controles, mede a estabilidade (carta U) e estima a probabilidade de ocorrências futuras (Poisson / binomial negativa).

## Como usar

1. Abra `index.html` no navegador (ou publique com GitHub Pages: *Settings → Pages → Deploy from a branch → main / root*).
2. Em **Base Histórica → Importar planilha**, envie o arquivo Excel/CSV com as ocorrências e as refeições.
3. Navegue pelas análises: Visão Geral, Previsão e Risco, Análise de Causas, Estabilidade do Processo e Detalhamento.

## Importação

Reconhece cada aba pelas colunas:

- **Ocorrências** (uma linha por anomalia): `Data`, `Unidade` (ou `Site`), `Anomalia`, `Quantidade`, `Descrição do Problema`, `Etapa de Origem`, `Subprocesso`, `Ponto de Causa`, `Causa Raiz Provável`, `Grupo da Causa Raiz`, `Tipo de Falha (4M)`, `Controles`, `Ação Recomendada`.
- **Refeições** (uma linha por restaurante e dia/mês): `Mês`, `Restaurante`, `Almoço Realizado`, `Jantar Realizado`. Refeições servidas = almoço + jantar.
- Modelo simples: `mês`, `restaurante`, `refeições`.

Reimportar a mesma planilha não duplica registros.

## Regras de cálculo

- Taxa = anomalias ÷ refeições × 10.000, usando só anomalias de restaurantes e meses com refeições informadas.
- Carta U: linha central ū = Σanomalias ÷ Σrefeições; limites ū ± 3·√(ū/n).
- Previsão: exige 12 meses de refeições por restaurante (configurável em Administração). P(≥1) = 1 − exp(−λ) no modelo de Poisson; binomial negativa quando há superdispersão.
- Nenhum valor é inventado: sem dados suficientes, o app avisa.

## Dados e privacidade

Os dados ficam **somente no navegador** (localStorage). Use **Base Histórica → Backup dos dados** para guardar uma cópia. **Não versione planilhas nem backups neste repositório** se contiverem informações internas.

## Limitações conhecidas

- Sem login, perfis de acesso ou banco compartilhado entre usuários (próximo passo: PostgreSQL/Supabase).
- Sem editor visual de fluxogramas com arrastar e soltar (o Mapa de Processos é hierárquico).
- Cpk e fatores de influência da previsão não são calculados.
