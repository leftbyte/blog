---
title: "Using Langchain, Pinecone, and Arize for Troubleshooting Search and Retrieval"
date: 2023-07-19
---

I recently watched
[this video](https://www.youtube.com/watch?v=eDW1EsXMNY4) on
troubleshooting search and retrieval that was ostensibly a tech
marketing workshop from Arize on their
[Phoenix](https://github.com/Arize-ai/phoenix) observability tool. The
video and sample code was a good introduction to
[Langchain](https://python.langchain.com/docs/get_started/introduction.html),
[Pinecone](https://www.pinecone.io/) and one approach on how to use an
LLM on a custom set of documents. Langchain is a fantastic library
that provides the many components and tools needed to build an
application powered by language models. Pinecone is a cloud-based
vector database.

The workshop go through ingesting a custom document store and using
the generated embeddings to augment a prompt to ChatGPT. The materials
also show how to interpose on the interfaces to collect some useful
trace data to feed to Phoenix, and then inspect that data to
understand the performance of the search and retrieval application.

# Overview

Phoenix provides a locally hosted app that displays a reduced
dimension visual of the embedding space of the prompt (input question)
and supporting documents. E.g. you can see a point cloud of the prompt
along with the supporting documents. Using the UI, you can select from
different metrics and views to drill down to why certain responses are
incorrect.

1. Bad responses (hallucinations): select 'Color By': dimension,
   user_feedback = dimension, and sort by lowest metric. When you look
   at the cluster with the lowest metric, you see a cluster with all
   the feedback being thumbs down (this is feedback from real
   users). Looking into this cluster's data, you see that all of the
   questions are related to cost, which doesn't have any good
   supported document responses. I.e. there is no documentation with
   any pricing information in the retrieved pieces of context. The
   problem with this information is that there is no ground truth and
   you're just going by user feedback.

2. KB coverage - Query Density: select "Euclidian Distance" as the
   metric. You can see from this view that there are some questions
   that are clustered and far away from the context (documents). This
   way of looking at the clustering is agnostic of external feedback
   and lets you get a sense of the coverage of the support
   context. The problem with this view is that it won't help you
   pinpoint any fine grained problems and is a pretty broad view of
   your data.

3. View Similar documents using cosine similarity - View the documents
   that have a high cosine similarity to the prompt. Select
   context_similarity_0 and view those how the cosine similarity score
   related to the prompt. This method lets you drill down to the
   verbatim documents that are being used to respond to the
   prompt. You're also able to see how this metric can provide
   incorrect results because there are many documents that have very
   similar words and phrases but are simply irrelevant. This view
   shows how similarity != relevance.

4. View Relevance - The workshop notebook shows how to take another
   step in the system and use the LLM to rate the relevance of the
   documents to the prompt. This seems to be another way to get
   "external" feedback when human feedback is difficult to get. This
   shows that gpt-3.5 is pretty good at labelling relevance, but is
   not 100% accurate and that we still need some ground truth.

# Learnings

Overall I enjoyed this overview of how to write a chatbot that can
search a custom set of documents. This example also shows the pitfalls
of language models and how these systems don't have an easy way to
provide a ground truth. If the application can tolerate some
inaccuracy then this method can be applied. However, if the results
aren't 100% accurate then the loss of user confidence and tarnishing
of the brand far outweighs the cost of building a robust search and
retrieval system for a knowledge base.

In the Q&A section, they referenced Travis Fisher's tweet on how to
improve your application. I looked that up and here's his advice on
how to use LLMs effectively.

Start simple - if results are lacking, trying breaking up into
subproblems or gradually moving up the ladder of complexity.
  
1. Prompting (i.e. prompt engineering)
1. Few-shot prompting i.e. prompt engineering)
1. Retrieval + prompting (using tools like langchain, llamaindex, etc.)
1. Iterative refinement (using tools like langchain, llamaindex, etc.)
1. Fine-tuning a hosted model
1. Fine-tuning an OSS model
1. Training an OSS model from scratch
1. Building a custom model from scratch
