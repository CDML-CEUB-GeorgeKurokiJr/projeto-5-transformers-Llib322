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

notebooks

Contém os notebooks utilizados no projeto.

O notebook principal é:

notebooks/projeto-5-ner-bertimbau-final-v4.ipynb

Esse é o notebook final, limpo e organizado, contendo a versão oficial do pipeline.

Ele inclui:

carregamento da base;
preparação textual;
análise exploratória;
criação das labels temáticas;
treinamento do NER personalizado;
geração de embeddings com BERTimbau;
construção da representação híbrida;
clusterização com KMeans;
interpretação dos clusters;
comparação entre versões;
salvamento dos resultados finais.
notebooks/notebooks_de_teste

Contém os notebooks usados durante o desenvolvimento do projeto.

Nessa pasta estão os testes intermediários, versões experimentais e também a versão utilizada na entrega temporária.

Esses notebooks foram mantidos para documentar a evolução do trabalho, mas não são a versão oficial para avaliação.

relatorios

Contém os relatórios do projeto.

O arquivo principal é:

relatorios/RELATORIO_FINAL.md

Esse é o relatório final, com a explicação completa da metodologia, das decisões tomadas, dos testes realizados, dos resultados obtidos e das limitações da solução.

Também há o arquivo:

relatorios/RELATORIO_TEMPORARIO.md

Esse relatório corresponde à entrega provisória e foi mantido no repositório apenas para registro histórico do desenvolvimento.

arquivos_zipados

Contém os resultados finais do notebook comprimidos em arquivo .zip.

O arquivo principal é:

arquivos_zipados/resultados_finais_v31.zip

Esse arquivo reúne os resultados gerados pelo notebook final, como tabelas de clusters, entidades extraídas, métricas, resumos e demais artefatos do pipeline.
