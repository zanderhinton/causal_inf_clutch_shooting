The Causal Impacts of ‘Clutch’ Situations on NBA Shooting Percentages
================
Alexander Hinton

### Background

  - talk about adrenaline and sweaty palms
  - Am I using playoff games?

Based on the perceived ability to perform in the most important
situations, fans, media members and players alike love to use the term
‘Clutch’ to describe certain players or teams. While highlight reels
and biased memories may make it seem like our favourite players always
deliver when the game is on the line, the reality is much different.
While the league average field goal percentage is around \(50\%\), a
recent article by Seth Partnow (link article in Athletic) showed that
the average shooting percentage in ‘clutch’ situations was only around
\(30\%\). Even players widely considered to be “clutch”, such as Kobe
Bryant, had clutch shooting percentages around this \(30\%\) accuracy.
The natural follow up question which arises is what makes these shots
have such a low conversion rate. There are two main drivers which I
hypothesize 1) the shots taken in these clutch situations are simply
more difficult shots in terms of distance from the basket and defensive
pressure, and 2) the intensity of the situation, with the crowd standing
on their feet, anticipation high in the building, adrenaline rising
causing faster heartrates and sweaty palms causes players to be less
accurate in their shooting. Of course, a combination of these two
effects is also possible.<br> I aim to do a causal inference style
analysis, to determine the causal impact a “clutch” situation has on
shooting in percetanges in the NBA. In terms of causal inference lingo,
the `treatment` group will be shot attempts deemed to be in clutch
situations, while the control group will be all shot attempts outside of
these situations, where the stakes, pressure and weight of the moment
are assumed to be significantly less. In order to isolate potential
causal impacts I will use propensity score matching, as well as a more
handpicked case-control matching strategy to find pairs of shots deemed
to be very similar, with the difference being whether each shot was in a
clutch situation or not.

#### Defining clutch

Before continuing, the definition of what constitutes a clutch shot in
this analysis needs to be established. I delegated any shot in the last
30" of a game, where the score was within 3 points (a one possession
game), as a clutch shot. These are the shots where a timeout will often
be takenbeforehand, the crowd is all on their feet, and nerves and
pressure of the moment would be highest.

#### Gathering data

In order to estimate a potential causal impact of the clutch situation
on shooting accuracy, the difficulty of the shot must be controlled for.
This requires quite granular data about each shot attempt. Fortunately,
the website NBA-savant\[1\] provides detailed data on all NBA shots,
including shot distance, distance to the closest defender, name of the
closest defender, number of dribbles before the shot, etc.
Unfortunately, the NBA stopped releasing this data in January 2016,
meaning the most recent NBA seasons could not be used in this analysis.
I used shots from the 2013-2014 season, 2014-2015 season, and 2015-2016
season until the granular shot data stopped being released in this
analysis. <br>

While game clock information was available from the shot data, the score
at the time the shot was taken was not. Using my definition of clutch
from above, this score information is required. This was done using
play-by-play data\[2\], matching on the shot, and adding in the
additional info unavailable in the shot charts. Approximately 80% of
shots were able to matched with shot events in the play by play data
(small discrepancies in game clock at the time of shot mean not all
shots were matched). Once I had gathered the shot information I
separated shots into four categories: clutch and non-clutch twos, and
clutch and non-clutch threes. For two pointers, I only included shots
that were at least 10 feet, as I wanted to be comparing jumpers, and did
not think the same clutch effect would apply to lay-ups, dunks,
put-backs etc. The control group of non-clutch shots consisted of any
shot taken in the first three quarters of a gameMy comparison group of
non-clutch shots were shots taken in the first thre quarters of any
game.

#### Summary statistics - two point shots

<table>

<thead>

<tr>

<th style="text-align:right;">

isClutch

</th>

<th style="text-align:right;">

fg\_percentage

</th>

<th style="text-align:right;">

defender\_height

</th>

<th style="text-align:right;">

dribbles

</th>

<th style="text-align:right;">

touch\_time

</th>

<th style="text-align:right;">

shot\_dist

</th>

<th style="text-align:right;">

defend\_dist

</th>

<th style="text-align:right;">

n

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

0

</td>

<td style="text-align:right;">

40.3

</td>

<td style="text-align:right;">

79.0

</td>

<td style="text-align:right;">

2.4

</td>

<td style="text-align:right;">

3.2

</td>

<td style="text-align:right;">

15.5

</td>

<td style="text-align:right;">

4.5

</td>

<td style="text-align:right;">

93458

</td>

</tr>

<tr>

<td style="text-align:right;">

1

</td>

<td style="text-align:right;">

23.8

</td>

<td style="text-align:right;">

78.6

</td>

<td style="text-align:right;">

6.0

</td>

<td style="text-align:right;">

6.5

</td>

<td style="text-align:right;">

16.9

</td>

<td style="text-align:right;">

3.9

</td>

<td style="text-align:right;">

408

</td>

</tr>

</tbody>

</table>

#### Summary statistics - three point shots

<table>

<thead>

<tr>

<th style="text-align:right;">

isClutch

</th>

<th style="text-align:right;">

fg\_percentage

</th>

<th style="text-align:right;">

defender\_height

</th>

<th style="text-align:right;">

dribbles

</th>

<th style="text-align:right;">

touch\_time

</th>

<th style="text-align:right;">

shot\_dist

</th>

<th style="text-align:right;">

defend\_dist

</th>

<th style="text-align:right;">

n

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

0

</td>

<td style="text-align:right;">

35.8

</td>

<td style="text-align:right;">

78

</td>

<td style="text-align:right;">

1.0

</td>

<td style="text-align:right;">

1.8

</td>

<td style="text-align:right;">

24.8

</td>

<td style="text-align:right;">

6.1

</td>

<td style="text-align:right;">

75186

</td>

</tr>

<tr>

<td style="text-align:right;">

1

</td>

<td style="text-align:right;">

22.7

</td>

<td style="text-align:right;">

78

</td>

<td style="text-align:right;">

2.2

</td>

<td style="text-align:right;">

2.7

</td>

<td style="text-align:right;">

26.7

</td>

<td style="text-align:right;">

4.6

</td>

<td style="text-align:right;">

484

</td>

</tr>

</tbody>

</table>

Clearly there are some big differences in shooting percentages between
non-clutch and clutch shots for both two pointers and three pointers.
While the differences in average accuracy between the two groups are
quite striking, the differences in average degree of difficulty between
the shot types is also glaring. For both two pointers and three
pointers, the average clutch shot attempt is from farther away, has a
closer defender and is off the back of a longer individual effort (more
dribbles and a longer touch time). These in total make it obvious that
average clutch shots three point attempts are much harder than average
non clutch attempts, and cannot be compared directly. A strategy to
account for the difficulty of a shot is needed in order to make a fair
comparison between a clutch shot and a non-clutch shot when trying to
estimate a causal impact of the clutch situation on shooting accuracy.

### Maybe include here the bias function thing …?

#### Causal Inference in Observational Studies

In an ideal world, randomized control trials are used to estimate the
causal impact of a treament. In these situations, the treatment of
interest is randomly allocated to the observational units (often people)
in the study. Some outcome measure is calculated for both treatmtent and
control groups, and the difference in this outcome between groups is
used as an estimate of the efficacy of the treatment. Due to the
randomized nature of treatment assignment, the chance of biased
estimates due to confounding is minimized. However experimental trials
are often impractical, and observational studies can be used to estimate
causal treatment effects in certain situations. The main drawback, and
danger when using observational studies for causal inference is the
potential for bias estimates due to confounding. In an observational
study, treatment assignments have not been done randomly, which can be a
problem if there are both outside variables associated with both the
treatment variable, and the outcome variable. These are called
confounders. For instance, in a study looking at the effectiveness of a
diet rich in vegetables on health outcomes it would be naive to simply
compare the outcomes of individuals eating veggie rich diets versus
those who are not. As the individuals eating more veggies may also be
excercising more, perhaps have higher incomes, or other differences
which may also be associated with higher health outcomes. This potential
bias in estimates due to confounding variables is the main difficulty of
performing causal inference on observational studies where treatments
have not been explicitly assigned.

#### Matching strategies to estimate causal impacts

Replicating a randomized experiment as closely as possible is the goal
when performing causal inference on observational data. Matching
strategies are often used in this attempt. In matching studies,
observations which receieved the treatment are matched as closely as
possible to individuals in the control group, in terms of the other
observed covariates.

In this analysis the treatment group (the clutch shots), clearly have
different levels in some of the covariates of interest than the control
groups (the non clutch shots). As seen above, defender distance, number
of dribbles, touch time, etc. are all different between the treatment
group levels.

I attempted two different matching strategies in order to estimate a
causal effect, the first was a brute force matching approach, and the
second was via propensity scores. I will first explain the brute-force
matching approach, and then discuss propensity scores.

In the brute force matching, I considered the following covariates to be
important when quantifying the difficulty of a shot: shot distance,
defender distance, defender, number of dribbles, and touch time. For
each shot in the clutch threes shot dataframe, I attempted to find a
shot in the non-clutch dataframe which would match on: \* name: must be
the same shooter \* season: must be in the same season \* shot distance:
+/- 1 foot \* defender distance, +/- 0.5 feet \* number of dribbles, +/-
1 \* touch time, +/- 0.5 seconds \* defender height, +/- 1 inch (Note:
matching explicitly on who was the defender made the results very
sparse, so I instead calculated the height of the closest defender, and
matched on defender height. While not exact, I think matching on the
distance to the closest defender plus the height of the defender
captures the degree the shot was contested quite well. \* shot type: In
the play-by-play data, a shot can be classified as a “jump shot”,
“fadeaway jump shot” etc., so I also matched on shot-type to control
for difficulty. All told, of the 484 clutch three point shot attempts
identified and 409 clutch two point attempts, 170 of the threes, but
only 72 of the two point attempts could be matched to a non-clutch shot
attempt using this brute force matching criteria. Note: if more than 1
non-clutch shot matched, I would randomly select one as the matching
shot.

The summary statistics of the brute force matching for these two
pointers and three pointers are listed below:

    ## Warning: Missing column names filled in: 'X1' [1]

<table>

<thead>

<tr>

<th style="text-align:right;">

isClutch

</th>

<th style="text-align:right;">

fg\_percentage

</th>

<th style="text-align:right;">

defender\_height

</th>

<th style="text-align:right;">

dribbles

</th>

<th style="text-align:right;">

touch\_time

</th>

<th style="text-align:right;">

defend\_dist

</th>

<th style="text-align:right;">

n

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

0

</td>

<td style="text-align:right;">

34.7

</td>

<td style="text-align:right;">

79.4

</td>

<td style="text-align:right;">

1.9

</td>

<td style="text-align:right;">

2.6

</td>

<td style="text-align:right;">

4.2

</td>

<td style="text-align:right;">

72

</td>

</tr>

<tr>

<td style="text-align:right;">

1

</td>

<td style="text-align:right;">

20.8

</td>

<td style="text-align:right;">

79.4

</td>

<td style="text-align:right;">

1.9

</td>

<td style="text-align:right;">

2.5

</td>

<td style="text-align:right;">

4.1

</td>

<td style="text-align:right;">

72

</td>

</tr>

</tbody>

</table>

    ## Warning: Missing column names filled in: 'X1' [1]

<table>

<thead>

<tr>

<th style="text-align:right;">

isClutch

</th>

<th style="text-align:right;">

fg\_percentage

</th>

<th style="text-align:right;">

defender\_height

</th>

<th style="text-align:right;">

dribbles

</th>

<th style="text-align:right;">

touch\_time

</th>

<th style="text-align:right;">

defend\_dist

</th>

<th style="text-align:right;">

n

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

0

</td>

<td style="text-align:right;">

34.7

</td>

<td style="text-align:right;">

77.7

</td>

<td style="text-align:right;">

0.4

</td>

<td style="text-align:right;">

1.3

</td>

<td style="text-align:right;">

4.9

</td>

<td style="text-align:right;">

170

</td>

</tr>

<tr>

<td style="text-align:right;">

1

</td>

<td style="text-align:right;">

25.9

</td>

<td style="text-align:right;">

77.8

</td>

<td style="text-align:right;">

0.5

</td>

<td style="text-align:right;">

1.2

</td>

<td style="text-align:right;">

4.8

</td>

<td style="text-align:right;">

170

</td>

</tr>

</tbody>

</table>

Even after controlling for shot difficult based on the observed
covariates, the shooting percentages of the clutch shots are about
10-15% lower than the non-clutch shot attempts. The next step is to
estimate a logistic regression model to see if these differences are
statistically significant.

    ## 
    ## Call:
    ## glm(formula = shot_made_flag ~ isClutch, family = "binomial", 
    ##     data = matched_twos)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -0.9236  -0.9236  -0.6835   1.4545   1.7712  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)  -0.6313     0.2475  -2.550   0.0108 *
    ## isClutch     -0.7037     0.3814  -1.845   0.0650 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 170.16  on 143  degrees of freedom
    ## Residual deviance: 166.67  on 142  degrees of freedom
    ## AIC: 170.67
    ## 
    ## Number of Fisher Scoring iterations: 4

### Propensity score matching

As I read in the causal inference literature, propensity score matching
is a standard statistical approach to carry out a similar matching
strategy to what I outlined above, but in a statistical model and not
rule-based way. In propensity score matching, the first step is to
perform logistic regression, with the treatment level (isClutch) as the
dependent variable, and the covariates as the independent variables.
This shows how likely each observation was to be a assigned to the
treatment. If there are no major differences between the treatment
assignments, then no adjustments need to be made. But if there are, then
adjustments need to be made.

## Including Plots

You can also embed plots, for example:

![](report_files/figure-gfm/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.
