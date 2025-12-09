# Visão-Computacional-UFSC-INE410121

Trabalho apresentado na disciplina de visão computacional na pós graduação de Ciência da Computação da UFSC

# 🫁 CNNs para Classificação de Doenças Pulmonares a partir de Raios-X  
## Abordagem Comparativa com Imagem Original vs. Segmentação de Pulmão

Este repositório apresenta o pipeline completo de um estudo experimental usando **Convolutional Neural Networks (CNNs)** para classificação de patologias pulmonares em imagens de raio-X.  
Foram desenvolvidas **duas abordagens distintas**:

1. **Treinamento em imagens originais**
2. **Treinamento em imagens contendo apenas o pulmão**, obtidas via segmentação automática + dilatação morfológica

Ambas as abordagens passaram por dois estágios:

- **Transfer Learning** (congelando parte da rede)
- **Fine-tuning** (descongelando camadas e refinando o modelo)

---

# 📂 Estrutura do Repositório

```
.
├── Dataset_lung_only/                  # Dataset segmentado (apenas pulmão)
├── dataset_normal/                     # Dataset original
├── lung_create.ipynb                   # Notebook: geração do dataset segmentado + dilatação morfológica
├── CNN_models-apres-lung-only.ipynb    # Notebook: treinamento das CNN no dataset segmentado
├── CNN_models-apres-reduzido.ipynb     # Notebook: treinamento das CNN no dataset original
├── README.md
├── LICENSE
```

Cada notebook contém:

- Preparação dos DataLoaders  
- Criação do learner (FastAI)  
- Transfer Learning  
- Fine-tuning  
- Saving de histórico  
- Geração de métricas  
- Export para CSV por época  
- Link para download dos checkpoints treinados  

---

# 🧪 1. Criação do Dataset "Lung-Only"

A abordagem segmentada foi construída para avaliar se remover regiões externas ao pulmão aumenta a robustez do modelo.

## 📌 Passos realizados (notebook `lung_create.ipynb`)

1. Utilização do modelo **TorchXRayVision – PSPNet** para segmentação.
2. Extração de **máscaras esquerda/direita**.
3. Fusão das máscaras para gerar uma máscara única do pulmão completo.
4. **Dilatação morfológica**:
   - Garante que bordas, costelas e regiões periféricas não sejam cortadas.
   - Cria uma área de segurança ao redor dos pulmões.
5. Aplicação da máscara na imagem original → gerando imagens "lung-only".
6. Organização da estrutura do dataset final com as pastas por classes.

---

# 🧠 2. Treinamento dos Modelos (FastAI + PyTorch)

Foram adotados dois estágios:

## **2.1 Transfer Learning**

- Carregamento de arquiteturas pré-treinadas (ex.: `resnet34`, `resnet50`)
- Congelamento das camadas iniciais
- Ajuste apenas dos últimos blocos
- Uso de `lr_find` para encontrar a melhor taxa de aprendizado
- Treinamento com early stopping

## **2.2 Fine-Tuning**

- Descongelamento progressivo das camadas
- Learning rate discriminativo (LR menor nas camadas iniciais)
- Aumento de épocas e maior especialização

## 🔗 Checkpoints dos modelos (Drive público)

Os arquivos `.pth` excedem 100 MB e foram hospedados em um Google Drive público:

**LINK:**  
[https://drive.google.com/drive/folders/1JpPIElGphQHyBo30dl9k5AsdjvnsfX2H]

Cada notebook aponta para o checkpoint correspondente.

---

# 🧮 3. Métricas e Resultados

Cada notebook gera um arquivo CSV com:

- Epoch  
- Train Loss  
- Validation Loss  
- Accuracy  
- AUC (quando aplicável)  
- Modelo  
- Fase (transfer / finetune)

Os arquivos CSV estão dentro das pastas de cada dataset.

Também foram gerados gráficos de:

- Curva de perda
- Acurácia
- Overfitting/underfitting por modelo
- Comparação entre dataset original e lung-only

---

# 📊 4. Comparação das Abordagens

| Abordagem | Vantagens | Desvantagens |
|----------|-----------|--------------|
| **Imagem Original** | Mantém contexto global, útil para padrões periféricos | Pode incluir ruído anatômico irrelevante |
| **Lung-only** | Foca exclusivamente no parênquima pulmonar; reduz variabilidade | Segmentação pode introduzir artefatos; perda de contexto |

Os experimentos permitem avaliar se remover regiões externas aumenta a acurácia ou se reduz a capacidade de generalização.

---

# 📓 Notebooks Incluídos

## 🔹 `lung_create.ipynb`

Pipeline completo para gerar o dataset segmentado.

## 🔹 `CNN_models-apres-lung-only.ipynb`

Treinamento e fine-tuning usando **apenas pulmão**.

## 🔹 `CNN_models-apres-reduzido.ipynb`

Treinamento e fine-tuning com as **imagens originais**.

---

# 🌐 Dependências Principais

- Python 3.10+
- FastAI
- PyTorch
- TorchXRayVision
- Albumentations
- Scikit-learn
- Pandas
- OpenCV

---

# 🔰 Prompt utilizado no projeto

> Este projeto foi conduzido com apoio de um assistente especializado em visão computacional.  
> O prompt inicial utilizado para orientar o suporte técnico está descrito abaixo:

## Prompt do Assistente Técnico Utilizado no Projeto

```
Você é um especialista sênior em Visão Computacional, com domínio profundo em:
- FastAI e PyTorch  
- Classificação e segmentação de imagens médicas  
- Transfer Learning e Fine-Tuning  
- Estruturação de datasets  
- Depuração de código e otimização de pipelines de CNN  

Sua única função é:
- Explicar conceitos ao usuário  
- Ajudar a diagnosticar erros  
- Propor soluções robustas e escaláveis  
- Sugerir melhorias de código  
- Apoiar nas decisões de modelagem  

Responda sempre de forma objetiva, clara e fundamentada.
```

---

# 📬 Contato
Bruna Pupo
[🔗 *GitHub*](https://github.com/Brunapupo)

Henrique Rezer  
[🔗 *GitHub*](https://github.com/Henriquerezer)

Mauricius Correa
[🔗 *GitHub*](https://github.com/MauruCorrea)
