# PageRank

In this project, I attempt to replicate the link-based ranking system developed by Sergey Brin and Larry Page in their 1998 paper "The PageRank Citation Ranking: Bringing Order to the Web”. This algorithm remains the foundation for Google's web search tools. Langville and Meyer provide additional guidance on the construction and components of PageRank in their 2004 paper "Deeper Inside PageRank". Professor Mike Izbicki provided valuable guidance throughout this project.

We begin by constructing a Markov matrix P, where each entry ij "is the proabability of moving from state i to state j" (Langville and Meyer 2004). This matrix is transformed into a stochastic, irreducible, and primitive matrix. This Markov matrix will converge to the dominant eigenvector. This vector is the PageRank vector which indicates the importance of each webpage within a graph. To do so, Brin and Page use the power method, which stores just the previous iterate for each iteration, and converges quickly for the stochastic, irreducible, and primitive P (P bar bar) matrix.

## Task 1: Implementing the Power Method
**Part 1: Basic Power Method Implementation**
After implementing the algorithm, we can test the basic algorithm on a 6 webpage graph, similar to the one used in *Langville and Meyer 2004*. We obtain the following result:
Note that the verbose tag has been turned on to demonstrate how in the DEBUG statements, the algorithm converges, toward the constant *epsilon* term 1e-6. The pages have been ranked with the fourth URL being the highest ranked, and the first URL being the lowest. These results are similar to those obtained in Prof. Izbicki's implementation.

<pre><code>[In]: run pagerank.py --data=small.csv.gz --verbose
DEBUG:root:computing indices
DEBUG:root:computing values
DEBUG:root:root=i=0 accuracy=2.7229e-01
DEBUG:root:root=i=1 accuracy=1.3318e-01
DEBUG:root:root=i=2 accuracy=8.1429e-02
DEBUG:root:root=i=3 accuracy=3.7537e-02
DEBUG:root:root=i=4 accuracy=2.4824e-02
DEBUG:root:root=i=5 accuracy=1.2412e-02
DEBUG:root:root=i=6 accuracy=8.0711e-03
DEBUG:root:root=i=7 accuracy=4.3977e-03
DEBUG:root:root=i=8 accuracy=2.7713e-03
DEBUG:root:root=i=9 accuracy=1.5882e-03
DEBUG:root:root=i=10 accuracy=9.7799e-04
DEBUG:root:root=i=11 accuracy=5.7459e-04
DEBUG:root:root=i=12 accuracy=3.4924e-04
DEBUG:root:root=i=13 accuracy=2.0761e-04
DEBUG:root:root=i=14 accuracy=1.2532e-04
DEBUG:root:root=i=15 accuracy=7.4923e-05
DEBUG:root:root=i=16 accuracy=4.5074e-05
DEBUG:root:root=i=17 accuracy=2.6996e-05
DEBUG:root:root=i=18 accuracy=1.6217e-05
DEBUG:root:root=i=19 accuracy=9.7244e-06
DEBUG:root:root=i=20 accuracy=5.8908e-06
DEBUG:root:root=i=21 accuracy=3.5000e-06
DEBUG:root:root=i=22 accuracy=2.0959e-06
DEBUG:root:root=i=23 accuracy=1.2712e-06
INFO:root:rank=0 pagerank=6.8019e-01 url=4
INFO:root:rank=1 pagerank=5.3070e-01 url=6
INFO:root:rank=2 pagerank=4.1114e-01 url=5
INFO:root:rank=3 pagerank=2.0103e-01 url=2
INFO:root:rank=4 pagerank=1.5937e-01 url=3
INFO:root:rank=5 pagerank=1.4440e-01 url=1</code></pre>

**Part 2: Search Queries**
Next, we can use a number of command line arguments to refine our search. The algorithm will now return those pages most relevant to our queries. The <code>--search_query</code> argument accepts a string and compares it with each links and filters out those links that do not include the query. For this portion, I use a dataset prepared by Professor Mike Izbicki that graphs hyperlinks from the defense blog www.lawfareblog.com. From this point forward, I will not use the verbose command to make results more concise. The results below are similar to those obtained in Prof. Izbicki's implementation.

If we make our search query 'corona' to find articles about the pandemic, the following links are the most relevant according to PageRank:
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --search_query=corona
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=9.3788e-04 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=8.3128e-04 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=6.4858e-04 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=6.3671e-04 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=6.1607e-04 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=6.0841e-04 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=5.9702e-04 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=7 pagerank=5.8340e-04 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=8 pagerank=5.6037e-04 url=www.lawfareblog.com/house-subcommittee-voices-concerns-over-us-management-coronavirus
INFO:root:rank=9 pagerank=5.5212e-04 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response</code></pre>

Next, we'll adjust our search query to 'trump':
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --search_query=trump
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=5.6705e-03 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=5.0165e-03 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=2 pagerank=4.9383e-03 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=4.4786e-03 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=4 pagerank=4.4514e-03 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=5 pagerank=4.1355e-03 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=3.9194e-03 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=3.5939e-03 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=3.3013e-03 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=3.2977e-03 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors</code></pre>

Finally, we'll change our search query to 'iran':
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --search_query=iran
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.4333e-03 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=1 pagerank=4.2419e-03 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=2 pagerank=2.5767e-03 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation
INFO:root:rank=3 pagerank=1.8622e-03 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=4 pagerank=1.4607e-03 url=www.lawfareblog.com/parsing-state-departments-letter-use-force-against-iran
INFO:root:rank=5 pagerank=1.4525e-03 url=www.lawfareblog.com/iranian-hostage-crisis-and-its-effect-american-politics
INFO:root:rank=6 pagerank=1.4419e-03 url=www.lawfareblog.com/announcing-united-states-and-use-force-against-iran-new-lawfare-e-book
INFO:root:rank=7 pagerank=1.3405e-03 url=www.lawfareblog.com/us-names-iranian-revolutionary-guard-terrorist-organization-and-sanctions-international-criminal
INFO:root:rank=8 pagerank=1.1074e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications
INFO:root:rank=9 pagerank=1.0752e-03 url=www.lawfareblog.com/israel-iran-syria-clash-and-law-use-force</code></pre>

**Part 3: Concerns about the structure of webpages**
Most websites have a lot of structure, as most pages are connected to the homepage and some other broad pages, like www.lawfareblog.com/topics. Because PageRank does link ranking, those sites that many pages link to will often, but not always, have a higher rating. If we examine the largest PageRanks across www.lawfareblog.com, we can see a several of these broad pages appear:

<pre><code>run pagerank.py --data=./lawfareblog.csv.gz
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=2.8757e-01 url=www.lawfareblog.com/masthead
INFO:root:rank=1 pagerank=2.8757e-01 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=2.8757e-01 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=2.8757e-01 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=2.8757e-01 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=5 pagerank=2.8757e-01 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=6 pagerank=2.8757e-01 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=7 pagerank=2.8757e-01 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=8 pagerank=2.8757e-01 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=9 pagerank=2.8757e-01 url=www.lawfareblog.com/topics
</code></pre>

But these pages are generally not useful if we want to learn what the blog's most popular content was. To collect data on articles, which will generally have fewer pages linking to them than broader pages, we can use the <code>--filter_ratio</code> argument. It removes "all pages with more links than the specified ration" (Prof. Izbicki). Now, we can estimate the most important articles:

<pre><code>run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=3.6463e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=3.0601e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=3.0129e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.4923e-01 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=1.4814e-01 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=5 pagerank=1.4171e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=6 pagerank=1.4171e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=7 pagerank=1.4137e-01 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=8 pagerank=1.4046e-01 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull
INFO:root:rank=9 pagerank=1.4039e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
</code></pre>

**Part 4: Eigengaps**
The eigengap of the P barbar matrix is bounded by the alpha parameter. The size of the eigengap determines determines the speed at which the algorithm converges. If the eigengap of a matrix is large, then it will converge quickly even at alpha values that approach 1. If the eigengap is small, then only at smaller alpha values will the convergence occur quickly. We can run the following four commands to illustrate this
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose 
[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
</code></pre>
While the first three commands took 15, 11, and 22 iterations, the final one takes 684 iterations. This is because the filtered P matrix has a smaller eigengap, so at larger alpha bounds, it takes longer to converge. This change in the alpha value also results in different PageRank rankings.
First, the results of the third command, which filters the P graph, but does not use a large alpha:
<pre><code> [In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
INFO:root:rank=0 pagerank=3.6463e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=3.0601e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=3.0129e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.4923e-01 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=1.4814e-01 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=5 pagerank=1.4171e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=6 pagerank=1.4171e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=7 pagerank=1.4137e-01 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=8 pagerank=1.4046e-01 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull
INFO:root:rank=9 pagerank=1.4039e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
</code></pre>
Then, the results of the fourth command, which filters the P graph and uses a large alpha:
<pre><code> [In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
INFO:root:rank=0 pagerank=7.0149e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-responselance-response                                                                         
INFO:root:rank=1 pagerank=7.0149e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.0552e-01 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=3.1757e-02 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=2.2040e-02 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=1.6027e-02 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=1.6026e-02 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=1.6023e-02 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=1.6020e-02 url=www.lawfareblog.com/water-wars-song-oil-and-fire
INFO:root:rank=9 pagerank=1.6020e-02 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations
</code></pre>

## Task 2: Implementing the Personalization Vector
**Part 1: Basic Power Method Implementation**
The personalization vector is alternative method of filtering via queries. The personalization vector determine which webpages are linked to most often from pages that are about the query itself. This is different than the previous <code>--search_query</code> argument which determines an overall PageRank, then filters for the highest ranked matches that are related to the query. To demonstrate this, we can compare the rankings of the same query when using the personalization vector versus the <code>--search_query</code> argument. 
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query=corona
</code></pre>
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query=corona
</code></pre>

**Part 2: Finding articles that are related, but don't mention our query**
The personalization vector is especially useful because it tracks which articles are most relevant to the query. This means that if we want to learn about the indirect effects our query may be having, we can still use the same personalization vector search query. Meanwhile, we can use the <code>--search_query</code> argument to present only those articles do not include the query itself. We get the following results for 'corona':
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query=corona --search_query=-corona
</code></pre>

**Part 3: Finding articles that are related, but don't mention our query (cont.)**
We can take a look at another example of the above, where articles are related, but do not explicitly mention the query. For 'iran', we ge the following results:
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query=iran --search_query=-iran
</code></pre>

