---
layout: post
title: Mining Massive Datasets (coursera)
categories:
- Programming
- Machine Learning
tags:
- Algorithm



---
* Overview to Data Mining
* MapReduce
* Finding Similar Items: Locality Sensitive Hashing
* Mining Data Streams
* Analysis of Large Graphs: Link Analysis, PageRank
* Frequent Itemset Mining & Association Rules
* Clustering
* Advertising on the Web
* Recommender Systems
* Analysis of Large Graphs
* Dimensionality Reduction: SVD&CUR

---

## Overview to Data Mining
What is Data Mining? It is a method to discover patterns and models that are Valid(hold on new data with some certainty), useful(should be possible to act on the item), unexpected(non-obvious to the system), understandable(humans should be able to interpret the pattern).

A risk with this technical is to "discover" patterns that are meaningless, statisticians call it [Bonferroni's principle](http://en.wikipedia.org/wiki/Bonferroni_correction).

Data mining overlaps with databases(large-scale data, simple queries), machine learning(small data, complex models), cs theory(algorithms).

## MapReduce
Much of the course is devoted to large scale computing for data mining. Map-reduce is an elegant way to distribute computation, invented by Google. Today, a standard architecture for analysing massive dataset is emerging. 1. cluster of commodity Linux nodes. 2. commodity network to connect them.

Map-Reduce takes care of:

* Partitioning the input data.
* Scheduling the execution across machines.
* Performing the group by key step
* handle machine failures
* managing required inter-machine communication

For a map-reduce algorithm:

* *Communication cost*=input file size + 2*(sum of the sizes of all files from Map processes to Reduce processes) + the sum of the output sizes of the Reduce processes
* *Elapsed communication cost* is the sum of the largest input + output for any map process, plus the same for any reduce process.
* Either the I/O(communication) or processing(computation) cost dominates.
* Elapsed cost is wall-clock time using parallelism

## Finding Similar Items: Locality Sensitive Hashing
Many problems can be expressed as finding "similar" sets:

* Pages with similar words
* Customers who purchased similar products
* Images with similar features

To define similarity, the **Jaccard similarity** of two sets is the size of their intersection divided by the size of their union. Jaccard distance: $$d(C_1, C_2) = 1-(C_1 \cap C_2) / (C_1 \cup C_2)$$

Main idea is, in pass I, take documents and hash them to buckets such that documents that are similar hash to the same bucket, in pass II, only compare documents that are candidates(they hashed to a same bucket). To compare N items, we need$$O(N^2)$$, we need O(N) comparisons to find similar documents.

To find similar Docs, We have three steps.

* 1.Shingling: Convert document to sets.
* 2.Min-Hashing: Convert large sets to short signatures, while preserving similarity.
* 3.Locality-Sensitive Hashing: focus on candidate pairs.

Step1. Shingling: converting document to sets is not a easy task. Simple methods include to treat the document as {set of words appearing in document} or {set of "important" words}. This naive approach doesn't work well because we need to account for ordering of words. **Shingles** is a different way.

A *k-shingle* for a document is a sequence of k tokens that appears in the doc for example. Document D=abcab, set of 2-shingles S1(D)={ab, bc, ca} we can even use multiset(bag), S2(D)={ab, bc, ca, ab}. To compress long shingles, we can **hash** them to (say)4 bytes, has the singles, h(S1(D))={1, 5, 7}. The working assumption here is: documents that have lots of shingles in common have similar text, even if the text appears in different order. In real case, k=5 is OK for short documents, and k=10 is better for long documents.


Step2. Why we need Minhash or LSH? Suppose that we need to compare 1 million, we need to compare N(N-1)/2. Here, we encode sets as bit vectors(0, 1 vector)and a big 0,1 table C can express the whole N sets. Then, the key idea is to "hash" each column C to a small signature h(C), such that:

* h(C) is small enough that the signature fits in RAM.
* sim(C1, C2) is the same as the "similarity" of signatures h(C1) and h(C2) (In fact, we just assume, if sim(C1, C2) is high, then with high probability, h(C1)=h(C2). if it is low, with high prob h(C1)!=h(C2))

Our goal is to find the hash function, clearly, the hash function depends on the similarity metric. For Jaccard similarity, there is a suitable hash funcion called **Min-Hashing**.

The rows of the boolean matrix permuted under *random permutation* $$\pi$$. The Min-Hashing defined as, $$h_\pi(C)$$=the index of first row in which column has value 1. we can use independent hash function (that is permutations) to create signature of a column. The *similarity of two signatures* is the fraction of the hash functions in which they agree. The signature of document C is 100 bytes(say we have 100 random permutations of the rows).

Try to prove: $$Pr[h_{\pi}(C_1) = h_{\pi}(C_2)] = sim(C_1, C_2)$$.

Step2. Locality Sensitive Hashing, focus on paris of signatures likely to be from similar documents. The general idea is still to find a function to tell whether x and y is a **candidate pair**. Big idea is to hash columns of signature matrix M several times. Arrange that (only) similar columns are likely to **hash to the same bucket** with high probability.

Divide matrix **M** into **b** bands of **r** rows and for each band, hash its portion of each column to a hash table with **k** buckets(make k as large as possible). Candidate column pairs are those that hash to the same bucket for >= 1 band. There are **enough buckets** that columns are unlikely to hash to same bucket unless they are identical in a particular band.
Let signatures of 100 integers, choose b=20 bands of r=5 integers and s=0.8. The probability that two similar doc doesn't hashed to at least one bucket is $$(1-s^r)^b$$ and it is about 0.00035. Consider the false positives, assume pair is about s=0.3 and the probability that they are identical in at least 1 band is $$1-(1-s^r)^b$$ which is about 0.0474.  LSH involves a tradeoff when choosing the b and r value and in fact the function is a S-curve. To summary, we use hashing to find candidate pairs of similarity >=s.

## Mining Data Streams
In many data mining situations, we do not know the entire data set in advance, we can think of the data as infinite and non-stationary(the distribution changes over time). Types of queries one wants on answer on a data stream.

- Filtering a data stream: Select elements with property x from the stream
- Counting distinct elements: Number of distinct elements in the last k elements of the stream.
- Estimating moments
- Finding frequent elements

These applications include, Google wants to know what queries are more frequent today than yesterday(Mining query streams). Yahoo wants to know which of its pages are getting an unusual number of hits in the past hour(Mining click streams), look for trending topics on Twitter, Facebook(Mining social network news feeds).Stochastic Gradient Descent(SGD) is an example of a stream algorithm, in machine learning we call this, online learning.

###Problem 1: Sampling a fixed proportion from a Data Stream.
In order to answer questions like *How often did a user run the same query in a ingle days*, we have space to store 1/10 of query stream. To sample 1/10, a naive solution is to generate a random integer in [0..9] for each query and store the query if the integer is 0. Assume, each user issues x queries once and d queries twice. If we sample by using this naive method, the proposed solution from the sample will be: x/10 singleton queries, d/100 pairs of duplicates, 18d/100 appear exactly once, answer is $$\frac{d}{10x+19d}$$. But the true answer is d/(x+d).

In fact, we need to pick 1/10 users and take all their searches in the sample by using hash function to hash their name into 10 buckets.

###Problem 2: Maintaining a fixed-size sample.
Suppose we need to maintain a random sample S with size s and at time n we have seen n items. Each item in the sample with equal prob. s/n. The algorithm is quite direct and can be easily proved by mathmatical induction.

- 1. Store all the first s elements of the stream to S.
- 2. Suppose we have seen n-1 elements, and now the nth element arrives, with s/n, keep the nth element, else discard it. If we pick the nth element, then replace one of s elements in S, picked uniformly at random.

###Problem 3: Queries over a (long) Sliding Window
A useful model of stream processing is that queries are about a **window** of length N, the N most recent elements received. N may be so large that the data cannot be stored in memory, or even on disk. Amazon example: for every product we keep 0/1 stream of whether that product was sold in the n-th transaction. We want answer queries, how many times have we sold X in the last k sales. Given a stream of 1 and 1, be prepared to answer queries of the form How many 1s are in the last k bits? where k<=N. To get an accurate, we have to store the most recent N bits. What if we cannot afford to store N bits? An approximate answer is acceptable.

- An simple attempt, is to use S to store number of 1s from the beginning of the stream and Z to store zero. In last N, it is about $$\frac{NS}{S+Z}$$. But what if stream is non-uniform ?

- DGIM method is to summarize blocks with specific number of 1s. A bucket in the DGIM method is a record consisting of **the timestamp of its end** and **the number of 1s between its beginning and end**. Buckets disappear when their end-time is>N time units in the past. Either one or two buckets with the same power-of-2-number of 1s(if there are more than 2, we combine them).

To estimate the number of 1s in the most recent N bits, first we sum the sizes of all buckets but the last, second add half the size of the last bucket. Remember, we do not know how many 1s of the last bucket are still within the wanted window. An extensions is for stream of positive integers and we want the sum of the last k elements, we can treat m bits of each integer(data in the stream) as a separate stream.

###Filtering Data Streams.
Each element of data stream is a tuple and given a list of keys S, we want to determine which tuples of stream are in S. An obvious solution is hash table, but suppose we do not have enough memory to store all of S in a hash table. Real applications include, "You are collecting lots of messages and people express interest in certain sets of keywords, determine whether each message matches user's interest." A first cut solution is,

- Given a set of keys S that we want to filter. Create **bit array** B of n bits, initially all 0s, choose a hash function h with range [0, n), hash each s from S to one of n buckets, and set that bit to 1, e.g. B[h(s)]=1. Hash each element of a of the stream and output only those that hash to bit that was set to 1, e.g. output a if B[h(a)]==1.

If we have m keys in S, and n bits for the bit array, the probability for each bit get at least one hashed value is $$1-(1-1/n)^m$$ which is about $$1-\exp^{-m/n}$$. Bloom filter is basic the same and it just use k independent hash function rather than 1 hash function. The fraction of 1s is $$(1-\exp^{-km/n})$$, but we have k independent hash functions, we only output x if all k hash x to a bucket of value 1. The false **positive** prob is $$(1-\exp^{-km/n})^k$$. Bloom filters guarantee no false negatives and use limited memory. It is great for pre-processing before more expensive checks and hash function computations can be parallelized.
### Counting Distinct Elements
Maintain a count of the number of distinct elements seen so far. Real examples include "How many distinct products have we sold in the last week", "How many different Web pages does each customer request in a week?".  Problem is "what if we do not have space to maintain the set of elements seen so far", we need to find a way to estimate the count in an unbiased way.

- Flajolet-Martin Approach: pick a hash function h that maps each of the N elements to at least \[log_2N\] bits. For each stream element a, let r(a) be the number of trailing 0s in h(a). Record R=the maximum r(a) seen, \(R=maxr(a)\) over all the items a seen so far. Estimate number of distinct elements \(2^R\). The probability of NOT seeing r trailing 0s is \[(1-2^(-r))^m\], so that when \[m<<2^r\], it is 1, when \(m>>2^r\), it tends to 0. Thus, \(2^R\) will almost always be around m. But actually \(E[2^R]\) is actually infinite because probability halves when R to R+1, but value doubles. We should work using many hash functions and getting many samples of \(R_i\), but how to combine \(R_i\)? A good choice is partition samples $R_i$ into small groups, then take the median of groups and take the average of the medians. In this way, we can avoid these situation, "Use average. What if one very large value $2^{R_i}$", "Use median. All estimates are a power of 2"

### Computing Moments
Suppose a stream has elements chosen from a set A of N values, $$m_i$$ be the number of times value i occurs in the stream, the $$k_{th}$$ moment is $$\sum_{i \in A}(m_i)^k$$. 0th moment is the number of distinct elements, 1st moment is the count of the number of element, 2nd moment is a measure of how uneven the distribution is.

- AMS method, we pick and keep track of many variables X, for each X, X.el corresponds to the item i, X.val corresponds to the count of item i. This requires a count in main memory, so number of Xs is limited. Assume stream has length n, pick random time t(t<n) to start, then we maintain count c(X.val=c) of the number of *i*s in the stream, then the 2nd can be written as S = n(2c-1), note that we have multiple *X*s, $$S=\frac{\sum_{j}^k f(X_j)}{k}$$. The expectation can be written as $$E[f(X)]=\frac{1}{n}\sum_{t=1}^n n(2c_t-1)$$, according to the randomize technique for time t. We have the second moment in expectation. Generally for kth moment, we use $$n(c^k-(c-1)^k)$$.

In practice, compute f(X) as many X as possible, then average them in groups, at last take median of averages. But in real case, streams never end so n is a variable - the number of inputs seen so far. The variables X have n as a factor, keep n separately; just hold the count in X. Suppose we can only store k counts, we must throw some Xs out as time goes on(here, we use fixed-size sampling to pick samples and store them in these Xs).

### Counting Itemsets
Given a stream, which items appear more than s times in the window? A possible solution is to think of the stream of baskets as one binary stream per item and use DGIM to estimate counts of 1s for all items. But the problem is, it is only approximate and number of itemsets is way too big.

- Exponentially Decaying Windows. A heuristic for selecting likely frequent item(sets). Instead of computing the raw count in last N elements, compute a **smooth aggregation** over the whole stream. If stream is $$a_1, a_2, ...$$, take the answer at time t to be: $$\sum_{i=1}^t a_i (1-c)^{t-i}$$, when new $$a_{t+1}$$ arrives, multiply current sum by (1-c) and add $$a_{t+1}$$. The intuition is as time goes, the early item in the stream weight less than new-comers. If each $$a_i$$ is an "item", we can compute **characteristic function** of each possible item x as an Exponentially Decaying Windows.
- Important property: Sum over all weights $$\sum_t (1-c)^t$$ is 1/c, so that if we just need item whose weight is bigger than 1/2, we will only find at most 2/c such items.

## Analysis of Large Graphs: Link Analysis, PageRank
![facebook](/png/facebook.png?raw=true) where is China and Russia? All web pages are not equally important. There is large diversity in the web-graph node connectivity, let's rank the pages by the link structure.

### PageRank: The "Flow" Formulation
- Idea: links as votes, page is more important if it has more links, in-coming links? Out-going links?
- Are all in-links are equal? Links from important pages count more, this is a recursive question.
![pagerank](/png/pagerank.png?raw=true)
A page is import if it is pointed to by other important pages, define a "rank" $$r_j$$ for page j, $$r_j = \sum_{i \rightarrow j} \frac{r_i}{d_i}$$, $$d_i is the out degree of node i$$. There is no unique solution, if there is no other constrain. Additional constraint forces uniqueness, for example $$\sum_i r_i=1$$. Gaussian elimination method works for small graph, but we need a better method for large web-size graphs. Use matrix formulation, define stochastic adjacency matrix M, and the flow equation can be written as r=Mr. **Rank vector** is an *eigenvector* of the stochastic web matrix M. We can use Power iteration to solve for r efficiently. To prove that the iteration works, just expand the initial value $$r^(0)$$ with the basis consist by eigenvector. Then, write down the $$M^k r^(0)$$ and prove that $$M^k r^(0)$$ is basically the same as $$c_1(\lambda_{1}^k x_1)$$, note that if $$c_1=0$$ the method will not converge. So, r is a stationary distribution for the random walk. For graphs that satisfy certain conditions, the stationary distribution is unique and eventually will be reached no matter what the initial probability distribution.

Problems include 1. dead ends, some pages are dead ends(have no out-links). 2. Spider traps(all out-links are within the group).
-The Google solution for spider traps is at each step, the random surfer has two options, with probability $$\beta$$ follow a link at random, with prob $$1-\beta$$, jump to some random page. Common value for $$\beta$$ is 0.8 to 0.9. The solution to dead ends is the same. Follow random teleport links with probability 1.0 from dead-ends.

In real cases, M is so big with $$N^2$$ elements, and is a sparse matrix. We need encode sparse matrix using only nonzero entries. Some other problems with Page Rank is measure generic popularity of a page (Topic-Specific PageRank), susceptible to link spam(artificial link topographies created in order to boost page rank) and so on.

- Topic-Specific PageRank. Instead of generic popularity, can we measure popularity within a topic. Goal is to evaluate web pages not just according to their popularity but by how close they are to a particular topic. Idea is to bias the random walk by using a topic-specific set of relevant pages(teleport set). How to create different PageRanks for different topics. The 16 top-level categories include arts, business, sports,... To determine which topic ranking to use, 1.User can pick from a menu, 2. Classify query into a topic. 3. Can use the context of the query. 4. Users context, e.g. user's bookmarks...

- Application to Measuring Proximity in Graphs. Shortest path is not good because two nodes may have multi-faceted relationships. Network flow is not good because it does not punish long paths. SimRank idea: random walks from a fixed node on k-partite graphs, and is similar to topic specific pagerank from the node with teleport set S={the node u}.

-TrustRank: Combating the Web Spam. Approximately 10-15% of web pages are spam. How to make your page appear to be about movies? 1. Add the word 1,000 times and set color to the background color. 2. search movie on you target search engine, and copy the result into your page, make it "invisible". There and similar techniques are term spam. *Google's solution is to believe what people say about you, rather what you say about yourself.*  Sometimes, it does not work. **Miserable failure** once linked to Bush in Google. Link farms is one technique that spammers use to fool Google by getting as many link as possible to target page t. Just like a farm of web page link to the target t. To combat link spam, detection and blacklisting of structures that look like spam farms is one method. TrustRank = topic-specific PageRank with a teleport set of trusted pages, include edu domains, gov domains. Basic principle: it is rare for a "nice" page to point to a "spam" page. We can sample a set of seed pages from the web or even have an oracle(human) to identify the nice page and add to the seed pages, this is quite expensive. One solution is consider $$r_p$$ PageRank of page p, $$r_p^+$$ PageRank of p with teleport into **trusted** pages only. Page with high $$p=\frac{r_p-r_p^+}{r_p} are spam.

###HITS(Hypertext-induced Topic Selection)
HITS is measure of importance of pages or documents, similar to PageRank. Say we want to find good newspapers, don't just find newspapers. Find "experts"- people who link in a coordinated way to good newspapers. Each page has 2 scores, **hub** total sum of votes of authorities pointed to, **authority** total sum of votes coming from experts.

Interesting pages fall into two classes. 1. Authorities are pages containing useful information, including newspaper home pages, course home pages and home pages of auto manufacturers. 2. Hubs are pages that link to authorities, including list of newspapers, course bulletin and list of us auto manufacturers.

Model using two scores for each node, **Hub** with h, **Authority** with a. HITS converges to a single stable point. With adjacency matrix A with elements $$A_{ij}$$ if $$i \rightarrow j$$, 0 otherwise, we have $$h=Aa$$ and $$a=A^th$$. Under reasonable assumptions about A, HITS converges to $$h^*$$ which is the principal eigenvector of matrix $$AA^T$$and $$a^*$$ which is the principal eigenvector of matrix $$A^TA$$. 

## Frequent Itemset Mining & Association Rules
Supermarket shelf management- Market-basket model: goal is to identify items that are bought together by sufficiently many customers. A classic rule: If someone buys diaper and milk, then he/she is likely to buy beer. Don't be surprised if you find six-packs next to diapers. Companys like Amazon want to discover **association rules**. People who bought {x,y,z} tend to buy {v,w}. **Confidence** of this association rule is the probability of j given set $$I={i_1,...,i_k}$$, can be written as  $$conf(I\rightarrow j)=\frac{support(I\cup j)}{support(I)}$$. Not all high-confidence rules are interesting. The rule X to milk may have high confidence for many X, because milk is just purchased very often. **Interest** of an association can be defined as $$Interest(I\rightarrow j)=conf(I\rightarrow j)-Pr[j]$$. Problem here is to *find all association rules with support >= s and confidence >= c. The support of an association the support of the set of items on the left side. In fact, the hard part is to find frequent itemsets $${i_1, i_2, ...i_k}$$ and $${i_1, i_2, ..., i_k, j}$$. Mining Association Rules can be divided into 2 steps.

- Find all frequent itemsets I.
- Rule generation, for every subset A of I, generate a rule $$A\rightarrow I\A$$. Compute rule confidence, and pay attention that if A,B,C to D is below confidence, so is A,B to C,D.

To reduce the number of rules, we can post-process them and only output **Maximal frequent itemsets** which means no immediate superset is frequent, or closed itemsets with no immediate superset has the same count.

### Finding Frequent Itemsets
Naive approach is to read file once, counting in main memory for each pair. For n items, generate n(n-1)/2. It may fail if the number exceeds main memory which surely happen. Two approaches include,

- Approach 1 is to count all pairs using a matrix, only need 4 bytes per pair, if item and  integers are 4 bytes.
- Approach 2 is to keep a table of triples [i,j,c] for the count of the pair of items {i,j} is c, we need 12 bytes for pairs with count>0.

Problem is if we have too many items so the pairs do not fit into memory. Can we do better? A two-pass approach called **A-Priori** limits the need for main memory. The key idea is, If a set of items I appears at least s times, so does every subset J of I.

- Pass 1: read baskets and count in main memory the occurrences of each individual item. Items that appear >= s times are the frequent items.
- Pass 2: Read baskets again and count in main memory **only** those pairs where both elements are frequent.

**PCY Algorithm**. In pass 1 of **A-Priori**, most memory is idle. In addition to item counts, keep a count for each bucket into which pairs of items are hashed, for each bucket just keep the count, not the actual pairs that hash to the bucket(we have no enough memory for this). If a bucket contains a frequent pair then the bucket is surely frequent, however, even without any frequent pair, a bucket can still be frequent. In pass 2, only count pairs that hash to frequent buckets. **PCY Algorithm** replace the buckets by a bit-vector, 1 means that the bucket count exceeded the support s. We can even do multistage algorithm. After pass 1 of PCY, rehash only those pairs that qualify for Pass 2 of PCY(using an independent hash function).

### Frequent Itemsets in <= 2 Passes.
**A-Priori, PCY**, etc., take k passes to find frequent itemsets of size k. Use 2 or fewer passes for all sizes, but may miss some frequent itemsets.

- Random Sampling is to take a random sample of the market baskets. Run a-priori or one of its improvements in main memory and at same time reduce support threshold. You may verify that candidate pairs are truly frequent in second pass. But you don't catch sets frequent in the whole but not in the sample
-  SON Algorithm is to repeatedly read small subsets of the baskets into main memory and run in-memory algorithm to find all frequent itemsets(just process the entire file in memory-sized chunks). An itemset become a candidate if it is found to be frequent in any one or more subsets of the baskets. *Key idea* is an itemset cannot be frequent in the entire set unless it is frequent in at least one subset. SON lends itself to distributed data mining.

## Clustering
Clustering is hard because many application involve not 2, but 10 or 10,000 dimensions. Almost all pairs of points are at about the same distance. Methods of Clustering includ hierarchical and point assignment.
### Hierarchical Clustering
Key operation is to repeatedly combine two nearest clusters. To represent a cluster of many points, we can use average of its points as **centroid** for euclidean case. To measure "nearness" of clusters, we may just consider distances of centroids. For non-euclidean case, there is no average for many points. Consider **clustroid** point "closest" to other points and when computing inter-cluster distances, treat clustroid as centroid. Still, we may explain closest point to other points in several ways.

- smallest maximum distance to other points
- smallest average distance to other points
- smallest sum of squares of distances to other points.

How to define the nearness of clusters? 1. Interculster distance = minimum of the distances between any two points, one from each cluster. 2. Pick a notion of "cohesion" of clusters, e.g. maximum distance from the clustroid. Merge clusters whose union is most cohesive. Other methods to get cohesion include "use diameter of the merged cluster=maximum distance between points in the cluster", "average distance between points" and "use a density-based approach, take the diameter or avg. distance then divide by the number of points in the cluster". By using priority queue, we can reduce time to $$O(N^2logN)$$, too expensive for big datasets.

### k-means Algorithm
- Assume Euclidean space, start by picking k and initialize clusters by picking one at random, then k-1 other points, each as far as possible from the previous points
- For each point, place it in the cluster whose current centroid it is nearest.
- After all are assigned, update the centroids of the k clusters, with its mean in the each cluster.
- Reassign all points to their closet centroid.
- Repeat until convergence.

How to select k? Try different k, looking at the change in the average distance to centroid as k increases.

###BFR [Bradley-Fayyad-Reina] Algorithm
It is a variant of k-means designed to handle large data sets.
Assume that clusters are normally distributed around a centroid in a Euclidean space. 3 sets of points which we keep track of. Discard set(DS), points close enough to a centroid to be summarized. Compression set(CS) groups of points that are close together but not close to any existing centroid. Retained set(RS), isolated points waiting to be assigned to a compression set. For each cluster, the DS is summarized by the number of points, the vector SUM, and the vector SUMSQ. 2d+1 values represent any size cluster, d is the number of dimensions.

Find those points that are sufficiently close to a cluster centroid and add those points to that cluster and the DS. Use any main-memory clustering algorithm to cluster the remaining points and the old RS. Adjust statistics of the clusters to account for the new points. Consider merging compressed sets in the CS. If this is the last round, merge all compressed sets in the CS and all RS points into their nearest cluster.

How close is close enough? Mahalanobis distance is defined as $$\sqrt{\sum_{i=1}^d (\frac{x_i-c_i}{\sigma_i})^2}$$. Accept a point for a cluster if its M.D.< some value, like 2. Should 2 CS subclusters be combined? Compute the variance of the combined subcluster, combine if the combined variance is below some threshold.

###The CURE Algorithm
BFR assumes clusters are normally distributed so that it will result. CURE assume a Euclidean distance and allow clusters to be any shape. Key idea is to use a collection of representative points to represent clusters.

- Pass1, pick a random sample of points that fit in main memory.
- Pass1, initial clusters.
- Pass1, pick representative points. For each cluster, pick a sample of points as dispersed as possible. From the sample, pick representatives by moving them (say) 20% toward the centroid of the cluster.
- Pass2, rescan the whole dataset and place it in the "closet cluster".

## Advertising on the Web
- Classic model of algorithms. You get to see the entire input, then compute some function of it, for example gradient descent algorithm, in this context "offline algorithm".
- Online Algorithm. You get to see the input one piece at a time and to make irrevocable decisions along the way.

### Online Bipartite Matching
Problem is to find a maximum matching for a given bipartite graph. Initially, we are given the set boys, in each round, one girl's choices are revealed(she may have several choices). At that time, we have to decide to either pair the girl with a boy or do not pair the girl with any boy. Another example is to assign tasks to servers. Greedy algorithm for the online graph is to pair the new girl with any eligible boy. **Competitive ratio** is defined as $$min_{inputs}(M_{greedy}/M_{opt})$$, what is greedy's worst performance over all possible inputs I. It can be proved that the **ratio** is bigger than 1/2.

###Performance-based Advertising
Introduced by overture around 2000, advertisers bid on search keywords, the highest bidder's ad is shown. Performance-based advertising is a multi-billion-dollar industry. The problem is what ads to show for a given query? To maximize search engine's revenues, instead of raw bids, use the "expected revenuse per click"(i.e. Bid*CTR), CTR is short for click through rate. The complications include budget and CTR of an ad is unknown. Each advertiser has a limited budget, search engine guarantees that advertiser will not be charged more than their daily budget. To solve the limited budget problem, several methods are considered for simplified environment. We assume only 1 ad shown for each query, all advertisers have same budget, all ads are equally likely to be clicked and value of each ad is the same.

- Greedy Algorithm. For a query pick any advertiser who has bid 1 for that query. It is deterministic.
- BALANCE algorithm. Pick the advertiser with the largest unspent budget. Worst competitive ration is 1-1/e. Worst case for BALANCE, N advertisers, $$A_1, A_2,... A_N$$, each with B>N, N*B queries appear in N rounds of B queries each. Sum of allocation for $$A_i$$ is $$\sum_{i=1}^{k-1}\frac{B}{N-(i-1)}$$. After k=N(1-1/e) rounds, we cannot allocate a query to any advertiser. The competitive ratio=1-1/e.
- Generalized BALANCE for different budget is to balance fraction of budget left over. It can have the same 1-1/e ratio for it.

## Recommender Systems
[How **Into Thin Air** made **Touching the Void** a bestseller](http://www.wired.com/wired/archive/12.10/tail.html) Types of recommendations include: editorial and hand curated(list of favorites), simple aggregates(top 10, most popular), and *tailored to individual users*(Amazon, Netflix,...). Key problems are 1. Gathering known ratings for matrix. 2. Extrapolate unknown ratings from the known ones, mainly interested in high unknown ratings. 3. Evaluating extrapolation methods(How to measure performance of your method).

###Content-based Recommender Systems.
Main idea is to recommend items to customer x similar to previous items rated highly by x. For each item, create an item profile. Profile is a set of features, movies: author, title, actor, director,.... Text: set of "important" words in document. Use **TF-IDF**(Term frequency * Inverse Doc Frequency) to pick important features. Pros and cons of content-based approach include:

- No need for data on other users.
- Able to recommend to users with unique tastes.
- Able to recommend new&unpopular items.
- Able to provide explanations.
- Finding features is hard.
- How to build a new user profile.
- Overspecialization, never recommends items outside user's content profile, people may have multiple interests, and unable to exploit quality judgments of other users.

###Collaborative Filtering
Consider user x, find set N of other users whose ratings are "similar" to x's ratings, estimate x's ratings based on ratings of users in N. Cosine similarity measure is better than Jaccard similarity measure(which ignore the value of the rating and is often used for measuring set).
This is a user-user method.

### Item-Item
To estimate the rate for given item, we find the similar items first and use the similarity value as weight to calculate the weighted mean. The problem is the baseline for each movie may be different, so that we should minus the mean for each movie first. We use Pearson correlation as similarity.  The algorithm will be:

- Define similarity $$s_{ij}$$ of items i and j
- Select k nearest neighbors N(i; x), Items most similar to i, that were rated by x
- Estimate rating $$r_{xi}$$ as the weighted average $$r_{xi}=b_{xi}+\frac{\sum_{j \in N(i;x) S_{ij}(r_{xj}-b_{xj})}}{\sum_{j \in N(i;x)S_{ij}}}$$ and $$b_{xi}=\mu+b_x+b_i$$, $$\mu$$ is overall mean movie rating, $$b_{x}$$ is rating deviation of user x, $$b_i$$ is the rating deviation of movie i.

In practice, it has been observed that **item-item** often works better than user-user. Why? Items are simpler, users have multiple tastes.

### Latent Factor Models
"SVD" on dataset: $$R=QR^T$$, for now assume we can approximate the rating matrix R as a product of two "thin" matrix(R has missing values which we would like to estimate). These ratings can be estimated by the products of factors. We know that SVD gives minimum reconstruction error. Our goal is to find P and Q such that $$min \sum_{(i,x)\in R}(r_{xi}-q_{i}p_{x})^2$$. Here we want to minimize SSE for unseen *test* data so we minimize SSE on *training* data. Large k(# of factors) capture all the signals, but SSE on *test* data may rise for increasing k. This is classical example of overfitting. To solve this, we introduce regularization for example $$\lambda_1 \sum_x \|p_x\|^2+\lambda_2 \sum_i \|q_i\|^2$$. We can use stochastic gradient descent(SGD) to optimize the formula.

### Extending Latent Factor Model to Include Biases
We have expectations on the rating by user x of movie i, even without estimating x's attitude towards movies like i. Use $$\mu, b_x, b_i$$ donate overall mean rating, bias for user x, and bias for movie i. we estimate $$r_{xi}=\mu + b_x + b_i + q_ip_x$$. We can add regularization for $$b_x$$ and $$b_i$$ as we do before. In real cases, for example Netflix prize, sudden rise in the average movie rating(due to improvements in Netflix, GUI improvements, meaning of rating change). Older movies are just inherently better than newer ones. These situation are called temporal biases of users which means these $$b$$ depend on time as b(t).


## Analysis of Large Graphs
We often think of networks being organized into *modules, cluster, communities*. Find new market by partitioning the query-to-advertiser graph. For example, muji(2015) will open a huge shop in ChengDu due to the reason that data from TaoBao shows many customers from southwest of China buy muji products and there is no shop there. Discovering social circles, circles of trust in Twitter and Facebook.

###Community Detection
We will work with undirected(unweighted) networks. Method 1 is to define edge betweenness as *number of shortest paths passing over the edge*. Edge between two community tends to have higher betweenness. **Girvan-Newman** algorithm is to remove the edge with highest between and then recompute betweenness. Using this method, we need to resolve 2 questions.

- 1. How to compute betweenness? Breath first search starting from point A, count the number of shortest paths from A to other nodes. Split the flow up based on the parent value. If there are multiple paths, count them fractionally.  e.g. if A to B is 1, A to C is 3, and C,B to D is 4, then the betweenness between B,D is 1/4 and C,D is 3/4. 
- 2. How to select the number of clusters? Need a measure of how well a network is partitioned into communities, given a partitioning of the network into groups s, Modularity Q is proportion to $$\sum_{s \in S}$$ \[(#edges within group s)-(expected  #edges within group s)\], the expected number of edges between nodes i and j of degrees $$k_i$$ and $$k_j$$ equals to $$\frac{k_ik_j}{2m}$$. To normalize the Q, $$Q(G,S) = \frac{1}{2m}\sum_{s \in S}\sum_{i \in s}\sum_{j \in s}(A_{ij}-\frac{k_ik_j}{2m})$$

###Spectral Clustering
Consider Bi-partitioning task, divide vertices into two groups. How can we define a good partition? How can we efficiently identify such a partition? A good partition should maximize the number of within-group connections and minimize the number of between-group connections. The edge cut can be expressed as cut(A,B)=$$\sum_{i\in A, j \in B} W_{ij}$$. Can we just minimum the cut? It only considers external cluster connections and does not consider internal cluster connectivity. Normalized-cut[Shi-Malik 97] try to consider the within group connections, $$ncut(A,B)=\frac{cut(A,B)}{vol(A)}+\frac{cut(A,B)}{vol(B)}$$. But computing optimal cut is NP-hard, using this criteria.

### Overlapping Communities
When the edge density in the overlaps is higher, it is the structure of community overlaps. Plan of attack include, 1 given a model, we generate the network, 2 given a network, find the "best" model. Community-Affiliation Graph generative model $$B(V,C,M,{p_c})$$ for graphs, nodes V, Communities C, Memberships M, each community c has single probability $$p_c$$ to connect two nodes within the community. How do we detect communities with AGM? Use the MLE! Fitting the original model is too hard, let's change the model.

From AGM to BigCLAM, assume $$F_{\mu A}$$ as the membership strength of node $$\mu$$ to community A. Each community A links nodes independently $$P_A(\mu, v)=1-exp(-F_{\mu A}F_{vA})$$. Construct big matrix F, its row to denote nodes and its column donates communities. Then at least one common community C links the nodes with probability $$P(\mu,v)=1-\prod_C(1-P_c(\mu,v))$$, the liklihood function for a network G(V,E) can be expressed as $$\prod_{(u,v)\in E}P(u,v)\prod_{(u,v)\not\in E}(1-P(u,v))$$ and we notice $$\sum_{v\not\in N(u)} F_v=(\sum_v F_v-F_u-\sum_{v\in N(u)} F_v)$$(BigCLAM 2.0), computing the left hand size takes **linear** time if we cache $$\sum_v F_v$$

Extension: Directed AGM. F:out-going community memberships. H:in-coming community memberships. Edge prob: $$P(u,v)=1-exp(-F_u H_v^T)$$ Optimize the likelihood as before.

## Dimensionality Reduction: SVD&CUR
Assumption: Data lies on or near a low d-dimensional subspace. Image matrix A is 3*3, but its rank is just 2. We can reduce the "dimensionality" of A to its rank. Basically, all data in 3 dimensional space lies in a plane. Reduce dimensions can discover hidden correlations/topics, remove redundant and noisy features, help interpret and visualize, and easily store and process dataset.
### SVD
It is always possible to decompose a real matrix A into $$A=U\Sigma V^T$$, where these matrix are unique, with $$\Sigma$$ diagonal and rank*rank.

- Interpretation 1, movies, users and concepts case, U: user to concept similarity matrix, V: movie-concept similarity matrix, $$\Sigma$$ its diagonal elements are strength of each concept.
- SVD is to pick "new" and less coordinate to reconstruct "old" coordinates. Try to minimum reconstruction error. $$U\Sigma$$ of $$A=U\Sigma V^T$$ gives the coordinates of the points in the projection axis. Interpretation 2, is to set smallest singular values to zero. How many $$\sigma$$ to keep? Keep 80%-90% of 'energy'.

###CUR Decomposition
Express A as a product of matrix C,U,R and make $$\|A-CUR\|_F$$ Frobenius norm  small. We pick rows of A to be rows of R, and prove the F norm value is not big. Since the basis vectors are actual columns and rows, it is easy to interpret and it is sparse basis.  But columns of large norms will be sampled many times.













