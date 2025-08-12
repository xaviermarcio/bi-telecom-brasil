# README_dados
**Fonte**: ANATEL — Conjunto “Meu Município: Acessos e Cobertura de Telecomunicações”.  
**Link**: https://dados.gov.br/dados/conjuntos-dados/meu-municipio---acessos-e-cobertura-de-telecomunicacoes

## Tabelas/Campos principais (Cobertura Móvel)
- `Operadora`: Prestadora do serviço de telefonia móvel.
- `Tecnologia`: 2G, 3G, 4G, 5G.
- `Cobertura`: Predição do sinal (modelo de propagação com base nas ERBs licenciadas).
- `Moradores Cobertos` e `Domicílios Cobertos`: estimativas por interseção da mancha de cobertura com setores censitários.
- `Área km2 Coberta`: área estimada coberta.
- `Moradores Município`, `Domicílios Município`, `Área Município km2`: dados de referência (IBGE).
- `Ano`, `Código IBGE`, `Município`, `UF`, `Região`.

## Observações
- Percentuais podem ser calculados a partir dos campos absolutos (moradores, domicílios, área).
- Caso a base já traga percentuais, utilizar média ponderada/adequar medidas para evitar dupla normalização.
