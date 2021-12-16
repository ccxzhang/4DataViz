---
layout: default
---


## Introduction

Ideology has been a central concept muddled by diverse uses in the study of politics. Philip Converse perhaps provided a classical operationalization of ideology-- a “belief system” that represents the configuration of attitudes and ideas.


## Data
### Source
<div>
  <div style="position:relative;padding-top:56.25%;">
    <iframe iframe src="dataviz/survey_statistics.html" frameborder="0" scrolling="no"
    title="Survey Map" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
  <figcaption font-size="12px"> The map above displays the number of responses, the average age of respondents, the percentage of male respondents, and the percentage of respondents holding a college degree or above. </figcaption>
</div>


The study employs the [_zuobiao_](http://www.zuobiao.me/) survey (Chinese Political Compass) that contains 50 questions about a wide array of critical issues in China. Peking University's graduate students and researchers designed the survey, the same source used in [Pan and Xu's study](https://www.journals.uchicago.edu/doi/abs/10.1086/694255) in 2018.

<div>
  <div style="position:relative;padding-top:56.7%;">
    <iframe iframe src="dataviz/stacked_barplot.html" frameborder="0" scrolling="yes"
    title="Survey Map" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
</div>

Each question in the survey is on a four-point Likert scale– “strongly disagree (1),” “disagree (2),” “agree (3),” and “strongly agree (4).” Q5 measures people’s perception of replicating western-style freedom in China, and Q3 sets up a hypothetical yet usual situation in China to let people make the judgment. As the figure above displays, around 55% of respondents disagree with the statement that indiscriminately imitating western-style freedom of speech will lead to social disorder in China. In comparison, 45% of respondents agree or strongly agree. Despite the increasing risks of social unrest, 47% of respondents support transparency under the crisis, and 53% disagree with the statement.

Q5 and Q3 reveal that respondents favor information transparency under the crisis, a key component of democratic accountability, yet reject the idea of imitating western-style freedom. Although the term “indiscriminate” in Q5 leads respondents to disagree with the statement, the underlying preferences should be consistent, suggesting that people who support information transparency should also promote free speech or vice versa, despite the cost of bringing social disorder.

Other than the survey data, the study obtains the data from Zhihu by a scraper. Zhihu is a Chinese version of Quora and attracts millions of users to share opinions on a specific question. The data is from a specific question on Zhihu about perceiving nationalism. That question has more than 500 answers with more than 6,700 comments and 14,800 votes.

## Methods
### Principle Component Analysis (PCA)
<div>
  <div style="position:relative;padding-top:56.25%;">
    <iframe iframe src="dataviz/screeplot.html" frameborder="0" scrolling="yes"
    title="Survey Map" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
  <figcaption font-size="12px"> The figure displays the eigenvalue's and cumulative explained variance’s changes along with the number of components. </figcaption>
</div>

50 questions on ideology can make one confused about which one matters. As a frequently-used dimension-reduction technique, PCA can remove the noise by reducing a large number of features to a few principal components (PCs). Rather than the random guess, Kaiser’s rule, which is to retain components whose eigenvalue is greater than 1, can help decide the best number of components. The optimal number of PCs should be 10. Meanwhile, 10 PCs merely explain 46.7% of the total variance, and we lose more than half of the information.  

### Synthetic Index Approach

<div>
  <div style="position:relative;padding-top:56.25%;">
    <iframe iframe src="dataviz/distplot.html" frameborder="0" scrolling="yes"
    title="Survey Map" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
  <figcaption font-size="12px"> The figure displays the distribution of each index.</figcaption>
</div>

The study manually creates synthetic indices based on questions revolving around the same theme. Building on previous studies, the survey questions are classified into seven themes: _Democracy_, _Freedom_, _Market_, _Socialism_, _Globalization_, _Traditionalism_, and _Nationalism_. Each question is labeled as +1 to represent the support and -1 to represent the non-support of one specific theme. Each index is a combination of such values. The mean of each index is 0.128 for _Democracy_, -0.525 for _Freedom_, -0.506 for _Market_, 1,556 for _Socialism_, -1.057 for _Globalization_, 0.755 for _Traditionalism_, and 1.007 for _Nationalism_. These metrics portray a fictitious average respondent in China: one slightly favors democracy, looks slightly conservative, believes socialism and traditional Chinese culture, prioritizes nationalism, and is suspicious of the free market and economic globalization.

To better make the inferences, here employs the logistic regression under L1-regularization for feature selections. The model is displayed as below:

$$\operatorname{logit} (p_i)=\ln \left({\frac {p_i}{1-p_i}}\right)=\beta _{0}+\beta_{1}I_i + \beta _{2}D_i $$

$$ -\sum_{i=1}^N\bigg[-{\ln(1+e^{(\beta _{0}+\beta _{1}x_i + \beta_{2}D_i)})+y_i \left(\beta _{0}+\beta_{1}I_i+ \beta _{2}D_i\right)\bigg]}+\lambda\sum_{j=1}^p{|\beta_j}| $$

where $\I_{i}$ represents the each index and $\D_{i}$ represents the demographic variables.

As the below left figure reveals, one that leans towards democracy, freedom, and the market is© more likely to support information transparency under the crisis, while the supporter of nationalism or/and traditionalism is less likely to agree. If one inclines to the socialist ideology, he or she may also agree. Freedom is the main force driving information transparency, and Nationalism is the primary opposite factor among all the indices. The result is in line with our intuition that freedom and government domination, the usual practice of withholding information, are incompatible. Besides being male, positively associated with information transparency, the remaining demographic variables do not display a clear pattern.


<div>
  <div style="position:relative;padding-top:56.25%;">
    <iframe iframe src="dataviz/stats_result_1.html" frameborder="0" scrolling="yes"
    title="Survey Map" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
  <figcaption font-size="12px"> The figure displays the logistic regression (under L1-regularization) results. </figcaption>
</div>

In light of the above figure, we can find that one supports democracy, freedom, and the market tends to disagree. One who follows traditionalism or nationalism also embraces the idea. Interestingly, respondents with a college education or above than those who fail to attend high school are more likely to agree with the statement. A possible explanation for this is that well-educated people in China may have a deeper understanding and see the flaws of freedom than the laypeople. Thus they tend to reject the idea of indiscriminate imitation. The same explanation could also apply to individuals whose income is between 150k and 300k. As for living in southwest China and being male, it is hard to figure out the exact reasons. We may attribute that phenomenon to Lasso’s arbitrary feature selections.


## What is Chinese talking about when they talk about nationalism?

<div>
  <div style="position:relative;padding-top:56.25%;">
    <iframe iframe src="dataviz/ldastats.html" frameborder="0" scrolling="yes"
    title="Survey Map" style="position:absolute;top:0;left:0;width:100%;height:100%;"></iframe>
  </div>
  <figcaption font-size="12px"> The figure displays the perplexity and coherence scores along with the number of components. </figcaption>
</div>

Here employs the Latent Dirichlet Allocation (LDA) to extract topics of a Zhihu (the Chinese version of Quora) question about “how to perceive nationalism.” Perplexity and coherence scores are measures to evaluate the model’s performance. The higher the coherence score, the more consistent the topic would be. Generally, the perplexity score would decrease along with the number of topics. The above figure displays that 12 topics seem to make the most sense. Then, the cloud of top words could help us understand each topic’s theme.

![WordCloud](/dataviz/wordcloud.png)

The above figure displays 9 out of 12 topics’ word clouds:
- Topic 1 contains 美帝 (American Imperialism), 左 (the leftist), 利维坦 (Leviathan), and 五分 (a pejorative term for a supporter of the Chinese government without thinking);
- Topic 2 focuses on politicians and parties, including 政客(politicians), 政党 (political parties), and 党同伐异 (factionalism);
- Topic 3 is primarily about 国与国 (state-to-state);
- Topic 4 involves 乌鲁克 (Uruk) and 苏美尔 (Sumerian);
- Topic 5 and 9 are similar and encompass [Sun Yat-sen’s](https://en.wikipedia.org/wiki/Three_Principles_of_the_People) 民权主义 (Government by the People) and 民生主义 (People's welfare/livelihood). Topic 5 exclusively contains 正统 (orthodoxy) and [汉奸](https://en.wikipedia.org/wiki/Hanjian) (a pejorative term for a traitor to the Han Chinese), while topic 9 leans towards 特权 (privilege) and 阶级矛盾 (class conflict);
- Topic 6 sheds light on ancestors (祖宗) and [夷](https://en.wiktionary.org/wiki/%E5%A4%B7#Definitions) (barbarian/foreigners) on the one side, and 无产者 (Proletariat) on the other;
- Topic 7 is about 犹太人 (Jewish) and 以色利 (Israel), both of which are closely related to Zionism;
- Topic 8 is similar to topic 5 in terms of 卖国 and 狗, and both are traitors’ different expressions in Chinese.

## Discussion
