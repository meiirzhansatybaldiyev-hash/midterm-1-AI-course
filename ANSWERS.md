# Midterm 1: answers

GitHub username: meiirzhansatybaldiyev
Session code: ember-92
Fingerprint: 035f1eba

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude (made by Anthropic) in this exam. Claude helped me with the calculations: recall, MRR, Wilson intervals, the costs of the four candidates, the costs of pipelines H and L, and the numbers for Part 3. Claude also helped me understand the topics, for example how the Wilson interval works, why rank 11 gives 0 in MRR, how the cost formula works, and why the language premium comes from the tokenizer. I sent Claude the exam text and my data (DATA.md, retrieval_ranks.csv, doc_results.csv). Claude also helped me write the decision, the next measurement, the memo and the Why lines in simple English. I read the numbers and checked the choices myself: the budget, the recall for each language, and L against H. I did not use any other AI tool. I can explain every number and every answer in the oral check.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.667 | 0.750 | 0.800 | 0.708 |
| B | 0.317 | 0.517 | 0.767 | 0.459 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.750 | 0.628 | 0.842 |
| A, k=5 | 0.800 | 0.682 | 0.882 |
| B, k=3 | 0.517 | 0.393 | 0.638 |
| B, k=5 | 0.767 | 0.646 | 0.856 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 19676 |
| A, k=5 | 26169 |
| B, k=3 | 12838 |
| B, k=5 | 14772 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

Only the two B candidates fit the budget of 17200. (B,5) costs 14772 per year and (B,3) costs 12838. (A,3) costs 19676 and (A,5) costs 26169, so they are too expensive. (B,5) has recall 0.767 (0.646 to 0.856). (B,3) has 0.517 (0.393 to 0.638), and its interval is below (B,5), so (B,5) is better. A has higher recall, but the intervals overlap with (B,5), so I cannot say A is better. Recall of (B,5) by language: EN 0.85 (17 of 20), RU 0.85 (17 of 20), KK 0.60 (12 of 20). Kazakh users are served worst. They are also the most expensive requests.

### 1e. Next measurement (at most 60 words)

I would add at least 100 more Kazakh test questions and compare (B,5) with (A,3) on Kazakh only. Now KK recall is 12 of 20 against 13 of 20, and the intervals are too wide. If A is clearly better for Kazakh, I would change the decision.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.872 | KK, clean | 0.800 | 0.627 | 0.905 | 28647 |
| L | 0.806 | RU, degraded (tie with KK, degraded) | 0.733 | 0.556 | 0.858 | 18000 |

Personal-data pages per year sent abroad under H: 180675

### 2b. Memo (at most 300 words)

To: Head of Operations. My choice: pipeline L.

Who is served worst. Overall, H is 0.872 (0.816 to 0.913) and L is 0.806 (0.742 to 0.857). The intervals overlap, so we cannot say H is better overall. The overall number hides problems. For L, the worst cells are RU degraded and KK degraded: 22 of 30 = 0.733 (0.556 to 0.858). For H, the worst cell is KK clean: 24 of 30 = 0.800 (0.627 to 0.905). Each cell has only 30 invoices and all intervals overlap. I am fairly sure degraded RU and KK scans are hard for L.

Personal data. In 9 % of pages (180,675 per year) there are a name and ИИН. Law No. 94-V covers ИИН. Where does the scan go? H: to a data centre abroad (known). L: to our server in Kazakhstan (known). Who stores it? H: unknown, we must ask the vendor. L: we do. For how long? H: unknown. L: we decide, but we have no rule yet. A vendor statement is not a compliance review.

Safeguards. For L: a written rule to delete scans after a fixed number of days, a deletion job with a log we can check, encrypted disk, an access list checked every quarter, and a tested firewall rule that blocks outgoing traffic. If we use H: zero retention in the contract, audit rights, and a log of pages sent abroad.

Choice. L costs 18000 per year (2 servers). H costs 28647. L keeps personal data in Kazakhstan, but accuracy is about 7 points lower. Remaining risk: up to 44 % of degraded RU and KK scans may be wrong (end of interval), so degraded scans need human review. We also keep the duty to protect the stored data.

## Part 3: Explain your own numbers

### Q1

Number: 1.64
Why (at most 40 words): Bytes are 2.16 times more, but tokens are 3.55 times more. So most of the Kazakh premium comes from the tokenizer, not from the encoding.

### Q2

Number: 2.7 %
Why (at most 40 words): Only English is cut, because 526 e5 tokens is more than 512. Russian (256) and Kazakh (152) fit. The cut part is dropped and is never embedded, so search cannot find it.

### Q3

Number: 86.4 %
Why (at most 40 words): The order of the tokens does not change. Low T makes Monday much stronger and the other tokens very small, so they are almost never sampled. No token is removed; that is top-p.

### Q4

Number: 28
Why (at most 40 words): The public leaderboard prefers X (73 against 69). A wrong answer costs nothing and "I don't know" gives 0, so a model learns to guess and not to say it does not know.

### Q5

Number: 5.55
Why (at most 40 words): It is over-trained: it has many more tokens per parameter than Chinchilla. Chinchilla said that models like GPT-3 were too big and under-trained, and should use more tokens.
