---
layout: default
title: "Protocolo - Análise de Sequências com BLAST"
permalink: /protocolos/blast/
---

# 🔬 Análise de Sequências com BLAST

Ao final da busca, o BLAST retorna uma lista de sequências do banco de dados (**subject**) que apresentam alinhamentos significativos com a sequência de interesse, classificando-as por ordem decrescente de significância estatística. Isso permite que os pesquisadores identifiquem rapidamente as sequências mais similares à **query** e possam então realizar análises adicionais, como:

- **Atribuição taxonômica**
- **Construção de árvores filogenéticas**
- **Análise funcional**

## 🔹 Módulos do BLAST

Existem cinco módulos do BLAST que podem ser utilizados para diferentes tipos de análise:

- **blastn** - Compara a sequência de nucleotídeos contra um banco de dados de nucleotídeos (**nt-nt**).
- **blastp** - Compara sequências de aminoácidos contra um banco de dados de proteínas (**aa-aa**).
- **blastx** - Traduz uma sequência de nucleotídeos nos 6 quadros de leitura possíveis e utiliza as traduções para buscas em bancos de dados de proteínas (**nt traduzido-aa**).
- **tblastn** - Busca por similaridade entre uma sequência de aminoácidos contra a tradução em 6 quadros de leitura possíveis do banco de dados de nucleotídeos (**aa - nt traduzido**).
- **tblastx** - Traduz uma sequência de nucleotídeo nos 6 quadros de leitura possíveis e compara os resultados contra a tradução das sequências nucleotídicas do banco de dados nos 6 quadros de leitura possíveis. É o mais lento dos algoritmos e tem por objetivo encontrar similaridades entre sequências de nucleotídeos distantemente relacionadas (**nt traduzido - nt traduzido**).

---

## 🌐 **BLAST Online**

Agora vamos fazer um exercício para analisar os resultados do BLAST online, utilizando o webserver do NCBI, de uma sequência desconhecida retirada do arquivo `illumina.fastq`.

### **Passos para executar o BLAST no NCBI**

1. **Acesse a página do BLAST no NCBI:** [Clique aqui](https://blast.ncbi.nlm.nih.gov/Blast.cgi)
2. **Escolha** “Nucleotide BLAST” (**blastn**).
3. **Abra o arquivo `AE001.fasta`** na pasta da `aula01`, copie e cole a sequência no campo **“Enter Query Sequence”**.
4. **Atente para os parâmetros da busca.**
5. **Analise os resultados:**
   - **A qual organismo a sequência pertence?**
   - **Qual o valor do e-value?**
   - **Qual o valor do bit-score?**
   - **Você acha este um resultado confiável?**

📌 **Dica:** O **e-value** indica a significância do alinhamento. Quanto **menor o e-value**, mais significativo é o resultado.

🔬 Agora, analise os resultados e registre suas observações! 😊
