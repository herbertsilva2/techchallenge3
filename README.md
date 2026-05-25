# Tech Challenge 3

Protótipo acadêmico de um assistente especializado em saúde da mulher, treinado com fine-tuning supervisionado sobre `Llama 3.1 8B Instruct` usando `Unsloth` e `LoRA`.

O projeto foi desenvolvido como prova de conceito para o Tech Challenge 3 da FIAP, com foco em triagem textual e apoio conversacional em cenários como:

- triagem ginecológica;
- apoio obstétrico;
- planejamento familiar;
- sinais de alerta para câncer;
- identificação de possíveis situações de violência doméstica.

O fluxo principal do repositório está no notebook [local_llama_unsloth_colab_v5.ipynb](local_llama_unsloth_colab_v5.ipynb), pensado para execução no Google Colab com GPU.

## Visão geral

O repositório reúne:

- um notebook de treinamento e teste do modelo;
- datasets em formato JSONL para fine-tuning;
- documentação técnica com escopo, limitações e decisões do experimento.

O modelo base utilizado no notebook é `unsloth/Meta-Llama-3.1-8B-Instruct-bnb-4bit`, com adaptação via LoRA para reduzir custo computacional e permitir treinamento em ambiente Colab.

## Estrutura do projeto

```text
.
|-- README.md
|-- RELATORIO_TECNICO.md
|-- local_llama_unsloth_colab_v5.ipynb
`-- finetunnig/
	|-- casos_triagem_obstetricia.jsonl
	|-- planejamento_familiar.jsonl
	|-- protocolos_atendimento_gine_obs.jsonl
	|-- protocolos_triagem_cancer.jsonl
	|-- triagem_saude_mulher_dataset.jsonl
	|-- violence_detection_finetuning.jsonl
	`-- duplicado/
```

## Como rodar

### Google Colab

Este é o fluxo mais aderente ao que já está implementado no projeto.

#### Pré-requisitos

- conta Google com acesso ao Google Colab e Google Drive;
- GPU habilitada no Colab, de preferência T4 ou superior;
- arquivos JSONL disponíveis no Drive.

#### Passo a passo

1. Abra o notebook [local_llama_unsloth_colab_v5.ipynb](local_llama_unsloth_colab_v5.ipynb) no Google Colab.
2. No Colab, habilite GPU em `Runtime > Change runtime type > GPU`.
3. Execute as células de instalação de dependências presentes no notebook:

```bash
!pip install -U unsloth
!pip install -U --no-deps trl peft accelerate bitsandbytes datasets transformers
```

4. Monte o Google Drive quando a célula solicitar:

```python
from google.colab import drive
drive.mount('/content/drive')
```

5. Crie no seu Drive a estrutura esperada pelo notebook:

```text
MyDrive/techchallenge3/data/
MyDrive/techchallenge3/outputs/
```

6. Copie os arquivos JSONL da pasta [finetunnig](finetunnig) para `MyDrive/techchallenge3/data/`.
7. Execute o notebook na ordem das células para:

- validar os datasets;
- carregar e formatar os dados;
- carregar o modelo base com Unsloth;
- aplicar o fine-tuning com LoRA;
- salvar o adapter e o tokenizer.

8. Ao final do treino, os artefatos são salvos em:

```text
/content/drive/MyDrive/techchallenge3/outputs/llama-womens-health-triage-lora
```

O notebook também inclui etapas de teste/inferência e trechos adicionais para experimentos complementares.


## Limitações e aviso importante

Este projeto é uma prova de conceito acadêmica.

- Não é um sistema clínico pronto para uso real.
- Não há validação clínica formal por especialistas no repositório.
- O material não apresenta métricas clínicas finais como sensibilidade, acurácia ou F1-score.
- O uso adequado é como experimento técnico supervisionado, e não como ferramenta autônoma de diagnóstico ou prescrição.

Mais contexto técnico e metodológico está documentado em [RELATORIO_TECNICO_v3.pdf](RELATORIO_TECNICO_v3.pdf).
