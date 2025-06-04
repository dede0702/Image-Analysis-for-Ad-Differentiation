# Challenge HP – Sprint 2: Comparação Visual de Anúncios com Modelos de Similaridade

## Integrantes
- Alan Maximiano (RM557088)
- André Rovai (RM555848)
- Antônio Vinicius (RM558014)
- Leonardo Zago (RM558691)
- Renan de França (RM558413)
- Thiago Almança (RM558108)

## 1. Introdução e Contexto
Esta documentação refere-se à Sprint 2 do Challenge HP. Dando continuidade à Sprint 1, onde foram elaboradas e testadas hipóteses iniciais para identificar diferenças visuais em anúncios, esta sprint foca na aplicação prática dessas ideias. O objetivo principal é desenvolver um sistema capaz de comparar visualmente diferentes anúncios a partir de suas imagens, utilizando representações visuais (embeddings) para medir similaridade, detectar variações importantes e agrupar anúncios visualmente parecidos.

Este sistema visa fornecer uma base para análises mais aprofundadas sobre a consistência visual de campanhas, identificação de anúncios genéricos, detecção de cópias ou imagens de baixa qualidade, e organização de acervos visuais.

## 2. Objetivos da Sprint
Os objetivos específicos desta sprint foram:
1.  **Construir embeddings visuais:** Escolher e aplicar um modelo de extração de embeddings (como ResNet50, EfficientNetB0, ou MobileNetV2) para gerar vetores de representação para cada imagem de anúncio.
2.  **Medir a similaridade entre anúncios:** Calcular distâncias (similaridade de cosseno) entre os vetores de embedding e criar uma matriz de similaridade para visualizar pares mais semelhantes e diferentes.
3.  **Clusterização e agrupamento (opcional):** Aplicar algoritmos como KMeans para agrupar visualmente os anúncios com base nos seus embeddings.
4.  **Análise e visualização dos resultados:** Identificar padrões como grupos de imagens genéricas, duplicatas, e variações em anúncios semelhantes, utilizando t-SNE para projeção 2D dos embeddings.
5.  **Recomendações para etapas seguintes:** Sugerir como os resultados podem ser utilizados para identificar imagens genéricas e avaliar qualidade visual em futuras fases do Challenge HP.

## 3. Estratégia e Metodologia

### 3.1. Preparação do Dataset
*   **Fonte:** Imagens de anúncios foram coletadas e organizadas em uma pasta específica (`ads_for_similarity`) no Google Drive.
*   **Pré-processamento:**
    *   As imagens foram copiadas do Google Drive para o ambiente local do Colab.
    *   Utilizou-se a biblioteca Pillow para validar cada imagem, converter para o modo RGB e salvar no formato JPEG. Este passo garante a consistência e compatibilidade com os modelos TensorFlow, prevenindo erros de formato de arquivo.
    *   Foram aceitos formatos de entrada como `.jpeg`, `.jpg`, `.png`, `.bmp`.

### 3.2. Construção de Embeddings Visuais
*   **Modelo Escolhido:** Foi implementada a flexibilidade para escolher entre `ResNet50`, `EfficientNetB0`, e `MobileNetV2` (com `ResNet50` como padrão). Estes modelos, pré-treinados na ImageNet, são eficazes na extração de features visuais ricas. A camada de classificação final foi removida (`include_top=False`) e pooling médio global (`pooling='avg'`) foi aplicado para obter um vetor de embedding para cada imagem.
*   **Processo:** Cada imagem pré-processada foi redimensionada para 224x224 pixels, passada pelo modelo de embedding escolhido, e o vetor de features resultante foi armazenado.

### 3.3. Medição de Similaridade
*   **Métrica:** A similaridade de cosseno foi calculada entre todos os pares de vetores de embedding. Esta métrica é adequada para medir a orientação (e, portanto, a semelhança de features) entre vetores em espaços de alta dimensão.
*   **Visualização:** Uma matriz de similaridade foi gerada usando `seaborn.heatmap` para uma visualização clara das relações de semelhança entre todos os anúncios.

### 3.4. Análise e Visualização dos Resultados
*   **Pares Semelhantes/Diferentes:** Foram identificados e exibidos os top N pares de anúncios mais semelhantes e mais diferentes com base nos scores de similaridade de cosseno.
*   **Detecção de Duplicatas:** Um threshold de similaridade (e.g., >0.98) foi usado para sinalizar potenciais duplicatas ou imagens quase idênticas.
*   **Redução de Dimensionalidade e Visualização 2D:** A técnica t-SNE foi aplicada para projetar os embeddings de alta dimensão em um espaço 2D, permitindo a visualização de clusters e relações de proximidade entre os anúncios.
*   **Clusterização (Opcional):** O algoritmo KMeans foi implementado para agrupar os anúncios em um número pré-definido de clusters com base em seus embeddings, e os resultados foram visualizados no gráfico t-SNE.

### 3.5. Ferramentas Utilizadas
*   Python 3.x, Google Colab
*   TensorFlow e Keras (para modelos de embedding)
*   Pillow (PIL) (para manipulação de imagens)
*   Scikit-learn (para similaridade de cosseno, t-SNE, KMeans)
*   Matplotlib e Seaborn (para visualizações estáticas)
*   (Opcional) Plotly (para visualizações interativas)

## 4. Resultados e Discussão (Resumo Esperado)
*(Esta seção deve ser preenchida com os resultados obtidos após a execução do notebook com o dataset específico.)*

*   **Geração de Embeddings:** Descrever se a extração de embeddings foi bem-sucedida para o conjunto de anúncios e o formato dos embeddings gerados.
*   **Matriz de Similaridade:** Analisar a matriz, destacando quaisquer blocos de alta similaridade ou anúncios que se mostraram muito distintos.
*   **Visualização t-SNE:** Comentar sobre a distribuição dos anúncios no espaço 2D. Agrupamentos visuais fazem sentido? Existem outliers claros?
*   **Pares Semelhantes/Diferentes:** Apresentar exemplos visuais dos pares mais semelhantes e diferentes identificados, discutindo se a percepção do modelo condiz com a percepção humana.
*   **Detecção de Duplicatas:** Listar quaisquer duplicatas ou quase-duplicatas encontradas.
*   **Clusterização (KMeans):** Se realizado, descrever as características dos clusters formados e se eles representam categorias visuais significativas.

## 5. Recomendações para Etapas Seguintes

Com base nos resultados desta sprint, as seguintes recomendações são propostas:

*   **Identificar Imagens Genéricas:**
    *   Anúncios que não se agrupam fortemente ou que apresentam similaridade moderada com diversos grupos podem ser investigados como potencialmente genéricos.
    *   Considerar o treinamento de um classificador específico ("genérico" vs. "de marca") ou usar técnicas de detecção de anomalias, tendo os anúncios padrão da HP como baseline.
*   **Avaliar Qualidade Visual:**
    *   Os embeddings atuais capturam features de alto nível, mas não medem diretamente a qualidade técnica (pixelização, artefatos de compressão).
    *   Explorar a integração com modelos específicos de avaliação de qualidade de imagem (e.g., BRISQUE, NIMA) ou treinar um modelo para prever escores de qualidade. Anúncios de baixa qualidade podem formar clusters ou aparecer como outliers se as distorções forem significativas.
*   **Aplicações Adicionais para a HP:**
    *   **Detecção de Duplicatas:** O sistema já fornece uma base sólida para isso.
    *   **Análise de Consistência de Campanhas:** Utilizar o agrupamento para verificar a coesão visual dentro de campanhas da HP.
    *   **Benchmarking Competitivo:** Mapear o "território visual" da HP em relação aos concorrentes.
    *   **Busca por Similaridade Visual (Visual Search):** Desenvolver uma ferramenta para encontrar anúncios visualmente semelhantes a uma imagem de referência.
    *   **Exploração de Modelos Avançados:** Investigar modelos como CLIP, que combinam compreensão visual e textual, para análises mais ricas, especialmente quando o texto no anúncio é crucial.

## 6. Conclusão da Sprint 2
A Sprint 2 demonstrou com sucesso a construção de um sistema funcional para comparação visual de anúncios utilizando embeddings de imagem. Foram gerados embeddings, calculadas e visualizadas similaridades, e exploradas técnicas de redução de dimensionalidade e clustering.

Esta abordagem provou ser valiosa para:
*   Quantificar a semelhança visual entre anúncios.
*   Identificar potenciais duplicatas.
*   Visualizar a estrutura e relações no conjunto de dados de anúncios.
*   Formar uma base para análises mais complexas, como a identificação de imagens genéricas e a avaliação de consistência visual.

As próximas etapas devem focar em refinar essas técnicas, expandir o dataset, e explorar modelos mais avançados para extrair insights ainda mais profundos relevantes para o Challenge HP.

## 7. Código Fonte
O código desenvolvido para esta sprint, incluindo o pré-processamento de dados, geração de embeddings, cálculo de similaridade, visualizações (matriz de similaridade, t-SNE) e clusterização opcional, está disponível no seguinte notebook Google Colab:

[Image_Analysis_for_Ad_Differentiation_PT2.ipynb](Image_Analysis_for_Ad_Differentiation_PT2.ipynb)

*(Nota: Substitua pelo link real e compartilhável do seu notebook no Google Colab se estiver hospedado online. Se estiver no mesmo repositório, o link relativo pode funcionar.)*

**Para executar o notebook:**
1.  Abra o link no Google Colab.
2.  Certifique-se de que seu Google Drive esteja montado e que o caminho `original_dataset_root_path_drive` no código aponte para a pasta raiz correta no seu Drive.
3.  Dentro dessa pasta raiz, deve haver uma subpasta chamada `ads_for_similarity` (ou o nome definido em `subfolder_for_ads`) contendo as imagens de anúncios a serem analisadas.
4.  Execute as células sequencialmente. O notebook irá processar as imagens, gerar embeddings, calcular similaridades e produzir as visualizações.
