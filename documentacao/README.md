# Documentação técnica (curta e direta)

Este é o bastidor do meu projeto **BI – Cobertura de Telefonia Móvel no Brasil (2G–5G)**:
o que usei, como tratei os dados, como está o modelo, como calculo os KPIs e como atualizar no futuro.

---

## 1) O que eu quis responder
- **Onde** cada tecnologia (2G/3G/4G/5G) chega no Brasil.
- **Quanto** cobre (moradores, domicílios e área).
- **Como evoluiu** por ano e por **operadora**.

**Fonte oficial:** ANATEL – *Meu Município: Acessos e Cobertura de Telecomunicações*  
Arquivo usado: `dados/Meu_Municipio_Cobertura.csv` (separador `;`, vírgula como decimal).  
**Observação:** “cobertura” aqui é **estimativa** (modelo de propagação com base nas ERBs da operadora).

Convenção deste painel: total Brasil = **5.568 municípios**  
(não conto **Distrito Federal** nem **Fernando de Noronha**, pois não são municípios).

---

## 2) Como tratei os dados (Power Query – resumo)
- Leitura do CSV com `;` e vírgula como decimal (UTF-8).
- Tipos: `Ano`/`Mês` inteiros; percentuais como número (0–100).
- Higiene: remoção de linhas vazias, ajuste de acentos/espacos em município/UF.
- **Clipping** dos percentuais para o intervalo **0–100** se aparecer outlier.
- Filtros usuais de análise no relatório:
  - **Operadora ≠ “Todas”**.
  - **Tecnologia** focada em 2G/3G/4G/5G (ignoro combinações como “3G4G” quando analiso uma tecnologia específica).

> Dica: uso um **Parâmetro** para o caminho dos dados (ex.: `CaminhoDados`) e leio:
> `File.Contents(CaminhoDados & "Meu_Municipio_Cobertura.csv")`.

---

## 3) Modelo estrela (como está organizado)
**Grão da Fato:** **Município × Operadora × Tecnologia × Ano** (e Mês, se tiver).  
**Fato Cobertura:** `% moradores`, `% domicílios`, `% área` + chaves.  
**Dimensões:** Calendário (Ano/Mês), Região, UF (Nome/UF), Município (Código IBGE, Nome), Operadora, Tecnologia.  
**Relacionamentos:** **1:\*** da dimensão para a fato, filtro **single** (os filtros “descem” para a fato).

> Região pode ser atributo da UF; preferi manter **Dim Região** para filtrar região direto.

---

## 4) KPIs (como calculo)
- **% Moradores** = `Moradores_Cobertos / Moradores_Município × 100`
- **% Domicílios** = `Domicílios_Cobertos / Domicílios_Município × 100`
- **% Área** = `Área_km²_Coberta / Área_km²_Município × 100`
- **Total de Municípios** = `DISTINCTCOUNT(Município)`  
  *(no Brasil inteiro, aplico a convenção de **5.568** acima).*

**Agregação correta** quando preciso recalcular em UF/Região/Brasil:  
**somar os numeradores e os denominadores e dividir no final**.  
Isso evita distorção de “média de médias”.

---

## 5) Qualidade (checagens rápidas que faço)
- Percentuais dentro de **0–100%**.
- Tendência anual coerente (ex.: 4G e 5G crescendo de 2021→2023).
- Coerência entre **% moradores**, **% domicílios** e **% área** (ordens de grandeza alinhadas).
- Conferência de volumes (quantos anos/UFs/operadoras a base trouxe).

---

## 6) Como atualizar a base
1. Baixe a versão mais recente no portal de dados da ANATEL (Meu Município).
2. **Substitua** `dados/Meu_Municipio_Cobertura.csv` mantendo `;` e vírgula como decimal.
3. Abra `painel/BI_Telecom_Brasil.pbix` e clique em **Atualizar**.
4. Revise rapidamente: anos presentes, UFs/operadoras e contagem de linhas.

---

## 7) Limitações e leitura responsável
- Cobertura é **predição** de alcance do sinal — não mede **qualidade** de serviço nem **adesão**.
- População/domicílios de referência (IBGE) podem defasar percentuais em anos recentes.
- A presença de **2024** e **2025** depende da atualização da fonte.

---

**Licenças**  
- **Código/Documentação deste repositório:** MIT.  
- **Dados (CSV):** seguem os termos/licença da **ANATEL** na página oficial.
