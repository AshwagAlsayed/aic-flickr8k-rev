# aic-flickr8k-rev
## Introduction
Image captioning is becoming increasingly important as visual media is created at an unprecedented rate on the web and social media platforms. Research on image captioning targeting English captions has received significant attention in the past decade (Kiros et al., 2014; Xu et al., 2015; Sharma et al., 2018; Cornia et al., 2020; Dubey et al., 2023). Unfortunately, image captioning for the Arabic language did not receive similar attention, and few existing works focused on Arabic image captioning (AIC). To solve the AIC problem, existing models developed for English can be used to generate captions in English, then use a text-to-text language translation model to get the captions in Arabic. However, there are many drawbacks to this approach. First, captioning inaccuracies may result from different sources, namely the captioning and translation models. In addition, using the model can be slower because it performs two steps to get the Arabic text from the image. Furthermore, generating captions in Arabic faces many challenges. The Arabic language has a vast morphology (22,400 tags compared to 48 tags in English) (Habash, 2010). Moreover, Arabic has a fairly complex inflectional system. For example, Arabic verbs have over 5,400 different inflected forms. Thus, generating meaningful Arabic captions that are grammatically correct is challenging. In addition to the challenges that stem from the inherent properties of the Arabic language, we identify three factors that impact machine learning models used for AIC. These factors are (1) the labels’ textual preprocessing techniques, (2) the datasets used for building AIC models, and (3) the techniques used to extract the features from the input images to generate the output captions.

## A Transformer-based Architecture for AIC
[![The encoder and decoder in the transformer model architecture are shown on the left and right. Each encoder contains two sublayers, feed forward neural network and self-attention. Therefore, the feed-forward layer is applied after the self-attention layer. The decoder includes the same layers as the encoder and a third layer that sits between the two that were just mentioned; this layer is known as the encoder—decoder attention to help the decoder to focus on relevant parts of the input sentence.](https://i.postimg.cc/htcynQyV/Transformer-Architecture2-1.png)](https://postimg.cc/jL3QQ2rj)

Proposed by Vaswani et al. (2017), the transformer model is one of the deep-learning models that can be used for machine translation. It has been used in a wide range of NLP applications Wang et al. (2019); Raganato and Tiedemann (2018). In addition to NLP applications, the transformer model has also been used for image captioning Sharma et al. (2018). We propose a variant based on Sharma et al. (2018).
Our proposed transformer-based architecture is shown in Fig. 1. The input image is translated into a vector with feature embedding using a CNN-based model. Then, the transformer encoder takes in the feature embedding of the image extracted by CNN, $x = (x_1,..., x_n)$. Thus, the image's feature vector $x$ is computed based on the equation.
\
   $x$ = $CNN(i)$
\
$CNN(i)$ in the equation represents an abstraction over any CNN-based model that is used for image feature extraction. 
After computing $x$, $x$ is used as input to the encoder block, which computes $z$ according to the equation.
$z$ = $Encoder(x)$
\
$Encoder(x)$ in the equation represents an abstraction of the set of encoder blocks. The encoder block generally creates an attention-based representation that can find a particular piece of data within a context that could be ‘‘infinitely” huge. The encoder is composed of one or more identical layers in a stack. Each layer has a simple position-wise, completely connected feed-forward network, and a multi-head self-attention layer. Each sub-layer adopts layer normalization and a residual connection. The same dimension of data is output by each sublayer. In our architecture, the encoder block with multi-head attention converts x to the attended visual representation vector followed by feed-forward, which is applied to every attention vector to transform it into a format that the following (encoder or decoder) layer can understand $z = (z_1, ..., z_t)$. $z$ is the output vector of the encoder block and is fed to the decoder block
$Decoder(z,s)$ in Equation \ref{eq:sdec} represents an abstraction of the set of encoder blocks. In our architecture, the decoder block uses $z$ along with the word embedding vector, $s$, of the input image's captions to generate the output sequence $y = (y_1,..., y_m)$. Word embedding is improved by positional encodings to take advantage of order information. $y$ is the output sequence of words of the decoder block according to the equation.

\
In general, the decoder is composed of one or more identical layers in a stack. Each layer has one sub-layer of a fully connected feed-forward network and two sub-layers of multi-head attention mechanisms. Each sub-layer adopts a residual connection and a layer normalization, just like the encoder. The initial multi-head attention sub-layer is adjusted to avoid positions from paying attention to subsequent positions so that the future of the target sequence is hidden when predicting the current position.
\
The word embedding goes through a masked multi-head attention mechanism which is different from a typical multi-head attention mechanism. The decoder block generates the sequence word by word, so it needs to prevent it from conditioning to future tokens. A second multi-headed attention layer takes the outputs of the encoder and the masked multi-headed attention layer to match the encoder's input to the decoder's input, enabling the decoder to select the encoder input that should receive the most attention. Then the output passes through a point-wise feed-forward layer.

## Text Preprocessing for AIC
In this section, we shed light on an important task which is preparing and preprocessing the dataset for model training and testing. While being a crucial element of any AIC machine-learning pipeline, details of the preprocessing step are often not discussed in the literature. In this paper, we study various techniques for text prepossessing for AIC.   

Most of the existing work in AIC preprocesses the Arabic text using standard preprocessing similar to English text. We refer to this as \emph{general preprocessing}. They segment the labels using a segmentation tool used for the English language. This approach may result in semantic errors in training labels. For example, the definite article "ال" (equivalent to \textbf{the} in English) is frequently prefixed to other words. It is not an integral element of that word. For instance, both "حاسوب" and "الحاسوب" (with the "ال" prefix) must be included in the vocabulary, resulting in a substantial quantity of redundancy. To solve this problem, we segment the Arabic labels into their component prefixes, stems, and suffixes, along with other preprocessing steps. In this paper, we develop a unified framework for preprocessing labels in AIC. Then, we use our framework to investigate the effect of using different approaches (i.e., CAMeL Tools Obeid et al., 2020, ArabertPreprocessor Antoun et al., 2020, and Stanza Qi et al., 2020) for preprocessing Arabic text labels on the performance of AIC.

In general, image captioning models take text labels along with images as input to train the model, and this text must be preprocessed and cleaned to reduce the text's noise and vocabulary size. We propose a framework that abstracts the preprocessing steps into three stages. These stages are general preprocessing (GPP), language-specific preprocessing (LSPP), and task-specific preprocessing (TSPP). It is illustrated in Figure \ref{Preprocessing_pipeline} and explained in detail below.

[![Preprocessing pipeline describes the steps involved in preprocessing where GPP is applied, which is the critical step for any language. After that, the processing steps for the Arabic language, which is LSPP, have been applied, and finally, TSPP makes the text fit to the image captioning model.](https://i.postimg.cc/3wpdZWhb/preprocessing-pipeline111-2.png)](https://postimg.cc/nsFFFFCv)

### General Preprocessing
General text preprocessing is a standard step used for text preprocessing in NLP and AI applications. It is suitable for most languages, like lower-casing all the words, removing punctuation, removing URLs, removing stop words, and removing numeric. Some general preprocessing steps are applied, such as eliminating punctuation and numeric, as shown in the first part (General Preprocessing) of Fig. 2.
### Language-specific Preprocessing
Language-specific preprocessing comprises steps that are specific to a language. In our case, the language is Arabic, as shown in the second part of Fig. 2. However, our framework can be used for other languages as well. The morphology of Arabic is especially significant because the language is highly ordered and derivational. Arabic is written from right to left. The Arabic alphabet has 28 letters in total, including Hamza (ء(, and Arabic letters have various shapes depending on where they are in a word (beginning, middle, or end). For instance, the letters ‘‘ع “and ‘‘ك“, as shown in Table 1, have different shapes depending on their position in the word. The complicated concatenative structure of Arabic is responsible for the Arabic language’s lexical sparsity Al-Sallab et al. (2017). Words can take on various shapes while yet having the same meaning. For instance, although the definite article ‘‘ال“, which corresponds to ‘‘the” in English, is usually prefixed to other words, it is not an essential component of those words. So Arabic NLP applications must handle the Arabic language’s complexity. The following are some of the steps that have been taken to preprocess the Arabic text and reduce its complexity.
\
_Tatweel Removal._ Some Arabic characters are stretched using the symbol known as the tatweel. In informal Arabic, the tatweel symbol is frequently used to express a sentiment or meaning. The tatweel must be eliminated because it generates redundant spellings of the same word [35].
\
*Unicode Normalization.* Unicode normalization transforms Unicode strings into their conventional form by normalizing them (characters that can be written as a combination of Unicode characters are converted into their decomposed form) [18].
\
*Orthographic Normalization.* Orthographic normalization is the process of unifying different letter forms or visually comparable letters. For example, all “ء” characters are removed, the letter "ة" is replaced by “ه” and the letter “ى” is replaced by “ي”. Table 2 summarizes the letter normalization scheme.
\
*Dediacritization.* The elimination of Arabic diacritical marks is known as dediacritization. Most Arabic NLP algorithms eliminate them because they make the data sparser [18]. For instance, the word مَكْتَبَةٌ is converted to مكتبة.
\
*Morphological Tokenization.* Standard word tokenization is the process of splitting sentences by whitespace. Morphological tokenization is a different kind of tokenization that separates Arabic words into their components, such as prefixes, stems, and suffixes. For example, the word "المكتبة" is rewritten as three tokens "ال", "مكتب", and "ة". The added "+" symbol denotes where the segment was cut off, allowing us to de-segment the text afterward.

### Task-specific Preprocessing
In the task-specific preprocessing stage, the steps that make the text suitable for the image captioning model are applied, as shown in the third part of Figure 2.
*Markup Tokens.* This step adds < start > and < end > tokens to each caption so that the model knows where the caption begins and ends.
\
*Vectorization.* The TextVectorization step associates a unique integer value with each token. It also unifies the caption vectors’ length to a specific length, cutting off more extended captions, padding the shorter ones with zeros, and then transforming them into a vector of integers.
\
