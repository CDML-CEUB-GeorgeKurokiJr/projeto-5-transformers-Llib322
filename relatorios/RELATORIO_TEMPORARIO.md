# Relatório provisório — Clusterização temática de projetos científicos com BERTimbau, NER e representação híbrida

## 1. Introdução

Este relatório apresenta os resultados parciais do projeto de clusterização temática de projetos científicos a partir de títulos e descrições públicas.

O trabalho parte de um notebook-base fornecido em aula, que utilizava técnicas de Processamento de Linguagem Natural, NER com spaCy e embeddings com BERTimbau. A partir desse ponto, o projeto foi expandido com uma metodologia mais organizada, incluindo limpeza textual, análise exploratória, treinamento de um NER personalizado, geração de embeddings, comparação de representações e clusterização final com uma representação híbrida.

O objetivo principal não foi criar uma classificação definitiva dos projetos, mas sim construir uma abordagem exploratória e interpretável para identificar padrões temáticos em uma base textual interdisciplinar.

---

## 2. Objetivo do projeto

O objetivo geral foi agrupar projetos científicos e tecnológicos em temas semelhantes a partir dos textos disponíveis.

Os objetivos específicos foram:

* preparar e limpar a base textual;
* analisar os principais termos do dataset com TF-IDF;
* construir um modelo de NER personalizado para identificar áreas temáticas;
* gerar embeddings semânticos com BERTimbau;
* comparar diferentes formas de representar os textos;
* aplicar clusterização com KMeans;
* interpretar os clusters gerados com apoio de TF-IDF, NER e títulos representativos;
* documentar limitações e possibilidades de melhoria.

---

## 3. Base de dados

A base utilizada contém títulos e descrições públicas de projetos científicos.

As colunas principais utilizadas foram:

* `Título_Público`;
* `Descricao_pública`.

Inicialmente, a base possuía 2725 registros.

Durante a preparação dos dados, foram identificados:

* alguns valores ausentes em título e descrição;
* textos duplicados;
* textos muito curtos;
* descrições explicitamente não divulgadas;
* registros cancelados ou pendentes.

Após a limpeza inicial, remoção de duplicatas e filtragem conservadora de ruídos, a base final utilizada na clusterização ficou com aproximadamente 2702 textos.

A estratégia de filtragem foi propositalmente conservadora. Foram removidos apenas casos com alta chance de não conter informação textual útil. Textos curtos, mas potencialmente informativos, foram mantidos.

---

## 4. Preparação textual

Foram criadas três representações principais dos textos:

### 4.1 Texto original

Combina título e descrição uma única vez.

Exemplo estrutural:

```python
texto_original = titulo + ". " + descricao
```

### 4.2 Texto com título reforçado

Repete o título antes da descrição.

Essa estratégia foi adotada porque o título costuma resumir o tema principal do projeto.

```python
texto_titulo_reforcado = titulo + ". " + titulo + ". " + descricao
```

### 4.3 Texto enriquecido com NER

Combina o texto com título reforçado e adiciona entidades temáticas extraídas pelo modelo NER.

```python
texto_enriquecido_ner_v2 = texto_titulo_reforcado + " ENTIDADES_TEMATICAS: " + entidades_extraidas
```

Essas três representações foram comparadas durante o projeto.

---

## 5. Análise exploratória com TF-IDF

A técnica TF-IDF foi utilizada para identificar termos relevantes da base.

O TF-IDF foi útil para:

* compreender os temas recorrentes nos projetos;
* apoiar a criação das labels do NER;
* identificar termos técnicos importantes;
* interpretar os clusters finais.

Entre os termos de destaque apareceram palavras e expressões associadas a:

* software;
* dados;
* plataforma;
* controle;
* monitoramento;
* sistemas;
* inteligência artificial;
* energia;
* materiais;
* dispositivos;
* IoT;
* resíduos;
* manufatura;
* automação;
* saúde;
* agricultura;
* baterias.

Essa análise indicou que a base é fortemente interdisciplinar, combinando áreas como computação, energia, engenharia, sustentabilidade, agro, saúde e sistemas técnicos.

---

## 6. Construção do NER personalizado

O NER foi usado de forma personalizada. Em vez de reconhecer entidades tradicionais, como pessoas ou locais, o modelo foi treinado para reconhecer áreas temáticas nos textos dos projetos.

As labels finais utilizadas foram:

| Label                                   | Interpretação                                                                   |
| --------------------------------------- | ------------------------------------------------------------------------------- |
| `AREA_COMPUTACAO_DADOS`                 | Computação, software, IA, algoritmos, IoT e dados                               |
| `AREA_ENERGIA`                          | Energia elétrica, energia solar, baterias e eficiência energética               |
| `AREA_MEIO_AMBIENTE_SUSTENTABILIDADE`   | Sustentabilidade, resíduos, carbono, reciclagem e impacto ambiental             |
| `AREA_SAUDE_BIOTECNOLOGIA`              | Saúde, biotecnologia, enzimas, vacinas e aplicações biomédicas                  |
| `AREA_ALIMENTOS_AGRO`                   | Alimentos, agricultura, fertilizantes, bioinsumos e agroindústria               |
| `AREA_ENGENHARIA_MATERIAIS`             | Materiais, manufatura, aço, ligas, corrosão e metalurgia                        |
| `AREA_SISTEMAS_AUTOMACAO_MONITORAMENTO` | Sistemas, automação, controle, monitoramento, sensores, hardware e equipamentos |

A última label foi adicionada durante os testes porque foi observado que muitos textos tratavam de sistemas, controle, monitoramento e equipamentos, mas não eram bem cobertos pelas seis labels iniciais.

---

## 7. Geração do conjunto de treinamento do NER

O `TRAINING_DATA` foi construído de forma semiautomática a partir de listas de termos de referência para cada label.

Foram usadas expressões regulares com limites de palavra para evitar falsos positivos. Por exemplo, o termo `aço` não deveria ser encontrado dentro da palavra `aplicação`.

Também foi feita validação dos spans com a tokenização do spaCy, evitando problemas de desalinhamento entre caracteres e tokens.

O conjunto final de treinamento teve:

* 570 exemplos;
* 0 problemas de alinhamento;
* exemplos positivos balanceados por label;
* exemplos negativos, ou seja, textos sem entidades.

Também foi aplicado um filtro de ambiguidade para termos como `diagnóstico`, que poderia aparecer tanto em contexto de saúde quanto em diagnóstico de falhas ou equipamentos.

---

## 8. Desempenho do NER V2

O modelo NER V2 foi treinado com spaCy e avaliado em um conjunto de validação.

As métricas gerais obtidas foram:

| Métrica   |  Valor |
| --------- | -----: |
| Precisão  | 0.9367 |
| Revocação | 0.8961 |
| F1-score  | 0.9159 |

As métricas por label também foram satisfatórias, com destaque para labels como computação, agro, meio ambiente, energia e sistemas/automação.

É importante destacar que essas métricas devem ser interpretadas com cautela, pois o conjunto de treinamento foi criado de forma semiautomática. Portanto, o resultado indica que o modelo aprendeu bem o padrão de anotação criado, mas ainda não substitui uma avaliação manual feita por especialistas.

Na aplicação do NER V2 sobre toda a base, foram extraídas 4684 entidades.

A distribuição geral das entidades extraídas foi:

| Label                                   | Quantidade aproximada |
| --------------------------------------- | --------------------: |
| `AREA_SISTEMAS_AUTOMACAO_MONITORAMENTO` |                  1575 |
| `AREA_COMPUTACAO_DADOS`                 |                  1191 |
| `AREA_ENGENHARIA_MATERIAIS`             |                   533 |
| `AREA_MEIO_AMBIENTE_SUSTENTABILIDADE`   |                   503 |
| `AREA_ENERGIA`                          |                   327 |
| `AREA_ALIMENTOS_AGRO`                   |                   326 |
| `AREA_SAUDE_BIOTECNOLOGIA`              |                   229 |

A nova label de sistemas, automação e monitoramento teve grande presença na base, confirmando que ela representava um padrão temático importante que não estava sendo coberto inicialmente.

---

## 9. Embeddings com BERTimbau

Para capturar o significado semântico dos textos, foi usado o modelo:

```python
neuralmind/bert-base-portuguese-cased
```

O BERTimbau foi escolhido por ser um modelo BERT treinado para a língua portuguesa.

Foi usada a técnica de `mean pooling`, calculando a média dos embeddings dos tokens válidos para obter um vetor único por texto.

Cada texto foi representado por um embedding de 768 dimensões.

O limite de tokens escolhido foi:

```python
MAX_LEN_BERT = 256
```

Essa escolha foi baseada na distribuição dos textos. A maior parte dos textos estava abaixo desse limite, e apenas uma parcela menor seria truncada.

---

## 10. Comparação de representações

Foram testadas diferentes formas de representar os textos:

1. BERTimbau com texto original;
2. BERTimbau com título reforçado;
3. BERTimbau com texto enriquecido por NER;
4. Representação híbrida com BERTimbau, TF-IDF/SVD e NER.

Durante os testes, observou-se que o melhor Silhouette isolado nem sempre produzia a melhor interpretação temática.

Por exemplo, uma configuração com menos clusters podia obter métrica melhor, mas gerar grupos muito amplos e pouco úteis para análise.

Por isso, a escolha final considerou tanto o Silhouette Score quanto a interpretabilidade dos clusters.

---

## 11. Representação híbrida final

A representação final combinou três blocos de informação:

### 11.1 BERTimbau

Captura o significado contextual dos textos.

Foi usada a versão com título reforçado.

### 11.2 TF-IDF/SVD

Captura termos técnicos importantes do domínio.

O TF-IDF foi reduzido com TruncatedSVD para gerar um bloco denso de features.

### 11.3 NER V2

Captura a presença e proporção de entidades temáticas reconhecidas no texto.

A representação híbrida final usou os seguintes pesos:

| Bloco      | Peso |
| ---------- | ---: |
| BERTimbau  |  1.0 |
| TF-IDF/SVD |  0.8 |
| NER V2     |  1.3 |

O objetivo desses pesos foi equilibrar semântica, vocabulário técnico e entidades temáticas.

---

## 12. Clusterização final

A clusterização foi feita com KMeans.

Foram testados valores de `k` entre 2 e 12.

A escolha final considerou:

* Silhouette Score;
* menor cluster com tamanho razoável;
* ausência de clusters excessivamente pequenos;
* interpretabilidade temática;
* coerência entre TF-IDF, NER e títulos representativos.

O melhor candidato interpretável foi:

```python
k = 8
```

O Silhouette Score final foi aproximadamente:

```python
0.0925
```

Embora esse valor não seja alto, ele é compreensível considerando que a base é interdisciplinar e muitos projetos combinam diferentes áreas.

---

## 13. Clusters finais encontrados

A representação híbrida final agrupou os projetos em oito clusters.

| Cluster | Nome atribuído                                                       | Quantidade aproximada de textos |
| ------: | -------------------------------------------------------------------- | ------------------------------: |
|       0 | Sustentabilidade, resíduos e carbono                                 |                             178 |
|       1 | Sistemas, automação e monitoramento                                  |                             530 |
|       2 | Computação, software e inteligência artificial                       |                             423 |
|       3 | Engenharia de materiais, manufatura e metalurgia                     |                             210 |
|       4 | Saúde, biotecnologia e aplicações biomédicas                         |                             118 |
|       5 | Energia, baterias e sistemas elétricos                               |                             156 |
|       6 | Processos, rotas tecnológicas e projetos técnico-operacionais gerais |                             944 |
|       7 | Agro, alimentos e fertilizantes                                      |                             143 |

---

## 14. Interpretação dos clusters

### Cluster 0 — Sustentabilidade, resíduos e carbono

Esse cluster reuniu projetos relacionados a resíduos, sustentabilidade, reaproveitamento, reciclagem, carbono e matérias-primas renováveis.

A label dominante foi `AREA_MEIO_AMBIENTE_SUSTENTABILIDADE`.

Esse grupo apresentou boa coerência temática.

---

### Cluster 1 — Sistemas, automação e monitoramento

Esse cluster reuniu projetos relacionados a monitoramento, dispositivos, equipamentos, sensores, hardware, coleta de dados e automação.

A label dominante foi `AREA_SISTEMAS_AUTOMACAO_MONITORAMENTO`.

Esse resultado confirma que a nova label adicionada ao NER era necessária, pois esse grupo apareceu de forma clara na base.

---

### Cluster 2 — Computação, software e inteligência artificial

Esse cluster reuniu projetos de software, plataformas digitais, inteligência artificial, algoritmos, sistemas computacionais e processamento de dados.

A label dominante foi `AREA_COMPUTACAO_DADOS`.

Esse cluster apresentou boa separação temática e foi um dos grupos mais interpretáveis.

---

### Cluster 3 — Engenharia de materiais, manufatura e metalurgia

Esse cluster reuniu projetos relacionados a fabricação, aço, manufatura, corrosão, metalurgia, polímeros, compósitos e materiais.

A label dominante foi `AREA_ENGENHARIA_MATERIAIS`.

O grupo também apresentou presença secundária de sistemas e automação, o que faz sentido em projetos industriais.

---

### Cluster 4 — Saúde, biotecnologia e aplicações biomédicas

Esse cluster reuniu projetos relacionados a saúde, enzimas, microrganismos, biotecnologia, aplicações biomédicas e tratamentos.

A label dominante foi `AREA_SAUDE_BIOTECNOLOGIA`.

Apesar de ser um cluster menor, ele apresentou boa coerência temática.

---

### Cluster 5 — Energia, baterias e sistemas elétricos

Esse cluster reuniu projetos relacionados a energia, baterias, veículos elétricos, sistemas elétricos, eficiência energética e armazenamento de energia.

A label dominante foi `AREA_ENERGIA`.

Também houve presença de entidades de sistemas e automação, o que é coerente, pois muitos projetos de energia envolvem controle, monitoramento e equipamentos.

---

### Cluster 6 — Processos, rotas tecnológicas e projetos técnico-operacionais gerais

Esse foi o maior cluster da solução final.

Ele reuniu textos com termos como sistemas, rotas tecnológicas, controle, plataformas, otimização, validação e processos.

Esse cluster apresentou baixa cobertura de NER, indicando que muitos textos desse grupo não foram bem explicados pelas labels temáticas existentes.

Esse resultado é uma das principais limitações do modelo. O cluster parece representar projetos mais genéricos, operacionais, interdisciplinares ou com descrições pouco específicas.

---

### Cluster 7 — Agro, alimentos e fertilizantes

Esse cluster reuniu projetos relacionados a soja, fertilizantes, agricultura, alimentos, biofertilizantes, bioinsumos e tecnologias agroindustriais.

A label dominante foi `AREA_ALIMENTOS_AGRO`.

O grupo também apresentou relação com sustentabilidade e biotecnologia, o que é esperado em projetos agroindustriais.

---

## 15. Comparação dos principais testes

Durante o desenvolvimento, foram observados alguns resultados importantes:

| Abordagem                     |  k | Silhouette aproximado | Observação                                                             |
| ----------------------------- | -: | --------------------: | ---------------------------------------------------------------------- |
| BERTimbau com texto original  |  2 |                0.0880 | Separação muito ampla, pouco interpretável                             |
| BERTimbau enriquecido com NER |  4 |                0.0985 | Melhor métrica, mas clusters menos específicos                         |
| Híbrido inicial               |  7 |                0.0921 | Boa interpretabilidade, mas um cluster grande ficou sem NER            |
| Híbrido final com NER V2      |  8 |                0.0925 | Escolhido como principal pelo equilíbrio entre métrica e interpretação |

A abordagem final não teve o maior Silhouette absoluto, mas foi considerada a melhor para o objetivo do projeto porque gerou clusters mais explicáveis e permitiu analisar melhor as áreas temáticas da base.

---

## 16. Principais descobertas

As principais descobertas do projeto foram:

1. A base é altamente interdisciplinar.

   Muitos projetos misturam computação, automação, energia, saúde, materiais, agro e sustentabilidade.

2. O título dos projetos possui informação temática importante.

   Por isso, o título reforçado foi considerado útil nas representações.

3. O NER inicial precisava de uma label operacional.

   A criação da label `AREA_SISTEMAS_AUTOMACAO_MONITORAMENTO` foi uma melhoria importante, pois muitos projetos envolviam sensores, hardware, controle, equipamentos e monitoramento.

4. O maior cluster final ainda é residual.

   O cluster de processos, rotas tecnológicas e projetos técnico-operacionais gerais mostra que ainda existem textos pouco cobertos pelas labels atuais.

5. A melhor métrica não foi necessariamente o melhor resultado interpretável.

   A escolha final priorizou equilíbrio entre Silhouette Score e coerência temática dos clusters.

---

## 17. Limitações

### 17.1 Textos curtos e públicos

A base utiliza descrições públicas dos projetos, não artigos completos ou documentos técnicos detalhados.

Isso limita a quantidade de informação disponível.

### 17.2 Interdisciplinaridade

Muitos projetos pertencem a mais de uma área ao mesmo tempo.

Por exemplo:

* IA aplicada à energia;
* sensores aplicados à saúde;
* resíduos aplicados à engenharia de materiais;
* automação aplicada ao agro.

Isso dificulta a criação de clusters totalmente separados.

### 17.3 NER semiautomático

O conjunto de treinamento do NER foi criado com listas de termos, não por anotação manual especializada.

Isso torna o processo mais rápido, mas pode introduzir vieses.

### 17.4 Labels ainda incompletas

Mesmo com sete labels, ainda há textos que não são bem cobertos pelo NER.

O cluster residual indica que novas categorias ou regras podem ser necessárias.

### 17.5 KMeans exige número fixo de clusters

O KMeans exige definir previamente o valor de `k`.

Isso pode limitar a identificação de temas naturais na base.

### 17.6 Métrica de Silhouette baixa

O Silhouette Score final não foi alto.

Isso pode indicar sobreposição entre clusters, mas também reflete a natureza interdisciplinar da base.

### 17.7 PCA e t-SNE são apenas visualizações

As projeções em duas dimensões ajudam a visualizar padrões, mas perdem informação da representação original.

Por isso, os gráficos não devem ser usados isoladamente para validar a clusterização.

---

## 18. Possíveis melhorias futuras

Para a versão final do projeto, algumas melhorias podem ser feitas.

### 18.1 Anotação manual de uma amostra

Selecionar uma amostra de textos e criar uma anotação manual das áreas temáticas.

Isso permitiria avaliar o NER com mais rigor.

### 18.2 Refinar o NER

Adicionar ou revisar labels, especialmente para o cluster residual.

Possíveis novas categorias:

* processos industriais;
* rotas tecnológicas;
* validação experimental;
* gestão operacional;
* plataformas e serviços;
* equipamentos industriais.

### 18.3 Testar UMAP e HDBSCAN

UMAP poderia melhorar a redução dimensional, e HDBSCAN poderia identificar clusters sem exigir um número fixo de grupos.

### 18.4 Testar BERTopic

O BERTopic poderia ser usado como abordagem alternativa de modelagem de tópicos.

Essa técnica combina embeddings, redução dimensional e agrupamento, sendo adequada para análise temática de textos.

### 18.5 Testar diferentes pesos na representação híbrida

Os pesos de BERTimbau, TF-IDF e NER foram definidos empiricamente.

Uma melhoria seria testar sistematicamente várias combinações de pesos.

### 18.6 Testar modelos de sentence embeddings

Modelos próprios para embeddings de sentenças poderiam representar melhor os textos completos do que o uso direto do BERTimbau com mean pooling.

### 18.7 Testar limites maiores de tokens

O notebook utilizou `MAX_LEN_BERT = 256`.

Futuramente, pode-se testar `384` ou `512`, avaliando se textos mais longos melhoram os clusters.

---

## 19. Conclusão

O projeto desenvolveu uma metodologia completa e interpretável para clusterização temática de projetos científicos.

A solução final combinou:

* limpeza textual conservadora;
* TF-IDF;
* NER personalizado com spaCy;
* embeddings BERTimbau;
* representação híbrida;
* KMeans;
* interpretação dos clusters com termos, entidades e títulos representativos.

A abordagem final escolhida foi:

```python
BERTimbau + TF-IDF/SVD + NER V2
```

com `k = 8`.

O resultado final identificou grupos temáticos coerentes, como computação, energia, saúde, sustentabilidade, agro, materiais e sistemas/automação.

Apesar disso, a presença de um grande cluster técnico-operacional geral mostra que a modelagem ainda pode ser refinada.

Para a entrega provisória, o projeto já apresenta uma metodologia funcional, resultados interpretáveis e um conjunto claro de limitações e próximos passos para orientar a continuidade do trabalho.
