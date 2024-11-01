# Fastai-with-Transformers-for-Sentiment-Analysis

Fastai with HuggingFace 🤗Transformers (BERT, RoBERTa, XLNet, XLM, DistilBERT)

![fastai-transformers-1](https://github.com/user-attachments/assets/d6d93e19-f4b9-4dc4-8767-8f07cb2b009c)

N.B. This implementation is a supplement of the Medium article "Fastai with 🤗Transformers (BERT, RoBERTa, XLNet, XLM, DistilBERT)".

Also, remember the upvote button is next to the fork button, and it's free too! 😉

Introduction : Story of transfer learning in NLP
In early 2018, Jeremy Howard (co-founder of fast.ai) and Sebastian Ruder introduced the Universal Language Model Fine-tuning for Text Classification (ULMFiT) method. ULMFiT was the first Transfer Learning method applied to NLP. As a result, besides significantly outperforming many state-of-the-art tasks, it allowed, with only 100 labeled examples, to match performances equivalent to models trained on 100× more data.

The first time I heard about ULMFiT was during a fast.ai course given by Jeremy Howard. He demonstrated how it was easy  -  thanks to the fastai library  -  to implement the complete ULMFit method with only a few lines of codes. In his demo, he used an AWD-LSTM neural network pre-trained on Wikitext-103 and get rapidly state-of-the-art results. He also explained key techniques - also demonstrated in ULMFiT - to fine-tune the models like Discriminate Learning Rate, Gradual Unfreezing or Slanted Triangular Learning Rates.

Since the introduction of ULMFiT, Transfer Learning became very popular in NLP and yet Google (BERT, Transformer-XL, XLNet), Facebook (RoBERTa, XLM) or even OpenAI (GPT, GPT-2) begin to pre-train their own model on very large corpora. This time, instead of using the AWD-LSTM neural network, they all used a more powerful architecture based on the Transformer (cf. Attention is all you need).

Although these models are powerful, fastai do not integrate all of them. Fortunately, HuggingFace 🤗 created the well know transformers library. Formerly knew as pytorch-transformers or pytorch-pretrained-bert, this library brings together over 40 state-of-the-art pre-trained NLP models (BERT, GPT-2, RoBERTa, CTRL…). The implementation gives interesting additional utilities like tokenizer, optimizer or scheduler.

The transformers library can be self-sufficient but incorporating it within the fastai library provides simpler implementation compatible with powerful fastai tools like Discriminate Learning Rate, Gradual Unfreezing or Slanted Triangular Learning Rates. The point here is to allow anyone — expert or non-expert — to get easily state-of-the-art results and to “make NLP uncool again”.

It worth noting that the integration of the HuggingFace transformers library in fastai has already been demonstrated in:

Keita Kurita's article A Tutorial to Fine-Tuning BERT with Fast AI which makes pytorch_pretrained_bert library compatible with fastai.
Dev Sharma's article Using RoBERTa with Fastai for NLP which makes pytorch_transformers library compatible with fastai.
Although these articles are of high quality, some part of their demonstration is not anymore compatible with the last version of transformers.


🛠 Integrating transformers with fastai for multiclass classification

Before beginning the implementation, note that integrating transformers within fastai can be done in multiple different ways. For that reason, I decided to bring simple solutions, that are the most generic and flexible. More precisely, I try to make the minimum of modification in both libraries while making them compatible with the maximum amount of transformer architectures.

🎬 The example task
The chosen task is a multi-class text classification on Movie Reviews.

For each text movie review, the model has to predict a label for the sentiment. We evaluate the outputs of the model on classification accuracy. The sentiment labels are:

0 → Negative
1 → Somewhat negative
2 → Neutral
3 → Somewhat positive
4 → Positive


Main transformers classes:
In transformers, each model architecture is associated with 3 main types of classes:

A model class to load/store a particular pre-train model.
A tokenizer class to pre-process the data and make it compatible with a particular model.
A configuration class to load/store the configuration of a particular model.
For example, if you want to use the Bert architecture for text classification, you would use BertForSequenceClassification for the model class, BertTokenizer for the tokenizer class and BertConfig for the configuration class. 

In order to switch easily between classes  -  each related to a specific model type  -  I created a dictionary that allows loading the correct classes by just specifying the correct model type name.

