---
layout: category-post
title:  "Deep Everything (part I)"
date:   2018-01-29 15:20:56 -0800
categories: writing
---

Back in the old days of machine learning -- roughly 2005 -- there wasn't much agreement as to which models would ultimately lead us to the AI promised land. There were a variety of competing theories, deeply rooted in the philosophy of machine learning, and academics were fiercely protective of their viewpoint.

The first question was whether focus should be placed on _generative_ or simply _discriminative_ models. A _generative_ model is, roughly, one in which "the full story of the data" is explained by the model -- that is, the model is based on a story of how the data was _generated_. A discriminative model, on the other hand, views every problem like a black box: the only relevant question is how to look at the data and make predictions, with no regard for how the data came to be, or, even less, what kind of data it is.

Deep methods represent the discriminative approach taken to the extreme -- even feature extraction is, in some sense, a black box.

Andrew Ng, one of the veritable leaders of the deep learning community, seemed to fall on the side of _generative_ models. One of his early successes was Latent Dirichlet Allocation, which proposed a generative model for text documents, and discovered latent "topics" therein. The results of LDA are easily competitive with modern neural methods, although decidedly less popular.

While such early generative models, championed largely by the Bayesians, were some of the first to show real promise on difficult tasks, they were soon superseded by deep nets. Research interest shifted quickly, and now we live in a world where nearly every paper you read has some deep net connection. Andrew Ng and others like him went from being the face of the Bayesians to the face of deep learning. 

But for all the research into applications and optimization heuristics, deep nets remain shrouded in mystery. From a philosophical standpoint, a mathematical standpoint, and a technical standpoint, much remains to be understood about deep nets and their "unreasonable effectiveness" [\[1\]](http://karpathy.github.io/2015/05/21/rnn-effectiveness/).

The AI practitioner will invariably look with excitement to the neverending parade of Keras diddies that seem to perform human tasks with relative ease and a few kilowatts of GPU juice. But is this dogged focus on one technique truly the best way forward? This series will explore the implications of a research community that has embarked on a path of Deep Everything, and what that means for the future of AI.

### The "magic"

Perhaps the most comonly cited criticism of deep learning is that much of it appears to be "magic" -- there is no intuitive or mathematical explanation for the effectiveness we often ascribe to it, and the philosophical discussion seems to be limited to the simple remark that deep methods appear to learn varying layers of abstraction -- a point that has recently been questioned extensively.

#### Parameter space magic

So what is magical about these machines?

---

**Technical explanation**

The parameter space of a deep net typically numbers in the millions, and while data sets have historically been large (although this has recently been shown not to be a _necessary_ criterion for successful deep learning [\[2\]](https://beamandrew.github.io/deeplearning/2017/06/04/deep_learning_works.html)), the common arguments about high-dimensional geometry that seek to rigorously study overfitting would say any such model would necessarily overfit horribly. However, the commonly used optimization heuristics (which typically involve initialization based on Gaussian noise and some variant of stochastic graident descent) seem to find solutions that generalize shockingly well -- particularly in tasks like image or voice recognition, where humans themselves tend to do well.

---

**Layman's explanation**

The traditional understanding in statistics is that the more the model has to learn in order to make predictions, the more data you need to have. And it's not a one-to-one relationship: the growth of the data needed is widely believed to be exponential in the amount the model needs to learn. Deep nets are unusual in that they need to learn literally millions of parameters -- by way of comparison, your vanilla "linear fit" in Excel needs to learn two parameters -- with a comparable amount of data. There's no way to find the true optimum of such a complex system, so instead we rely on algorithms that search for "good enough" solutions to these millions of parameters. Shockingly, these methods manage to do extremely well -- particularly at tasks like image or voice recognition, where humans themselves tend to do well.

---

So we start with a random solution, and iteratively improve it until it seems, objectively, "pretty good." And, there are usually infinitely many other such solutions that would yield identical results, if we find ourselves in a valley or along a ridge. However, we don't concern ourselves with these geometric curiosities -- we instead content ourselves with the fact that the algorithm killed it on the benchmark data set. And that's that.

Indeed, criticism of the magic came into the spotlight when Ali Rahimi referred to deep learning research as "alchemy" in his NIPS "test of time" award speech [\[3\]](https://www.reddit.com/r/MachineLearning/comments/7hys85/n_ali_rahimis_talk_at_nipsnips_2017_testoftime/), in which he urged researchers instead to return to their more pure mathematical roots in an attempt to more deeply understand the nature of our solutions. This brought a torrent of criticism, including from the father of deep learning himself, Yann LeCunn, who wrote a scathing rant [\[4\]](https://www.reddit.com/r/MachineLearning/comments/7i1uer/n_yann_lecun_response_to_ali_rahimis_nips_lecture/) in response to Rahimi.

#### The Kansas City shuffle

This magic solution-finding doesn't come without its flaws. If one aims to deceive a deep learning system, one certainly can. Problem is, many deep nets are remarkably prone to "adversarial perturbations." In other words, small changes to the input data -- small enough that a human wouldn't notice them -- can be engineered to force the net to return virtually anything. That's the Kansas City shuffle: regardless of their phenomental classification accuracy, they can be swiftly tricked by a carefully engineered picture of a dog into predicting that dog is definitely a mongoose.

Obviously there are implications for potential applications. Anything that functions as a "screening mechanism" should clearly avoid deep nets. For example, a group out of Germany successfully engineered malware capable of avoiding detection by neural nets used for malware classication [\[5\]](https://arxiv.org/abs/1606.04435).

It also means embarrassing incidents can arise for software vendors that rely on deep learning. For example, the esteemed Google Photo classifier famously tagged a number of African-American women as "gorillas" [\[6\]](https://www.wnyc.org/story/deep-problem-deep-learning/). The Kansas City shuffle can arise out of nowhere, it would seem, if the scale of deployment is large enough. 

Google responded quickly to the incident, which was announced on Twitter. They temporarily disabled the gorilla classification entirely -- the fire-drill fix, I suppose -- and eventually were able to tune their model to not make that mistake. One can only imagine what other mistakes it might be making -- perhaps less politically and socially sensitive mistakes -- that go unnoticed and unreported.

#### The deep disappearing act

Perhaps nothing irks the data scientist more than the great deep disappearing act. You train a model. You test on some sample data. It looks awesome. You re-train the model to show off your incredible results, and your colleagues are met with... garbage? 

Indeed, the parameter space problem can mean that one training run of a deep net may yield excellent results, yet the very same procedure irreconcilably produces terrible results the next run. LSTMs, widely known for their ability to spin fake Shakespeare or Bible stories, often take several human-assisted cracks at the old optimization apple before converging to a working solution. GANs, widely known for their ability to paint fake bedrooms and faces, are known for having this problem in the form of "mode collapse" [\[7\]](http://aiden.nibali.org/blog/2017-01-18-mode-collapse-gans/), which is essentially the phenomenon of a generator getting stuck in a part of parameter space with unusual geometry. (Remember, as deep net researchers, we don't care about high-dimensional geometry.) This makes convergence impossible, or at least unpredictable -- which, in turn, makes research impossible to reproduce with any certainty.

There's nothing inherently wrong with a human in the loop; in fact, human-assisted methods are probably the subject of more stigma than they deserve. Commercial groups, for example, can easily afford some minor human intervention to get their product just right, unscientific though the process may be. And, there is a new and growing body of theory around "training heuristics" for deep nets, which hope to formalize some understanding of how gradient descent, learning rates, and optimization algorithms play together to produce good results. The ultimate goal would be to avoid just these kinds of problems: uncertainty, failure to converge, difficulty with reproducing results, and so on.

But in academic circles, the inability to reproduce results is a mortal sin. Being able to prove the correctness of an experiment is one of the underpinnings of our globally united scientific ecosystem. Without it, we are stuck with the decision of either a.) trusting all authors to not fudge data or b.) ignoring potentially valuable research because its results are impossible (or very expensive) to reproduce.

#### The flipside

Magic may not be science. It may not be good for science. But at the end of the day, engineers don't care much what the purity of the source is if the source is good. There's a place for magic, for sure.

And, understanding the magic may lead us into fantastic parts of science we'd never dreamed. We may learn more not only about statistical methods but about humans and how we learn from information. There is a grand hope to deep learning, for sure, and nothing would be bigger than this. If we understood why it worked, we could fix the adversarial perturbations problem, and likely bring deep learning methodologies to a new level of sophistication. But for the moment, it seems more like a trick in the data science toolbag than a foundational mathematical result.

The question is -- what does this mean for the future of AI?

_(To be continued...)_

---

[\[1\]](http://karpathy.github.io/2015/05/21/rnn-effectiveness/): http://karpathy.github.io/2015/05/21/rnn-effectiveness/

[\[2\]](https://beamandrew.github.io/deeplearning/2017/06/04/deep_learning_works.html): https://beamandrew.github.io/deeplearning/....

[\[3\]](https://www.reddit.com/r/MachineLearning/comments/7hys85/n_ali_rahimis_talk_at_nipsnips_2017_testoftime/): https://www.reddit.com/r/MachineLearning/...

[\[4\]](https://www.reddit.com/r/MachineLearning/comments/7i1uer/n_yann_lecun_response_to_ali_rahimis_nips_lecture/): https://www.reddit.com/r/MachineLearning/...

[\[5\]](https://arxiv.org/abs/1606.04435): https://arxiv.org/abs/1606.04435

[\[6\]](https://www.wnyc.org/story/deep-problem-deep-learning/): https://www.wnyc.org/story/deep-problem-deep-learning/

[\[7\]](http://aiden.nibali.org/blog/2017-01-18-mode-collapse-gans/): http://aiden.nibali.org/blog/2017-01-18-mode-collapse-gans/

_Updated September, 2020_

<div id="comments" class="zp-main">
  <div class="zp-inputs">
      <p>
          <span class="zp-br">&nbsp;</span>
          <span class="zp-description"> </span>
      </p>
      <p class="zp-help-message">&nbsp;</p>
      <p><span class="zp-input-label"> </span></p>
      <input  type="text" />
      <p><span class="zp-pass-label"> </span></p>
      <input type="password" />
      <div class="zp-comments-edit-profile">
          <p><span class="zp-textarea-description">Description</span></p>
          <textarea></textarea>
      </div>
  </div>
  <div class="zp-comments-links">
      <a class="zp-submit-link zp-form-link" href="#">submit</a>
      <a class="zp-signup-link zp-form-link" href="#">signup</a>
      <a class="zp-login-link zp-form-link" href="#">login</a>
      <a class="zp-edit-profile-link zp-form-link" href="#">edit profile</a>
  </div>
  <div class="zp-comments"></div>
</div>
<script>
  // Takes ID, subuser token, div ID, and options. 
  commentsify('044afe26-7cf1-44b6-a27a-a2303a30e655', 'cjs_token', 'comments', {layout: "standard", auth: "3pa"})
</script>