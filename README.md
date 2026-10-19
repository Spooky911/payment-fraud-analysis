# Payment fraud: what deserves a closer look?

## Download the Power BI files

- [Credit card report (.pbix)](https://github.com/Spooky911/payment-fraud-analysis/releases/download/v1.0.0/Credit.Card.Fraud.Analysis.-.V2.-.Reviewed.pbix)
- [Financial transactions report (.pbix)](https://github.com/Spooky911/payment-fraud-analysis/releases/download/v1.0.0/Financial.Transactions.-.Fraud.Study.pbix)
- [Full project package, including ModelData](https://github.com/Spooky911/payment-fraud-analysis/releases/tag/v1.0.0)

Open either PBIX file in Power BI Desktop. The package also includes the prepared tables used by the financial report.

A Power BI study of two transaction datasets, with a short presentation that brings the findings together.

The question behind this work was practical: **if a team can only review a limited number of payments, which patterns are worth its time?** Counting fraud cases is useful, but it leaves out two things: how many legitimate customers would also be flagged, and how much payment value those cases represent.

This project follows those questions through payment size, timing, repeated amounts and account history. The result is two interactive reports and a comparison that explains where the evidence is useful—and where it stops.

## Start here

- [The comparison and recommendations](https://github.com/Spooky911/payment-fraud-analysis/blob/main/Fraud%20Behaviour%20-%20Comparative%20Presentation.pdf)
- [Credit card study — 15 pages](https://github.com/Spooky911/payment-fraud-analysis/blob/main/Credit%20Card%20Fraud%20-%20Analytical%20Report.pdf)
- [Financial transactions study — 8 pages](https://github.com/Spooky911/payment-fraud-analysis/blob/main/Financial%20Transactions%20-%20Fraud%20Study.pdf)
- **Editable Power BI files:** download the project package from this repository's Releases page. The package keeps the large PBIX files out of Git history and includes the financial report's supporting ModelData folder.

## The two datasets

| | Credit card transactions | Financial transactions |
|---|---:|---:|
| Transactions | 284,807 | 5,000,000 |
| Labelled fraud | 492 | 179,553 |
| Fraud rate | 0.173% | 3.591% |
| Time coverage | 47 hours, 59 minutes, 52 seconds | 1 January 2023–1 January 2024 |
| Useful context | Elapsed seconds, amount, fraud label | Timestamps, account IDs, payment context and supplied scores |

**These are separate populations, not a before-and-after experiment.** The first project's folder says “2026”, but its source file has no calendar dates. It cannot establish what happened in 2026, or prove that behaviour changed since 2023. The different headline fraud rates should not be read as a rise in fraud over time.

## What came out of the work

**Small payments and large payments tell different stories.** In the card file, payments below 10 account for 50.61% of fraud cases. Payments of 250 or more account for 73.92% of fraud-labelled value. A team trying to catch more cases and a team trying to protect more value would therefore be looking at different queues.

**A repeated small amount is a clue, not a verdict.** An amount of 1.00 appears in 113 fraud cases, but also in 13,575 legitimate payments. Flagging every payment of that amount would bring about 120 legitimate payments into review for each fraud case found. Without card identities, these rows cannot establish a sequence of test payments by one offender.

**The card file has short bursts, but only two days of evidence.** Some elapsed hours have a much higher fraud rate than the file as a whole. That deserves investigation. It does not justify a permanent “fraud happens at this clock time” rule. Clock labels in this report assume the recording began at midnight; the source does not provide that starting time.

**The larger file does not reveal a strong standalone time or city rule.** Hourly rates are close, and city rates sit around 3.6%. Turning tiny differences into rankings would give them more weight than they deserve. The report uses compact comparisons where a chart would add little.

**Account history helps frame questions, but simple rules still miss most cases.** An observed same-sender payment below 100 followed by one above 1,000 within 24 hours finds 168 fraud cases out of 179,553. Its fraud rate is 4.07%, but its coverage is only 0.094%. A rule can look promising in a small selected group and still contribute very little overall.

**City changes create a large review queue.** Flagging every observed sender city change would select 71.8% of all payments and capture 75.1% of fraud cases, alongside about 3.45 million legitimate payments. That is context for a broader decision, rather than a useful blocking rule on its own.

## The parts that needed care

The supplied `time_since_last_transaction` field in the financial file was not reliable as waiting time. More than two million values were negative. Checking the calculation showed that the available values followed sender records in CSV row order, in hours. Sender histories were rebuilt after sorting by timestamp before studying gaps and sequences.

The same file has no unchanged-device comparison in consecutive observed sender payments. Its `fraud_type` field also contains one outcome label for fraud rows. Neither supports a claim that device changes or that label independently predict fraud.

The card file contains 1,081 extra identical rows. They were retained because there is no transaction ID that would justify calling them accidental duplicates. Repeated-event sensitivity checks were kept separate from the main totals.

January 2024 contributes only one day in the financial file. It is excluded from full-month comparisons. Currency and timestamp timezone are not supplied, so payment amounts are not presented as a verified currency or converted across datasets.

## What I would test next

Build review queues from linked account or card history, including failed attempts where available. Compare each proposed rule with the current process on a later, untouched period. Track fraud captured, legitimate payments interrupted, analyst workload and confirmed loss—not just a higher fraud percentage in the flagged group. For online card payments, targeted extra authentication is another option to test.

The reports describe labelled transactions. They do not contain a trained prediction model, demonstrate an offender's intent, measure recovered money, or prove that a proposed control will reduce losses.

## Opening and refreshing the reports

Open the PBIX files from the release package in Power BI Desktop. They contain imported data, so the saved report can be viewed without running a refresh. The financial report also uses prepared CSV tables in `ModelData/Focused`; keep that folder with its project.

Local refresh requires the original source CSV files and updated source paths in Power Query. The saved paths belong to the original project machine. Publishing the imported reports to Power BI Service does not set up a cloud refresh schedule; local file refresh would need its own supported source/gateway configuration.

The release manifest records the delivered files and their SHA-256 hashes. The raw CSV files are not separately redistributed here. Dataset provenance and redistribution terms need to be checked at the original source before reuse; no bank ownership, collection year for the card file, or dataset licence is claimed.

## Tools and deliverables

Power BI Desktop, Power Query and DAX for the reports; Python/pandas for source checks and ordered histories; PDF presentations for the findings. Report pages are organised around individual questions, with explicit axes and plain-language notes.

Prepared by Ahmed Maged.
