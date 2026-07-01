# Relatório do Projeto

# Clusterização Temática de Projetos Científicos com BERTimbau, NER e KMeans

## 1. Introdução

Este projeto teve como objetivo construir uma solução de clusterização temática para uma base de projetos científicos e tecnológicos, utilizando títulos e descrições públicas dos projetos.

A base de dados não possuía uma coluna oficial de categoria temática. Por isso, o problema foi tratado como uma tarefa de aprendizado não supervisionado, mais especificamente uma tarefa de clusterização textual.

O principal desafio foi que os textos da base eram muito interdisciplinares. Muitos projetos misturavam áreas como inteligência artificial, saúde, energia, materiais, automação, software, agroindústria, biotecnologia, petróleo, mineração e processos industriais. Além disso, vários textos possuíam linguagem genérica, com termos como “sistema”, “produção”, “plataforma”, “tecnologia”, “protótipo” e “controle”, que aparecem em diversas áreas e dificultam a separação temática.

Por esse motivo, uma abordagem baseada apenas em embeddings semânticos poderia não ser suficiente para gerar clusters interpretáveis. A solução final combinou três fontes de informação:

- embeddings semânticos com BERTimbau;
- representação léxica com TF-IDF e SVD;
- sinais temáticos estruturados extraídos por um modelo NER personalizado.

A versão final do projeto foi chamada de V3.1.

---

## 2. Objetivo

O objetivo principal foi agrupar projetos semelhantes em clusters temáticos interpretáveis.

Os objetivos específicos foram:

- preparar e limpar a base textual;
- analisar os termos mais frequentes e relevantes da base;
- criar labels temáticas para auxiliar a identificação de áreas;
- treinar um modelo NER personalizado com spaCy;
- gerar embeddings semânticos com BERTimbau;
- construir uma representação híbrida combinando BERTimbau, TF-IDF/SVD e NER;
- aplicar KMeans para clusterização;
- interpretar os clusters com base em termos, entidades e títulos representativos;
- comparar a evolução entre versões do modelo;
- reduzir o tamanho do cluster residual.

---

## 3. Base de dados

A base utilizada contém projetos científicos e tecnológicos com as seguintes colunas principais:

- `Título_Público`;
- `Descricao_pública`.

Essas duas colunas foram unidas para formar o texto principal usado no projeto.

Também foi criada uma versão com título reforçado, na qual o título foi repetido antes da descrição. Essa estratégia foi adotada porque o título normalmente contém forte informação temática sobre o projeto.

Foram criadas as seguintes colunas textuais:

- `texto_original`: união do título com a descrição;
- `texto_titulo_reforcado`: título repetido e unido à descrição;
- `texto`: texto principal utilizado nas análises gerais.

---

## 4. Preparação textual

A preparação textual incluiu:

- preenchimento de valores ausentes;
- remoção de espaços duplicados;
- junção de título e descrição;
- cálculo da quantidade de palavras;
- remoção de duplicatas exatas;
- remoção conservadora de ruídos.

A remoção de ruídos foi feita de forma conservadora para evitar excluir projetos válidos. Foram removidos apenas textos extremamente curtos ou sem informação suficiente.

A base final utilizada na versão V3.1 ficou com aproximadamente 2700 textos.

---

## 5. Análise exploratória com TF-IDF

Antes da criação das labels temáticas, foi feita uma análise exploratória com TF-IDF.

Essa etapa permitiu observar os principais termos da base e entender quais áreas apareciam com maior frequência.

Entre os termos mais recorrentes estavam palavras relacionadas a:

- software;
- dados;
- plataforma;
- controle;
- monitoramento;
- sistemas;
- inteligência artificial;
- energia;
- materiais;
- dispositivos;
- automação;
- gestão;
- produção;
- resíduos.

Essa análise mostrou que a base tinha forte presença de projetos tecnológicos aplicados, mas também confirmou que muitos termos eram genéricos e apareciam em diferentes áreas.

Por isso, foi necessário criar uma camada adicional de conhecimento de domínio, representada pelas labels temáticas do NER.

---

## 6. Estratégia geral da solução

A solução final seguiu a seguinte lógica:

1. preparar a base textual;
2. identificar termos relevantes por análise exploratória;
3. criar labels temáticas;
4. gerar exemplos de treinamento para NER de forma semiautomática;
5. treinar um modelo NER personalizado;
6. extrair entidades temáticas dos textos;
7. gerar embeddings com BERTimbau;
8. gerar representação TF-IDF/SVD;
9. combinar BERTimbau, TF-IDF e NER em uma representação híbrida;
10. aplicar KMeans;
11. interpretar os clusters;
12. comparar versões e escolher a melhor.

A ideia central foi transformar parte do conhecimento de domínio em features estruturadas por meio do NER. Assim, a clusterização não dependeria apenas da semântica geral dos textos, mas também de sinais temáticos explícitos.

---

## 7. Criação das labels temáticas

As labels temáticas foram criadas de forma iterativa.

No início, as labels eram mais gerais. Com o avanço dos testes, observou-se que alguns temas importantes ainda estavam sendo agrupados no cluster residual. A partir da análise desse residual, novas áreas foram adicionadas.

A versão final, chamada de V3.1, utilizou 19 labels:

1. `AREA_COMPUTACAO_DADOS`;
2. `AREA_ENERGIA`;
3. `AREA_RESIDUOS_MEIO_AMBIENTE`;
4. `AREA_SAUDE_BIOTECNOLOGIA`;
5. `AREA_ALIMENTOS_AGRO`;
6. `AREA_ENGENHARIA_MATERIAIS`;
7. `AREA_SISTEMAS_AUTOMACAO_MONITORAMENTO`;
8. `AREA_AUTOMOTIVA_MOBILIDADE`;
9. `AREA_PETROLEO_GAS_PETROQUIMICA`;
10. `AREA_QUIMICA_PROCESSOS_INDUSTRIAIS`;
11. `AREA_CONSTRUCAO_CIVIL_MATERIAIS`;
12. `AREA_SOFTWARE_PLATAFORMAS_DIGITAIS`;
13. `AREA_METROLOGIA_TESTES_QUALIDADE`;
14. `AREA_GESTAO_LOGISTICA_NEGOCIOS`;
15. `AREA_TELECOM_FOTONICA_CONECTIVIDADE`;
16. `AREA_SEGURANCA_IDENTIFICACAO_AUTENTICACAO`;
17. `AREA_MINERACAO_SIDERURGIA`;
18. `AREA_BIOPROCESSOS_BIOATIVOS_FERMENTACAO`;
19. `AREA_MANUFATURA_PRODUCAO_OPERACIONAL`.

A V3.1 foi criada para cobrir temas que antes estavam pouco representados, como telecomunicações, metrologia, mineração, bioprocessos, gestão/logística, software e manufatura operacional.

---

## 8. Motivo para criar a V3.1

Durante os experimentos anteriores, a V3 já apresentava bons clusters, mas ainda possuía um cluster residual muito grande.

Esse residual reunia projetos que não eram bem explicados pelas labels disponíveis até então. Ao analisar os termos e títulos desse agrupamento, apareceram temas recorrentes como:

- metrologia, testes e qualidade;
- software e plataformas digitais;
- telecomunicações, conectividade e fotônica;
- gestão, logística e negócios;
- mineração e siderurgia;
- bioprocessos e fermentação;
- manufatura e produção operacional.

Esses temas não estavam suficientemente representados na V3. Por isso, a V3.1 adicionou novas labels para capturar esses padrões.

A expectativa era que a V3.1 aumentasse a cobertura do NER, reduzisse o cluster residual e gerasse clusters mais específicos e interpretáveis.

---

## 9. Construção do training data do NER

A base não possuía entidades anotadas manualmente. Por isso, o conjunto de treinamento do NER foi criado de forma semiautomática.

Para cada label, foi definida uma lista de termos de referência. Esses termos foram buscados nos textos com expressões regulares. Quando encontrados, eram convertidos em entidades anotadas.

A estratégia incluiu:

- busca de termos completos;
- uso de fronteiras para evitar capturas incorretas;
- priorização de entidades maiores em caso de sobreposição;
- balanceamento de exemplos por label;
- inclusão de exemplos negativos;
- validação dos spans com spaCy.

Essa abordagem permitiu treinar um NER temático sem necessidade de anotação manual completa. Porém, ela também traz uma limitação importante: o modelo aprende a partir de padrões semiautomáticos, e não de uma base gold standard anotada por especialistas.

---

## 10. Modelo NER personalizado

O NER foi treinado com spaCy a partir das 19 labels da V3.1.

O objetivo do NER não foi reconhecer entidades tradicionais, como pessoas ou organizações, mas identificar indícios temáticos nos textos.

Exemplos:

- “inteligência artificial” → `AREA_COMPUTACAO_DADOS`;
- “baterias” → `AREA_ENERGIA`;
- “resíduos” → `AREA_RESIDUOS_MEIO_AMBIENTE`;
- “pacientes” → `AREA_SAUDE_BIOTECNOLOGIA`;
- “software” → `AREA_SOFTWARE_PLATAFORMAS_DIGITAIS`;
- “fibra óptica” → `AREA_TELECOM_FOTONICA_CONECTIVIDADE`;
- “minério” → `AREA_MINERACAO_SIDERURGIA`;
- “fermentação” → `AREA_BIOPROCESSOS_BIOATIVOS_FERMENTACAO`.

O NER foi usado como uma forma de inserir conhecimento de domínio na representação textual.

Na versão final principal, o modelo foi treinado com 25 épocas.

---

## 11. BERTimbau

Para capturar o significado semântico dos textos, foi utilizado o modelo BERTimbau:

`neuralmind/bert-base-portuguese-cased`

O BERTimbau foi escolhido por ser um modelo BERT treinado para a língua portuguesa.

Os embeddings foram gerados usando a versão com título reforçado dos textos. Essa escolha foi feita porque o título costuma resumir o tema central do projeto.

Foi usado mean pooling sobre os embeddings dos tokens, considerando a máscara de atenção.

Após a geração dos embeddings, foi aplicada normalização e redução dimensional com PCA.

---

## 12. TF-IDF e SVD

Além dos embeddings semânticos, foi usada uma representação TF-IDF.

O TF-IDF preserva informações importantes sobre termos técnicos específicos, que podem ser relevantes para diferenciar áreas.

Depois da matriz TF-IDF, foi aplicado TruncatedSVD para reduzir a dimensionalidade.

Essa etapa gerou uma representação léxica compacta, complementar ao BERTimbau.

---

## 13. Features do NER

A terceira parte da representação veio das entidades extraídas pelo NER.

Para cada texto, foram criadas duas informações por label:

- proporção de entidades daquela label no texto;
- indicador binário informando se a label apareceu ou não.

Como a V3.1 possui 19 labels, o bloco de features do NER ficou com 38 colunas.

Essas features ajudaram a clusterização a considerar explicitamente a presença de áreas temáticas.

---

## 14. Representação híbrida

A representação final combinou três grupos de features:

- BERTimbau/PCA;
- TF-IDF/SVD;
- NER.

Antes da combinação, cada grupo foi padronizado com `StandardScaler`.

Foram usados os seguintes pesos:

| Componente | Peso |
|---|---:|
| BERTimbau | 1.0 |
| TF-IDF/SVD | 0.8 |
| NER | 1.3 |

O peso do NER foi maior porque as entidades temáticas foram consideradas especialmente importantes para tornar os clusters mais interpretáveis.

Após a combinação, a matriz final foi normalizada com norma L2.

---

## 15. Clusterização com KMeans

A clusterização foi feita com KMeans.

Foram testados diferentes valores de `k`, e a escolha final foi:

| Parâmetro | Valor |
|---|---:|
| K final | 20 |

A escolha de `k = 20` foi baseada em:

- Silhouette Score;
- tamanho dos clusters;
- redução do cluster residual;
- interpretabilidade temática;
- separação visual no t-SNE.

Embora 20 clusters tornem o modelo mais granular, esse valor permitiu separar áreas que antes ficavam misturadas.

---

## 16. Resultados finais da V3.1

A versão final V3.1 apresentou os seguintes resultados:

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

Esses resultados indicam que a V3.1 conseguiu aumentar a cobertura temática da base e melhorar a separação dos clusters em comparação com a V3.

---

## 17. Comparação entre V3 e V3.1

A V3 foi a versão anterior mais importante antes da V3.1.

A comparação entre as duas versões foi:

| Versão | Labels | Entidades | Cobertura NER | K | Silhouette | Residual |
|---|---:|---:|---:|---:|---:|---:|
| V3 | 11 | 4310 | 63,14% | 11 | 0,109889 | 1016 |
| V3.1 | 19 | 6381 | 77,83% | 20 | 0,133795 | 599 |

A V3.1 apresentou melhora em todos os pontos principais.

A cobertura do NER aumentou de 63,14% para 77,83%.

O Silhouette Score aumentou de 0,109889 para 0,133795.

O cluster residual foi reduzido de 1016 textos para 599 textos.

A redução foi de 417 textos.

---

## 18. Interpretação dos clusters finais

A versão V3.1 gerou 20 clusters.

Os grupos identificados foram:

1. Energia, baterias e sistemas elétricos;
2. IA, ciência de dados e visão computacional;
3. Automotiva, mobilidade e veículos elétricos;
4. Projetos técnico-operacionais gerais e rotas tecnológicas;
5. Resíduos, meio ambiente e reaproveitamento;
6. Gestão, logística e negócios digitais;
7. Telecomunicações, conectividade e fotônica;
8. Química, polímeros, revestimentos e formulações;
9. Agro, alimentos e fertilizantes;
10. Segurança, identificação e autenticação;
11. Metrologia, testes, validação e qualidade;
12. Software, plataformas e serviços digitais;
13. Petróleo, gás e petroquímica;
14. Engenharia de materiais, manufatura e metalurgia;
15. Mineração, siderurgia e beneficiamento mineral;
16. Manufatura, produção operacional e indústria 4.0;
17. Sistemas, IoT, sensores e automação;
18. Construção civil e materiais de construção;
19. Saúde, biotecnologia e aplicações biomédicas;
20. Bioprocessos, bioativos e fermentação.

A nomeação dos clusters foi feita manualmente a partir de:

- termos TF-IDF principais;
- entidades NER dominantes;
- títulos representativos;
- cobertura do NER;
- visualização t-SNE.

É importante destacar que o KMeans não gera nomes para os clusters. Ele gera apenas agrupamentos numéricos. Os nomes foram atribuídos por interpretação humana.

---

## 19. Cluster residual

Mesmo após o refinamento da V3.1, ainda permaneceu um cluster residual.

Esse cluster teve 599 textos e foi nomeado como:

`Projetos técnico-operacionais gerais e rotas tecnológicas`

Esse grupo contém projetos com linguagem genérica ou transversal.

Os termos mais associados ao residual costumam ser palavras como:

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

Esses termos são muito comuns em projetos de pesquisa e desenvolvimento, mas não indicam necessariamente uma área temática específica.

Por isso, o residual não deve ser interpretado apenas como falha. Ele também representa a existência de textos curtos, genéricos ou interdisciplinares.

---

## 20. Visualização t-SNE

A visualização t-SNE mostrou que vários clusters ficaram bem separados visualmente.

Clusters como energia, automotiva, petróleo, software, telecomunicações, mineração, saúde, bioprocessos e resíduos apresentaram regiões visuais mais claras.

O cluster residual ficou maior e mais espalhado, o que é coerente com sua interpretação como grupo de textos genéricos ou transversais.

A visualização confirmou que a V3.1 conseguiu separar melhor diversas áreas específicas que antes apareciam misturadas.

---

## 21. Decisão final

A V3.1 foi escolhida como versão final porque apresentou o melhor equilíbrio entre métrica e interpretabilidade.

A versão final:

- aumentou a cobertura do NER;
- extraiu mais entidades temáticas;
- melhorou o Silhouette Score;
- reduziu o cluster residual;
- separou áreas tecnológicas mais específicas;
- tornou os clusters mais interpretáveis.

Apesar de ser mais complexa, com 19 labels e 20 clusters, a V3.1 foi a melhor solução obtida para o objetivo exploratório do trabalho.

---

## 22. Experimento adicional com mais épocas

A versão principal foi treinada com 25 épocas.

Como experimento adicional, será testado um treinamento com 500 épocas.

Esse experimento tem como objetivo verificar se um treinamento mais longo do NER pode melhorar:

- as métricas de validação do NER;
- a quantidade de entidades extraídas;
- a cobertura do NER;
- o Silhouette Score;
- o tamanho do cluster residual;
- a separação visual dos clusters.

No entanto, esse experimento deve ser interpretado com cautela.

Como o training data foi criado de forma semiautomática, um número muito alto de épocas pode fazer o modelo decorar padrões específicos das labels, causando overfitting.

Por isso, o resultado oficial principal permanece sendo a versão com 25 épocas. O teste com 500 épocas será tratado como experimento complementar.

Para reduzir o risco de overfitting, o dropout do NER será ajustado de 0.25 para 0.30 durante esse experimento.

---

## 23. Limitações

A solução possui algumas limitações.

A primeira limitação é que o training data do NER foi construído de forma semiautomática, e não por anotação manual de especialistas.

A segunda limitação é que as áreas dos projetos são naturalmente sobrepostas. Um projeto pode envolver, ao mesmo tempo, inteligência artificial, saúde, sensores e software.

A terceira limitação é a existência de textos curtos ou genéricos, que dificultam a identificação temática.

A quarta limitação é que o KMeans exige a escolha prévia do número de clusters.

A quinta limitação é que o Silhouette Score, embora tenha melhorado, ainda não é alto em termos absolutos. Isso é esperado em bases textuais reais, interdisciplinares e com temas sobrepostos.

---

## 24. Trabalhos futuros

Como melhorias futuras, poderiam ser realizados:

- anotação manual de uma amostra dos textos;
- criação de uma base gold standard;
- revisão manual das entidades extraídas pelo NER;
- teste com outros valores de `k`;
- teste com outros algoritmos de clusterização;
- uso de UMAP + HDBSCAN;
- uso de BERTopic;
- teste de modelos específicos de sentence embeddings;
- refinamento das labels menores;
- avaliação manual dos clusters por especialistas.

Também seria interessante comparar a versão de 25 épocas com a versão experimental de 500 épocas, verificando se o aumento do treinamento realmente melhora a clusterização ou apenas gera overfitting.

---

## 25. Conclusão

Este projeto mostrou que a combinação de BERTimbau, TF-IDF/SVD e NER personalizado pode melhorar a clusterização temática de textos científicos e tecnológicos.

O uso do BERTimbau permitiu capturar relações semânticas entre os textos.

O TF-IDF/SVD preservou termos técnicos relevantes.

O NER personalizado adicionou conhecimento de domínio à representação final.

A versão V3.1 foi escolhida como solução final porque apresentou melhor cobertura temática, maior Silhouette Score, redução do cluster residual e melhor interpretabilidade dos agrupamentos.

A clusterização final não deve ser vista como uma classificação definitiva dos projetos, mas como uma ferramenta exploratória para organizar, analisar e compreender padrões temáticos em uma base interdisciplinar.

Para o objetivo do trabalho, a V3.1 foi considerada a melhor versão obtida.
