---
layout: post
title: "TTree::MakeClass modify to TChain"
description: ""
category: 
tags: []
---


1. MakeClass of the EventTree:
in root open a file in Phys14_ggNtuples_DYToEE
{% highlight text %} root[0] TFile::Open("/eos/....") {% endhighlight %}
{% highlight text %} root[1] ggNtuplizer->cd(){% endhighlight %}
{% highlight text %} root[2] EventTree->MakeClass()("/eos/....") {% endhighlight %}
EventTree.h and EventTree.C generated from TTree: EventTree


2. Modify the codes, use the TChain to load all the files:

a)Modify EventTree.h

Add:
{% highlight text %}
#include<iostream>
using namespace std;
{% endhighlight %}

replace:
{% highlight text %}TTree    *fChain;   //!pointer to the analyzed TTree or TChain
 {% endhighlight %}
by:

{% highlight text %}TChain    *fChain;   //!pointer to the analyzed TTree or TChain
{% endhighlight %}

replace:
{% highlight text %}EventTree(TTree *tree=0); {% endhighlight %}
by:
{% highlight text %}TChain * chain; EventTree(TChain * chain=0);{% endhighlight %}

replace the constructor:
{% highlight text %}
EventTree::EventTree(TTree *tree) : fChain(0)
{
// if parameter tree is not specified (or zero), connect the file
// used to generate this class and read the Tree.
if (tree == 0) {
TFile *f = (TFile*)gROOT->GetListOfFiles()->FindObject("/eos/uscms/store/user/areinsvo/DYToEE_M-50_Tune4C_13TeV-pythia8/Phys14_ggNtuples_DYToEE/150126_213933/0000/ggtree_mc_90.root");
if (!f || !f->IsOpen()) {
f = new TFile("/eos/uscms/store/user/areinsvo/DYToEE_M-50_Tune4C_13TeV-pythia8/Phys14_ggNtuples_DYToEE/150126_213933/0000/ggtree_mc_90.root");
}
TDirectory * dir = (TDirectory*)f->Get("/eos/uscms/store/user/areinsvo/DYToEE_M-50_Tune4C_13TeV-pythia8/Phys14_ggNtuples_DYToEE/150126_213933/0000/ggtree_mc_90.root:/ggNtuplizer");
dir->GetObject("EventTree",tree);
}
Init(tree);
} 
{% endhighlight %}

by:
{% highlight text %}
EventTree::EventTree(TChain *chain)

{
TChain *chain=new TChain("ggNtuplizer/EventTree");
chain->Add("/eos/..../..ggtree_mc_*.root");
std::cout<<"Total number of entries in chain"<<chain->GetEntries()<<std::endl;
Init(chain);
}
{% endhighlight %}

replace:
{% highlight text %}
virtual void Init(TTree *tree);
{% endhighlight %}

by:
{% highlight text %}
virtual void Init(TChain *tree);
{% endhighlight %}

replace:
{% highlight text %}
 void EventTree::Init(TTree *tree)
{% endhighlight %}

by:
{% highlight text %}
 void EventTree::Init(TChain *tree)
{% endhighlight %}


b)Modify EventTree.C
modify the loop

3.Run
in root
{% highlight text %} root[0].L EventTree.C {% endhighlight %}
{% highlight text %} root[0] EventTree t{% endhighlight %}
{% highlight text %} root[0] t.Loop(){% endhighlight %}



