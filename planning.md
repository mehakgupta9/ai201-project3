# TakeMeter Planning

## Community

### Selected Community

I chose the football discussion community from Reddit, primarily discussions from r/soccer and related football match discussion threads.

### Why This Community?

Football discussions contain a wide variety of discourse styles. Some users provide detailed tactical analysis, statistics, and match insights. Others make bold claims about players, managers, or teams without evidence. Many comments are purely emotional reactions to goals, refereeing decisions, or match outcomes.

This variety makes football discussions an excellent classification task because the differences between thoughtful analysis, unsupported opinions, and emotional reactions are meaningful to community members and occur frequently.



## Label 1: `analysis`

### Definition
A comment that supports its opinion with football knowledge, tactical reasoning, statistics, or specific examples.

### Characteristics
- Explains **why** something happened.
- References tactics, formations, player roles, statistics, or historical context.
- Uses evidence or reasoning to support a claim.
- Focuses on analysis rather than emotion.

### Examples
- Arsenal struggled because their midfield couldn't progress the ball through Liverpool's press.
- Haaland scores a lot, but City's chance creation drops significantly when De Bruyne is unavailable.
- Real Madrid controlled possession by overloading the left side and forcing Bayern's midfield to shift.
- Liverpool's high defensive line works because their center backs recover quickly in transition.

---

## Label 2: `hot_take`

### Definition
A strong or controversial football opinion that is asserted confidently but lacks sufficient reasoning or evidence.

### Characteristics
- Makes bold predictions or judgments.
- Provides little or no supporting evidence.
- Often designed to provoke disagreement or debate.
- Relies primarily on opinion rather than analysis.

### Examples
- Haaland would be average on any team other than Manchester City.
- Arteta will never win the Champions League.
- Manchester United will finish outside the top ten this season.
- Jude Bellingham is already better than every midfielder of the last generation.

---

## Label 3: `reaction`

### Definition
An emotional, humorous, celebratory, or frustrated response that does not attempt to make a substantive argument.

### Characteristics
- Expresses feelings or excitement.
- Often short and spontaneous.
- Does not explain or justify an opinion.
- Includes jokes, celebrations, complaints, or shock.

### Examples
- WHAT A GOAL!!!
- This referee is a joke.
- No way that just happened 😭
- We are so back!
- Absolute scenes.

---

## Annotation Rules

### Choose `analysis` when:
- The comment explains a football event, result, player performance, or tactical decision.
- The author provides reasoning, evidence, statistics, or football-specific knowledge.

### Choose `hot_take` when:
- The comment makes a bold claim without meaningful support.
- The statement is primarily opinion-based and controversial.

### Choose `reaction` when:
- The comment mainly expresses emotion, excitement, frustration, humor, or disbelief.
- There is no substantial reasoning or argument.

---


## Edge Cases

### Analysis vs. Hot Take

**Analysis**
> Arsenal's buildup struggles against low blocks because they lack a consistent aerial threat in the box.

Why?
- The comment gives a specific football explanation.
- It identifies a tactical problem (buildup against low blocks).
- It provides reasoning (lack of an aerial threat).
- Even if someone disagrees, the argument can be evaluated using football knowledge.

**Hot Take**
> Arsenal will never win the Premier League under Arteta.

Why?
- The comment makes a strong prediction.
- No evidence or reasoning is provided.
- The statement is based on confidence rather than analysis.

**Decision Rule:**
If a comment explains *why* something is happening using tactics, statistics, or football reasoning, label it **analysis**. If it mainly makes a bold claim without support, label it **hot_take**.

---

### Hot Take vs. Reaction

**Hot Take**
> Rashford is the most overrated player in Europe.

Why?
- The comment expresses a broad opinion about a player.
- It is intended as a judgment rather than an immediate emotional response.
- No evidence is provided, making it a hot take.

**Reaction**
> Rashford was awful today.

Why?
- The comment reacts to a specific match or performance.
- It expresses frustration or disappointment.
- It does not attempt to make a larger claim about the player overall.

**Decision Rule:**
If the comment makes a sweeping claim about a player, manager, or team, label it **hot_take**. If it simply reacts to a recent event or performance, label it **reaction**.

---

### Analysis vs. Reaction

**Analysis**
> The defense collapsed after the substitution because the team lost midfield coverage.

Why?
- The comment identifies a cause-and-effect relationship.
- It explains why the defense struggled.
- The reasoning is football-specific and can be evaluated.

**Reaction**
> Our defense is terrible.

Why?
- The comment expresses frustration.
- No explanation is given.
- It focuses on emotion rather than reasoning.

**Decision Rule:**
If the comment provides an explanation or argument, label it **analysis**. If it only expresses a feeling, praise, frustration, or excitement, label it **reaction**.

---
## Mutual Exclusivity Check

Every comment receives exactly one label.

Analysis:
Contains football reasoning, tactics, statistics, or evidence.

Hot_take:
Contains a strong opinion without sufficient support.

Reaction:
Primarily expresses emotion, celebration, frustration, or humor.

---

## Data Collection Plan

### Data Sources

I will collect comments from:

- r/soccer
- Premier League match threads
- Champions League match threads
- Post-match discussion threads
- Transfer discussion threads

### Dataset Size

**Minimum dataset size:** 200 examples

### Final Dataset Distribution

The final annotated dataset contained:

| Label | Count |
|---------|---------:|
| Analysis | 89 |
| Hot Take | 68 |
| Reaction | 43 |

Although reaction comments were less common than originally planned, no class exceeded 70% of the dataset.

### Handling Class Imbalance

If one label becomes underrepresented:

- Collect additional examples specifically from threads where that label naturally occurs.
- Continue sampling until each label represents at least 20% of the dataset.
- Avoid oversampling by duplicating comments.

---

## Evaluation Metrics

### Overall Accuracy

Accuracy measures the percentage of comments classified correctly.

### Per-Class Precision

Precision helps determine whether the classifier is assigning labels correctly, especially for hot takes which may be confused with reactions.

### Per-Class Recall

Recall measures whether the classifier successfully identifies all examples of each label.

### F1 Score

F1 combines precision and recall and provides a balanced measure of performance.

### Confusion Matrix

The confusion matrix helps identify systematic mistakes, such as confusing hot takes with reactions.

### Why Accuracy Alone Is Not Enough

A model could achieve high accuracy simply by predicting the most common label. Per-class metrics are needed to verify that all discourse categories are being recognized effectively.

### Definition of Success

#### Minimum Success

- Accuracy ≥ 70%
- F1 score ≥ 0.65 for every label

#### Strong Success

- Accuracy ≥ 80%
- Macro F1 ≥ 0.75
- No label has recall below 70%

### Real-World Deployment Threshold

For use as a moderation or discourse-analysis tool, I would want:

- Accuracy ≥ 80%
- Macro F1 ≥ 0.75
- Consistent performance across all three labels

The model does not need perfect agreement with human annotators because discourse quality is inherently subjective.


## AI Tool Plan

### Label Stress-Testing

Before beginning annotation, I will use Claude to test my label definitions and edge case rules. I will provide Claude with the definitions for analysis, hot_take, and reaction and ask it to generate 5–10 football comments that sit near the boundaries between labels.

If Claude generates examples that I cannot classify confidently, I will revise my label definitions and decision rules before collecting and annotating my dataset.

---

### Annotation Assistance

I will use Claude to suggest labels for some football comments during the annotation process. These labels will only serve as initial suggestions.

Every example will be reviewed manually, and I will make the final labeling decision myself. Any examples that were initially pre-labeled by Claude will be documented in the project's AI usage section for transparency.

---

### Failure Analysis

After evaluating the model, I will use Claude to analyze incorrectly classified examples and identify possible error patterns.

For example, I will ask Claude whether the model tends to:
- Confuse emotional analysis with hot takes.
- Misclassify short comments as reactions.
- Mistake player criticism for hot takes.

I will manually review all suggested patterns and verify that they are supported by the actual prediction results before including them in my evaluation report.



## Difficult Annotation Cases

### Example 1

**Comment:**
> Germany had lower possession than Switzerland or Turkey in their respective games.

**Why It's Tricky:**
This comment only states a statistic and does not provide an explicit argument or conclusion. It could be interpreted as a neutral observation rather than analysis.

**Possible Labels:**
- analysis
- reaction

**Final Label:**
`analysis`

**Reasoning:**
Although the comment does not explicitly explain the significance of the statistic, it contributes factual football information and encourages analytical discussion. It is more substantive than an emotional reaction and therefore fits best under analysis.

---

### Example 2

**Comment:**
> Great start for Morocco... they're playing some beautiful football.

**Why It's Tricky:**
The comment references the quality of play, which could appear analytical at first glance. However, it does not explain why Morocco is playing well or provide any football reasoning.

**Possible Labels:**
- analysis
- reaction

**Final Label:**
`reaction`

**Reasoning:**
The comment primarily expresses admiration and excitement about the team's performance. Since it lacks explanation, evidence, or tactical reasoning, it is classified as a reaction.

---

### Example 3

**Comment:**
> Vini was the only Brazilian who looked like an athlete.

**Why It's Tricky:**
The comment criticizes the overall Brazilian team and could be interpreted as an observation about player performance. However, it provides no evidence or explanation to support the claim.

**Possible Labels:**
- hot_take
- reaction

**Final Label:**
`hot_take`

**Reasoning:**
The comment makes a strong judgment about the players without providing reasoning or supporting evidence. Because it is a broad evaluative claim rather than an emotional reaction to a specific moment, it is classified as a hot take.


