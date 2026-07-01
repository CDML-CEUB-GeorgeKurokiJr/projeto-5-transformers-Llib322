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
