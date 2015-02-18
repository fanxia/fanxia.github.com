---
layout: post
title: "TTree::MakeClass modify to TChain"
description: ""
category: 
tags: []
---
{% include JB/setup %}

1. MakeClass of the EventTree:
in root open a file in Phys14_ggNtuples_DYToEE
{% highlight text %} root[0] TFile::Open("/eos/....") {% endhighlight %}
{% highlight text %} root[1] ggNtuplizer->cd(){% endhighlight %}
{% highlight text %} root[2] EventTree->MakeClass()("/eos/....") {% endhighlight %}


