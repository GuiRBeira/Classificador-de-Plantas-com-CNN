# Detecção de Doenças em Plantas com Transfer Learning (ResNet50) 🌱

> ⚠️ **Nota de Arquivo:** Este projeto foi desenvolvido originalmente durante o 6º semestre da Engenharia da Computação. Este repositório serve como registro histórico da minha evolução em Deep Learning e Computer Vision.

## 🎯 O Objetivo
Desenvolver um classificador de imagens capaz de identificar **38 classes** de doenças em plantas (dataset PlantVillage), utilizando técnicas de Aprendizado Profundo e Transferência de Aprendizado.

## 🛠️ Tech Stack
* **Framework:** TensorFlow / Keras
* **Arquitetura:** ResNet50 (pré-treinada na ImageNet)
* **Processamento:** ImageDataGenerator (Data Augmentation)
* **Otimização:** Adam Optimizer, Callbacks (EarlyStopping, ReduceLROnPlateau, ModelCheckpoint)
* **Análise:** Scikit-learn (Confusion Matrix, Classification Report), Matplotlib

## 🏗️ Arquitetura do Modelo
Implementei uma abordagem de **Transfer Learning**:
1.  **Base:** ResNet50 (camadas convolucionais congeladas) para extração de features genéricas.
2.  **Top Head (Customizada):**
    * `GlobalAveragePooling2D` para reduzir a dimensionalidade.
    * `Dense` (256 neurônios, ativação ReLU).
    * `Dropout` (0.5) para combater overfitting. (OBS: Não combateu kkkkkkkkkk)
    * `Dense` (38 neurônios, Softmax) para classificação final.

## 📉 Análise "Post-Mortem": Por que a acurácia estagnou em 30%?
Revisitando o projeto com minha experiência atual, identifiquei que o gargalo não estava na arquitetura da rede, mas na **Qualidade dos Dados**:

1.  **Desbalanceamento Severo:** O relatório de classificação revelou uma disparidade crítica. Classes como `Orange___Haunglongbing` possuíam **1100+ amostras**, enquanto `Potato___healthy` tinha apenas **30 amostras**. O modelo enviesou para as classes majoritárias.
2.  **Learning Rate Inicial:** A taxa de aprendizado de `0.005` pode ter sido agressiva demais para o fine-tuning, dificultando a convergência nos mínimos locais.
3.  **Complexidade x Dados:** A ResNet50 é uma rede profunda que exige um volume de dados mais balanceado para generalizar bem entre 38 classes distintas.

## 🚀 Roadmap: Como eu resolveria hoje (V2.0)
Para levar este projeto ao estado da arte (SOTA) hoje, eu aplicaria:

* **Data Engineering:** Tratamento rigoroso do desbalanceamento usando técnicas de *Oversampling* (SMOTE) ou *Class Weights* na função de perda (Loss).
* **Fine-Tuning Progressivo:** Descongelaria gradualmente os blocos finais da ResNet50 com um learning rate muito menor (`1e-5`) para adaptar os pesos às características específicas das folhas.
* **Arquiteturas Modernas:** Testaria *EfficientNetB0* ou *MobileNetV2*, que costumam ter melhor performance/custo para esse tipo de feature extraction.

---
*Desenvolvido por [Guilherme Ricardo Beira](https://www.linkedin.com/in/guilherme-r-beira)*
