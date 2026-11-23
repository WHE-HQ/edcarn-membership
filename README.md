# EDCARN membership confirmation

Tools used by WHO's Clinical Management & Operations team to check, with each member of **EDCARN** (WHO's Emergency Disease Clinical Assessment and Response Network), that the details we hold are correct and that they are happy to stay on the network database.

There are two parts:

| Folder | What it is |
|---|---|
| `survey/` | The survey, ready to import into the WHO dataform (LimeSurvey 6) |
| `report/` | A dashboard that summarises the replies and produces a reviewed master list in Excel |

## How it works

1. **Each member receives a personal email** showing the details we hold (name, email, organisation, specialization/unit, country, sex, working languages) with three buttons:
   *My details are correct* · *I need to update my details* · *Please remove me*.
2. **Each button opens a short confirmation page** with the member's details already filled in. They click *Submit* (and correct anything if they chose to update).
   Members based in the WHO European Region are also asked whether their details may be shared with the WHO Regional Office for Europe.
3. **The report** matches the replies to the invitation list using each member's personal access code, applies their corrections, and shows:
   - response, confirmations, updates and removal requests;
   - a world map and response by WHO region;
   - every edit, with the previous and the new value;
   - an Excel master list in which changed cells are highlighted (yellow = changed, green = added).

## Using the survey

1. In the dataform: **Create survey → Import** → `survey/EDCARN_confirm_membership.lss`.
2. **Participants**: create the participant table, then import a CSV with the columns
   `firstname, lastname, email, language, attribute_1 … attribute_7`
   (organisation, specialization/unit, country, country code, sex, languages, language codes).
   `report/data/tokens_000000.csv` shows the expected content with dummy people.
3. Activate the survey and send the invitations from **Participants → Invitations & reminders**.

## Running the report

Requirements: [R](https://cran.r-project.org/) and [Quarto](https://quarto.org/), with the R packages
`data.table, plotly, DT, htmltools, bslib, openxlsx, jsonlite`.

1. Export from the dataform into `report/data/`:
   - **Participants → Export → CSV** (all fields) → `tokens_<surveyID>.csv`
   - **Responses → Export → CSV** (all responses) → `results-survey<surveyID>.csv`
2. Delete the two dummy files `tokens_000000.csv` and `results-survey000000.csv` (or keep them only to try the report).
3. From the `report/` folder:

   ```bash
   quarto render edcarn_membership_report.qmd
   ```

   This produces `edcarn_membership_report.html` (the dashboard, with the Excel file downloadable from it) and `EDCARN_masterlist_reviewed_<date>.xlsx`.

Options (add `-P name:value` to the command):

| Option | Default | Use |
|---|---|---|
| `exclude_emails` | none | Secretariat or test entries to leave out, e.g. `-P exclude_emails:"a@who.int; b@who.int"` |
| `show_personal` | `true` | `false` gives an aggregate-only version: no names, e-mails, free text or Excel |

## Data protection

The real participant list, the exports and the rendered outputs contain members' personal data. **They are never committed**: `.gitignore` keeps everything in `report/data/` out of the repository except the two dummy files, and ignores all `.html` and `.xlsx` outputs. Keep real data in approved WHO storage only.

## Contact

Antoine Chaillon – chaillona@who.int
WHO Clinical Management & Operations

## Licence

© World Health Organization 2026. Available under CC BY-NC-SA 3.0 IGO – see [LICENSE](LICENSE). The WHO logo is not covered by the licence.
