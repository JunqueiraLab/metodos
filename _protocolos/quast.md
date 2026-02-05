---
layout: default
title: "Protocolo - QUAST"
permalink: /protocolos/quast/
---
[⬅ Voltar para a página principal](/metodos/)


# 🧬 QUAST - Qualidade de Montagens

Após a montagem de um genoma, é fundamental avaliar sua contiguidade e integridade para assegurar a confiabilidade das análises subsequentes. Para isso, utilizamos programas que calculam métricas de contiguidade e completude dos genomas. O QUAST (Quality Assessment Tool for Genome Assemblies) é um programa que calcula métricas como N50, L50, tamanho total da montagem, número de contigs, além de gerar gráficos sobre a distribuição do tamanho dos contigs e/ou scaffolds e do conteúdo de GC (Mikheenko et al. 2018). Ele também permite a comparação da sua montagem com genomas de referência, quando disponíveis, facilitando a identificação de possíveis falhas ou fragmentações na montagem. É amplamente utilizado em análises de bioinformática por sua eficiência, praticidade e resultados detalhados.

1. Certifique-se de que o programa está instalado e disponível no ambiente de gerenciamento criado para a disciplina. Para isso, ative o ambiente com o seguinte comando:

   `micromamba activate metodos`

2. Para testar se o QUAST está instalado no seu sistema, vamos chamar o comando do programa do seu terminal:

   `quast`

   Caso o programa esteja instalado, você verá um resumo das opções disponíveis para o programa. Se não estiver instalado, um warning vai aparecer na linha de comando:

   `-bash: quast: command not found`

   Se o programa estiver instalado, pule para o passo 4.

3. Caso o QUAST não esteja instalado no seu ambiente, vamos instalá-lo a partir do repositório de programas de bioinformática chamado Bioconda, com o comando:

   `micromamba install -c bioconda quast`

4. Com o programa instalado, vamos iniciar a análise de qualidade do genoma montado com o seguinte comando:

   `quast nome_do_arquivo.fasta -o output`

❗ Não esqueça de substituir o “nome_do_arquivo” pelo do seu genoma montado, no diretório `aula_04`, e dê o nome de sua preferência para o diretório de output.

5. Após a análise ter terminado, mude para o diretório de output e abra o arquivo **report.html**.

Analise os resultados:
- ❓ Quantos contigs/scaffolds tem o genoma montado?
- ❓ Qual o maior contig/scaffold da montagem?
- ❓ Qual o tamanho total do genoma montado? Este tamanho é parecido com outros genomas de _E. anophelis_?
- ❓ Qual o N50 da sua montagem? O que significa essa métrica?
- ❓ Qual o número de scaffolds contém 50% do genoma montado?



[⬅ Voltar para a página principal](/metodos/)


---
### 📖 **Referências**

- Mikheenko A, Prjibelski A, Saveliev V, et al (2018) Versatile genome assembly evaluation with QUAST-LG. Bioinformatics 34:i142–i150. https://doi.org/10.1093/bioinformatics/bty266

