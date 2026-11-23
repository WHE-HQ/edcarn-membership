# Data folder

Put the two exports from the dataform here before running the report:

- `tokens_<surveyID>.csv` – **Participants → Export → CSV** (all fields)
- `results-survey<surveyID>.csv` – **Responses → Export → CSV** (all responses)

The newest file of each kind is used.

`tokens_000000.csv` and `results-survey000000.csv` are **dummy files** (invented people at example.org) so the report can be tried straight away. Delete them when you use real exports.

Real exports contain personal data and are excluded from the repository by `.gitignore`. Do not change that.
