---
layout: post
title: Bounded Outcomes for Simplified Modeling
categories: [machine-learning, GenAI]
excerpt: Deep learning models have proven to be highly adaptable and applicable in almost every industry. As widespread as the models have become, deploying and scaling their performance in production comes with recurring challenges. One challenge area is classification tasks with a high number of possible classes.
---

![](https://www.dropbox.com/scl/fi/itmqzcys9v995bhmzoxwr/bound-outcomes-hero-copy.png?rlkey=gmgmv7faupqhz91r0ajgby4tf&st=c907jetk&dl=0 "Image created by ChatGPT using OpenAI’s DALL·E image generation tool.")

Deep learning models have proven to be highly adaptable and applicable in almost every industry. As widespread as the models have become, deploying and scaling their performance in production comes with recurring challenges. One challenge area is classification tasks with a high number of possible classes. 

As categories are added, the network needs to capture increasingly granular subtleties to make a determination between one category and another. To maintain performance, the model architecture often grows in size, training time becomes much longer, the cost of compute rises with server requirements, and the data preparation scales alongside the required data. In short, a high number of categories results in the entire process becoming substantially more expensive to bootstrap and maintain. 

Due to the potential operational complexity of generalizing models, utilizing simpler models with a constrained set of possible outcomes has enabled product launches.

### Vector-Search Approach

Generalizing a model is not always an option. An alternative is to create easy to compare representations of classification targets and doing lots of the comparisons to arrive at an answer. Some examples of classification targets are an image or a chunk of text to be tagged with a category.

A common pattern is to leverage easy to compare embeddings and context information to reduce model complexity requirements. The two elements can be described as follows: 
* **Embeddings**: Embeddings are vector representations of an object, which could be text, images, or the relationships of an entity in a graph. The vector representations are the output of a trained neural network. Simply, our object is put through an existing model and we get a short representation of the object. The representation can be compared to other representations to determine similarity.
* **Context Information**: Context information is gathered at the time of classification and can be used to bound the possible outcomes. Bounding possible outcomes greatly simplifies the problem, removing the need for the model complexity. 

#### Pattern Summary:

1. Generate embeddings using pre-trained or fine-tuned simple model. 
2. Use event context to determine a subset of possible categories.
3. Compare a target embedding (classification target) to reference embeddings (previously calculated) to get a relevance score.
4. The most relevant is the classification.

### Example Cases

#### Image Classifications in a Department Store:

* **Challenge**: The number of products makes generalizing a model very difficult.
* **Bounded Outcomes**: Ancillary services for tracking a customers behavior could provide a set of products the person has interacted with.
* **Pattern Application**: Embed the image from the event, use the bounded outcome context for a confusion set, compare embeddings to get ranked confidences of the target product

#### Similar Product Recommendations

* **Challenge**: Possible product recommendations extends to the entirety of the catalog. Maintaining a lookup of pre-computed product to all possible products cannot be held in memory. 
* **Bounded Outcomes:** Use customer behavior to have a collection of high interest products where the number of products per customer bounded to a pre-determined value.
* **Pattern Application:** When a customer views an product, use the products embedding to query nearby embeddings within the customers pool. 

#### Retrieval Augmented Generation (RAG) Systems

* **Challenge:** Training an LLM from scratch is incredibly expensive and fine-tuning is also an asynchronous process. The information encoded into the LLM is stale and may have hallucinations. RAG setups have been shown to be able to provide fresh context and lower hallucinations. The amount of documents provided to a RAG system could be very large and the amount of context provided to an LLM prompt is limited.
* **Bounded Outcomes:** A set of fresh documents is chunked, embedded, and stored in a vector database. The query provided by the user is embedded and used to query against the database. The top similar chunks and surrounding context are the bounded outcomes.
* **Pattern Application:** Using the bounded chunks, they are converted into a context to provide the LLM prompt. 
