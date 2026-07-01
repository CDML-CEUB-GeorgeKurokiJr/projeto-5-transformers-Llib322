# Projeto 5 — Clusterização Temática de Projetos Científicos com BERTimbau, NER e KMeans

Este repositório contém o projeto final da disciplina de Deep Learning / Transformers.

O objetivo do projeto foi desenvolver uma metodologia para agrupar projetos científicos e tecnológicos em temas semelhantes a partir de seus títulos e descrições públicas.

Como a base de dados não possuía uma coluna oficial de categoria temática, o problema foi tratado como uma tarefa de **clusterização textual exploratória**.

A solução final combina:

- embeddings semânticos com BERTimbau;
- representação léxica com TF-IDF/SVD;
- NER personalizado com spaCy;
- representação híbrida;
- clusterização com KMeans;
- interpretação dos clusters com termos, entidades e títulos representativos.

---

## Arquivos principais para avaliação

Os principais arquivos a serem avaliados são:

| Arquivo | Descrição |
|---|---|
| `notebooks/projeto-5-ner-bertimbau-final-v4.ipynb` | Notebook final e oficial do projeto |
| `relatorios/RELATORIO_FINAL.md` | Relatório final documentando o processo, decisões, resultados e limitações |
| `arquivos_zipados/resultados_finais_v31.zip` | Arquivo compactado com os principais resultados gerados pelo notebook final |

O notebook oficial a ser avaliado é:

`notebooks/projeto-5-ner-bertimbau-final-v4.ipynb`

O relatório oficial a ser avaliado é:

`relatorios/RELATORIO_FINAL.md`

---

## Organização do repositório

A estrutura principal do repositório é:

```text
.
├── README.md
├── notebooks
│   ├── projeto-5-ner-bertimbau-final-v4.ipynb
│   └── notebooks_de_teste
│       ├── notebooks experimentais
│       └── versão da entrega temporária
├── relatorios
│   ├── RELATORIO_FINAL.md
│   └── RELATORIO_TEMPORARIO.md
└── arquivos_zipados
    └── resultados_finais_v31.zip

---

```

## Descrição das pastas

`notebooks`

Contém os notebooks utilizados no projeto.

O notebook principal é:

`notebooks/projeto-5-ner-bertimbau-final-v4.ipynb`

Esse é o notebook final, limpo e organizado, contendo a versão oficial do pipeline.

Ele inclui:

- carregamento da base;
- preparação textual;
- análise exploratória;
- criação das labels temáticas;
- treinamento do NER personalizado;
- geração de embeddings com BERTimbau;
- construção da representação híbrida;
- clusterização com KMeans;
- interpretação dos clusters;
- comparação entre versões;
- salvamento dos resultados finais.


`notebooks/notebooks_de_teste`

Contém os notebooks usados durante o desenvolvimento do projeto.

Nessa pasta estão os testes intermediários, versões experimentais e também a versão utilizada na entrega temporária.

Esses notebooks foram mantidos para documentar a evolução do trabalho, mas não são a versão oficial para avaliação.

`relatorios`

Contém os relatórios do projeto.

O arquivo principal é:

`relatorios/RELATORIO_FINAL.md`

Esse é o relatório final, com a explicação completa da metodologia, das decisões tomadas, dos testes realizados, dos resultados obtidos e das limitações da solução.

Também há o arquivo:

`relatorios/RELATORIO_TEMPORARIO.md`

Esse relatório corresponde à entrega provisória e foi mantido no repositório apenas para registro histórico do desenvolvimento.

`arquivos_zipados`

Contém os resultados finais do notebook comprimidos em arquivo .zip.

O arquivo principal é:

`arquivos_zipados/resultados_finais_v31.zip`

Esse arquivo reúne os resultados gerados pelo notebook final, como tabelas de clusters, entidades extraídas, métricas, resumos e demais artefatos do pipeline.

---

## Objetivo do projeto

O objetivo principal foi agrupar projetos científicos e tecnológicos em temas semelhantes de forma interpretável.

A base possuía textos com títulos e descrições públicas, mas não possuía uma classificação temática oficial.

Por isso, a solução foi construída como um problema de aprendizado não supervisionado, com foco em clusterização.

Além de simplesmente agrupar os textos, o projeto buscou tornar os agrupamentos interpretáveis, permitindo entender quais temas aparecem em cada cluster.

---

## Principais desafios

A base apresentou alguns desafios importantes:

- Os textos eram interdisciplinares.
- Muitos projetos misturavam mais de uma área tecnológica.
- Alguns textos eram curtos ou genéricos.
- Termos como “sistema”, “produção”, “plataforma”, “controle” e “tecnologia” apareciam em várias áreas diferentes.
- Uma clusterização baseada apenas em embeddings poderia gerar grupos difíceis de interpretar.

Por isso, a solução final não usou apenas embeddings semânticos. Ela combinou BERTimbau, TF-IDF/SVD e NER personalizado.

---

## Metodologia

A metodologia final seguiu as seguintes etapas:

- Carregamento da base de dados.
- Limpeza e preparação textual.
- Criação de textos com título reforçado.
- Análise exploratória com TF-IDF.
- Construção de labels temáticas.
- Criação semiautomática do conjunto de treinamento para NER.
- Treinamento de um modelo NER personalizado com spaCy.
- Aplicação do NER na base completa.
- Geração de embeddings com BERTimbau.
- Redução dimensional dos embeddings com PCA.
- Criação de representação TF-IDF/SVD.
- Construção das features do NER.
- Combinação das três representações em uma matriz híbrida.
- Clusterização com KMeans.
- Interpretação dos clusters.
- Comparação entre versões.
- Salvamento dos resultados finais.

---

## Resultado final principal

A versão final principal foi a V3.1.

Os principais resultados foram:

| Métrica | Resultado |
|---|---:|
| Labels NER | 19 |
| Entidades extraídas | 6381 |
| Textos com entidade NER | 2103 |
| Cobertura NER | 77,83% |
| K escolhido | 20 |
| Silhouette Score | 0,133795 |
| Cluster residual | 4 |
| Tamanho do cluster residual | 599 textos |

Esses resultados podem sofrer pequenas variações caso o notebook seja reexecutado, devido a diferenças de treinamento e inicialização, mas a versão oficial do projeto está documentada no notebook final e no relatório final.

---

## Comparação com a versão anterior

A versão V3.1 foi comparada com a versão V3.

| Versão | Labels | Entidades | Cobertura NER | K | Silhouette | Residual |
|---|---:|---:|---:|---:|---:|---:|
| V3 | 11 | 4310 | 63,14% | 11 | 0,109889 | 1016 |
| V3.1 | 19 | 6381 | 77,83% | 20 | 0,133795 | 599 |

A V3.1 foi escolhida como versão final porque:

- aumentou a cobertura do NER;
- extraiu mais entidades temáticas;
- aumentou o Silhouette Score;
- reduziu o cluster residual;
- separou melhor áreas que antes estavam misturadas;
- gerou clusters mais interpretáveis.

Em relação à V3, a V3.1 aumentou a cobertura temática da base e reduziu significativamente a quantidade de textos concentrados no cluster residual.

A redução do cluster residual foi especialmente importante porque esse era um dos principais problemas das versões anteriores. Na V3, muitos textos ainda ficavam agrupados em um grande bloco genérico. Com a V3.1, parte desses textos passou a ser explicada por novas labels, como metrologia, telecomunicações, mineração, bioprocessos, software, gestão e manufatura operacional.

---

## Interpretação dos clusters

A clusterização final gerou 20 grupos temáticos.

Entre os grupos identificados estão:

- energia, baterias e sistemas elétricos;
- IA, ciência de dados e visão computacional;
- automotiva, mobilidade e veículos elétricos;
- resíduos, meio ambiente e reaproveitamento;
- gestão, logística e negócios digitais;
- telecomunicações, conectividade e fotônica;
- química, polímeros, revestimentos e formulações;
- agro, alimentos e fertilizantes;
- segurança, identificação e autenticação;
- metrologia, testes, validação e qualidade;
- software, plataformas e serviços digitais;
- petróleo, gás e petroquímica;
- engenharia de materiais, manufatura e metalurgia;
- mineração, siderurgia e beneficiamento mineral;
- manufatura, produção operacional e indústria 4.0;
- sistemas, IoT, sensores e automação;
- construção civil e materiais de construção;
- saúde, biotecnologia e aplicações biomédicas;
- bioprocessos, bioativos e fermentação;
- projetos técnico-operacionais gerais e rotas tecnológicas.

Os nomes dos clusters foram atribuídos manualmente com base nos termos TF-IDF, entidades NER, títulos representativos e visualizações.

É importante destacar que o KMeans não gera nomes semânticos para os clusters. Ele apenas separa os textos em grupos numéricos. A nomeação foi uma etapa interpretativa posterior, feita a partir da análise dos padrões presentes em cada agrupamento.

---

## Cluster residual

Mesmo após o refinamento da V3.1, ainda permaneceu um cluster residual.

Esse cluster foi nomeado como:

**Projetos técnico-operacionais gerais e rotas tecnológicas**

Ele reúne textos com linguagem mais genérica ou transversal, contendo termos como:

- produção;
- sistema;
- plataforma;
- protótipo;
- rota;
- tecnologia;
- controle;
- otimização;
- componentes;
- equipamento.

Esse cluster não representa necessariamente uma falha do modelo. Ele também reflete a existência de textos curtos, genéricos ou interdisciplinares na base.

Muitos projetos de pesquisa e desenvolvimento usam vocabulário técnico amplo, mas sem explicitar claramente uma área temática específica. Por isso, mesmo com o refinamento das labels, alguns textos continuam sendo mais difíceis de separar.

A presença do cluster residual mostra uma limitação natural da base: nem todo texto possui informação temática suficiente para ser associado com segurança a uma área específica.

---

## Experimento adicional com mais épocas

Além da versão principal com 25 épocas, foi planejado um experimento adicional com mais épocas de treinamento do NER.

O objetivo desse experimento é verificar se um treinamento mais longo melhora:

- as métricas do NER;
- a cobertura de entidades;
- o Silhouette Score;
- o tamanho do cluster residual;
- a separação visual dos clusters.

Como o conjunto de treinamento do NER foi construído de forma semiautomática, o experimento com muitas épocas deve ser interpretado com cautela, pois pode ocorrer overfitting.

Overfitting, nesse caso, significa que o modelo pode aprender de forma muito específica os padrões dos termos usados para construir automaticamente as labels, sem necessariamente melhorar a capacidade real de generalização.

Por isso, a versão com 25 épocas permanece como versão oficial principal, e o treinamento com mais épocas é tratado como experimento complementar.

Caso o experimento com mais épocas apresente melhora consistente nas métricas e na interpretabilidade dos clusters, ele poderá ser documentado como uma análise adicional. Caso contrário, a versão de 25 épocas será mantida como resultado final principal.

---

## Limitações

A solução final possui algumas limitações.

A primeira limitação é que o conjunto de treinamento do NER foi construído de forma semiautomática. As entidades foram geradas a partir de termos de referência definidos manualmente e encontrados nos textos por expressões regulares. Essa abordagem permitiu criar um modelo funcional, mas não substitui uma anotação manual feita por especialistas.

A segunda limitação é que não houve uma base gold standard anotada manualmente. Sem uma base desse tipo, não é possível medir com total segurança a qualidade real das entidades extraídas pelo NER. As métricas de validação mostram se o modelo aprendeu bem os padrões definidos no treinamento, mas não garantem que todas as entidades estejam corretas do ponto de vista de um especialista.

A terceira limitação é que muitos projetos pertencem naturalmente a mais de uma área. Por exemplo, um projeto pode envolver inteligência artificial, saúde, sensores e software ao mesmo tempo. Essa interdisciplinaridade dificulta a separação perfeita dos clusters.

A quarta limitação está relacionada aos textos curtos ou genéricos. Alguns projetos possuem descrições muito pequenas ou usam vocabulário amplo, como “sistema”, “plataforma”, “tecnologia” e “produção”. Esses termos aparecem em diversas áreas e dificultam a identificação temática.

A quinta limitação é que o KMeans exige a escolha prévia do número de clusters. Neste projeto, foi escolhido K = 20 com base em métricas e interpretabilidade, mas outros valores poderiam gerar agrupamentos diferentes.

A sexta limitação é que o Silhouette Score, embora tenha melhorado em relação à V3, ainda não é alto em termos absolutos. Isso é esperado em bases textuais reais, interdisciplinares e com temas sobrepostos.

Essas limitações não invalidam o resultado, mas mostram que a clusterização deve ser entendida como uma ferramenta exploratória, e não como uma classificação definitiva.

---

## Trabalhos futuros

Possíveis melhorias futuras incluem:

- anotação manual de uma amostra dos textos;
- criação de uma base gold standard;
- revisão manual das entidades extraídas pelo NER;
- teste de outros valores de K;
- uso de UMAP + HDBSCAN;
- uso de BERTopic;
- teste de embeddings de sentença;
- refinamento das labels menores;
- avaliação manual dos clusters por especialistas.

Uma melhoria importante seria revisar manualmente exemplos de entidades extraídas para cada label, classificando-as como corretas, incorretas ou duvidosas. Isso permitiria medir melhor a qualidade real do NER.

Também seria útil testar modelos de sentence embeddings, pois eles podem representar textos completos de forma mais adequada do que o uso direto do BERTimbau com mean pooling.

Outra possibilidade seria testar algoritmos de clusterização que não exigem a definição prévia do número de clusters, como HDBSCAN. Esse tipo de abordagem poderia lidar melhor com clusters de tamanhos diferentes e com textos que ficam entre várias áreas.

Também seria interessante comparar a versão de 25 épocas com a versão experimental de 500 épocas, verificando se o aumento do treinamento realmente melhora a clusterização ou apenas gera overfitting.

---

## Conclusão

Este projeto mostrou que a combinação de BERTimbau, TF-IDF/SVD e NER personalizado pode melhorar a clusterização temática de textos científicos e tecnológicos.

O BERTimbau contribuiu com a representação semântica dos textos, permitindo capturar relações de significado que não aparecem apenas pela contagem de palavras.

O TF-IDF/SVD preservou termos técnicos importantes, ajudando a diferenciar áreas com vocabulário específico.

O NER personalizado adicionou conhecimento de domínio à representação final, transformando indícios temáticos em features estruturadas.

A versão V3.1 foi escolhida como solução final porque apresentou melhor cobertura temática, maior interpretabilidade dos clusters, maior Silhouette Score e redução do cluster residual em relação à versão anterior.

Em comparação com a V3, a V3.1 aumentou o número de labels, extraiu mais entidades, cobriu mais textos com NER e reduziu a quantidade de projetos no cluster residual.

A clusterização final não deve ser vista como uma classificação definitiva dos projetos, mas como uma ferramenta exploratória para organizar, analisar e compreender padrões temáticos em uma base interdisciplinar.

Para o objetivo do trabalho, a V3.1 foi considerada a melhor versão obtida.
