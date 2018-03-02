---
layout: post
title:  "Managing The Technical Debt Risk"
date:   2012-01-13 21:03:13
categories: blog
---

The technical debt metaphor was first coined by Ward Cunningham in 1992 while drawing a comparison between the technical complexity of IT projects and debt in general. There are multiple definitions that exist and if I had to put in my own, here’s how I would lay it down.

> “When technical work is delayed knowingly or unknowingly either to meet specific deadlines or  to save time and/ or effort which must be eventually done a technical debt is incurred.”

Like debt in general, where it is incurred to meet valid business objectives. There may also be valid cases where this technical debt is incurred for perfectly valid business reasons.Regardless of the reason, debt eventually needs to be paid off and a technical debt is not an exception as well.

However, unlike the financial debt, the technical debt is almost impossible to measure accurately.You do not have specific interest rates or payment schedules but nevertheless it is like any other debt and needs to be paid off (except in a few rare cases which we will touch upon later)

# So How Does a Project Incur Technical Debt?

There may be a variety of causes but a few prominent ones could include

* **Poor System Design** – Most projects do not do a adequate job during the architecture assessment phase or skip the process altogether. This by far has been the leading cause of incurring a technical debt early on, something I have seen this over and over again.
* **Lack of Business Process Understanding** – The second most common cause of incurring technical debt is a lack of understating the business process for which the IT system is being build. I have seen cases where the IT team has to put in time later on to understand the business process rather than investing time early on thereby incurring higher cost to pay off the debt.
* **Inadequate or inaccurate requirements** – Remember technical debt is incurred both willingly and unwillingly. In case the system itself is being designed and built on incorrect or inadequate requirements, there is a strong likely hood of your project will incurring a technical debt during its life-cycle.
* **Lack of coding standards or processes** – Skipping standard coding practices or ignoring standards is another leading cause of technical debt.

# Why is Technical Debt a risk?

If I were to rephrase the question it would be why is technical debt a risk and not an issue ? After all if technical debt has been identified and recognized, it has to be paid off. In other words it has already materialized into an issue and hence should no longer be classified as a risk alone. However for now I am still inclined to manage this as a risk as I am still uncertain on its impacts and the cost to get to pay off this debt. Also often overlooked is the opportunity cost of the technical debt or the ‘positive’ risks or in simple terms just plain opportunities.

# Identifying The Technical Debt

Like with other project risks, risk identification is the first step here as well. Your ability to proactively identify the technical debt early on will directly influence your ability to effectively manage the technical debt. Identification can be both proactive and reactive.

* **Proactive Identification** – Proactive identification includes process within the project that enable you to look out for technical debts before your project or system actually starts experiencing the symptoms of technical debt. Proactive process include code reviews, architecture assessments and design reviews to name a few.
* **Reactive Identification** – Reactive identification is when the project team starts the identification process based on symptoms of technical debt like a constant increase in the number of defects or when you are constantly overshooting your time-lines. Reactive identification could involve process like defect analysis and issue root cause analysis. For obvious reasons reactive identification will most certainly incur higher pay-off costs.

# Technical Debt Assessment
Risk assessment traditionally has focused on two key variables, the severity of the risk and the probability of its occurrence. This is mathematically represented as ‘Severity * Probability’. Assessing a technical debt is a little different. The variables that play in here include

* **Quantifiable Debt** – The technical debt needs to be quantified. You can do this by assigning weights or by other variables like lines of code. Either way whats is important is have a quantifiable value that can be used for assessment.
* **Pay-off Cost** – This should include the actual cost of going back and fixing the code or design as well the complexity and effort involved. For example if the cost of fixing technical debt involves the risk of introducing additional defects that should be accounted as well.
* **The Interest** –  Like debt in general, technical debt also accumulates interest. The interest in case of technical debt could include the costs to maintain a system that been coded or designed badly.

# The Technical Debt Risk Strategies
Once you have identified and assessed your technical debt successfully it’s now time to have a strategy or a plan to manage these.

## Avoid The Debt

If you can avoid incurring technical debt altogether in the first place, that would be an ideal scenario, but an ideal scenario is anything but common. That said, there are a certain options that the project team has like adopting for a standardized solution as opposed to a highly customized solutions will significantly reduce the probability of incurring technical debt.

## Mitigate

There is a three phased approach to this strategy

* **Mitigate the pay-off cost of the technical debt** – The cost involved to fix a bad system design or dirty code can be both high and also potentially add in risk. Based on your risk assessment above you should have a plan to pay off your most profitable debts first.
* **Mitigate the interest incurred from the existing debts** – The interest here is a metaphor to the cost of maintaining a system that is subject to high technical debt.
* **Stop incurring new debts** – As you work on mitigating the impact and the cost of the technical technical debt, you must take or build in steps to ensure you do not incur any new technical debts. So as you work towards managing your technical debt, watch your corners and build this into your process.

## Acceptance or Retention
There may be a rare scenario where you will probably end up choosing to accept the risk related to technical debt and the associate cost to pay it off. In such scenarios a trade-off is made between the risks associated with the technical debt and the cost associated to pay off the debt. These scenarios could include

* **When it is NOT profitable to pay off the debt** –  An example could be where the system in question is due to retire very soon. In such a case it might make sense to accept the cost to maintain the system rather than incurring cost to pay-off the debt.
* **When there is a positive opportunity cost involved** – Let’s face it, resources and budget are never unlimited. Programs will be faced with a dilemma where they will have to choose between paying-off a technical debt and an opportunity. This is where the opportunity cost factor comes in. If the cost paying-off of the technical debt is significantly lower than the cost of the missed opportunity of not undertaking another initiative, programs may choose to accept or defer the technical debt.

Again these are rare scenarios, which need to be evaluated carefully before adopting a retention strategy.
Lastly the timing and decision to pay off a technical debt will vary with organizations and departments. This will also depend on the tolerance level these organizations and departments have towards the technical debt as well as the trade-offs involved in paying -off the debt.
