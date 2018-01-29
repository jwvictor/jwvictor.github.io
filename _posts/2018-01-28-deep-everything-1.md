---
layout: category-post
title:  "Deep Everything (pt. I)"
date:   2016-08-05 20:20:56 -0400
categories: writing
---

Back in the old days of machine learning -- roughly 2006 -- there wasn't much agreement as to which models would ultimately lead us to the AI promised land. There were a variety of competing theories, deeply rooted in the philosophy of machine learning, and academics were fiercely protective of their viewpoint.

The first question was whether focus should be placed on _generative_ or simply _discriminative_ models. A _generative_ model is, roughly, one in which "the full story of the data" is explained by the model -- that is, the model is based on a story of the data was _generated_. A discriminative model, on the other hand, simply looked at every problem like a black box: the question was just how to look at the data and make predictions, with no regard for how the data came to be, or, even less, what kind of data it is.

Deep methods represent the discriminative approach taken to the extreme -- even feature extraction is, in some sense, a black box.

Andrew Ng, one of the veritable leaders of the deep learning community, seemed to fall on the side of _generative_ models. One of his early successes was Latent Dirichlet Allocation, which proposed a generative model for text documents, and discovered latent "topics" therein. The results of LDA are easily competitive with modern neural methods, although decidedly less popular.

While such early generative models, championed largely by the Bayesians, were some of the first to show real promise on difficult tasks, they were soon superseded by deep nets. Research interest shifted quickly, and now we live in a world where nearly every paper you read has some deep net connection. Andrew Ng and others like him went from being the face of the Bayesians to the face of deep learning. 

But for all the research into applications and optimization heuristics, deep nets remain shrouded in mystery. From a philosophical standpoint, a mathematical standpoint, and a technical standpoint, much remains to be understood about deep nets and their [unreasonable effectiveness](http://karpathy.github.io/2015/05/21/rnn-effectiveness/).

The AI practitioner will invariably look with excitement to the neverending parade of Keras diddies that seem to perform human tasks with relative ease and a few kilowatts of GPU juice. But is this dogged focus on one technique truly the best way forward? This series will explore the implications of a research community that has embarked on a path of Deep Everything, and what that means for the future of AI.
