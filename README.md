# tutoria_modelagem_2
# Classificação de Discurso de Ódio em Português com DistilBERT

Este projeto demonstra o processo de fine-tuning de um modelo de linguagem pré-treinado para a tarefa de classificação de discurso de ódio em português. O notebook `tutoria_modelagem_desafio.ipynb` executa todo o fluxo de trabalho, desde o pré-processamento dos dados até a avaliação e comparação de modelos.

Todos os experimentos, métricas e artefatos gerados são registrados na plataforma Weights & Biases (W&B) para garantir a rastreabilidade e reprodutibilidade.

## Metodologia

O projeto foi executado em um ambiente Google Colab e segue os seguintes passos:

1.  **Preparação do Ambiente**: Instalação das bibliotecas necessárias, como `transformers`, `datasets`, `evaluate` e `wandb`.

2.  **Carregamento e Divisão dos Dados**:
    *   O dataset Portuguese Hate Speech Dataset é baixado a partir de um arquivo CSV.
    *   A coluna de label é normalizada para um formato padrão (`no-hate`, `hate`).
    *   O dataset é dividido de forma estratificada em três conjuntos: **treino (64%)**, **validação (16%)** e **teste (20%)**.

3.  **Tokenização**: Os textos de todos os conjuntos são processados pelo tokenizer do modelo `adalbertojunior/distilbert-portuguese-cased`, convertendo o texto em um formato numérico que o modelo pode entender.

4.  **Avaliação Baseline (Zero-Shot)**:
    *   O modelo `adalbertojunior/distilbert-portuguese-cased` é usado "fora da caixa", sem nenhum treinamento, para fazer previsões no conjunto de validação.
    *   A métrica **F1-Score (macro)** é calculada para estabelecer uma linha de base de desempenho.
    *   O resultado é salvo localmente (`baseline_val_results.json`) e registrado no W&B.

5.  **Fine-Tuning do Modelo**:
    *   O mesmo modelo é treinado (fine-tuned) usando o conjunto de treino.
    *   A cada época, o modelo é avaliado no conjunto de validação, e as métricas (loss, F1-Score, acurácia) são automaticamente enviadas para o W&B.
    *   O melhor modelo encontrado durante o treinamento é salvo ao final do processo.

6.  **Avaliação Final**:
    *   O melhor modelo fine-tuned é carregado e avaliado nos conjuntos de **validação** e **teste**.
    *   Os resultados finais são salvos localmente (`fine_tuned_results.json`) e enviados para o W&B.
    *   O modelo treinado é salvo como um "artefato" no W&B, permitindo seu versionamento e reuso.

7.  **Comparação de Resultados**: Os resultados do modelo baseline e do modelo fine-tuned são comparados para quantificar a melhoria obtida com o treinamento.

## Resultados

A comparação entre o modelo pré-treinado (baseline) e o modelo após o fine-tuning demonstra uma melhoria significativa no desempenho, validando a eficácia do processo de treinamento.

| Modelo | F1-Score (Macro) no Conjunto de Validação |
| :--- | :--- |
| Baseline (sem treino) | ~0.37 |
| **Fine-Tuned** | **~0.73** |

*Os resultados detalhados de cada execução, incluindo gráficos de aprendizado e matrizes de confusão, podem ser visualizados no dashboard do projeto no Weights & Biases.*

## Como Executar

1.  **Ambiente**: Este notebook foi projetado para ser executado no **Google Colab**.

2.  **Instalação**: A primeira célula do notebook (`tutoria_modelagem_desafio.ipynb`) cuida da instalação de todas as dependências necessárias.

3.  **Weights & Biases (W&B)**:
    *   Para registrar os experimentos, você precisará de uma conta no Weights & Biases.
    *   Ao executar o código, será solicitado que você insira sua chave de API do W&B. Você pode encontrá-la em `wandb.ai/authorize`.

4.  **Execução**: Após a configuração, basta executar as células do notebook em sequência.

## Estrutura do Repositório

- `tutoria_modelagem_desafio.ipynb`: Notebook principal contendo todo o código do projeto.
- `README.md`: Este arquivo.
- `.gitignore`: Arquivo para ignorar arquivos e pastas desnecessários no controle de versão (como os checkpoints do modelo).
