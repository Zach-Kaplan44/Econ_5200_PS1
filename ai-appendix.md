# Econ_5200_PS1
[Prep] Act as a partner reviewing an analytics deliverable before it
goes to a client board. You are looking for the objection that would
embarrass us in the room.

[Request] I audited a retailer's "average basket value" metric. I found
the naive mean was inflated by a right-skewed B2B tail and by a change
in what was logged between years. I recommend the dashboard show a
trimmed mean with an explicit B2B exclusion rule. Give me the three
strongest objections to that recommendation, ordered by how likely the
client is to raise them, and for each one tell me what evidence would
settle it.

[Iterate] Be specific to this metric. Do not give me generic data
quality advice.

[Mechanism Check] For each objection, say whether it is a question
about the data, about the statistic, or about the business definition
of the metric -- these need different kinds of answer.

[Evaluate] Which single additional analysis would most reduce our
exposure?
Untangling whether logging changes mask the real metric shift.
Reviewing this as the partner who has to sit next to you: the deliverable has a real finding and a fix that doesn't fully match it. That gap is where the room will get you.

1. "You've excluded our growth segment, and now the metric doesn't tie to revenue." Mostly a business definition objection.

This is the one a board member raises first, and it comes in two parts. Commercially, the right-skewed B2B tail may be the story rather than the contamination — if wholesale is the fastest-growing book, a headline metric engineered to be blind to it will understate the business and nobody will trust the dashboard again. Mechanically, a trimmed mean with an exclusion rule no longer multiplies back to net sales, so finance will find that ABV × transactions misses the P&L and assume the dashboard is broken.

What settles it: the revenue-and-transaction share of the excluded population by year, side by side. If B2B is 2% of baskets and 30% of revenue, the answer is two metrics plus a reconciling total, not one cleaned metric. You should also be able to name the decisions ABV feeds. If it drives assortment, promo and store-level comparisons, a retail-only figure is defensible. If anyone uses it for forecasting or valuation, it isn't, and you need to say so before they do.

2. "Your B2B rule is a guess, and it will drift." A data objection with a definitional edge.

This is the sharp one, and it depends on what the rule keys on. If it's a basket-value threshold, the rule is circular: you're defining B2B by the same quantity that's inflating the mean, so trimming and excluding are doing the same work twice, and a genuine high-spend consumer — holiday bulk buy, registry, contractor shopping retail — gets labelled B2B and deleted. Worse, if the rule is applied with current-year logic to both years, it reintroduces exactly the comparability break you flagged.

What settles it: the rule scored against an independent ground truth. Tax-exempt or resale-certificate status, account entity type, delivery address classification, purchase cadence. Show precision and recall, not just a count of flagged accounts. Then a sensitivity table of the metric across threshold settings, and the rate at which accounts cross the boundary between years. If the metric moves materially between a 1% and a 5% trim, or between two plausible B2B definitions, you don't have a recommendation yet.

3. "Trimming doesn't fix the logging change, and the trim fraction is your choice." A statistic objection sitting on a measurement problem.

This is the one that should worry you most, because it's the one where your fix doesn't address your own diagnosis. Trimming operates on the tail. A logging change — split shipments logged as separate orders, BOPIS entering the transaction table, line-level versus order-level rollup, returns netting in, shipping or tax moving inside the basket — shifts the body of the distribution and the denominator. Trimming the top 5% does nothing to any of that. You will have presented two causes of inflation and a remedy for one.

Then the estimator itself: a 5% trimmed mean has no interpretation a board can hold, it doesn't aggregate to anything, and the trim fraction is a free parameter that a skeptical director will read as a dial you turned until the trend looked clean.

What settles it: an overlap window where both logging schemas exist, with the newer data re-aggregated to the old basket definition, plus the metric reported under several estimators — median, 5% and 10% trimmed, winsorized, retail-only untrimmed mean. If the conclusion survives all of them, the estimator choice stops being contestable and becomes presentational.

The one analysis to run before this goes out

Decompose the year-over-year change in mean basket value into three quantified pieces: how much came from the logging definition change, how much from shift in B2B mix, and how much from actual retail behavior. Compute it on a common re-based basket definition, with B2B flagged by an independent identifier rather than by value, and present it quantile by quantile rather than as a single mean.

It is the highest-leverage analysis for three reasons. It answers the question the board actually has, which is how much of the inflation was artifact. It validates or kills the exclusion rule as a byproduct, since a value-based rule and an identifier-based rule will visibly disagree. And it turns your estimator choice into a consequence of the evidence rather than a judgment you have to defend.

Right now the deliverable asserts two causes and apportions neither. That's the question I'd expect from the most numerate person in the room, and not having a number is what would make the audit look soft.
