---
layout: post
title:  "Thoughts on Price Discrimination and Experimentation"
date:   2026-01-31
last_modified_at: 2026-01-31
categories: [economics]
pinned: false
---

&nbsp;

In light of some companies catching heat over price experimentation, I’ve seen quite a few takes from data scientists and economists. Many of these takes have lamented that it’s such a shame - not just because the loss of such amazing data (which, yes, consumer level data on price-quantity? Yeah, that’s a dream), but because price discrimination is welfare improving. 

&nbsp;

Although I can forgive an arbitrary data scientist not trained in the standard microeconomic theory PhD economists go through for this faulty claim, its absolutely unforgivable in my opinion for PhD economists to blindly argue that price discrimination is welfare improving. Depending on the nature of the price discrimination, *it can be welfare improving.* However, save for one case where it is always welfare improving, it is not a theoretical guarantee that price discrimination will improve welfare. Moreover, in the one case where welfare is guaranteed to improve, that improvement comes at the cost of all consumer surplus - the entire welfare gain is due to an increase in profits.

&nbsp;

In this post, I review the basic theory of *perfect price discrimination*. Without a doubt, many firms in the age of “AI” pricing are using price experimentation as a means to approximate this mythical ideal. As we will see, for all the talk of wanting to make consumers better off with “personalization”, basic theory quickly reveals that firms likely dream of approximating this case due to the profits to be reaped. After reviewing the theory, I provide a simple simulated example that confirms the predictions. 

&nbsp;

## Perfect Price Discrimination: A Welfare Utopia...For the Firm

Let $$q=D(p)$$ denote the demand curve for some product. We will assume typical properties -- including $$D'(p) \leq 0$$. In addition, we will denote the choke price (the price at which $$D(p) = 0$$) as $$\bar{M}$$. Note that depending on functional form, $$\bar{M}$$ may be “equal” to $$\infty$$.

&nbsp;

Under the classical model in which the firm can only charge a single price, the firm sets $$p$$ such that $$D’(p) = c$$ where $$c$$ is marginal cost. Under our simplifying assumption that $$c = 0$$, we will have that $$D’(p) = 0$$ at the firm’s optimal price - which we will denote as $$p^*$$. The firm’s profit, $$\pi$$, will be given by 

$$\pi^{sp} = p^*D(p^{*})$$

and consumer surplus, $$CS$$, will be given by 

$$CS^{sp} = \lim_{M\rightarrow \bar{M}}\int_{p^{*}}^{M} D(p)dp.$$ 

Consequently, total welfare, $$W$$, is given by 

$$W^{sp} = p^{*} D(p^{*}) + \lim_{M\rightarrow \bar{M}}\int_{p^{*}}^{M} D(p)dp.$$

Now suppose that the firm is able to perfectly price discriminate. In the case of perfect price discrimination, the firm charges each consumer exactly their willingness to pay. Consequently, the marginal revenue curve will be equal to $$D(p)$$. Of course the firm will never want to set $$p < c$$ -- in this case, the firm will never want to set $$p < 0$$. Hence, the lowest price charged is 0. Any price above $$c$$ will be desirable. Hence, the highest price is $$p$$ such that $$\bar{p} = \lim_{M\rightarrow \bar{M}} M$$. 

&nbsp;

As a result, the firm’s profit will be expressed as 

$$\pi^{ppd}=\lim_{M\rightarrow \bar{M}} \int_{0}^M D(p)dp$$ 

Moreover, since each consumer pays exactly their willingness to pay, it must be that consumer surplus is 0. Consequently, welfare is 

$$W^{ppd} = \pi^{ppd} + 0.$$

Now, let $p^{*}$ refer to the optimal price under the classic single price model above. Then, note that we can write

$$\pi^{ppd}=\lim_{M\rightarrow \bar{M}} \int_{0}^{M} D(p)dp = \int_{0}^{p^{*}} D(p)dp + \lim_{M\rightarrow \bar{M}} \int_{p^{*}}^M D(p)dp.$$ 

Comparing to $$W^{sp}$$, note that

$$W^{ppd} - W^{sp} = \int_{0}^{p^{*}} D(p)dp - p^{\*} D(p^{*})$$

Suppose that $$\int_{0}^{p^{*}} D(p)dp \leq p^{*}D(p^{*})$$. Since $$D’(p) \leq 0$$ (by assumption), it must be that $$D(p) \geq D(p^{*})$$ for all $$p \leq p^{*}$$. The only way this can be true is if $$D(p) = D(p^{*})$ on $[0, p^{*}]$$. But, this would imply $$pD(p) = pD(p^{*})$$ is strictly increasing in $$p$$. Consequently, $$p^{*}$$ cannot be the maximizer, which is a contradiction. Hence, it must be that $$\int_{0}^{p^{*}} D(p)dp > p^{*}D(p^{*})$$, meaning, $$W^{ppd} > W^{sp}$$.

&nbsp;

Moreover, note that at the lowest price of 0, $$q=\lim_{p\rightarrow 0} D(p) = \bar{M}$$. Under the ideal of perfect competition, the optimal price is $$p$$ such that $$p=c$$. In our case, this would imply $$p=0$$ under perfect competition. It follows then that the perfect competition result will have $$q = \lim_{p\rightarrow 0} D(p) = \bar{M}$$. It is easy to see profit will be 0 in this case and consumer surplus will be given by $$\lim_{M\rightarrow \bar{M}} \int_{0}^M D(p)dp$$. Hence, under single price perfect competition, 

$$W^{pc} = \lim_{M\rightarrow \bar{M}}\int_{0}^M D(p)dp.$$

In summary, we have the following result:

$$W^{pc} = \pi^{ppd} = \lim_{M\rightarrow \bar{M}} \int_{0}^M D(p)dp.$$

What does this all mean? In words, perfect price discrimination achieves the ideal welfare that perfect competition yields. However, it does so by permitting the firm to extract *all* welfare -- consumers are left perfectly indifferent between buying and not buying. In short, **perfect price discrimination allows the firm to extract the maximum amount of profit that is theoretically possible**.[^1]

&nbsp;

## Price Experimentation: Personalization...All in the Name of Profit

The problem with perfect price discrimination is that it requires the firm to know each consumers’ willingness to pay. For most of human history, this has been the hurdle preventing firms from being able to achieve the dream. Today, though, the ability to collect consumer level data permit the possibility of learning a consumer’s willingness to pay. In this section, we sketch a simple example involving a firm attempting to learn the willingness to pay of a single consumer.

&nbsp;

Assume a consumer values a product at $$v$$ where $$v$$ is drawn from a distribution $$f(v)$$. We will assume $$f(v)$$ has support over $$[0, \bar{v}]$$. The consumer will buy if and only if the price, $$p$$ is $$\leq v$$. If the firm could perfectly price discriminate, it would charge this consumer exactly $$v$$. Starting out, however, the firm does not know $$v$$. We will, assume, however that the firm knows $$v$$ is drawn from $$f(v)$$. 

&nbsp;

If the firm is able to track this consumer’s behavior, it can learn $$v$$. To see this, let $$v^{*}$$ denote the consumer’s true willingness to pay. Since the firm knows $$v^{*}$$ is drawn from $$f(v)$$, suppose the firm sets an initial price, $$p_0$$ equal to $$E[v] = \int_{0}^{\bar{v}} vf(v)dv$$. After setting a price, the firm observes the following random variable, $$\pi_0$$:

$$\pi_0 = \begin{cases} 0 \text{ if } p > v^{*} \\ p \text{ if } p \leq v^{*} \end{cases}$$

Since $$p_0 = E[v]$$, this is

$$\pi_0 = \begin{cases} 0 \text{ if } E[v] > v^{*} \\ E[v] \text{ if } E[v] \leq v^{*} \end{cases}$$

Consider the two cases.

&nbsp;

In the first case, if the firm observes $$\pi_0 = 0$$, then it must be the case that $$E[v] > v^{*}$$. Then, the firm knows that $$v$$ must be drawn from the truncated distribution $$f_1(v \vert \pi_0 = 0)$$ with support $$[0, E[v]]$$. It follows then that a logical price to charge is $$p_1 = E_{f_1(v \vert pi_0=0)}[v]$$.

&nbsp;

In the second case, if the firm observes $$\pi_0 = E[v]$$, then it must be the case that $$E[v] \leq v^{*}$$. Then, the firm knows that $$v$$ must be drawn from the truncated distribution $$f_1(v \vert \pi=E[v])$$ with support $$[E[v], \bar{v}]$$. It follows then that a logical price to charge is $$p_1 = E_{f_1(v \vert \pi_0 = E[v])}[v]$$. 

&nbsp;

In summary, the firm’s second price, $$p_1$$, will be

$$\begin{aligned}p_1 = \begin{cases} E_{f_1(v \vert \pi_0=0)}[v] \text{ if } \pi_0 = 0 \\ 
E_{f_1(v \vert \pi_0 = E[v])}[v] \text{ if } \pi_0 = E[v] \end{cases}\end{aligned}$$

We can extrapolate to the $$ith$$ price for $$i\geq 1$$. To do so, let $$b_i$$ be an indicator for whether the consumer purchases in round $$i$$. Then,

$$\begin{aligned} p_i = \begin{cases} E[v|b_{i-1}=0] \text{ if } b_{i-1} = 0 \\ 
E[v|b_{i-1}=1] \text{ if } b_{i-1} = 1\end{cases}\end{aligned}$$

where $$E[v \vert b_{i-1}]$$ denotes the distribution conditional on whether the consumer purchases in round $$i-1$$. Note that whenever $$b_{i-1}=0$$, the firm learns that $$p_{i-1}$$ must be the supremum for the feasible range of $$v$$. In turn, whenever $$b_{i-1}=1$$, the firm learns that $$p_{i-1}$$ must be the infimum for the feasible range of $$v$$. Under the uniform distribution, $$E[v \vert b_{i-1}]$$ will always be equal to the mean of the currently known infimum and supremum. Consequently, whenever the firm learns a new infimum (when $$b_{i-1}=0$$), the new infimum will be greater than the previous. In turn, whenever the firm learns a new supremum (when $$b_{i-1}=1$$), the new supremum will be less than the previous. Therefore, in the limit as $$i \rightarrow  \infty$$, we will have $$E[v \vert b_{i-1}] = v^{*}$$. This means, in this limit $$p_i \rightarrow v^*$$. 

&nbsp;

**In summary, in the limit, this experimentation strategy permits the firm to achieve perfect price discrimination and extract all surplus[^2]**. In a pre-digital age, the tracking required to perform this strategy would be laborious. In today’s world though, it is easy to imagine a database tracking all past price-purchase decision behaviors of a given consumer. Moreover, it is easy to imagine such tracking for *all* consumers -- not just one. And make no mistake: The incentive to engage in such tracking is abundantly clear. It has nothing to do with wanting to make the consumer better off with personalization - but rather, it has to do with the firm seeking to extract as much profit as is possible from every individual

&nbsp;

### A A Simulation With Many Consumers

It is easy to take the theory and map it to a simultation. One pesky thing we have to confront though is that the above strategy is only guranteed to converge if we have continuous prices (i.e., $$p \in \mathbf{R}$$). In real life, consumer prices tend to only be to two decimal places. And although we can approximate continuous prices via a simulation, computer precision is only so fine. Hence if we naievly implement the strategy outlined above, we'd run the risk of possibly getting "stuck." The simulation below takes a different approach than the strategy above to avoid getting "stuck." In words, the strategy below exploits the relatively small grid of prices (0 up to 1 in increments of 0.01). For each consumer, the idea is to charge them each price until they do not buy. Once we see them not buy, we know the last price we charged them was their $$v$$.

```
# Simulation Parameters
set.seed(42) # For reproducibility
n_consumers <- 1000
possible_valuations <- seq(0, 1, by = 0.01)

# 1. Initialize Consumers
# Valuations are drawn from U(0,1) and rounded to 2 decimal places
consumers <- data.frame(
  id = 1:n_consumers,
  true_v = round(runif(n_consumers, 0, 1), 2)
)

# 2. The Firm's Learning Process
# The firm runs an ascending price probe for each consumer
learned_valuations <- numeric(n_consumers)

for (i in 1:n_consumers) {
  # Start price at 0 and increase by the smallest increment (0.01)
  current_price <- 0.00
  
  # The firm increases the price until the consumer refuses to pay
  # In this logic, the last price they accepted is their v_i
  while (current_price <= consumers$true_v[i]) {
    last_accepted_price <- current_price
    current_price <- round(current_price + 0.01, 2)
    
    # Safety break to prevent infinite loops with floating point math
    if (current_price > 1.01) break
  }
  
  learned_valuations[i] <- last_accepted_price
}

# 3. Results Comparison
consumers$firm_learned_v <- learned_valuations
consumers$match <- consumers$true_v == consumers$firm_learned_v

print(consumers)

# Verify Accuracy
accuracy <- mean(consumers$match) * 100
cat("\nFirm Learning Accuracy:", accuracy, "%")

Firm Learning Accuracy: 100 %
```

Note the output at the end: The firm learns $v$ perfectly for every consumer! Again, this approach is not sophisticated and there is plenty of research on optimizing experimentation. The point isn't about the optimality of the experimentation approach though - rather, it is about the fact that price experimentation can permit a firm to learn valuations perfectly and allow them to extract all surplus accordingly.

&nbsp;

## Points of Contention

There are some subtleties that people may be want to criticize about the arguments above. First, some may point out that it is good that people who do not buy under a single price will buy under perfect discrimination. Second, some Finally, some will point out there are others degrees of price discrimination in which consumer surplus actually *can* improve under certain conditions will note we have implicitly assumed a monopoly structure. I address these points below.

&nbsp;

### New People Buy Under Perfect Discrimination That Do Not Under a Single Price

Yes, this is indeed true. We know that under a single price structure, $$p^{*}$$ is chosen by the firm so that $$D’(p^{*}) = c$$. Again we will assume $$c$$ is constant and equal to 0 for simplicity. This means that anyone with valuation $$v < p^{*}$$ will not buy. To make this concrete, assume $$D(p)$$ is such that $$D(0) = 1$$ and $$D(\bar{M}) = 0$$. In essence, we can interpret quantity as the percent of consumers who purchase.

&nbsp;

Under this setup, under perfect competition, we know $$p = 0$$, in which case all consumers buy (since $$D(0) = 1$$). It is obvious that $$p^{*}$$ will be $$\geq 0$$. Hence, under the firm’s choice, only $$D(p^{*})$$ percent of consumers will buy. This means that under the firm’s single price regime, relative to perfect competition, $$1 - D(p^{*})$$ do not purchase the good. Of course, under perfect price discrimination, we achieve the result of all consumers purchasing the good (or, in the case of $c > 0$, we will have the same percent purchasing the good as do under perfect competition). 

&nbsp;

With $$c=0$$, the “new” marginal surplus, $$\Delta W$$, will be given by

$$\Delta W = \int_0^{p^{*}} D(p)dp.$$

By “new”, we mean that this is the surplus coming from consumers who did not buy under the single price regime - but do under the perfect discrimination regime. Of course, it is critical to keep in mind that such consumers are indifferent between buying and not buying. I.e., their surplus is 0 since it all goes to the firm. Moreover, under the single price regime, since such consumers do not buy, their surplus is 0 in that case as well. Therefore, from a strictly economic lens, “gaining access” to the good via perfect discrimination does not make these consumers “better off.”

&nbsp;

Moreover, the $$D(p^{*})$$ percent of consumers that purchased under the single price regime are made worse off. Under the single price-regime, we know these consumers have the following consumer surplus (CS):

$$CS = \lim_{M\rightarrow \bar{M}} \int_{p^{*}}^{M} D(p)dp.$$

But under perfect discrimination, these consumers lose all of their surplus. Like the new consumers who now purchase the good, the “original” consumers are left with zero surplus!

&nbsp;

However, a key phrase above is “from a purely economic standpoint.” Perhaps there may be reason to believe it is “good” the new consumers can purchase. This is certainly a possibility. However, such a notion moves away from the rigorous lens of economic efficiency and instead moves towards normative ideals. We can reason through some possible heuristics that might suggest whether the cost to original consumers is “good” in the name of other consumers being able to buy.

&nbsp;

First, Suppose, for example, that $$D(p^{*}) = 0.99$$. This means that under the single price regime $$99\%$$ of consumers purchase the product. This means that if the firm towards perfect discrimination, $$99\%$$ of consumers have their surplus wiped out all in the name of the remaining $$1\%$$ of consumers being able to buy. Surly this feels less than “good.” In the other extreme, though, say $$D(p^{*}) = 0.01$$. This means only $$1\%$$ buy under the single price regime. Perhaps in this case we feel it is justifiable to wipe out that $$1\%$$ of consumers’ surplus in the name of enabling the other $$99\%$$ to buy. In other words, the amount by which quantity increases is a useful metric for thinking through whether the cost to the original consumers is worth it.

&nbsp;

Second, we might also want to think about the ratio of new marginal surplus, $$\Delta W$$ to the original consumer surplus. If $$\Delta W$$ is “big” relative to the original consumer surplus, this indicates there was a lot of value being restricted by the single price regime. In contrast, if $$\Delta W$$ is “small” relative to original consumer surplus, this means that the firm is killing off all consumer surplus in the name of small profit gains.

&nbsp;

Ultimately, though, regardless of normative nuances we want to add, the fundamental economic conclusion remains the same: Perfect price discrimination wipes out all consumer surplus in the name of profits. Firms do not dream about it in the name of making consumers better off!

&nbsp;

### Competition Matters

Implicit in all of the above is the assumption of a monopolistic firm. Introducing competition adds nuances that can change results fundamentally. In the simplest case, competition will simply see to it that price discrimination is not possible.

&nbsp;

To see why, suppose we have firms that compete on price. One firm decides to price discriminate. If the other firm knows that the other firm is price discriminating, the other firm can do the following: Charge every consumer their willingness to pay *minus* some $$\epsilon$$ where $$\epsilon > 0$$. What happens? Well, now consumers will just want to buy from the other firm! The first firm knows this though. So the optimal strategy would be to charge all consumers their willingness to pay minus $$k\epsilon$$ for $$k>1$$. You can see where this is going: Eventually, the competition drives prices down to marginal cost - yielding the perfect competition result in which consumers will reap all the surplus!

&nbsp;

Though the ability to price discriminate (whether perfectly or not) does not require having monopoly, *it does require having meaningful market power*. In the absence of monopoly, this will usually mean some kind of product differentiation existing. Moreover, beyond simple cases, modeling the nuances of cases will usually require a good bit of game theory. 

&nbsp;

Even so, we do not need to sketch out any complex models to reason the following: Regardless of setting, firms will only engage in price discrimination if and only if profits can be increased. To do otherwise would contradict the standard assumption of profit maximization! 

&nbsp;

### Less than Perfect Price Discrimination Can Improve Consumer Surplus

Our focus throughout has been on the idealized perfect price discrimination, in which a firm learns - or knows - each consumer’s willingness to pay and charges them exactly that. However, there are weaker versions of price discrimination. I do not want to get into the intricacies of lower degrees of price discrimination. I do want to comment on one fact though: Under certain conditions, it is possible for lower degrees of price discrimination to improve consumer welfare.

&nbsp;

Specifically, lesser degrees of price discrimination *can* improve consumer welfare if total quantity increases. The operative word is *can*: Just because quantity does increase under discrimination relative to no discrimination does not guarantee consumers are made better off in aggregate. Whether it actually depends on the demand of different consumer groups. Moreover, we still have the fundamental result: Even if we knew price discrimination under a certain case could improve consumer welfare, firms are only going to choose to do it if it increases their profits relative to no discrimination!

&nbsp;

## Concluding Thoughts

To be normative for a moment, we ought to be suspicious whenever firms try to justify large-scale consumer level price experiments. Chances are they aren’t engaging in it because they want to make their customers better off - rather, they’re engaging in it probably because they’re salivating at the thought of being able to extract every last dollar they can out of each individual person. Moreover, the firms that have the means and scale to engage in such practices print enough money as is. Let’s not fool ourselves into thinking they’re out here providing “personalized” prices to make people better off.

&nbsp;

---

[^1]: Our assumption of a constant marginal cost structure is not necessary for this result. The math becomes a bit uglier, but with some basic fixed point theory, one can achieve the same qualitative result with a quantity dependent cost structure given some standard regularity conditions.


[^2]: Our use of the uniform distribution is not material here - the mathematics are just much easier to sketch out. In turn, that we focused on one consumer is not material either. Provided the firm can enforce different prices to different consumers, and provided the firm can engage in the tracking required, nothing prevents this example from extending to $$n$$ consumers.

---

