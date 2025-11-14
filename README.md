# Data Modelling - Sports Betting

---

## 1. Thought process
[Sports Betting Data Build.xlsx](https://github.com/user-attachments/files/23343003/Sports.Betting.Data.Build.xlsx)

It began with a simple curiosity, ***“How do betting companies come up with their odds ?”***  

I’d look at match predictions and wonder, ***“What magic formula makes one team 2.1 odds and another 3.6 ?”***  
That curiosity turned into a full-blown data exploration rabbit hole about 6 months ago. 

<img width="963" height="737" alt="image" src="https://github.com/user-attachments/assets/9fcc8359-405c-4abd-8896-f6515282920b" />


##### After a few YouTube tutorials, endless Google searches, and some late-night Excel experiments, I stumbled upon something called the **Poisson distribution** 

<img width="447" height="435" alt="image" src="https://github.com/user-attachments/assets/e2795bf8-b626-498e-9713-ad5065067193" />




It appears "poission distribution" is used in sports analytics to model events like ***goals, shots, or even wins*** - that’s when it became clearer.

My couriosity sparked, and then I decided to translate. 
I’ve come to realize that many of those basic math concepts we used to joke about back in secondary school actually have real-world value, we just were never shown how powerful they could truly be.

#### I started with getting current EPL and Laliga Standings 
It required a lot of data cleaning to ensure everything appeared properly, as I treated it as my “database” for efficiently calculating goal possibilities.

From there, I carefully
- _Rebuild the logic behind betting odds from scratch_
- _Use Excel formulars_


<img width="1440" height="736" alt="image" src="https://github.com/user-attachments/assets/6dd84302-32ee-45f9-8d66-5e1eabbdbf7a" />

Football is a game defined by goals, winning is almost impossible without scoring one.
I realized I needed an existing/ongoing league table to predict goals possibilities and averages. I first built the framework using random numbers and waited for Premier League Matchday 10 to test the model. This helped me see, psychologically, that my deep dive into the data could actually connect to real-world predictions.



#### The result ?  
- _A working mini “odds engine”_
- _Predicts match probabilities using historical goal averages_
- _Poisson modeling and some excel lookups_

<img width="1440" height="736" alt="image" src="https://github.com/user-attachments/assets/fdca30bc-3482-41c9-8572-167f1142bf51" />


##### Turned out to be understanding how numbers predicts the final story of a football game.


---


## 2. What this project is about ?

- I built a model for **predicting sports match outcomes** using statistics.  
- Apply **Poisson probability** to estimate event frequencies such as goals scored etc.  
- Use **Excel formulas** to automate lookups and link datasets across multiple sheets.  
- Demonstrate data cleaning, transformation, and structured modeling within Excel.

#### Spreadsheet contains

| Sheet                       | Description                                                                                                          |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Raw Data / Links**        | A table of team statistics, match results, or betting odds sourced online.         |
| **Model Sheet**             | Where you apply formulas like Poisson, INDEX/MATCH, and lookups to calculate expected outcomes.                      |
| **Analysis / Output Sheet** | Where predicted probabilities or match forecasts are displayed, maybe compared to actual results or bookmaker odds. |

Raw Data / Links	A table of team statistics, match results, or betting odds sourced online (CSV imports or manual entries).
Model Sheet	Where you apply formulas like Poisson, INDEX/MATCH, and lookups to calculate expected outcomes.
Analysis / Output Sheet	Where predicted probabilities or match forecasts are displayed, maybe compared to actual results or bookmaker odds.


---


## 3. The formulas and what they show about your approach

Here’s a summary of the key logic;

| Formula Type                                    | Purpose             | Analytical Skill Shown                     |
| ----------------------------------------------- | ------------------- | ------------------------------------------ |
| `=($B$39^D39)*EXP(-$B$39)/FACT(D39)`            | Poisson probability | Statistical thinking, probability modeling |
| `INDEX/MATCH` with multiple criteria            | Lookup automation   | Data modeling, relational data handling    |
| `SUM`               | Data summarization  | Exploratory analysis, aggregation          |
| Data Validation, Conditional Formatting | Add style    | Data preprocessing awareness  |


---


## 4. 💡 The Formula


```excel

=($B$39^D39)*EXP(-$B$39)/FACT(D39)

```

This is the Poisson Probability Mass Function (PMF). It sole purpose is to describe how rare events occur over a fixed interval.


| Element       | Meaning                                                                    | In Your Context                                                           |
| ------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `$B$39`       | λ (lambda) – the **average number of goals expected per match**            | e.g. if a team averages 2.4 goals per game, λ = 2.4                       |
| `D39`         | k – the **actual number of goals you’re testing the probability for**      | e.g. “What’s the chance they score exactly 3 goals?” → k = 3              |
| `EXP(-$B$39)` | e⁻ˡᵃᵐᵇᵈᵃ – the **base decay factor**; ensures total probabilities sum to 1 | represents how unlikely very high goal counts become                      |
| `$B$39 ^ D39` | λᵏ – raises the average rate to the number of events being tested          | adjusts the probability for your specific k-goal scenario                 |
| `FACT(D39)`   | k! – factorial of k (1×2×3...)                                             | normalizes the probability so it’s proportionate to all possible outcomes |

##### The Concept (In Plain English)

Imagine a team scores on average 2.4 goals per game.
You want to know: “What’s the probability they’ll score exactly 3 goals in their next match?”

Plug λ = 2.4 and k = 3 into the formula:

P(X=3) = (2.43)∗e−2.43!
        -----------------
                3!
	​

When you calculate this, you’ll get something like 0.214, meaning there’s roughly a 21.4% chance the team will score exactly 3 goals.
Ran the same query across all goal level home and away 

The result is 
<img width="1284" height="237" alt="image" src="https://github.com/user-attachments/assets/afb4d858-f91f-4075-b8a4-f2e6fb870cda" />


#### What I eventually realized ?

Bookmakers use this kind of model under the hood to generate odds, they don’t just pick numbers randomly,

- they estimate each team’s attack strength (average goals scored)
- estimate each team’s defense strength (average goals conceded)
- combine these to get the expected goals (λ) for each side.

##### Use the Poisson formula to find probabilities of all possible scorelines (0–0, 1–0, 2–1, etc.).
-  Convert those probabilities into odds.

For example:
If the Poisson model says a team has a 25% chance to win, odds = 1 / 0.25 = 4.0 odd.

##### So the formula is literally simulating how betting markets price games mathematically turning random occurrences into probabilistic insights.


---


## 5. Predicition Tab

The Prediction Sheet — The Heart of the Model

This is the sheet where all the analysis comes together
It connects your data tables, team stats, and Poisson logic into a working prediction model.

##### What It Does

- The Prediction sheet automates the process of generating expected outcomes for football matches based on team performance data
- It uses lookups averages
- Retrieve each team’s average goals scored and conceded
- Apply the Poisson distribution formula to those averages
- Generate match outcome predictions — who’s more likely to win, draw, or lose

##### How It Works Inside Excel

Lookup Logic: Using formulas like;

```excel

=INDEX($F$2:$F$41, MATCH(1, ($A$2:$A$41=K2)*($G$2:$G$41=I3), 0))

```

<img width="1440" height="736" alt="image" src="https://github.com/user-attachments/assets/9774741d-7b81-4be2-9f9b-fa566673fdae" />


---


## 6. Tools & Technologies

- Just Excel, lol.


---



## 7. Learnings

Been over 6months on figuring this out. 
Yes it took me so long, but I had other presonal stuffs lined up simultaneously 
This project taught me how curiosity can evolve into structured analysis. It started with a simple question
- How do betting companies come up with their odds ? 
- Led me into exploring probability, distributions

Most importantly, I learned that data analytics isn’t about coding alone, it’s about thinking critically, testing assumptions, and finding meaning behind the numbers you see on any screen.



---


## 8. Author

Obinna - (binnatheanalyst)
Data Analyst | Excel •  SQL  •  Python  •  Power BI  •  Tableau •

📫 [Connect with me on LinkedIn](https://www.linkedin.com/in/joseph-obinna-2a3b811b3/) OR

--
📫 [Connect with me on X](https://x.com/binnatheanalyst)

I'll be modifying the data from time.

But if you have any questions or suggestions as regards my solutions to this, you can send me a message on LinkedIn

Thank you for reading! Hopefully you'll read about my next case study too, right? 

See you soon.

