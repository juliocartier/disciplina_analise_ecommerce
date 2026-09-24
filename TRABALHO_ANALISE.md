# TRABALHO DA DISCIPLINA DE ANÁLISE PREDITIVA DE VENDAS NO E-COMMERCE

### Prof. Me. Julio Cartier Maia Gomes
### Valor Total: 5,0 pontos
### Formato: Individual
### Prazo Limite de Entrega: 07 de outubro, até as 23h59
### Ambiente de Desenvolvimento: Fica a critério (Google Colab, Visual Studio Code em formatos .ipynb ou .py).

---

## 1. Contexto e Objetivo Geral

No ecossistema de e-commerce, a precificação dinâmica, a competitividade entre lojistas e a qualidade cadastral dos anúncios determinam diretamente a conversão em vendas e a margem de lucro.

O objetivo deste trabalho prático é aplicar técnicas de **Engenharia de Dados (ETL/Web Scraping)**, **Limpeza e Tratamento de Dados (Data Wrangling)** e **Análise Exploratória de Dados (EDA)** em dados reais extraídos do comparador de preços **Buscapé**.

Cada estudante deverá selecionar **duas (2) categorias distintas** para realizar a extração, estruturação e análise comparativa.

---

## 2. Categorias Disponíveis

Cada estudante deve selecionar exatamente duas categorias da lista abaixo:

1. Celulares e Smartphones
2. TVs
3. Notebooks
4. Ar-Condicionados
5. Lavadoras
6. Geladeiras
7. Câmeras e Filmadoras
8. Eletroportáteis
9. Games
10. Cama, Mesa e Banho

---

## 3. Esquema Canônico dos Dados (Schema)

Os dados capturados devem ser estruturados em um DataFrame do Pandas, respeitando a convenção descrita a seguir. É permitido adicionar novos campos caso o anúncio forneça informações complementares relevantes; da mesma forma, campos não disponibilizados pela categoria podem ser ajustados ou omitidos:

```python
item = {
    'loja': dado.get('sellerName') or dado.get('merchantName'),
    'preco_vista': dado.get('price'),
    'nome_anuncio': dado.get('name'),
    'id_oferta': dado.get('id'),
    'preco_total': dado.get('totalPrice'),
    'qtd_parcelas': dado.get('numParcels'),
    'valor_parcela': dado.get('parcelValue'),
    'total_parcelado': dado.get('totalParceledValue'),
    'tem_juros': dado.get('hasInterest'),
    'marketplace': dado.get('sellerIsMarketplace'),
    'estoque': dado.get('stock'),
    'indice_competitividade': dado.get('competitivenessIndex'),
    'desconto_perc': dado.get('loweringPercentage'),
    'url_imagem': dado.get('imageUrl'),
    'categoria': 'Nome da Categoria Escolhida'  # Coluna discriminadora obrigatória
}
```

---

## 4. Etapas de Execução e Entregáveis

O trabalho está estruturado em 4 etapas cumulativas e obrigatórias:

### Etapa 1: Coleta e Pipeline de Ingestão (Peso: 1,0 ponto)
* Implementação do pipeline de extração automatizada (via requisições HTTP / scraping com paginação) para as duas categorias escolhidas.
* Coleta de volume representativo (recomenda-se extrair no mínimo de 5 a 10 páginas por categoria).
* Consolidação em DataFrame único contendo a coluna `categoria`.
* Exportação e persistência da base bruta em arquivo `.csv`.

### Etapa 2: Higienização, Auditoria e Engenharia de Features (Peso: 1,5 pontos)

* **Auditoria de Nulos e Inconsistências:** Mapear e reportar o percentual de dados faltantes em cada coluna (`df.isna().mean() * 100`).
* **Tratamento de Dados:**
  * Remoção fundamentada de registros duplicados pela chave `id_oferta`.
  * Conversão estrita de tipos (`preco_vista`, `valor_parcela` e `total_parcelado` para `float`; `qtd_parcelas` para `int`).
  * Tratamento documentado de valores ausentes (justificar no notebook se preencheu `desconto_perc` nulo com 0 ou se descartou linhas sem preço/título).

**Engenharia de Atributos (Feature Engineering):** 
* **Extração Textual via Regex:** Extrair ao menos uma especificação técnica a partir do `nome_anuncio` (ex.: Armazenamento em GB para celulares/notebooks; Voltagem 110V/220V/Bivolt para eletrodomésticos; Polegadas " para TVs; BTUs para Ar-Condicionado).
* **Custo Efetivo do Parcelamento:** Criar a feature `acrescimo_parcelamento_pct` calculada como:

$`\text{Acrescimo}(\%) = \left( \frac{\text{total\_parcelado} - \text{preco\_vista}}{\text{preco\_vista}} \right) \times 100`$
    
  * **Posicionamento Relativo de Preço:** Criar indicador de desvio percentual do preço de cada item em relação à mediana da sua respectiva categoria.

### Etapa 3: Análise Descritiva e Exploração Estatística (Peso: 1,5 pontos)
Resolução obrigatória das 4 questões analíticas abaixo, acompanhadas de gráficos interpretáveis (títulos, legendas e eixos identificados):

1. **Poder e Concentração de Lojas:**
   * Quais são as 5 lojas que concentram o maior número de ofertas em cada categoria?
   * A loja com o maior volume de anúncios é necessariamente a mais barata? Compare a média e a mediana de preços praticadas pelos 5 principais lojistas.
2. **Impacto da Modalidade Marketplace:**
   * Existe diferença estatisticamente perceptível no preço médio praticado por lojistas parceiros (`marketplace == True`) versus lojas próprias/oficiais (`marketplace == False`)?

### Etapa 4: Conclusões Estratégicas e Relatório Técnico (Peso: 1,0 ponto)
* Organização exemplar do Jupyter Notebook, intercalando células de código com explicações em Markdown analítico e fluido.

---

## 5. Matriz de Avaliação e Critérios de Correção (Rubrica)

| Critério | Descrição | Nota Máxima |
| :--- | :--- | :---: |
| **Pipeline e Ingestão** | Coleta automatizada com paginação funcional, mapeamento no esquema canônico e volume adequado de registros. | **1,0** |
| **Data Wrangling e Regex** | Tratamento de nulos documentado, tipagem correta, deduplicação e extração bem-sucedida de features via Regex. | **1,5** |
| **Profundidade Analítica** | Resolução completa das 2 questões com cálculos estatísticos precisos e gráficos elucidativos (eixos e títulos). | **1,5** |
| **Storytelling e Conclusões** | Interpretação aplicada ao negócio, limpeza do código, organização do notebook. | **1,0** |
| **Total** | | **5,0** |

---

## 6. Instruções de Entrega

1. **Prazo Improrrogável:** **07 de outubro, até as 23h59**.
2. **Arquivos Exigidos:**
   * Notebook executado com todos os outputs pré-renderizados (`.ipynb`).
   * Base de dados tratada/coletada no formato `.csv`.
3. **Originalidade:** Códigos clonados, duplicados ou com plágio detectado receberão nota **0,0 (zero)**.