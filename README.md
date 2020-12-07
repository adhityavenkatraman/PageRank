# PageRank

In this project, I attempt to replicate the link-based ranking system developed by Sergey Brin and Larry Page in their 1998 paper "The PageRank Citation Ranking: Bringing Order to the Web”. This algorithm remains the foundation for Google's web search tools. Langville and Meyer provide additional guidance on the construction and components of PageRank in their 2004 paper "Deeper Inside PageRank". 

We begin by constructing a Markov matrix <img src="https://render.githubusercontent.com/render/math?math=P">, where each entry <img src="https://render.githubusercontent.com/render/math?math=ij"> "is the proabability of moving from state i to state j" (Langville and Meyer 2004). This hyperlink matrix is transformed into a stochastic, irreducible, and primitive matrix. This Markov matrix will converge to the dominant eigenvector. This vector is the PageRank vector which indicates the importance of each webpage within a graph. To do so, Brin and Page use the power method, which stores just the previous iterate for each iteration, and converges quickly for the stochastic, irreducible, and primitive <img src="https://render.githubusercontent.com/render/math?math=\bar{P}"> matrix.

Visit the pagerank.py file to view the implementation. In particular, the power_method and make_personalization_vector functions are at the core of the implementation. The power_method function implements <img src="https://render.githubusercontent.com/render/math?math=x^{(k)T}=\alpha%20x^{(k-1)T}%20P%20%2B%20(\alpha%20x^{(k-1)T}%20a%20%2B%20(1-\alpha))v^{T}">. This algorithm is iterated until it converges to a sufficiently low error,, often 1e-6; the resulting eigenvector yeilds the PageRank vector. Note that because the function applies the power method to thex relatively sparse hyperlink matrix, converting it into a stochastic, irreducible, and primitive matrix, the runtime of the algorithm is considerably less than if we explicitly defined the latter matrix. Meanwhile, the make_personalization_vector method accepts a query, then creates a vector of articles that mention the query. This allows our power_method function to complete a more targeted ranking of the pages that are most often linked to pages we already know are relevant.

<h3>UPDATE: 12/2/20</h3>
All of the results below have been updated to reflect the following changes. I updated the pagerank.py implementation to expand the scope of the --search_query argument. Rather than finding urls that have only that term in them, the updated program uses the gensim glove-twitter-25 model to identify the top 5 most similar words. These additional words are then included as queries as well, helping to find related articles that may happen to not include the query we directly identified. For example, the ranking associated with a query for "pets" might include links that reference "dogs". Once again, the results below have been updated based on this new ranking system. 
To illustrate, a search for 'weapons' results in a number of results about drones in the example below.

<pre><code>run pagerank.py --data=./lawfareblog.csv.gz --search_query=weapons
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=4.5715e-03 url=www.lawfareblog.com/why-did-you-wait-moral-emptiness-and-drone-strikes
INFO:root:rank=1 pagerank=3.1107e-03 url=www.lawfareblog.com/dc-district-court-dismisses-journalists-drone-lawsuit
INFO:root:rank=2 pagerank=2.0231e-03 url=www.lawfareblog.com/revived-cia-drone-strike-program-comments-new-policy
INFO:root:rank=3 pagerank=1.9667e-03 url=www.lawfareblog.com/us-court-appeals-dc-circuit-dismisses-suit-over-us-drone-strike
INFO:root:rank=4 pagerank=1.1788e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications
INFO:root:rank=5 pagerank=1.1620e-03 url=www.lawfareblog.com/slaughterbots-and-other-anticipated-autonomous-weapons-problems
INFO:root:rank=6 pagerank=1.1276e-03 url=www.lawfareblog.com/german-courts-weigh-legal-responsibility-us-drone-strikes
INFO:root:rank=7 pagerank=8.3738e-04 url=www.lawfareblog.com/shift-jsoc-drone-strikes-does-not-mean-cia-has-been-sidelined
INFO:root:rank=8 pagerank=7.8704e-04 url=www.lawfareblog.com/atomwaffen-division-member-pleads-guilty-firearms-charge
INFO:root:rank=9 pagerank=7.8570e-04 url=www.lawfareblog.com/waiving-imminent-threat-test-cia-drone-strikes-pakistan</code></pre>

Some more examples are included at the bottom of these results for "drones" and "targets" that help illustrate the effect of the new system, though all of the results in this file apply the new ranking system.

**Part 1: Testing the Basic Power Method Implementation**

After implementing the algorithm, we can test the basic algorithm on a 6 webpage graph, similar to the one used in *Langville and Meyer 2004*. We obtain the following result:
Note that the verbose tag has been turned on to demonstrate how in the DEBUG statements, the algorithm converges, toward the constant *epsilon* term 1e-6. The pages have been ranked with the fourth URL being the highest ranked, and the first URL being the lowest. These results are validated by Mike Izbicki's implementation. 

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
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=1.0038e-03 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=8.9224e-04 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=7.0390e-04 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=6.9153e-04 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=6.7041e-04 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=6.6256e-04 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=6.5046e-04 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=7 pagerank=6.3620e-04 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=8 pagerank=6.1248e-04 url=www.lawfareblog.com/house-subcommittee-voices-concerns-over-us-management-coronavirus
INFO:root:rank=9 pagerank=6.0187e-04 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response</code></pre>
Next, we'll adjust our search query to 'trump':
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --search_query=trump
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=5.7826e-03 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=5.2338e-03 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=2 pagerank=5.1297e-03 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=4.6599e-03 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=4 pagerank=4.5934e-03 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=5 pagerank=4.3071e-03 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=4.0935e-03 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=3.7591e-03 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=3.4509e-03 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=3.4484e-03 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors</code></pre>

Finally, we'll change our search query to 'iran':
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --search_query=iran
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=5.1297e-03 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=1 pagerank=5.0168e-03 url=www.lawfareblog.com/update-military-commissions-continued-health-issues-recusal-motion-and-new-cell-al-iraqi
INFO:root:rank=2 pagerank=4.5746e-03 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=3 pagerank=4.4174e-03 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=4 pagerank=4.3659e-03 url=www.lawfareblog.com/haftar-attacking-tripoli-us-needs-re-engage-libya
INFO:root:rank=5 pagerank=3.4237e-03 url=www.lawfareblog.com/france-makes-play-try-foreign-fighters-iraq
INFO:root:rank=6 pagerank=2.6928e-03 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation   
INFO:root:rank=7 pagerank=2.2567e-03 url=www.lawfareblog.com/document-sens-kaine-and-young-introduce-bill-revoke-iraq-force-authorizations
INFO:root:rank=8 pagerank=1.9391e-03 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=9 pagerank=1.8263e-03 url=www.lawfareblog.com/its-not-only-iraq-and-syria</code></pre>

**Part 3: Concerns about the structure of webpages**
Most websites have a lot of structure, as most pages are connected to the homepage and some other broad pages, like www.lawfareblog.com/topics. Because PageRank does link ranking, those sites that many pages link to will often, but not always, have a higher rating. If we examine the largest PageRanks across www.lawfareblog.com, we can see a several of these broad pages appear:

<pre><code>run pagerank.py --data=./lawfareblog.csv.gz
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=2.8741e-01 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=1 pagerank=2.8741e-01 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=2.8741e-01 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=3 pagerank=2.8741e-01 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=4 pagerank=2.8741e-01 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=5 pagerank=2.8741e-01 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=6 pagerank=2.8741e-01 url=www.lawfareblog.com/support-lawfare
INFO:root:rank=7 pagerank=2.8741e-01 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=8 pagerank=2.8741e-01 url=www.lawfareblog.com/topics
INFO:root:rank=9 pagerank=2.8741e-01 url=www.lawfareblog.com/documents-related-mueller-investigation
</code></pre>

But these pages are generally not useful if we want to learn what the blog's most popular content was. To collect data on articles, which will generally have fewer pages linking to them than broader pages, we can use the <code>--filter_ratio</code> argument. It removes "all pages with more links than the specified ration" (Prof. Izbicki). Now, we can estimate the most important articles:

<pre><code>run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=3.4696e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns       
INFO:root:rank=1 pagerank=2.9521e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.9040e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.5179e-01 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=1.5099e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=5 pagerank=1.5099e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=6 pagerank=1.5071e-01 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=7 pagerank=1.4957e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=1.4367e-01 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=9 pagerank=1.4240e-01 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull
</code></pre>

**Part 4: Eigengaps**
The eigengap of the P barbar matrix is bounded by the $\alpha$ parameter. The size of the eigengap determines determines the speed at which the algorithm converges. If the eigengap of a matrix is large, then it will converge quickly even at alpha values that approach 1. If the eigengap is small, then only at smaller alpha values will the convergence occur quickly. We can run the following four commands to illustrate this:
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose 
[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
[In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
</code></pre>
While the first three commands took 15, 11, and 22 iterations, the final one takes 684 iterations. This is because the filtered P matrix has a smaller eigengap, so at larger alpha bounds, it takes longer to converge. This change in the alpha value also results in different PageRank rankings.
First, the results of the third command, which filters the P graph, but does not use a large alpha:
<pre><code> [In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
INFO:root:rank=0 pagerank=3.4696e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns       
INFO:root:rank=1 pagerank=2.9521e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.9040e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.5179e-01 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=1.5099e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=5 pagerank=1.5099e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=6 pagerank=1.5071e-01 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=7 pagerank=1.4957e-01 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=1.4367e-01 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=9 pagerank=1.4240e-01 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull
</code></pre>
Then, the results of the fourth command, which filters the P graph and uses a large alpha:
<pre><code> [In]: run pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
INFO:root:rank=0 pagerank=7.0149e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=7.0149e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.0552e-01 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=3.1755e-02 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=2.2040e-02 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=1.6027e-02 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=1.6026e-02 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=1.6023e-02 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=1.6020e-02 url=www.lawfareblog.com/water-wars-song-oil-and-fire
INFO:root:rank=9 pagerank=1.6020e-02 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations</code></pre>

## Task 2: Implementing the Personalization Vector
**Part 1: Basic Power Method Implementation**
The personalization vector is alternative method of filtering via queries. The personalization vector determines which webpages are linked to most often from pages that are about the query itself. This is different than the previous <code>--search_query</code> argument which determines an overall PageRank, then filters for the highest ranked matches that are related to the query. To demonstrate this, we can compare the rankings of the same query when using the personalization vector versus the <code>--search_query</code> argument. 
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query=corona
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=6.3213e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=6.3211e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.4962e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=1.1626e-01 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=4 pagerank=1.1626e-01 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=5 pagerank=8.8833e-02 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=6 pagerank=8.5443e-02 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=7 pagerank=8.5443e-02 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=8 pagerank=7.1883e-02 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=9 pagerank=6.8968e-02 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response</code></pre>

<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query=corona
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=8.1320e-03 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=1 pagerank=7.7908e-03 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=2 pagerank=5.2262e-03 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=3 pagerank=3.9584e-03 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=4 pagerank=3.8114e-03 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=5 pagerank=3.3973e-03 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=6 pagerank=3.3633e-03 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus
INFO:root:rank=7 pagerank=3.3557e-03 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=8 pagerank=3.2160e-03 url=www.lawfareblog.com/congress-needs-coronavirus-failsafe-its-too-late
INFO:root:rank=9 pagerank=3.1036e-03 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus</code></pre>

**Part 2: Finding articles that are related, but don't mention our query**
The personalization vector is especially useful because it tracks which articles are most relevant to the query. This means that if we want to learn about the indirect effects our query may be having, we can still use the same personalization vector search query. Meanwhile, we can use the <code>--search_query</code> argument to present only those articles do not include the query itself. Because the minus sign will not be in the gensim model, these results pass through without searching for related words. I get the following results for 'corona':
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query=corona --search_query=-corona
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=6.3213e-01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=6.3211e-01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=1.4962e-01 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=8.8833e-02 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=4 pagerank=6.8562e-02 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=5 pagerank=6.5838e-02 url=www.lawfareblog.com/fault-lines-foreign-policy-quarantined
INFO:root:rank=6 pagerank=6.1389e-02 url=www.lawfareblog.com/limits-world-health-organization
INFO:root:rank=7 pagerank=5.5939e-02 url=www.lawfareblog.com/chinatalk-dispatches-shanghai-beijing-and-hong-kong
INFO:root:rank=8 pagerank=5.4060e-02 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=9 pagerank=4.9363e-02 url=www.lawfareblog.com/us-moves-dismiss-case-against-company-linked-ira-troll-farm</code></pre>
While these articles are mostly still related to the coronavirus, there are also some topics that are more tangential. For example, there's an important missile defense hearing ranked 9th that probably took place during the pandemic and was influenced by it, but is obviously not directly related to coronavirus.

**Part 3: Finding articles that are related, but don't mention our query (cont.)**
We can take a look at another example of the above, where articles are related, but do not explicitly mention the query. For 'iran', we get the following results:
<pre><code>[In]: run pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query=iran --search_query=-iran
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=3.9977e-01 url=www.lawfareblog.com/omphalos
INFO:root:rank=1 pagerank=2.4971e-01 url=www.lawfareblog.com/haftar-attacking-tripoli-us-needs-re-engage-libya
INFO:root:rank=2 pagerank=2.4398e-01 url=www.lawfareblog.com/cancellation-algerias-elections-opportunity-democratization
INFO:root:rank=3 pagerank=2.3989e-01 url=www.lawfareblog.com/yemen-houthi-strategy-has-promise-and-risk
INFO:root:rank=4 pagerank=2.3954e-01 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=5 pagerank=2.1294e-01 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=6 pagerank=1.5762e-01 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=7 pagerank=1.5762e-01 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=8 pagerank=1.1823e-01 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=9 pagerank=8.0923e-02 url=www.lawfareblog.com/senate-examines-threats-homeland
</code></pre>
I thought that this was an interesting extension especially to learn more about Iran's international influence. Iran has slowly been growing as a regional player in the Middle East and parts of North Africa  over the past few decades. That impact is pretty clear in the articles that are ranked highest, as there are articles about Algeria, Libya, and Israel. The appearance of a few articles about Iran's relationship with the US also speaks to the bombing and resulting tensions that arose in early 2020. These results indicate that http://www.lawfareblog.com/ considers American relations and regional Middle Eastern politics to be some of the most important topics related to Iran.

Here are a few more examples that highlight the updated ranking system:
These results are for "drones":
<pre><code>run pagerank.py --data=./lawfareblog.csv.gz --search_query=drones
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=4.5715e-03 url=www.lawfareblog.com/why-did-you-wait-moral-emptiness-and-drone-strikes
INFO:root:rank=1 pagerank=3.1107e-03 url=www.lawfareblog.com/dc-district-court-dismisses-journalists-drone-lawsuit
INFO:root:rank=2 pagerank=2.0231e-03 url=www.lawfareblog.com/revived-cia-drone-strike-program-comments-new-policy
INFO:root:rank=3 pagerank=1.9667e-03 url=www.lawfareblog.com/us-court-appeals-dc-circuit-dismisses-suit-over-us-drone-strike
INFO:root:rank=4 pagerank=1.7738e-03 url=www.lawfareblog.com/whats-point-charging-foreign-state-linked-hackers
INFO:root:rank=5 pagerank=1.2885e-03 url=www.lawfareblog.com/video-justice-department-announces-indictment-two-chinese-government-hackers
INFO:root:rank=6 pagerank=1.1973e-03 url=www.lawfareblog.com/daisy-chain-associated-forces-potential-use-force-niger-against-al-mourabitoun
INFO:root:rank=7 pagerank=1.1788e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications
INFO:root:rank=8 pagerank=1.1620e-03 url=www.lawfareblog.com/slaughterbots-and-other-anticipated-autonomous-weapons-problems
INFO:root:rank=9 pagerank=1.1506e-03 url=www.lawfareblog.com/dhss-joint-task-forces-next-chapter</code></pre>

These results are for "targets":
<pre><code>run pagerank.py --data=./lawfareblog.csv.gz --search_query=targets
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=6.0299e-03 url=www.lawfareblog.com/update-military-commissions-big-september-911-case
INFO:root:rank=1 pagerank=5.0168e-03 url=www.lawfareblog.com/update-military-commissions-continued-health-issues-recusal-motion-and-new-cell-al-iraqi
INFO:root:rank=2 pagerank=4.6573e-03 url=www.lawfareblog.com/targeting-al-baghdadi-and-selective-notification-congress-assessing-issues
INFO:root:rank=3 pagerank=3.2598e-03 url=www.lawfareblog.com/federal-judge-dismisses-military-commissions-defendants-8th-amendment-claim
INFO:root:rank=4 pagerank=3.2329e-03 url=www.lawfareblog.com/military-commissions-judge-rules-against-government-privilege-claim-nashiri-case
INFO:root:rank=5 pagerank=3.2204e-03 url=www.lawfareblog.com/week-military-commissions-98-session-kangaroo-lapel-pin-edition
INFO:root:rank=6 pagerank=3.1839e-03 url=www.lawfareblog.com/military-commission-judge-bars-government-using-defendants-statements-fbi-clean-teams-911-case
INFO:root:rank=7 pagerank=3.1759e-03 url=www.lawfareblog.com/last-week-military-commissions-defense-moves-classification-review-process
INFO:root:rank=8 pagerank=3.1533e-03 url=www.lawfareblog.com/court-military-commissions-review-upholds-life-sentence-al-bahlul
INFO:root:rank=9 pagerank=2.9309e-03 url=www.lawfareblog.com/summary-dc-circuit-vacates-military-judges-rulings-al-nashiri</code></pre>
