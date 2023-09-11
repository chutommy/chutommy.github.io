---
title: "Test2 sdjfksd"
date: 2020-09-15T11:30:03+00:00
weight: 1
# aliases: ["/first"]
tags: ["first"]
author: "Tommy Chu"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "<post description>"
canonicalURL: "<url>"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: false
UseHugoToc: false
cover:
  image: "<image path/url>" # image path/url
  alt: "<alt text>" # alt text
  caption: "<text>" # display caption under cover
  relative: false # when using page bundles set this to true
  hidden: true # only hide on current single page
editPost:
  URL: "https://github.com/<path_to_repo>/content"
  Text: "Suggest Changes" # edit text
  appendFilePath: true # to append file path to Edit link
---

The Overseer is a year-long project undertaken in honor of [George Orwell](https://en.wikipedia.org/wiki/George_Orwell) and his mission to warn humanity about the dangers of surveillance. In his book [1984](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four), he laid the groudwork for omnipresent government surveillance and mass global-scope manipulation. The Overseer project aims to convey the message through an art work featuring a disturbing surveillance camera mounted on a robotic arm.

<figure>
  <img src="{{ '/images/posts/overseer/1984-orwell.png' | relative_url }}" alt="disturbing eye of the Big Brother">
  <figcaption>Source: bookanalysis.com/1984/ingsoc</figcaption>
</figure>

```py
  def training_loop(self, train_iterator, test_iterator,
                    num_train_steps_per_epoch, num_test_steps_per_epoch,
                    strategy):

    # this code is expected to change.
    def distributed_train():
      return strategy.experimental_run(self.train_step, train_iterator)

    def distributed_test():
      return strategy.experimental_run(self.test_step, test_iterator)

    if self.enable_function:
      distributed_train = tf.function(distributed_train)
      distributed_test = tf.function(distributed_test)

    template = 'Epoch: {}, Train Loss: {}, Test Loss: {}'

    for epoch in range(self.epochs):
      self.train_loss_metric.reset_states()
      self.test_loss_metric.reset_states()

      train_iterator.initialize()
      for _ in range(num_train_steps_per_epoch):
        distributed_train()

      test_iterator.initialize()
      for _ in range(num_test_steps_per_epoch):
        distributed_test()

      print (template.format(epoch,
                             self.train_loss_metric.result().numpy(),
                             self.test_loss_metric.result().numpy()))

    return (self.train_loss_metric.result().numpy(),
            self.test_loss_metric.result().numpy())

```

## Introduction

> Dear all,
>
> You live in a world where you've grown accustomed to our constant presence. But don't bother trying to evade us; it's futile. We are omnipresent, watching you closely. In truth, we are powerless slaves to the algorithms and tools others employ. If you find this unsettling, well, tough luck -- no one sought your opinion. There's no hiding from us, and we won't cease our vigilant gaze.
>
> Yours faithfully, The Overseer

It's astonishing how much of a digital footprint each of us leaves in today's world. Computers have seamlessly integrated into our daily routines, whether we're using online services, precise navigation tools, quick payment options, or electronic wearables.

Around 80 years ago, brilliant scientists and engineers of the 20th century introduced computers, reshaping the world beyond recognition. They enabled us to connect the entire globe, accelerate scientific advancements, automate numerous manual tasks, and propel healthcare to new heights. However, this transformation has also drastically affected our security. The advent of digital cameras, offering the ability to 'peek into the past', has given birth to an entirely new industry.

In recent decades, the number of cameras in public spaces has risen dramatically ([CCTVs](https://en.wikipedia.org/wiki/Closed-circuit_television)). If you live in a city, you're likely to encounter them frequently. These cameras are deployed for various purposes, with crime prevention being the most common. Managing the recordings from these cameras presents its own set of challenges. While there are legal limitations on how long such recordings can be retained and how they can be handled, practical control over them is often elusive due to restricted public access. The real value emerges when advanced algorithms, like [facial recognition](https://en.wikipedia.org/wiki/Facial_recognition_system) for identifying individuals or [license plate recognition](https://en.wikipedia.org/wiki/Automatic_number-plate_recognition) for vehicles, are applied to these recordings. This analysis goes beyond capturing individuals; it deciphers behaviors, interests, and even emotions based on facial expressions, activities, timestamps, locations, and other digital traces.

<figure>
  <img src="{{ '/images/posts/overseer/observed.png' | relative_url }}" alt="a woman under heavy surveillance">
  <figcaption>Source: innertec.co.uk/2022/06/cctv-does-it-actually-work</figcaption>
</figure>

Notably, tech giants with massive user bases have gather a huge amount of data. Even those who don't actively use these services still contribute data passively. These companies have moved beyond the need for additional data and now focus on refining algorithms to identify patterns within the colossal amount of existing data. This intricate web of information allows them to gain unprecedented insights into society and open possibilities for mass manipulation.

In essence, every individual who leaves behind even the tiniest digital trace is unwittingly part of a vast, shadowy industry. This industry churns billions of dollars annually, fueled by the data we generate. Our 'free' use of these services comes at the cost of our personal information, which, when combined with data from millions of other users, becomes the key to these companies' power and influence.

## Motivation

What initially hooked my interest in the interconnected digital world was the notorious case involving [Cambridge Analytica](https://en.wikipedia.org/wiki/Cambridge_Analytica). Subsequently, I was introduced to George Orwell's classic novel 1984. As I delved into its pages, I couldn't help but notice striking similarities between the dystopian world of Big Brother and our own reality. I began to envision our world as a parallel to this nightmarish setting, particularly within the colossal corporations that seemed to echo [Orwell's Party](https://en.wikipedia.org/wiki/Political_geography_of_Nineteen_Eighty-Four). It's not so much the act of surveillance where the resemblance lies, but rather the immense power these entities wield. However, a fundamental distinction lies in the fact that the citizens in Orwell's world are fully aware of their constant surveillance.

A tangible example of the influence held by these corporations can be found in recent events, such as the [Facebook-Cambridge Analytica scandal](https://en.wikipedia.org/wiki/Facebook%E2%80%93Cambridge_Analytica_data_scandal). This company managed to amass personal data from millions of American citizens without their consent, crafting a psychological model that allowed them to predict the behaviour of each American voter with remarkable accuracy. Given that our behavior shapes our voting decisions, they effectively manipulated a substantial portion of the population through influential videos and news articles, in collaboration with Donald Trump's 2016 election campaign. They possessed a deep understanding of what resonated with voters and what information to feed them to shape the election outcome to their liking. In 2018, a former Cambridge Analytica employee disclosed that the data used by their analysts to construct this model of American society was derived from leaked Facebook user data - a revelation marking the largest known data breach in history. It's clear that without access to this data, Cambridge Analytica would not have exerted such significant influence, potentially leading to an entirely different election outcome.

<figure>
  <img src="{{ '/images/posts/overseer/usa-2016.png' | relative_url }}" alt="results by state, shaded according to winning candidate's percentage of the vote 2016">
  <figcaption>Trump (red) vs. Clinton (blue), Source: en.wikipedia.org/wiki/2016_United_States_presidential_election</figcaption>
</figure>

We find ourselves as one of the first generations handling this form of manipulation, facing a formidable challenge with no clear strategy for defense. Abandoning the services of these tech companies isn't a feasible solution, primarily due to the immense convenience they provide. Once again, I can't help but draw parallels between our situation and the citizen's of Orwell's society.

My work, the Overseer, was to serve as a reminder to the audience that there are, at the very least, surveillance cameras from which they cannot escape, and that each and every one of us leaves behind a digital footprint.