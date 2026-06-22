# Milestone 1: Community and Label Design

## Community Analysis

I chose r/formula1 as my target community. After reviewing recent posts and discussions, I observed three major styles of discourse. Some users provide detailed analysis using race strategy, technical explanations, regulations, or performance data. Others post strong opinions or predictions that are intended to spark debate but provide little supporting evidence. A third group consists of emotional reactions to races, team decisions, driver performances, and Formula 1 news.

What makes some posts more substantive than others is the amount of reasoning and evidence provided. Regular members of the community generally recognize detailed explanations and strategic analysis as higher-quality discussion, while unsupported claims and emotional reactions are viewed as less informative, even when they generate engagement.

## Label Taxonomy

### Label 1: Analysis

Definition:

A post that supports its claims with evidence, technical reasoning, strategy discussion, statistics, or detailed explanation.

Clear Examples:

1. "McLaren's pace advantage came from superior tire management during the final stint, allowing them to maintain lap times while Red Bull's degradation increased."

2. "Ferrari's undercut failed because they pitted after the optimal tire crossover window, giving competitors track-position advantage."

Uncertain Example:

"Verstappen only wins because Red Bull has the fastest car."

Reason:

If the post provides evidence and reasoning, it is Analysis. If it is merely an unsupported assertion, it belongs to Hot Take.

### Label 2: Hot Take

Definition:

A strong opinion, prediction, or controversial claim expressed with little or no supporting evidence.

Clear Examples:

1. "Lando Norris will never win a Formula 1 championship."

2. "Hamilton is the most overrated driver in Formula 1 history."

Uncertain Example:

"Verstappen is overrated because most of his wins came in a dominant car."

Reason:

This contains a small amount of supporting information, but the primary purpose is making a provocative claim rather than presenting a balanced argument.

### Label 3: Reaction

Definition:

An emotional response to a race event, driver performance, team decision, or Formula 1 news.

Clear Examples:

1. "What an incredible finish! I can't believe Piastri pulled that off."

2. "Ferrari ruined another race weekend. This is painful to watch."

Uncertain Example:

"That strategy call was terrible."

Reason:

If the post only expresses frustration, it is Reaction. If it explains why the strategy was poor, it becomes Analysis.

## Hard Edge Case

One difficult category involves posts that criticize a team or driver while also providing some reasoning.

Example:

"Ferrari lost the race because they pitted Leclerc too early and the hard tires never reached operating temperature."

This could be interpreted as either Reaction or Analysis.

Decision Rule:

If the reasoning can stand on its own as an explanation of the event, label the post as Analysis. If the primary purpose is expressing emotion, frustration, or celebration, label it as Reaction.

## Mutual Exclusivity Check

The labels are designed to be mutually exclusive.

- Analysis focuses on evidence and reasoning.
- Hot Take focuses on unsupported opinions and predictions.
- Reaction focuses on emotional responses.

When a post contains characteristics of multiple labels, the decision rule prioritizes the author's primary purpose. This allows most posts to be assigned to exactly one label.

## Community Description

The r/formula1 community contains a mixture of technical race analysis, controversial opinions, and emotional reactions to Formula 1 events. This project classifies posts into Analysis, Hot Take, and Reaction because these categories represent meaningful differences in how Formula 1 fans communicate. Distinguishing between these discussion styles helps measure the quality and substance of discourse within the community.

---

# Milestone 2: Project Specification

## Data Collection Plan

Data will be collected from public comments and posts within the r/formula1 subreddit.

Target dataset size: 200 labeled examples.

Final dataset size: 354 labeled examples.

Target distribution:

- Analysis: approximately 70 examples
- Hot Take: approximately 65 examples
- Reaction: approximately 65 examples

Because Formula 1 discussions naturally contain many reactions, I expected Reaction examples to be easier to collect than Analysis examples.

If one label was underrepresented after collecting 200 examples, I planned to continue collecting additional examples specifically for that category until the distribution became more balanced.

The final dataset is stored as a CSV file containing:

- text
- label
- notes

The notes column was used to document difficult or ambiguous examples.

---

## Evaluation Metrics

Accuracy was used as an overall measure of performance, but accuracy alone is not sufficient because the dataset may contain class imbalance.

### Precision

Precision measures how often predictions for a label are correct.

### Recall

Recall measures how many true examples of a label are successfully identified.

### F1 Score

F1 Score balances precision and recall and provides a more complete picture of performance for each label.

### Confusion Matrix

The confusion matrix shows which labels are most commonly confused.

I expected the largest confusion to occur between Analysis and Hot Take because both may contain opinions and Formula 1 terminology.

---

## Definition of Success

The classifier would be considered successful if it met the following criteria:

- Overall test accuracy of at least 75%
- F1 score of at least 0.70 for each label
- Fine-tuned model outperforming the zero-shot baseline by at least 10 percentage points
- No single label having an F1 score below 0.60

For a real community moderation or analytics tool, I would consider the classifier good enough for deployment if it achieved:

- At least 80% accuracy
- At least 0.75 F1 score for all labels
- Consistent performance across all classes without heavily favoring one label

---

## AI Tool Plan

### Label Stress-Testing

Before beginning annotation, I provided my label definitions and edge case rules to ChatGPT and asked it to generate borderline Formula 1 comments that could belong to multiple categories.

### Annotation Assistance

ChatGPT was used to suggest labels for difficult examples during annotation.

All labels were reviewed manually before inclusion in the dataset.

### Failure Analysis

After evaluating the model, I used ChatGPT to help identify common error patterns.

Potential patterns included:

- Analysis confused with Hot Take
- Short comments lacking context
- Emotionally charged comments with supporting evidence
- Driver-specific or team-specific bias

All identified patterns were verified manually before inclusion in the evaluation report.

---
## Difficult Annotation Examples

### Example 1

Comment:

> Verstappen only wins because Red Bull has the fastest car.

Possible labels:
- Analysis
- Hot Take

Decision:
Hot Take

Reason:
The comment makes a broad claim without providing evidence or detailed reasoning.

### Example 2

Comment:

> Ferrari ruined another race with that strategy.

Possible labels:
- Reaction
- Analysis

Decision:
Reaction

Reason:
The comment expresses frustration but does not explain why the strategy failed.

### Example 3

Comment:

> Championship back on.

Possible labels:
- Reaction
- Hot Take

Decision:
Hot Take

Reason:
The comment is making a prediction about the championship battle rather than expressing emotion.

---
# Milestone 3 Reflection

During data collection, I expanded the dataset beyond the original target of 200 examples and ultimately collected 354 Formula 1 comments and posts.

One challenge I encountered was class imbalance. Many Formula 1 discussions naturally contain technical explanations, race strategy discussion, and news-related content, which caused the Analysis category to be overrepresented during early annotation.

To address this issue, I collected additional examples that fit the Reaction and Hot Take categories. Despite these efforts, creating a perfectly balanced dataset proved difficult because many comments contained characteristics of multiple labels.

This challenge highlighted the importance of clear label definitions and consistent annotation rules.

---

# Final Evaluation Reflection

The final model did not fully achieve the success criteria defined earlier in this document.

Original success criteria:

- Overall accuracy of at least 75%
- F1 score of at least 0.70 for each label
- Fine-tuned model outperforming the baseline by at least 10 percentage points
- No label with F1 below 0.60

In practice, the model learned to distinguish Analysis and Reaction better than Hot Take. The confusion matrix showed that Hot Take remained the most difficult category to identify. Many Hot Take examples were predicted as either Analysis or Reaction.

The primary reason appears to be overlap between the labels. Formula 1 discussions often contain strong opinions mixed with technical terminology, making it difficult to separate unsupported opinions from genuine analysis.

Future work would focus on collecting more clear Hot Take examples, reducing label ambiguity, and improving class balance.

### Key Lesson Learned

Designing good labels and collecting consistent training data is often more important than the choice of machine learning model. Many of the model's errors can be traced back to ambiguous examples rather than limitations of DistilBERT itself.