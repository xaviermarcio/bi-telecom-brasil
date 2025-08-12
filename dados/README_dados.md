# Dados — Meu Município: Cobertura de Telefonia Móvel (ANATEL)

**Arquivo:** `Meu_Municipio_Cobertura.csv`  
**Fonte oficial:** ANATEL — *Meu Município: Acessos e Cobertura de Telecomunicações*  
Link: https://dados.gov.br/dados/conjuntos-dados/meu-municipio---acessos-e-cobertura-de-telecomunicacoes  
**Observação:** os percentuais de cobertura são **estimativas** (predição com base em ERBs e modelo de propagação).

## 📦 Tamanho & escopo da base 
- **Linhas:** 1.013.741
- **Colunas:** 13
- **Período:** 2021–2023 (`Ano`)
- **UFs:** 27 (Ano)
- **Municípios:** 5.568 (`não contamos o Distrito Federal nem o distrito estadual de Fernando de Noronha, pois não são municípios.`)
- **Operadoras:** ALGAR, CLARO, LIGUE, NEXTEL, OI, SERCOMTEL, TIM, VIVO, *e* “Todas”
- **Tecnologias:** 2G, 3G, 4G, 5G, *e* categorias combinadas (3G4G, 4G5G, “Todas”)
- **Nulos nos %** (`% moradores`, `% domicílios`, `% área`): ~**0,09%** em cada

> Os valores acima refletem este arquivo específico; podem mudar quando a base for atualizada.


## 🧰 Power Query - Como os dados foram tratados

1. **Leitura do CSV** com `;` como separador e vírgula como decimal (UTF-8).
2. **Padronização de tipos**: `Ano`/`Mês` inteiros; percentuais como **float** (0–100).
3. **Higiene básica**:
   - Remoção de linhas totalmente vazias.
   - Ajuste de espaços/acentos em `Município`, `Nome UF`.
   - Clipping dos percentuais para o intervalo **0–100** (caso outliers).
4. **Filtros analíticos** (no Power BI), para comparações limpas:
   - **Operadora ≠ “Todas”**.
   - **Tecnologia ∈ {2G, 3G, 4G, 5G}** (excluo 3G4G, 4G5G e “Todas” quando analiso só uma tecnologia).
5. **Modelagem** (esquema estrela):
   - **Fato**: linhas por Município–Operadora–Tecnologia–Ano com `% moradores`, `% domicílios`, `% área`.
   - **Dimensões**: Calendário (Ano), Região, UF, Município (c/ código IBGE), Operadora, Tecnologia.

## 🗂️ Dicionário rápido
- **Ano, Mês** — referência temporal.
- **Operadora** — prestadora do SMP (ex.: CLARO, TIM…).
- **Tecnologia** — 2G/3G/4G/5G (e categorias combinadas).
- **Código IBGE, Município, UF, Nome UF, Região** — localização (IBGE/ANATEL).
- **Código Nacional** — DDD do município.
- **% moradores cobertos** — moradores na área de cobertura / moradores do município × 100.
- **% domicílios cobertos** — domicílios na área de cobertura / domicílios do município × 100.
- **% área coberta** — área (km²) coberta / área (km²) do município × 100.

## ✅ Qualidade & checagens que faço
- Totais por UF/região dentro de **0–100%**.
- Tendência anual coerente (ex.: avanço de 4G/5G).
- Coerência entre `% moradores`, `% domicílios` e `% área` (não precisam ser iguais, mas devem ter ordem de grandeza compatível).

## 🔄 Como atualizar a base
1. Baixe a versão mais recente no link oficial e **substitua** `Meu_Municipio_Cobertura.csv` nesta pasta.
2. Mantenha o **mesmo separador** `;` e **decimais com vírgula**.
3. No Power BI, abra `painel/BI_Telecom_Brasil.pbix` e clique em **Atualizar**.
4. Revise rapidamente: contagem de linhas, anos presentes, UFs e operadoras.

## 📄 Licença
- **Código/Docs deste repositório:** MIT.  
- **Dados (CSV):** seguem os **termos/licença da ANATEL** na página oficial da fonte.
