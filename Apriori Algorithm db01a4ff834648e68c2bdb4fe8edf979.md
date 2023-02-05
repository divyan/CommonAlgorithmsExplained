# Apriori Algorithm

## A beginner’s guide to the frequent itemset mining algorithm used for Association Rule Mining.

**By: Divya Naiken** 

---

### Association Mining

The goal of association mining is to find relationships and patterns within the variables of (*generally*) large databases. The importance of this technique is evident when counterintuitive patterns are discovered. 

A famous example of Association Mining is the **if-then** relationship found when reviewing the purchases of diapers at Wal-Mart. One may assume an association between baby formula or even pacifiers was found, but it was discovered that:

 **if** an individual purchases diapers, **then**, they will also purchase beer.  

---

### How does the Apriori Algorithm fit into this?

This is the association mining pipeline:

![Untitled](CommonAlgorithmsExplained/Untitled.png)

The Apriori Algorithm is found in the second step of the pipeline. 

In order to optimize this step, we want to consider the **least** number of **itemsets** (*this is a concept that will be extensively examined below*). *Compared to techniques previously used, the Apriori Algorithm handles this well.* 

> **itemsets -** set of items

e.g. all the items found on a receipt at Shoppers
{eye mask 🥽, newspaper 📰}, {toothpaste 🦷, lotion🧴 }, {spiral notebook 🗒️}
> 

---

### Parameters of the Apriori Algorithm

Before we begin examining the steps of the algorithm it is important to consider the parameters and metrics that will be used: 

User-Defined: typically set by industry experts

> **minimum support threshold -** the cut-off for ****how frequently (popular) an itemset appears in the data set, this is found as a percentage.
> 

Calculated: 

> **minimum support count -  [** $minimum\space support \space threshhold \times number \space of \space transactions \times \frac{1}{100}$ **]**

e.g. the minimum support threshold is 60% and the number of transactions is 5 so,
minimum support count is (60*5/100) = 3
> 

---

### Significant Techniques of the Algorithm

When working on the example below, consider these two items, they will make more intuitive sense later on: 

### 1. Join

This combines the itemsets

### 2. Prune

This removes itemsets that are found when computing the self-join BUT it’s subsets are not found in the previous frequency table. 

**All subsets of a frequent itemset must also be frequent**

If $\{XY\}$ **is** a frequent itemset, both $\{X\}$ and $\{Y\}$**must** be a frequent itemset.

If  $\{XZ\}$ **is not** a frequent itemset then, $\{XYZ\}$ **cannot be** a frequent itemset.

![Screen Shot 2022-04-03 at 2.56.28 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_2.56.28_PM.png)

---

### How does the Apriori Algorithm Work (worked example)

Let’s consider this dataset of transactions at the Movie Theater

![Screen Shot 2022-04-03 at 12.44.11 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_12.44.11_PM.png)

Step 1: Find the frequencies of each item 

Let’s create a frequency Table $L_{1}$ that contains the count of **1 item in the data set.**

![Screen Shot 2022-04-03 at 1.03.07 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_1.03.07_PM.png)

Step 2: Generate candidates 

Now, let’s generate candidates into Table $C_2$, from Table $L_1$.

In order to do this, we need to perform a **Self-Join** with the ****items ****and their respective frequencies 

Step 2.1: Prune candidates

When we perform a self-join, there will be entries that are insignificant - they can be removed.
This is done by checking if the entries found in the self-join are **also** in the previous table. However, since this is our first table, we simply remove itemsets with duplicate items. 

Popcorn, popcorn **←insignificant** 
Popcorn, coke, 
Popcorn, candy, 
Popcorn, nachos, 
Coke, coke,           **←insignificant** 
Coke, candy,
Coke, nachos,
Candy, candy,      **←insignificant** 
Candy, nachos 

 

![Screen Shot 2022-04-03 at 3.49.41 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_3.49.41_PM.png)

Step 4: Remove Entries with frequency < **minimum support count** 

Now, let’s generate $L_2$. 

In order to do this, we need to remove entries that have a frequency **under** the **minimum support count.** 

Let’s assume our minimum support threshold is 50%, since we have 6 transactions:

$min \space support \space count \space =33 \times 6 \times 0.01 = 1.98 = 2$ **(always round up)**

Now, every entry under a frequency of 2 will be removed. 

![Screen Shot 2022-04-03 at 1.43.50 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_1.43.50_PM.png)

Step 4: Find the frequencies 

Now, let’s generate candidates into Table $C_3$, from table $L_2$.

In order to do this, we need to perform another **Join** with the ****items ****and their respective frequencies 

Step 4.1: Prune candidates 

Once again, there will be entries that are insignificant - they can be removed. This is done by checking if the entries found during the self-join are **also** in the previous table. They are kept if they are and discarded if not. 

The self-join produces:

{ popcorn, coke, candy }
{ popcorn, candy, nachos }
{ coke, candy, nachos } 

For example, the subset { coke, nachos } is not in the previous frequency table $L_2$  so the entry { coke, candy, nachos } is discarded 

![Screen Shot 2022-04-03 at 4.00.00 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_4.00.00_PM.png)

Step 5: Remove Entries with frequency < **minimum support count**

Now, let’s generate $L_3$. 

In order to do this, we need to remove entries that have a frequency **under** the **minimum support count.** 

In our case, all entries have a frequency that is above 2. 

As the final step, we will generate candidates into Table $C_4$, however, this entry does not exist in the original dataset so it can be discarded. 

![Screen Shot 2022-04-03 at 2.23.49 PM.png](Apriori%20Algorithm%20db01a4ff834648e68c2bdb4fe8edf979/Screen_Shot_2022-04-03_at_2.23.49_PM.png)

🏁 We have finished all the steps are we are left with 3 frequent itemsets: 

{popcorn, coke, candy}
{popcorn, candy, nachos}

These items can now be applied to a strong association rule to fulfill the third step of the Association Mining Pipeline. 

---

### Pseudocode

In words:

Join C[k] and C[k-1]

Prune C[k] 

Compute L[k] by keeping items in C[k] that satisfy the min_support_count

In code: 

```python
L[1] = get_frequency_1_itemset() #find frequencies for every individual item 
minimum_support_count = get_min_s_c(min_s_threshold, num_entries)
k = 2

while(L[k-1] != 0):

    C[k] = join(L[k-1]) #find frequencies using join 

    C[k] = prune(C[k]) #remove entries with subsets not in the previous table 

    L[k] = remove_insig(C[k], minimum_support_count) #uses min_s_c

		k ++

end

solution = union(Lk)
```

---

### Advantages and Disadvantages

Advantages

- fairly easy to understand
- joins are easily implemented on databases that are large

Disadvantages:

- depending on the parameters of the database and the decided upon minimum support value, the implementation can be computational complex as the whole database will need to be looked at

---

### Use Cases

The Apriori algorithm is an advantageous algorithm, some of the use-cases include:

- Market Basket Analysis: if items are known to be purchased together (strong association is found between items), this can lead to:
    - increased sales
    - customer engagement through strategic marketing (e.g. combos at McDonald)
- Recommender Systems: collaborative filtering (e.g. Netflix recommending content)
- Web Page Development: optimized user interface

---

### Similar Algorithms:

AIS (candidate itemsets are computed during the passes of a database)

DIC ( can be better than Apriori as it tries to reduce passes through a database )

SETM (same as AIS) 

There are advantages and disadvantages to each of these algorithms and the most optimal one is dependent on many factors. 

---

by: Divya Naiken
