---
layout: default
title: "Protocolo - QUAST, Anotação e Resistoma"
permalink: /protocolos/genoma/
---
[⬅ Voltar para a página principal](/metodos/)


<div style="max-width: 600px; margin: 2em auto; padding: 1em; border: 2px solid #ddd; border-radius: 10px; background-color: #f9f9f9; box-shadow: 0 0 5px #ccc;">
  <h3 style="text-align: left;">🔗 Índice</h3>
  <ul style="list-style-type: none; padding-left: 0; font-size: 16px; line-height: 1.8;">


# Qualidade de Montagens, Anotação e Resistoma

### 1. QUAST - Qualidade de montagens
Para a avaliação da qualidade de montagens, vamos utilizar o programa QUAST (Mikheenko et al. 2018), que é capaz de calcular métricas comumente utilizadas para a avaliação de montagens, como o N50 e o L50, além de gerar análises sobre a distribuição dos tamanhos dos contigs e/ou scaffolds e do conteúdo GC do genoma. 
Vamos instalar o programa no ambiente criado para a disciplina. Para isso, primeiro precisamos ativar o ambiente com o seguinte comando:

`micromamba activate metodos`

Caso o QUAST não esteja instalado no seu ambiente, vamos instalá-lo a partir do repositório de programas de bioinformática chamado Bioconda, com o comando:

`micromamba install -c bioconda quast`

Quando o programa estiver instalado, vamos iniciar a análise de qualidade do genoma montado com o seguinte comando:

`quast nome_do_arquivo.fasta -o output`

❗ Não esqueça de substituir o “nome_do_arquivo” pelo do seu genoma montado e dê o nome de sua preferência para o diretório de output.

Após a análise ter terminado, mude para o diretório de output e abra o arquivo **report.html**.

Analise os resultados:
- ❓ Quantos contigs/scaffolds tem o genoma montado?
- ❓ Qual o maior contig/scaffold da montagem?
- ❓ Qual o tamanho total do genoma montado? Este tamanho é parecido com outros genomas de _E. anophelis_?
- ❓ Qual o N50 da sua montagem? O que significa essa métrica?
- ❓ Qual o número de scaffolds contém 50% do genoma montado?



[⬅ Voltar para a página principal](/metodos/)


---
### 📖 **Referências**
