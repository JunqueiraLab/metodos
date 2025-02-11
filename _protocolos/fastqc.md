---
layout: default
title: "Protocolo - FastQC"
permalink: /protocolos/fastqc/
---
[⬅ Voltar para a página principal](/metodos/)

# 🔬 FastQC - Análise da qualidade das sequências
Os sequenciadores de nova geração geram reads no formato fastq, que contém a informação da chamada da base nucleotídica em cada sítio (**basecalling**) juntamente com o valor de qualidade da chamada. A qualidade das bases reflete um valor de confiança associado a uma probabilidade de erro de leitura. Este valor é codificado no arquivo fastq e programas como o FastQC podem decodificar estes valores em gráficos e informações que ajudam no controle de qualidade dos dados gerados em plataformas de nova geração. Cada teste recebe um *status* (pass, warning ou fail) e desta forma é possível avaliar a qualidade dos dados que você possui para dar prosseguimento (ou não) às análises do seu projeto. Para um conjunto de dados de sequências gerados em plataforma Illumina, o valor da qualidade associada à probabilidade de erro é dado de acordo com a tabela abaixo:

<img src="/metodos/images/phred_score.png" alt="bascalling" width="20%" style="border-radius: 10px;">

