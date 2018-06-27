There are a few interesting levels (not quite layers) at which it may be interesting to quantify consolidation, n terms of dependence of the Internet ecosystem on a (too-)small number of actors in a given slot in the ecosystem. Here are some of them, from my note to the IAB of 27 June:

Layer 3 interconnection
=======================

This is, indeed, the motivation behind Geoff Huston's 2017 rant/warning on the subject. Here there's been useful academic work recently: IIJ has put together a measure of "Autonomous System Hegemony", which is a quantity of the relationship between a given AS and those that depend on it. 

See the RIPE Labs article: https://labs.ripe.net/Members/romain_fontugne/as-hegemony-measuring-as-interdependence

Or the PAM paper: https://www.iij-ii.co.jp/en/members/romain/pdf/romain_pam2018.pdf

Ted's comments thereon

> for the underlying paper.  Reading it, I think there are two issues in this approach.  The first is that the measurement of betweenness centrality (which they pulled from an earlier paper by Liu et al) seems to ignore the possibility that aggregation is being hidden in the choice of organizations to present multiple ASes.  Though those are different from the BGP selection algorithm perspective, from an economic perspective, they are not.  That means that the overall flattening they observe may be in part a function of the choice by key providers to use multiple ASes.  Put another way, the hierarchical aspects may be worse when seen from an economic perspective.

> The other issue is flow weighting.  In fairness, they weren't trying to do this, but it's important to understanding the impact.  If the upshot of this effect is that there are already some flows which cannot happen or are severely impacted by long routes, some sense of how many potential flows are impacted would be useful.  Collecting that data is hard at this scale, but maybe we should encourage it (we use some time-series data for this internally, showing big changes in traffic to our front-ends, either positive or negative, and I'm guessing that others doe similar things).


Layer 7 service provision
=========================

This is the concern we've articulated that the (architecturally-supported) ease of DDoS (the democratization of censorship) means that a publisher must have access to a particular scale of infrastructure if it wants to say anything that might be remotely interesting to knock off the network (i.e., anything remotely interesting at all).

A simple proposed measurement methodology
-----------------------------------------

There's a fairly simple measurement methodology here that could produce a snapshot of the consolidation in Web content provision, a variant of which I think Christian said he'd run at least once:

1. resolve all addresses for a set of web domains ordered by popularity (e.g. the Alexa top million, or the Cisco top domains list)
2. classify those addresses by hosting/infrastructure provider (e.g. using https://stat.ripe.net to associate a prefix with the ASs announcing it).
3. for a site of rank n, with m addresses, assign a frequency according to a Zipf distribution, evenly split among all its addresses. (Pick a Zipf exponent randomly for this exercise. One is a fine number to start with.)
4. sum frequency by AS, rank ASs and plot the CDF of this.

Comparing the slope of the CDF over time gives you the ability to see how much "popularity" the top N infrastructure providers at layer 7 have been able to attract. Steeper slopes are less diverse.

The MAMI project maintains a couple of tools (https://github.com/mami-project/hellfire, https://github.com/britram/canid) that would make this a simple matter of a few tens of lines of code to run a current measurement; I haven't had the time but might be able to find some after the draft deadline. Getting access to historical DNS data would make it possible to look back in time.

Other work in the area
----------------------

Kashaf et al, [Oh, What a Fragile Web We Weave: Third-party Service Dependencies In
Modern Webservices and Implications](https://arxiv.org/pdf/1806.08420.pdf) look at this question more broadly, pulling in authoritative DNS and OCSP infrastructure as well, and looking at interdependency as well as rank. They find (from the abstract) that:

- 73.14% of the top 100,000
popular services are vulnerable to reduction in availabil-
ity  due  to  potential  attacks  on  third-party  DNS,  CDN,
CA  services  that  they  exclusively  rely  on; 
- the  use
of  third-party  services  is  concentrated,  so  that  if  the
top-10 providers of CDN, DNS and OCSP services go
down, they can potentially impact 25%-46% of the top
100K most popular web services;
- transitive depen-
dencies significantly increase the set of webservices that
exclusively  depend  on  popular  CDN  and  DNS  service
providers, in some cases by ten times;
- targeting even
less  popular  webservices  can  potentially  cause  signifi-
cant  collateral  damage,  affecting  up to  20%  of  the  top-
100K  webservices  due  to  their  shared  dependencies

Software infrastructure 
=======================

This is a little fuzzier; it's basically the concern that large parts of the Internet ecosystem are dependent on the work of very small numbers of organizations or people, due paradoxically to the scale that can be achieve by largely turning software development into an integration process while simultaneously failing to solve the software dependency problem. There have been anecdotal incidents here: Heartbleed is one, there was an NPM package problem with a fairly basic chunk of Javascript everyone included that brought down bits of the Web, and so on.

This is more a trend in software engineering than in Internet engineering, and tends to be more of a problem at the edges than the core. I'm also not at all sure how to quantify this, but I think it should be on our radar.

