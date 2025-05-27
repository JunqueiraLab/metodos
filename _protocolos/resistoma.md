---
layout: default
title: "Protocolo - BLAST"
permalink: /protocolos/resistoma/
---
[⬅ Voltar para a página principal](/metodos/)

# 🔬 Resistoma

Para identificar genes que podem estar envolvidos na resistência a antimicrobianos, iremos utilizar o programa RGI (Resistance Gene Identifier). O RGI é um programa que faz a predição do resistoma a partir do alinhamento de sequências de nucleotídeos ou proteínas (incluindo dados metagenômicos) contra o banco curado de dados CARD (The Comprehensive Antibiotic Resistance Database) (Alcock et al. 2022). Os resultados se baseiam na homologia dos hits contra o CARD e em modelos de detecção de polimorfismos (SNPs). O RGI pode ser usado com os alinhadores BLAST ou DIAMOND e as análises podem ser conduzidas no portal RGI ou localmente, a partir da instalação com o conda/mamba/micromamba. Para a análise do resistoma do genoma montado, vamos usar a versão do RGI online:

1.	Acesse o link: [https://card.mcmaster.ca/analyze/rgi](https://card.mcmaster.ca/analyze/rgi){:target="_blank"}
2.	Faça o upload da montagem em formato FASTA (`aula_04`) e mantenha as opções default selecionadas (DNA sequence, Perfect and Strict hits only, Exclude Nudge, High Quality/Coverage)
3.	Clique no botão Submit. Uma tabela com os resultados vai aparecer quando as análises estiverem prontas.

Explore os resultados das abas disponíveis (AMR Genes, Drug Class, Resistance Mechanism)

[⬅ Voltar para a página principal](/metodos/)

### 📖 **Referências**
- Alcock BP, Huynh W, Chalil R, et al (2022) CARD 2023: expanded curation, support for machine learning, and resistome prediction at the Comprehensive Antibiotic Resistance Database. Nucleic Acids Res 51:D690–D699. [https://doi.org/10.1093/nar/gkac920](https://doi.org/10.1093/nar/gkac920){:target="_blank"}
