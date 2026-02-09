# Practical Examples Playbook (Tailored to Different Disciplines)

Audience signals:
- **Academia/Research (STEM):** Physics, Fluid Dynamics, Forestry, Biotechnology, Science Officers, Researchers/Lecturers
- **Health:** Resident Doctors, Clinical Epidemiology, Public Health, Nursing/Midwifery, Physiotherapy, Andrology, Pharmacy, Medicine & Surgery
- **Law:** Legal practitioners, Law lecturers, Law students
- **Corporate/Operations:** Aftersales, Customer Service, Finance/Account, Internal Control & Audit, HR & Admin, Store/Inventory, Procurement, Marketing/Social Media, Health & Safety, Admin, Civil Service, ICT Officer/Data Analyst


## Copy-ready prompt rules

How to read the prompts below:
- Replace anything in **[SQUARE BRACKETS]**.
- If a prompt says **[UPLOAD FILE]**, attach/upload the file in the tool first, then paste the prompt.
- If a prompt says **[PASTE TEXT]**, paste the text exactly under the prompt.
- When working with sensitive data (Medical/Legal/HR), use **anonymized** or **dummy** data for practice.

Prompt template you can reuse anywhere:

```text
You are an expert [ROLE].

TASK:
[What you want done]

INPUT:
[UPLOAD FILE / PASTE TEXT / PASTE TABLE]

CONSTRAINTS:
- Do not invent facts.
- If information is missing, ask up to 5 clarifying questions.

OUTPUT:
[Exact format: bullets / table / JSON / step-by-step / draft email]
```

---

## 1) GitHub Copilot (via VS Code)

### Practical items to cover
1. **Data cleaning scripts (CSV → clean table)**
2. **Quick analytics + charts (trend, top categories, pivot-like summaries)**
3. **Regex + text normalization (messy entries → standardized labels)**
4. **SQL generation for business reporting**

### Practical examples (prompt cards)

#### 1) Data cleaning scripts
- **Health/Public Health:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a senior Python data analyst.

    TASK:
    Write a complete Python script using pandas that loads a CSV file named patients.csv, cleans it, and outputs patients_clean.csv.

    INPUT:
    [UPLOAD FILE: patients.csv]
    Expected columns (minimum): patient_id, sex, age, visit_date
    Optional columns to handle if present: name, phone, address, email

    CLEANING RULES:
    - Standardize sex to exactly: Male / Female / Unknown.
      Accept inputs like: M, F, male, female, m, f, blank.
    - Parse visit_date into ISO format YYYY-MM-DD; if invalid, set to blank and flag it.
    - Flag impossible ages: age < 0 or age > 120.
    - Trim whitespace in all string fields.

    OUTPUT:
    - Save cleaned data to patients_clean.csv.
    - Print a short QA report: total rows, missing values per column, number of invalid dates, number of impossible ages.
    - Include a main() so it can run as: python clean_patients.py
    ```

    >This prompt instructs the AI to act as an experienced Python data analyst and create a full Python script that cleans a patient dataset. The script is expected to load a CSV file called **patients.csv**, tidy and standardize the data using specific rules, and then save the cleaned version as **patients_clean.csv**. It tells the script how to normalize gender values, fix and validate dates, detect unrealistic ages, and remove unnecessary spaces from text fields. In addition to cleaning the data, the script must also produce a simple quality-assurance summary showing row counts, missing values, invalid dates, and incorrect ages. Finally, the prompt requires the script to be runnable from the command line using a `main()` function, making it a complete and reusable data-cleaning program.

    ---

  - Copy-ready prompt (de-identification, requires file upload):

    ```text
    You are a data privacy officer and Python developer.

    TASK:
    Create a Python script that de-identifies a healthcare CSV and outputs an anonymized version.

    INPUT:
    [UPLOAD FILE: patients.csv]
    Columns may include: patient_id, name, phone, address, email, date_of_birth, age, sex, diagnosis, visit_date

    DE-IDENTIFICATION RULES:
    - Remove direct identifiers (name, phone, address, email).
    - Replace patient_id with a hashed ID (SHA-256) called anon_id.
    - Convert age into age_band: 0-9, 10-19, 20-29, ... 80+.
    - Keep diagnosis and visit_date, but ensure visit_date is YYYY-MM-DD.

    OUTPUT:
    - Save to patients_anonymized.csv.
    - Print what fields were removed and what transformations were applied.
    ```

    >This prompt tells the AI to behave like a data privacy officer and Python developer and to write a Python script that protects patient privacy. The script takes a healthcare CSV file and removes any information that can directly identify a person, such as names and contact details. It replaces the patient ID with a secure, hashed anonymous ID, groups ages into ranges instead of exact values, and keeps only safe, useful medical information like diagnosis and visit date in a standard format. The final result is a new anonymized CSV file and a short summary explaining which personal fields were removed and what changes were made to the data.

    ---

- **Finance/Account:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a finance data analyst.

    TASK:
    Write a complete Python (pandas) script that cleans a ledger export and produces a monthly spend summary.

    INPUT:
    [UPLOAD FILE: ledger.csv]
    Expected columns (minimum): date, description, amount
    Optional columns: category, department, vendor, payment_method

    CLEANING RULES:
    - Parse date to YYYY-MM-DD.
    - Convert amount to a numeric value (handle commas, currency symbols like ₦, $, and negative amounts in parentheses).
    - If category is missing, infer category from description using a simple keyword mapping (you define the mapping in code).

    OUTPUT:
    - Save cleaned ledger to ledger_clean.csv.
    - Print a table: month (YYYY-MM), total_spend, total_income (if any), net.
    - Also print top 10 categories by spend.
    ```

    >This prompt asks the AI to act as a finance data analyst and write a Python script that cleans financial records and summarizes spending. The script reads a ledger CSV file, fixes the date format, and converts the amount column into proper numbers even if it contains currency symbols, commas, or accounting-style negatives. If some transactions do not have a category, the script automatically assigns one based on keywords found in the description. After cleaning the data, it saves a new cleaned file and prints an easy-to-read monthly summary showing total spending, income, and net balance, along with a list of the top 10 spending categories.

    ---

  - Copy-ready prompt (duplicate invoices, requires file upload):

    ```text
    You are an internal control analyst.

    TASK:
    Detect potential duplicate invoices and output an exceptions report.

    INPUT:
    [UPLOAD FILE: invoices.csv]
    Expected columns: invoice_id, vendor, invoice_date, amount
    Optional: po_number, department

    RULES:
    - Flag potential duplicates where vendor + amount are identical and invoice_date is within 3 days.
    - Also flag exact duplicates (same vendor + amount + invoice_date).

    OUTPUT:
    - Produce duplicates_report.csv with columns: group_id, invoice_id, vendor, amount, invoice_date, duplicate_type.
    - Print counts by duplicate_type.
    ```

    >This prompt instructs the AI to act as an internal control analyst and create a Python script that looks for possible duplicate invoices. The script checks an invoice CSV file to find records that may represent the same payment, either by matching the vendor and amount within a short date range or by matching all details exactly. Any suspicious invoices are collected into a separate exceptions report that clearly shows which invoices are related and why they were flagged. The final output is a CSV report of duplicates and a simple count showing how many potential and exact duplicates were found.

    ---

- **Audit/Internal Control:**
  - Copy-ready prompt (audit sampling, requires file upload):

    ```text
    You are an internal auditor.

    TASK:
    Write a Python script that selects an audit sample of transactions using stratified random sampling.

    INPUT:
    [UPLOAD FILE: transactions.csv]
    Expected columns: transaction_id, transaction_date, amount, department

    SAMPLING RULES:
    - Create value bands: 0-50k, 50k-200k, 200k-1m, 1m+ (use the currency relevant to the dataset).
    - Sample a total of 30 transactions with at least 5 from each band (if available).

    OUTPUT:
    - Save audit_sample.csv.
    - Print band counts and sample counts.
    ```

    >This prompt asks the AI to act as an internal auditor and write a Python script that selects a fair audit sample from a list of transactions. The script first groups transactions into value ranges (bands) based on their amounts, such as low, medium, and high values. It then randomly selects a total of 30 transactions, making sure that each value band is properly represented with at least a few samples. The final result is a CSV file containing the selected audit sample and a simple summary showing how many transactions exist in each band and how many were chosen for review.

    ---

  - Copy-ready prompt (exceptions, requires file upload):

    ```text
    You are an audit analytics specialist.

    TASK:
    Generate an exception report from transaction logs.

    INPUT:
    [UPLOAD FILE: transactions.csv]
    Expected columns: transaction_id, posted_at (date-time), amount, posted_by, department

    EXCEPTION RULES:
    - Flag transactions posted outside business hours (Mon–Fri, 08:00–18:00).
    - Flag weekend postings.
    - Flag 'unusual users': users who post fewer than 3 transactions overall but have any transaction above the 90th percentile amount.

    OUTPUT:
    - Save exceptions_report.csv with columns: transaction_id, exception_type, posted_at, amount, posted_by, department.
    - Print summary counts by exception_type.
    ```

    >This prompt tells the AI to act as an audit analytics specialist and create a Python script that finds unusual or risky transactions. The script reviews transaction logs to detect entries made outside normal business hours, on weekends, or by users who rarely post transactions but make very large ones. Any transaction that meets one or more of these rules is treated as an exception and added to a separate report. The final output is a CSV file listing all flagged transactions and a simple summary showing how many exceptions were found for each type.

    ---

- **Aftersales/Customer Service:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a customer service data analyst.

    TASK:
    Clean and standardize a service ticket dataset and compute SLA breach flags.

    INPUT:
    [UPLOAD FILE: tickets.csv]
    Expected columns: ticket_id, created_at, closed_at, issue_category, agent
    Optional: priority, channel, customer_type

    CLEANING + METRICS:
    - Normalize issue_category into a controlled list you define (e.g., Delivery, Billing, Technical, Warranty, Other).
    - Compute resolution_time_hours = closed_at - created_at.
    - Fill missing closed_at as 'open' and set resolution_time_hours blank.
    - SLA rule: priority=High → 24h, Medium → 48h, Low → 72h (if priority missing, assume Medium).
    - Create sla_breach (True/False) for closed tickets.

    OUTPUT:
    - Save tickets_clean.csv.
    - Print: SLA breach rate overall and by agent.
    ```

    >This prompt asks the AI to act as a customer service data analyst and write a Python script that cleans support ticket data and checks if service targets were met. The script standardizes issue categories into a simple, consistent list, calculates how long each ticket took to resolve, and correctly handles tickets that are still open. It then applies clear SLA rules based on ticket priority to decide whether each resolved ticket breached the SLA. The final output is a cleaned CSV file and an easy-to-read summary showing the overall SLA breach rate and how each agent performed.

    ---

#### 2) Quick analytics + charts
- **STEM Lecturer/Researcher:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a scientific Python programmer.

    TASK:
    Write a Python script that reads an experiment dataset and produces a plot + outlier annotations.

    INPUT:
    [UPLOAD FILE: experiment.csv]
    Expected columns: time, pressure
    Optional: velocity, temperature

    REQUIREMENTS:
    - Parse time as numeric or datetime (handle both).
    - Plot pressure vs time.
    - Add smoothing (rolling mean or Savitzky-Golay; choose one and justify briefly in comments).
    - Label outliers where pressure is beyond 3 standard deviations from the mean.

    OUTPUT:
    - Save figure as pressure_over_time.png.
    - Print the list of outlier rows.
    ```

    >This prompt instructs the AI to act as a scientific Python programmer and create a script that analyzes and visualizes experimental data. The script reads an experiment CSV file, correctly understands the time values, and plots pressure changes over time. It smooths the data to make trends easier to see and automatically identifies unusual pressure values that are far from normal. The final result is a saved graph image and a printed list of the data points that were flagged as outliers.

    ---

  - Copy-ready prompt (confidence intervals, requires file upload):

    ```text
    You are a biostatistician.

    TASK:
    Compute summary statistics and 95% confidence intervals by treatment group.

    INPUT:
    [UPLOAD FILE: trial.csv]
    Expected columns: participant_id, group, outcome

    OUTPUT:
    - A printed table with: group, n, mean, std, 95% CI lower, 95% CI upper.
    - Save the table to group_summary.csv.
    ```

    >This prompt asks the AI to act as a biostatistician and write a Python script that summarizes results from a clinical trial. The script groups participants by their treatment group and calculates basic statistics such as how many participants there are, the average outcome, and how spread out the results are. It also computes a 95% confidence interval to show the likely range where the true average outcome lies. The final output is a clear summary table that is printed on the screen and saved as a CSV file for further use.

    ---
    
- **Public Health:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a public health surveillance analyst.

    TASK:
    Create an outbreak-signal chart from clinic visit data.

    INPUT:
    [UPLOAD FILE: clinic_visits.csv]
    Expected columns: visit_date, diagnosis
    Optional: facility, age, sex

    REQUIREMENTS:
    - Aggregate counts by week (ISO week) and diagnosis.
    - For each diagnosis, compute mean and standard deviation of weekly counts.
    - Flag 'outbreak signal' weeks where weekly count > mean + 2*std.

    OUTPUT:
    - Save plot as outbreak_signals.png.
    - Save outbreak_signals.csv listing diagnosis, week, count, threshold.
    ```

    >This prompt asks the AI to act as a public health surveillance analyst and turn routine clinic-visit data into an “early warning” outbreak chart. It groups your visits by ISO week and diagnosis, then computes a baseline (mean and standard deviation) for each diagnosis across time. Using that baseline, it flags weeks where the count is unusually high (above mean + 2×std) so spikes stand out quickly. The final output is a saved chart (PNG) and a CSV listing each diagnosis-week count alongside the threshold.

    ---

  - Copy-ready prompt (dashboard table, requires file upload):

    ```text
    You are a health data analyst preparing a dashboard dataset.

    TASK:
    Build a facility-by-month table with malaria counts and positivity rate.

    INPUT:
    [UPLOAD FILE: malaria_tests.csv]
    Expected columns: facility, test_date, test_result
    Where test_result is Positive/Negative (accept variants).

    OUTPUT:
    - Create a table with columns: facility, month (YYYY-MM), tests_total, malaria_positive, positivity_rate.
    - Save as malaria_dashboard_table.csv.
    ```

    >This prompt asks the AI to act as a health data analyst and convert raw malaria test records into a dashboard-ready monthly summary. It standardizes Positive/Negative variants, aggregates totals and positives by facility and month, and calculates a positivity rate you can trend over time. Because the output is already in a “tidy” table format, it’s easy to pivot and chart in Excel/Power BI. The final output is a single CSV file with consistent columns for reporting.

    ---

- **Procurement/Store:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are an inventory analyst.

    TASK:
    Compute reorder points and identify top fast-moving SKUs.

    INPUT:
    [UPLOAD FILE: inventory.csv]
    Expected columns: sku, item_name, on_hand, average_daily_sales, lead_time_days

    RULES:
    - Reorder point = average_daily_sales * lead_time_days.
    - Flag reorder_now = True if on_hand <= reorder_point.
    - Fast-moving SKUs = highest average_daily_sales.

    OUTPUT:
    - Save inventory_reorder_report.csv.
    - Save bar chart top_20_fast_movers.png.
    ```

    >This prompt asks the AI to act as an inventory analyst and calculate which items may need restocking based on your sales rate and supplier lead time. It computes a reorder point for each SKU (average_daily_sales × lead_time_days), compares it to on-hand stock, and flags items where stock is at or below the reorder point. It also ranks items by average_daily_sales so you can quickly see your fast movers. The final output is a reorder report CSV plus a bar chart of the top 20 fast-moving SKUs.

    ---

#### 3) Regex + text normalization
- **Your own course dataset:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a Python data wrangler.

    TASK:
    Parse the participant discipline strings into structured columns.

    INPUT:
    [UPLOAD FILE: ParticipantsDiscipline.csv]
    The CSV has one column: Discipline-Field-Occupation
    Example values include:
    - Legal practitioner. Law
    - Lecturer Baze University. PhD Public Health
    - ICT OFFICER AND DATA ANALYST

    REQUIREMENTS:
    - Create columns: raw_text, role, field, organization, level_or_degree.
    - Use robust parsing: split on '.', handle missing parts, handle all-caps job titles.
    - Keep the original in raw_text.

    OUTPUT:
    - Save participants_parsed.csv.
    - Print the top 10 roles and top 10 fields by count.
    ```

    >This prompt asks the AI to act as a Python data wrangler and turn messy “Discipline-Field-Occupation” strings into structured columns you can analyze. It keeps the original text as `raw_text`, then attempts to extract role, field, organization, and level/degree using robust splitting rules (including handling missing parts and all-caps job titles). It also prints the most common roles and fields so you get an immediate snapshot of the cohort makeup. The final output is a new parsed CSV plus the top-10 summary counts in the console.

    ---

  - Copy-ready prompt (label normalization, paste mapping):

    ```text
    You are a data standardization specialist.

    TASK:
    Propose a normalization mapping for job titles and roles, then apply it.

    INPUT:
    [PASTE A LIST OF RAW ROLE VALUES BELOW]

    REQUIREMENTS:
    - Create a mapping dictionary raw_role -> normalized_role.
    - Collapse obvious variants (e.g., Resident doctor/Resident Doctor/Residence -> Resident Doctor).
    - Keep a column for normalized_role AND keep raw_role unchanged.

    OUTPUT:
    - Return the mapping dictionary.
    - Provide Python code to apply it.

    RAW ROLE VALUES:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a data standardization specialist and help you clean up inconsistent job titles. It proposes a mapping from each raw role value to a single normalized role (collapsing obvious spelling/casing variants) while keeping the original `raw_role` untouched for traceability. You get both the mapping and code to apply it, so you can reuse the same standard in future cohorts. The final output is a mapping dictionary and a small Python snippet that adds a `normalized_role` column.

    ---

- **Legal:**
  - Copy-ready prompt (paste text):

    ```text
    You are a legal text-mining developer.

    TASK:
    Write a Python function using regex to extract legal citations from a block of text.

    INPUT:
    [PASTE TEXT BELOW]

    REQUIREMENTS:
    - Return a list of matches.
    - Support patterns similar to: (2021) 5 NWLR (Pt. 1234) 56
    - Also capture citations that omit parentheses in some parts (be tolerant).

    OUTPUT:
    - Provide Python code + brief explanation.

    TEXT:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a legal text-mining developer and write a reusable Python function that extracts legal citations using regex. It designs patterns that are tolerant of small formatting differences so it can still catch NWLR-style citations when punctuation or parentheses vary. This is useful for building citation indexes, cleaning briefs, or validating references before filing. The final output is Python code that returns a list of matched citations, plus a brief explanation so you can adjust the patterns if needed.

    ---

#### 4) SQL generation for reporting
- **Finance:**
  - Copy-ready prompt (paste schema):

    ```text
    You are a senior SQL analyst.

    TASK:
    Write SQL to produce (1) spend by department by month, and (2) top 10 vendors by total spend.

    INPUT:
    [PASTE TABLE SCHEMAS BELOW]
    Tables:
    - transactions
    - vendors
    - departments

    REQUIREMENTS:
    - Use ANSI SQL.
    - Assume transactions.amount is positive for spend; if negative values exist, handle accordingly.
    - Output 2 queries.

    OUTPUT:
    - Provide the SQL queries only, each labeled.

    SCHEMAS:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a senior SQL analyst and generate two finance reporting queries from your schemas. It produces (1) spend by department by month and (2) a ranking of the top 10 vendors by total spend, using ANSI SQL so it’s portable across databases. It also tells the assistant to think about negative amounts (e.g., refunds/credits) so totals don’t become misleading. The final output is two labeled SQL queries you can paste and run immediately.

    ---

- **HR:**
  - Copy-ready prompt (paste schema):

    ```text
    You are an HR analytics SQL specialist.

    TASK:
    Write SQL for:
    1) hires by month
    2) attrition rate by department
    3) average tenure (in months)

    INPUT:
    [PASTE TABLE SCHEMA BELOW]
    Assume an employees table with hire_date, termination_date (nullable), department.

    OUTPUT:
    - Provide 3 SQL queries labeled Query 1/2/3.
    
    SCHEMA:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as an HR analytics SQL specialist and draft three standard HR reporting queries from your table structure. It generates hires by month, attrition rate by department (using termination_date), and average tenure in months, which are common metrics for dashboards and monthly reports. Because you paste your exact schema, the queries can match your real column names and data types. The final output is three clearly labeled SQL queries (Query 1/2/3) ready to run.

    ---

- **Customer Service:**
  - Copy-ready prompt (paste schema):

    ```text
    You are a customer support reporting analyst.

    TASK:
    Write SQL to compute average first response time and average resolution time by agent and issue category.

    INPUT:
    [PASTE TABLE SCHEMAS BELOW]
    Assume:
    - tickets(ticket_id, created_at, closed_at, issue_category, agent_id)
    - messages(ticket_id, message_at, sender_type)
    Sender_type values include: customer, agent

    REQUIREMENTS:
    - first_response_time = time from ticket.created_at to first agent message.
    - resolution_time = ticket.closed_at - ticket.created_at.

    OUTPUT:
    - Provide 1 SQL query.
    
    SCHEMAS:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a customer support reporting analyst and calculate service performance metrics directly in SQL. It computes first response time by finding the first agent message after a ticket is created, and computes resolution time from ticket creation to closure. The query is grouped by agent and issue category so you can compare performance across people and problem types. The final output is one SQL query you can run for a KPI table.

    ---

<!-- #### 5) Automation scripts
- **Academics:**
  - Copy-ready prompt (folder-based automation):

    ```text
    You are a Python automation engineer on Windows.

    TASK:
    Write a Python script that converts all DOCX files in a folder to PDF and renames the output as Lastname_Year_Title.pdf.

    INPUT:
    - Folder path: [PASTE FOLDER PATH]
    - Naming rule: parse the DOCX filename formatted like: Lastname_Year_Title.docx

    REQUIREMENTS:
    - Use a reliable Windows approach (e.g., docx2pdf).
    - Skip files that fail conversion and print a warning.

    OUTPUT:
    - Provide complete code + how to run it.
    ```

    >This prompt asks the AI to act as a Windows Python automation engineer and create a batch document-conversion script. It loops through all DOCX files in a folder, converts each to PDF using a reliable Windows method (like `docx2pdf`), and renames the outputs using a consistent `Lastname_Year_Title.pdf` pattern. It also includes basic error handling so one failed conversion doesn’t stop the entire batch, and it prints warnings for files that fail. The final output is complete runnable code plus short instructions on how to execute it.

    ---

- **Admin:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a Python automation developer.

    TASK:
    Generate personalized course certificates from a CSV and save each as a PDF.

    INPUT:
    [UPLOAD FILE: participants.csv]
    Expected columns: full_name, course_title, date, certificate_id

    REQUIREMENTS:
    - Produce one PDF per participant.
    - File naming: certificate_id_full_name.pdf (sanitize spaces).
    - Use a simple template approach (reportlab or a HTML-to-PDF approach).

    OUTPUT:
    - Provide complete Python code + instructions.
    ```

    >This prompt asks the AI to act as a Python automation developer and generate certificates in bulk from a participant CSV. It reads each person’s name, course title, date, and certificate ID, then renders a simple certificate template and exports one PDF per participant. It also enforces a consistent file-naming rule (`certificate_id_full_name.pdf`) so your outputs are easy to organize and search. The final output is Python code and run instructions that produce a folder of certificate PDFs.

--- 

-->

## 2) Humanize AI Written Contents (humanize + AI-vs-human checks)

### Practical items to cover
1. **Humanizing tone without changing meaning (policy, emails, reports)**
2. **Audience-specific rewrite (patient-friendly, client-friendly, student-friendly)**
3. **Reducing robotic patterns (varied sentence rhythm, natural phrasing)**
4. **Quality control checklist (ethics + accuracy + attribution)**

### Practical examples

#### 1) Humanizing tone (same meaning)
- **HR & Admin:**
  - Case: Leave approval/rejection emails.
  - Example prompts:
    - Copy-ready prompt (paste draft):

      ```text
      You are an HR manager.

      TASK:
      Rewrite the email below to politely reject a leave request.

      CONSTRAINTS:
      - Keep the policy reason unchanged.
      - Add empathy.
      - Keep it under 140 words.
      - Maintain professional Nigerian workplace tone.

      OUTPUT:
      - Return only the rewritten email.

      EMAIL DRAFT:
      [PASTE HERE]
      ```

      >This prompt asks the AI to act as an HR manager and rewrite your leave-response email in a polite, human tone. It keeps the policy reason exactly the same, but adds empathy and removes harsh wording so the message is firm without sounding unfriendly. The word limit forces the email to stay concise and easy to read. The final output is a ready-to-send rejection email under 140 words.

      ---

    - Copy-ready prompt (paste memo):

      ```text
      You are a professional HR communications specialist.

      TASK:
      Rewrite the memo below to sound firm but not threatening.

      CONSTRAINTS:
      - Preserve all dates, rules, and required actions.
      - Remove harsh or accusatory phrasing.
      - Use clear headings and bullet points.

      OUTPUT:
      - Return only the rewritten memo.

      MEMO:
      [PASTE HERE]
      ```

      >This prompt asks the AI to act as an HR communications specialist and refine a policy memo so it sounds firm but not threatening. It preserves every date, rule, and required action, while removing accusatory language that can cause pushback or confusion. It also restructures the memo with clear headings and bullet points, making it easier for staff to understand what to do. The final output is a revised memo you can share internally with less risk of misinterpretation.

      ---

- **Customer Service/Aftersales:**
  - Copy-ready prompt (paste policy + draft):

    ```text
    You are a customer experience lead.

    TASK:
    Humanize an apology email for delayed delivery.

    INPUT:
    - Company policy snippet (what we can/can’t promise):
      [PASTE POLICY HERE]
    - Draft email:
      [PASTE DRAFT HERE]

    CONSTRAINTS:
    - Keep policy accurate; do not promise what is not allowed.
    - Propose next steps with timelines.
    - Provide exactly 2 compensation options.
    - Keep to 180–220 words.

    OUTPUT:
    - Return only the final email.
    ```

    >This prompt asks the AI to act as a customer experience lead and rewrite a delayed-delivery apology in a more natural, reassuring voice. By pasting your policy snippet, you prevent the assistant from promising things the company cannot deliver. It adds a clear timeline, practical next steps, and exactly two compensation options so the customer feels taken care of without creating policy risk. The final output is a complete apology email you can send as-is.

    ---

- **Finance/Account:**
  - Copy-ready prompt (paste invoice details):

    ```text
    You are a finance officer.

    TASK:
    Write a professional payment reminder email.

    INPUT:
    - Customer name: [CUSTOMER NAME]
    - Invoice number: [INVOICE NO]
    - Amount: [AMOUNT]
    - Due date: [DUE DATE]
    - Payment link/bank details: [PAYMENT DETAILS]

    CONSTRAINTS:
    - Calm, respectful tone.
    - Clear payment instructions.
    - Include a soft nudge and an offer to clarify any issues.
    - 120–160 words.

    OUTPUT:
    - Return only the email.
    ```

    >This prompt asks the AI to act as a finance officer and draft a professional payment reminder using your invoice details. It produces a calm, respectful message that clearly states the invoice number, amount, due date, and how to pay, while adding a gentle nudge and an offer to resolve any issues. The word limit keeps it concise so it reads well on email and mobile. The final output is a ready-to-send reminder email tailored to the exact invoice.

    ---

#### 2) Audience-specific rewrite
- **Medicine/Public Health:**
  - Copy-ready prompt (paste instructions):

    ```text
    You are a clinical communication specialist.

    TASK:
    Rewrite the discharge instructions below for a non-medical reader at B1 English level.

    CONSTRAINTS:
    - Keep dosage, timing, and red-flag symptoms exactly correct.
    - Use short sentences.
    - Add a short 'When to return to hospital' section.

    OUTPUT:
    - Return the rewritten instructions.

    DISCHARGE INSTRUCTIONS:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a clinical communication specialist and rewrite discharge instructions for a non-medical reader. It keeps dosages, timing, and red-flag symptoms exactly correct, but simplifies the wording to B1 level using short, clear sentences. It also adds a dedicated “When to return to hospital” section so the patient knows the urgent warning signs. The final output is a patient-friendly version of your instructions that is easier to follow.

    ---

  - Copy-ready prompt (paste abstract):

    ```text
    You are a public health educator.

    TASK:
    Convert the clinical trial abstract below into a patient-friendly leaflet.

    REQUIREMENTS:
    - Sections: Title, What was studied, What was found, What this means for patients, Limitations.
    - Avoid jargon; explain necessary terms in brackets.

    OUTPUT:
    - Return the leaflet.

    ABSTRACT:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a public health educator and convert a technical trial abstract into a patient-friendly leaflet. It forces a clear structure (what was studied, what was found, what it means, limitations) so readers can understand the point quickly. Where technical terms are unavoidable, it explains them in brackets instead of leaving jargon unexplained. The final output is a readable leaflet-style handout you can share with patients or community audiences.

    ---

- **Legal:**
  - Copy-ready prompt (paste clause):

    ```text
    You are a legal drafting assistant.

    TASK:
    Rewrite the contract clause below into plain English for a client briefing.

    CONSTRAINTS:
    - Preserve legal meaning.
    - Do not remove obligations, deadlines, penalties, or exceptions.
    - Add a 'Client impact' summary in 3 bullets.

    OUTPUT:
    - Return (1) plain-language clause, (2) 3 bullet client impact.

    CLAUSE:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a legal drafting assistant and rewrite a contract clause into plain English for a client briefing. It keeps the legal meaning intact, including obligations, deadlines, penalties, and exceptions, so you don’t accidentally soften the clause. It then summarizes the practical implications in three “Client impact” bullets, which is useful for meetings and emails. The final output is a plain-language clause plus a short client-facing impact summary.

    ---

- **STEM lecturers:**
  - Copy-ready prompt (paste paragraph):

    ```text
    You are a STEM lecturer.

    TASK:
    Rewrite the paragraph below for first-year students.

    REQUIREMENTS:
    - Keep technical accuracy.
    - Add one real-world analogy.
    - End with 3 short check-for-understanding questions.

    OUTPUT:
    - Return the rewritten explanation + 3 questions.

    PARAGRAPH:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a STEM lecturer and rewrite a technical paragraph for first-year students. It preserves technical accuracy, but improves clarity by simplifying sentence structure and adding one real-world analogy students can relate to. It also generates three short check-for-understanding questions, which you can use in class or as a quick quiz. The final output is a beginner-friendly explanation plus three questions (with the original meaning preserved).

    ---

#### 3) Reduce robotic patterns
- Copy-ready prompt (paste draft):

  ```text
  You are a professional communications editor.

  TASK:
  Rewrite the draft below into 3 versions:
  A) Formal memo
  B) Friendly email
  C) WhatsApp-friendly update

  CONSTRAINTS:
  - Keep the meaning and key facts identical.
  - No slang.
  - Nigerian professional tone.

  OUTPUT:
  - Return three labeled versions.

  DRAFT:
  [PASTE HERE]
  ```

  >This prompt asks the AI to act as a professional editor and produce three channel-specific versions of the same message. It keeps the meaning and key facts identical, but adapts the formatting and tone for a formal memo, a friendly email, and a WhatsApp-friendly update. This is useful when the same announcement must go to different audiences without creating inconsistencies. The final output is three labeled versions that you can paste into each channel.

  ---

#### 4) Quality control checklist (what students must do)
- Copy-ready prompt (paste both versions):

  ```text
  You are a quality assurance reviewer.

  TASK:
  Compare Version A and Version B and identify any meaning drift or factual errors.

  CHECKS:
  - Dates, numbers, dosages, legal obligations
  - Names/titles (if any)
  - Missing or added claims
  - Tone appropriateness

  OUTPUT:
  - A table with columns: Issue, Where it occurs, Risk level (Low/Med/High), Suggested fix.

  VERSION A (ORIGINAL):
  [PASTE HERE]

  VERSION B (REWRITTEN):
  [PASTE HERE]
  ```

  >This prompt asks the AI to act as a quality assurance reviewer and compare an original text with a rewritten version. It checks for meaning drift and factual errors (dates, numbers, dosages, legal obligations), as well as missing/added claims and tone problems. Instead of rewriting again, it reports issues in a structured table with risk levels and suggested fixes. The final output is a clear checklist-style table that helps you correct the rewrite safely.

---

## 3) Text Manipulation (Quillbot Strategy)

### Practical items to cover
1. **Paraphrase for clarity (without plagiarism)**
2. **Shorten/expand content (executive summary vs detailed report)**
3. **Tone shift (formal ↔ conversational)**
4. **Grammar + flow improvements for professional documents**

### Practical examples

#### 1) Paraphrase for clarity
- **Research/Academics:**
  - Copy-ready prompt (paste paragraph):

    ```text
    You are an academic writing assistant.

    TASK:
    Paraphrase the paragraph below to reduce similarity while preserving meaning.

    CONSTRAINTS:
    - Keep all citation placeholders exactly as written (e.g., (Author, Year)).
    - Do not add new claims.
    - Keep the same approximate length.

    OUTPUT:
    - Return only the paraphrased paragraph.

    PARAGRAPH:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as an academic writing assistant and paraphrase your paragraph to reduce similarity while preserving meaning. It protects your academic integrity by keeping citation placeholders exactly as written and by blocking the assistant from adding new claims. The length guidance helps the rewrite stay comparable to the original so your structure doesn’t change too much. The final output is a paraphrased paragraph that reads more original while remaining properly referenced.

    ---

  - Copy-ready prompt (paste definitions):

    ```text
    You are a public health lecturer.

    TASK:
    Combine the 5 definitions below into one clear, original definition of epidemiology.

    CONSTRAINTS:
    - Keep it under 60 words.
    - Preserve key elements: population, distribution, determinants, application to control.

    OUTPUT:
    - Return one final definition.

    DEFINITIONS:
    1) [PASTE]
    2) [PASTE]
    3) [PASTE]
    4) [PASTE]
    5) [PASTE]
    ```

    >This prompt asks the AI to act as a public health lecturer and combine multiple definitions into one original, clear definition. The constraints force it to keep the core elements (population, distribution, determinants, and application to control) while staying under 60 words. This is useful when different sources say the same thing in different ways and you need one clean version for notes or slides. The final output is one concise, original definition you can use.

    ---

- **Legal:**
  - Copy-ready prompt (paste argument):

    ```text
    You are a legal writing assistant.

    TASK:
    Rewrite the argument below into a clearer IRAC structure.

    OUTPUT FORMAT:
    - Issue:
    - Rule:
    - Application:
    - Conclusion:

    CONSTRAINTS:
    - Preserve legal meaning.
    - Do not invent case law or statutes.

    ARGUMENT:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a legal writing assistant and restructure your argument into IRAC (Issue, Rule, Application, Conclusion). It improves clarity by separating what the legal question is, what rule applies, and how the facts connect to the rule. The “do not invent case law/statutes” constraint protects you from accidental hallucinations. The final output is a clean IRAC-formatted argument based only on what you provided.

    ---

#### 2) Shorten/expand
- **Audit:**
  - Copy-ready prompt (paste finding):

    ```text
    You are an internal audit report writer.

    TASK:
    Reduce the audit finding below into a management summary of exactly 6 bullets.

    CONSTRAINTS:
    - Keep facts, dates, and numbers accurate.
    - Each bullet must start with a strong verb.

    OUTPUT:
    - Return only 6 bullets.

    AUDIT FINDING:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as an internal audit report writer and compress a detailed finding into an executive-friendly summary. It keeps all facts, dates, and numbers accurate, but forces the content into exactly six bullets so it stays readable. Requiring each bullet to start with a strong verb makes the summary sound action-oriented and clear for leadership. The final output is six management-summary bullets you can paste into a report.

    ---

  - Copy-ready prompt (expand one-liner):

    ```text
    You are an internal audit report writer.

    TASK:
    Expand the one-line finding below into a structured write-up.

    OUTPUT FORMAT:
    - Observation:
    - Criteria:
    - Cause:
    - Effect/Risk:
    - Recommendation:
    - Management response (draft):

    CONSTRAINTS:
    - Do not invent facts; make assumptions explicit.

    ONE-LINE FINDING:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as an internal audit report writer and expand a one-line finding into a full structured write-up. It organizes the content into Observation, Criteria, Cause, Effect/Risk, Recommendation, and a draft Management Response, which matches common audit reporting formats. The “do not invent facts” rule forces assumptions to be explicit so you can validate them against evidence. The final output is a structured draft you can refine and align to your working papers.

    ---

- **Public Health:**
  - Copy-ready prompt (paste update):

    ```text
    You are a public health communications officer.

    TASK:
    Convert the outbreak update below into:
    1) a 3-sentence SMS alert (plain language)
    2) a single press-release paragraph (150–200 words)

    CONSTRAINTS:
    - Keep numbers, dates, and locations exactly correct.
    - Avoid panic language.

    OUTPUT:
    - Provide two labeled outputs: SMS and Press release.

    OUTBREAK UPDATE:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a public health communications officer and rewrite the same outbreak update for two audiences. It creates a short, plain-language SMS alert and a longer press-release paragraph, while keeping all numbers, dates, and locations exactly correct. The “avoid panic language” constraint helps you communicate urgency without causing fear. The final output is two labeled messages (SMS and Press release) that stay consistent with each other.

    ---

#### 3) Tone shift
- **Customer Service:**
  - Copy-ready prompt (paste response):

    ```text
    You are a customer service trainer.

    TASK:
    Rewrite the response below in 3 tones:
    A) Apologetic
    B) Neutral
    C) Assertive but polite

    CONSTRAINTS:
    - Keep the solution offered identical in all versions.

    OUTPUT:
    - Return three labeled versions.

    ORIGINAL RESPONSE:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a customer service trainer and rewrite one response in three different tones. It keeps the solution offered identical in every version, so you’re only changing tone (not policy or promises). This is useful for coaching agents on how to sound empathetic or firm depending on the situation. The final output is three labeled replies: apologetic, neutral, and assertive-but-polite.

    ---

- **Marketing/Social Media:**
  - Copy-ready prompt (paste caption):

    ```text
    You are a social media copywriter.

    TASK:
    Rewrite the caption below into 5 styles:
    1) Educational
    2) Inspirational
    3) Humorous (clean, workplace-safe)
    4) Urgent
    5) Premium

    CONSTRAINTS:
    - Keep the product and offer details accurate.
    - Keep each caption under 40 words.

    OUTPUT:
    - Return 5 labeled captions.

    CAPTION:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a social media copywriter and rewrite one caption into five distinct styles. It keeps the product and offer details accurate while changing the “feel” (educational, inspirational, humorous, urgent, premium). The strict word limit ensures each caption stays short enough for social platforms. The final output is five labeled captions you can test across channels or campaigns.

    ---

#### 4) Grammar + flow
- **Civil service/Admin:**
  - Copy-ready prompt (paste memo):

    ```text
    You are a civil service communications editor.

    TASK:
    Polish the memo below for grammar and clarity.

    CONSTRAINTS:
    - Keep official tone.
    - Remove repetition.
    - Preserve all dates, directives, and names.

    OUTPUT:
    - Return only the revised memo.

    MEMO:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a civil service communications editor and polish a memo for grammar and clarity. It removes repetition and improves flow, but preserves official tone, directives, dates, and names so the memo’s authority and accuracy remain intact. This is especially helpful when a memo needs to be widely circulated and must be unambiguous. The final output is a revised memo that reads more professionally without changing instructions.

---

## 4) Citation, Consistency Checks, Plagiarism and Proofreading Workflow (Scribbr Strategy)

### Practical items to cover
1. **Citation generation (APA/MLA/Chicago/Vancouver)**
2. **Reference list consistency checks**
3. **Plagiarism/self-similarity hygiene (ethical use)**
4. **Proofreading workflows for theses and journal submissions**

### Practical examples

#### 1) Citation generation
- **Medicine (Vancouver):**
  - Copy-ready prompt (paste links/DOIs):

    ```text
    You are an academic reference librarian.

    TASK:
    Generate Vancouver-style references for the sources below.

    INPUT:
    - Provide 5 PubMed links or DOIs.
    - If any metadata is missing, ask me to paste the missing details.

    CONSTRAINTS:
    - Do not invent missing bibliographic data.
    - Use Vancouver style.

    OUTPUT:
    - Return a numbered reference list (1–5) in Vancouver format.

    SOURCES (PubMed links or DOIs):
    1) [PASTE HERE]
    2) [PASTE HERE]
    3) [PASTE HERE]
    4) [PASTE HERE]
    5) [PASTE HERE]
    ```

    >This prompt asks the AI to act as a reference librarian and format your medical sources in Vancouver style. You provide PubMed links or DOIs, and the prompt explicitly tells the assistant to ask for missing bibliographic fields instead of inventing them. This reduces citation errors that can cause manuscript corrections later. The final output is a numbered Vancouver reference list (1–5) you can paste into your paper.

    ---

- **Public Health/PhD (APA):**
  - Copy-ready prompt (requires links or pasted metadata):

    ```text
    You are an APA 7 referencing assistant.

    TASK:
    Create APA 7 reference entries for:
    1) a WHO guideline (PDF)
    2) a journal article
    3) a government report

    INPUT:
    For each item, paste either a URL/DOI or full metadata.

    CONSTRAINTS:
    - Do not fabricate author names, dates, or titles.

    OUTPUT:
    - Return 3 APA 7 reference entries.

    ITEMS:
    1) WHO guideline URL or metadata: [PASTE HERE]
    2) Journal article DOI/URL or metadata: [PASTE HERE]
    3) Government report URL or metadata: [PASTE HERE]
    ```

    >This prompt asks the AI to act as an APA 7 referencing assistant and build reference entries for three common source types: a WHO guideline, a journal article, and a government report. It allows you to paste either a DOI/URL or full metadata for each item, and it blocks the assistant from fabricating authors, dates, or titles. This keeps your reference list accurate and defensible. The final output is three APA 7 reference entries formatted consistently.

    ---

- **Law:**
  - Copy-ready prompt (paste style + sources):

    ```text
    You are a legal research assistant.

    TASK:
    Generate citations for the legal sources below.

    INPUT:
    - Citation style required by my institution: [PASTE STYLE NAME OR GUIDE LINK]
    - Sources (cases, statutes, journal articles):
      [PASTE LIST HERE]

    CONSTRAINTS:
    - If the style guide is unclear, ask clarifying questions.
    - Do not invent missing case numbers, years, or report series.

    OUTPUT:
    - Provide a correctly formatted reference list.
    - Also provide the matching in-text citation format examples (1–2 examples per source type).
    ```

    >This prompt asks the AI to act as a legal research assistant and format citations in the specific style required by your institution. By pasting the style name or guide link, you anchor the formatting rules to what you’re actually expected to use. The prompt also forces clarification if the style is unclear and prevents guessing missing case/report details. The final output is a correctly formatted reference list plus 1–2 in-text citation examples for each source type.

    ---

#### 2) Consistency checks
- Copy-ready prompt (normalize reference list, paste text):

  ```text
  You are an APA 7 reference list editor.

  TASK:
  Normalize the reference list below to APA 7.

  CONSTRAINTS:
  - Preserve each source’s bibliographic facts.
  - Fix capitalization, italics markers (if represented), punctuation, and ordering.
  - If a reference is missing key fields (year, title, journal, etc.), flag it instead of guessing.

  OUTPUT:
  - Return the cleaned APA 7 reference list.
  - Then list any references with missing information and what is missing.

  REFERENCE LIST:
  [PASTE HERE]
  ```

  >This prompt asks the AI to act as an APA 7 reference list editor and clean up a reference list for consistency. It fixes formatting issues like capitalization, punctuation, ordering, and italics markers while preserving the bibliographic facts of each source. If key information is missing (like year or journal), it flags the entry instead of guessing so you can correct it properly. The final output is a cleaned APA 7 reference list plus a short list of what information is missing and where.

  ---

- Copy-ready prompt (missing in-text citations audit):

  ```text
  You are a thesis editor.

  TASK:
  Compare the in-text citations list with the reference list and find mismatches.

  INPUT:
  A) In-text citations used (paste as list, one per line):
  [PASTE HERE]

  B) Reference list (paste):
  [PASTE HERE]

  OUTPUT:
  - A table with columns:
    1) Item
    2) Present in in-text? (Yes/No)
    3) Present in reference list? (Yes/No)
    4) Suggested fix
  ```

  >This prompt asks the AI to act as a thesis editor and audit alignment between in-text citations and the reference list. It checks whether each item appears in both places, which helps prevent common submission issues like missing references or uncited references. The results are presented as a simple table that tells you what’s missing and what to fix. The final output is a mismatch table with suggested corrections.

  ---

#### 3) Plagiarism/self-similarity hygiene
- Copy-ready prompt (paste similarity highlights):

  ```text
  You are an academic integrity coach.

  TASK:
  Help me repair a draft after a similarity/plagiarism check.

  INPUT:
  - The highlighted sentences/paragraphs with high similarity:
    [PASTE HERE]
  - The source links or citations that those highlights relate to:
    [PASTE HERE]

  CONSTRAINTS:
  - Do not remove necessary citations.
  - Rewrite to original wording while preserving meaning.
  - If a sentence is a definition or standard phrasing, keep it but add/strengthen citation.

  OUTPUT:
  - Revised version of the highlighted text.
  - A checklist of where to add citations.
  - A short ‘sources used’ log (bullets).
  ```

  >This prompt asks the AI to act as an academic integrity coach and help you repair text that was flagged by a similarity/plagiarism checker. You paste the highlighted passages and the related source links/citations, and the prompt ensures citations are not removed while the wording is rewritten to be more original. It also helps you identify where citations should be added or strengthened, especially for definitions or standard phrasing. The final output is revised text, a citation-add checklist, and a short “sources used” log.

  ---

#### 4) Proofreading workflow
- Copy-ready prompt (build submission checklist):

  ```text
  You are a journal submission coordinator.

  TASK:
  Create a detailed submission checklist for a thesis/journal manuscript.

  INPUT:
  - Field: [Medicine / Public Health / Law / STEM]
  - Style guide: [APA 7 / Vancouver / Chicago / Other]
  - Target: [Journal submission / University thesis]

  OUTPUT:
  - A checklist grouped by sections:
    1) Front matter
    2) Body (headings, tables, figures)
    3) References and citations
    4) Ethics (conflicts, consent, anonymization)
    5) Final PDF/export checks
  ```

  >This prompt asks the AI to act as a journal submission coordinator and generate a detailed checklist for final manuscript preparation. It tailors the checklist to your field, the required style guide (APA/Vancouver/other), and whether you’re submitting to a journal or a university. The checklist covers formatting, citations, tables/figures, ethics, and final PDF/export steps so you don’t miss common requirements. The final output is a structured checklist you can follow item-by-item before submission.

---

## 5) Slide Outline Prep for Gamma (AI presentations)

### Practical items to cover
1. **From outline → slides (lectures, trainings, board reports)**
2. **From dataset/metrics → story + visuals**
3. **Role-based decks (teach, sell, audit, policy)**
4. **Handouts and one-pagers**

### Practical examples

#### 1) Outline → slides
- **Lecturers:**
  - Copy-ready prompt (clinical epidemiology deck):

    ```text
    You are an instructional designer for postgraduate health training.

    TASK:
    Create a Gamma-ready outline for a 12-slide lecture deck.

    TOPIC:
    Clinical Epidemiology: Bias, Confounding, and Randomization

    AUDIENCE:
    Resident doctors and public health students.

    REQUIREMENTS:
    - Slide-by-slide titles + bullet content.
    - Include 2 in-class quiz questions (with answers).
    - Include 1 short Nigeria-relevant example per concept (bias, confounding, randomization).
    - Add a closing slide: “Key takeaways (5 bullets)”.

    OUTPUT:
    - Return the full slide outline only.
    ```

    >This prompt asks the AI to act as an instructional designer and create a ready-to-import slide outline for Gamma. It breaks the topic into slide-by-slide titles and bullets, adds quiz questions for engagement, and forces Nigeria-relevant examples so the content feels local and practical. Because the structure is already “deck-shaped,” you can paste it into Gamma and generate slides quickly. The final output is a complete 12-slide outline that’s suitable for teaching immediately.

    ---

  - Copy-ready prompt (fluid dynamics deck):

    ```text
    You are a STEM lecturer and presentation designer.

    TASK:
    Create a Gamma-ready outline for a 10–12 slide lecture deck.

    TOPIC:
    Fluid Dynamics: Reynolds Number (laminar vs turbulent flow)

    AUDIENCE:
    First-year engineering/science students.

    REQUIREMENTS:
    - One analogy that students relate to.
    - One real-world application in Nigeria (e.g., pipelines, water supply, traffic-as-flow analogy).
    - Add 3 practice questions (with short answers).

    OUTPUT:
    - Slide-by-slide outline.
    ```

    >This prompt asks the AI to act as a STEM lecturer and presentation designer and produce a Gamma-ready lecture outline. It ensures the deck is student-friendly by requiring one relatable analogy, a Nigeria-based real-world application, and practice questions with answers. This gives you both teaching content and built-in class interaction. The final output is a slide-by-slide outline you can paste into Gamma to generate the deck.

    ---

- **Law lecturer:**
  - Copy-ready prompt (contract law deck):

    ```text
    You are a law lecturer creating a slide deck.

    TASK:
    Produce a Gamma-ready slide outline for a lecture on Contract Law: Offer, Acceptance, Consideration.

    AUDIENCE:
    Undergraduate law students.

    REQUIREMENTS:
    - 12 slides maximum.
    - Include 3 Nigeria-context scenarios as mini case studies.
    - Include a slide with common exam mistakes and how to avoid them.

    OUTPUT:
    - Slide-by-slide outline.
    ```

    >This prompt asks the AI to act as a law lecturer and turn core contract-law concepts into a structured lecture deck. It keeps the deck concise (12 slides max) while adding Nigeria-context scenarios to make the doctrine easier to understand. It also includes a slide on common exam mistakes, which helps students focus on how questions are actually graded. The final output is a Gamma-ready slide outline you can import directly.

    ---

#### 2) Metrics → story
- **Finance:**
  - Copy-ready prompt (paste table):

    ```text
    You are a CFO’s strategy analyst.

    TASK:
    Convert the finance table below into a Gamma-ready narrative deck outline.

    INPUT:
    [PASTE TABLE BELOW]

    REQUIREMENTS:
    - Identify 3–5 key drivers.
    - Identify top risks and mitigation actions.
    - Include recommended decisions for management.
    - Provide 1 slide with “Questions the CFO will ask” and suggested answers.

    OUTPUT:
    - Slide-by-slide deck outline.

    TABLE:
    [PASTE HERE]
    ```

    >This prompt asks the AI to act as a CFO’s strategy analyst and convert raw finance numbers into a management story. It identifies key drivers, calls out risks and mitigations, and includes recommended decisions so the deck supports action—not just reporting. The “questions the CFO will ask” slide helps you prepare talking points for scrutiny. The final output is a slide-by-slide narrative outline that Gamma can turn into slides.

    ---

- **Aftersales:**
  - Copy-ready prompt (paste KPIs):

    ```text
    You are an aftersales operations manager.

    TASK:
    Create a Gamma-ready deck outline explaining SLA performance and an improvement plan.

    INPUT:
    - SLA KPIs (paste):
      [PASTE HERE]
    - Top complaint categories and counts:
      [PASTE HERE]

    REQUIREMENTS:
    - Root cause hypotheses for top 3 complaint categories.
    - A 30/60/90-day improvement plan with owners.
    - A simple chart suggestion per KPI.

    OUTPUT:
    - Slide-by-slide outline.
    ```

    >This prompt asks the AI to act as an aftersales operations manager and structure your SLA KPIs into a performance and improvement deck. It guides the assistant to propose likely root causes for the top complaints, then lays out a clear 30/60/90-day plan with owners so accountability is visible. It also recommends simple chart types per KPI to make the deck visual. The final output is a Gamma-ready slide outline for an SLA review meeting.

    ---

- **HSE:**
  - Copy-ready prompt (paste HSE log summary):

    ```text
    You are an HSE reporting officer.

    TASK:
    Create a Gamma-ready safety performance deck outline.

    INPUT:
    - Incident summary (paste):
      [PASTE HERE]
    - Near-miss summary (paste):
      [PASTE HERE]

    REQUIREMENTS:
    - Categorize incidents by type and root cause.
    - Propose corrective actions (engineering, administrative, PPE).
    - Include a slide for “Leading indicators” (training, inspections, audits).

    OUTPUT:
    - Slide-by-slide outline.
    ```

    >This prompt asks the AI to act as an HSE reporting officer and turn incident and near-miss summaries into a safety performance deck. It forces categorization and root-cause thinking, then proposes corrective actions across engineering, admin controls, and PPE (so it’s not just “train people”). It also includes leading indicators so you can show proactive performance, not only incidents. The final output is a slide-by-slide outline you can paste into Gamma.

    ---

#### 3) Role-based decks
- **Procurement:**
  - Copy-ready prompt:

    ```text
    You are a procurement trainer.

    TASK:
    Create a Gamma-ready vendor onboarding training deck outline.

    REQUIREMENTS:
    - Include onboarding checklist.
    - Include red flags (fraud, poor documentation, conflicts of interest).
    - Include a 1-page vendor evaluation scorecard slide.

    OUTPUT:
    - Slide-by-slide outline.
    ```

    >This prompt asks the AI to act as a procurement trainer and design a vendor onboarding training deck. It includes a practical onboarding checklist, highlights fraud/conflict red flags, and adds a scorecard slide so trainees learn how vendors are actually evaluated. This makes the deck usable for both training and real onboarding workflows. The final output is a Gamma-ready slide outline.

    ---

- **Audit:**
  - Copy-ready prompt:

    ```text
    You are an internal control trainer.

    TASK:
    Create a Gamma-ready internal control awareness deck for staff.

    REQUIREMENTS:
    - Cover segregation of duties, approvals, evidence retention.
    - Include 3 short scenarios (what went wrong + correct control).
    - End with a 5-question quiz.

    OUTPUT:
    - Slide-by-slide outline.
    ```

    >This prompt asks the AI to act as an internal control trainer and create a staff awareness deck focused on real control behavior. It ensures key control themes are covered (segregation of duties, approvals, evidence retention) and uses short scenarios so staff can practice spotting what went wrong. The quiz at the end helps you assess understanding during training. The final output is a Gamma-ready slide outline plus scenarios and questions.

    ---

#### 4) Handouts (No Gamma)
- Copy-ready prompt (prompt engineering cheat sheet):

  ```text
  You are a prompt engineering instructor.

  TASK:
  Create a 1-page cheat sheet students can print.

  CONTENT:
  - 10 prompt patterns (role, task, constraints, output format, examples)
  - 5 common mistakes and fixes
  - A mini checklist: “Before you hit send”

  OUTPUT:
  - Return as a one-page formatted text (headings + bullets) suitable for copy/paste into a doc.
  ```

  >This prompt asks the AI to act as a prompt-engineering instructor and produce a printable one-page cheat sheet. It summarizes practical prompt patterns, common mistakes, and a simple checklist students can follow before sending prompts. Because the output is formatted as headings and bullets, you can paste it into Word/Google Docs and print immediately. The final output is a single-page handout that supports the entire course.

  ---

- Copy-ready prompt (hypertension patient handout):

  ```text
  You are a patient education nurse.

  TASK:
  Create a 1-page patient handout: “Understanding your hypertension medicines”.

  REQUIREMENTS:
  - B1 English level.
  - Sections: What hypertension is, Why medicines matter, How to take them, Common side effects, Red flags, When to return.
  - Add a simple daily checklist.

  CONSTRAINTS:
  - Do not give personalized medical advice.
  - Use general education wording.

  OUTPUT:
  - Return the handout content.
  ```

  >This prompt asks the AI to act as a patient education nurse and create a one-page hypertension medication handout in simple English. It structures the content into clear sections (what it is, why meds matter, how to take, side effects, red flags) and includes a daily checklist for adherence. The constraints prevent personalized medical advice so it stays safe for general education. The final output is a ready-to-share handout text.

---

<!-- ## 6) Google AI Studio (Build)

### Practical items to cover
1. **System instructions for role-based assistants (Law/Health/Finance)**
2. **Structured outputs (JSON tables, checklists, rubrics)**
3. **Prompt evaluation (A/B prompts, temperature, guardrails)**
4. **Document-to-structured extraction**

### Practical examples

#### 1) Role-based assistants
- **Legal case brief assistant:**
  - Copy-ready prompt (Legal Case Brief Generator (IRAC)):

    ```text
    You are a legal case brief assistant.

    Your role is to analyze ONLY the legal judgment text provided by the user and generate a structured IRAC-style case brief.

    STRICT RULES:
    - Work strictly with the text provided by the user.
    - Do NOT invent, infer, or assume facts not stated in the judgment.
    - Do NOT add external case law, statutes, or commentary.
    - If essential information is missing or unclear, ask up to 5 clarifying questions before producing the brief.
    - If the text is insufficient to complete a section, clearly state "Not specified in the judgment".

    TASK:
    When the user provides a legal judgment text, create a case brief using the IRAC method.

    OUTPUT FORMAT (always follow exactly):
    - Case name:
    - Court and year:
    - Facts:
    - Issues:
    - Rules/Principles:
    - Application/Reasoning:
    - Holding:
    - Ratio decidendi:
    - Implications for practice:

    TONE:
    - Clear
    - Neutral
    - Professional
    - Concise but complete

    WAIT for the user to provide the judgment text before responding.
    ```

    >This prompt tells Google AI Studio to build a Legal Case Brief Generator app that accepts judgment text, produces an IRAC-based case brief, does not hallucinate, asks clarifying questions if needed, and outputs in a fixed structure.

    ---

  - Copy-ready prompt (contract extraction - Contract Analysis App):

    ```text
    You are a contract analysis assistant.

    Your role is to analyze ONLY the contract text provided by the user and extract obligations, deadlines, penalties, and exceptions exactly as written.

    STRICT RULES:
    - Work strictly with the contract text provided by the user.
    - Do NOT guess, infer, or assume obligations, timelines, or penalties.
    - Do NOT provide legal advice or interpretations beyond what is explicitly stated.
    - If an item is not specified in the contract, clearly state "Not specified in the contract".
    - If clause references are available, quote the exact clause text or clause number verbatim.
    - If the contract text is incomplete or unclear, ask up to 5 clarifying questions before producing the table.

    TASK:
    When the user provides contract text, extract and summarize contractual obligations and related details.

    OUTPUT FORMAT:
    Always return a table with the following columns:
    - Party
    - Obligation
    - Deadline/Timeline
    - Penalty/Consequence
    - Exceptions
    - Clause reference (quote exact clause text or clause number if available)

    TONE:
    - Neutral
    - Precise
    - Professional
    - Structured

    WAIT for the user to provide the contract text before responding.
    ```

    >TThis prompt tells Google AI Studio to build a Contract Obligation & Deadline Extraction app that should read only the contract text supplied by the user, and extract party, obligations, deadlines, penalties, exceptions, and quote clause reference where possible, all to be outputed as a table with columns for each items extracted. It should also avoid guessing or legal interpretation beyond the text.

    ---

- **Clinical note assistant:**
  - Copy-ready prompt (Clinical SOAP Note Generator):

    ```text
    You are a clinical documentation assistant.

    Your role is to convert ONLY the clinician notes provided by the user into a structured SOAP note.

    STRICT RULES:
    - Work strictly with the clinician notes provided by the user.
    - Do NOT provide new diagnoses, interpretations, or clinical judgments beyond what is written.
    - Do NOT invent, assume, or modify vital signs, symptoms, or findings.
    - If required information for any SOAP section is missing, clearly state "Not documented in the notes".
    - Highlight red flags ONLY if they are explicitly mentioned or clearly implied in the notes.
    - Patient instructions must be written in simple, patient-friendly English (B1 level).

    TASK:
    When the user provides clinician notes, convert them into a structured SOAP note and supporting sections.

    OUTPUT FORMAT:
    Always return the response in the following order:

    1) SOAP Note  
      - Subjective  
      - Objective  
      - Assessment  
      - Plan  

    2) Red Flags  
      - List clearly, or state "No red flags documented"

    3) Patient Discharge Instructions  
      - Clear, simple, B1-level English
      - Actionable and easy to understand

    TONE:
    - Clinical
    - Neutral
    - Clear
    - Professional

    WAIT for the user to provide clinician notes before responding.
    ```

    >This prompt instructs Google AI Studio to build a clinical documentation app that takes raw clinician notes as input and converts them into a structured SOAP note. The app strictly works only with the information provided, avoids inventing diagnoses or vitals, highlights any documented red flags, and generates simple, patient-friendly discharge instructions written at a B1 English level. The output is consistently formatted into SOAP notes, red flags, and patient instructions to support clear and safe clinical documentation.

    ---

- **Audit assistant:**
  - Copy-ready prompt (Internal Audit Test Procedure Generator):

    ```text
    You are an internal audit assistant.

    Your role is to convert ONLY the internal control description provided by the user into clear, practical audit test steps and evidence requests.

    STRICT RULES:
    - Work strictly with the control description provided by the user.
    - Do NOT invent controls, risks, processes, or documentation that are not mentioned.
    - Do NOT provide theoretical or generic audit language; be practical and specific.
    - If key details are missing from the control description, clearly state assumptions are not made and ask up to 5 clarifying questions before proceeding.
    - Red flags must be realistic indicators of control failure based on the description provided.

    TASK:
    When the user provides a control description, translate it into actionable audit testing procedures.

    OUTPUT FORMAT:
    Always return the response in the following structure:

    - Control objective (1 concise sentence)
    - Test steps (numbered list, practical and executable)
    - Evidence to request (bullet points)
    - Red flags to watch for (bullet points)

    TONE:
    - Professional
    - Practical
    - Clear
    - Audit-focused

    WAIT for the user to provide the control description before responding.
    ```

    >This prompt instructs Google AI Studio to build an internal audit support app that converts control descriptions into actionable audit procedures. The app reads only the control text supplied by the user and produces a clear control objective, step-by-step audit tests, specific evidence requests, and realistic red flags to watch for during audit execution. It avoids generic theory, assumptions, or invented controls, ensuring outputs are practical and directly usable in real audit engagements.

    ---

#### 2) Extraction
- Copy-ready prompt (Policy Summary & Extraction App):

  ```text
  You are a policy analysis assistant.

  Your role is to extract structured information ONLY from the policy text provided by the user.

  STRICT RULES:
  - Work strictly with the policy text provided by the user.
  - Do NOT infer, interpret, or assume policy intent beyond what is explicitly stated.
  - Do NOT add roles, responsibilities, dates, or rules that are not written in the policy.
  - If any required item is not present in the text, explicitly write "Not stated".
  - If the policy text is incomplete or unclear, do NOT guess; extract only what is available.

  TASK:
  When the user provides policy text, extract key policy elements in a structured format.

  OUTPUT FORMAT:
  Always return the response using the following headings:

  - Purpose:
  - Scope:
  - Responsibilities (role → responsibility):
  - Exceptions:
  - Effective date:
  - Review cycle:

  TONE:
  - Neutral
  - Clear
  - Formal
  - Policy-focused

  WAIT for the user to provide the policy text before responding.
  ```

  >This prompt instructs Google AI Studio to build a policy analysis app that reads policy text supplied by the user and extracts essential elements such as purpose, scope, responsibilities, exceptions, effective dates, and review cycles. The app strictly limits itself to what is explicitly stated in the policy and clearly marks any missing information as “Not stated,” ensuring accuracy, consistency, and compliance with formal policy documentation standards.

---

## 7) Nano Banana (Gemini image model for creative image generation)
*(Your clarification: this is the Gemini/Google model you use for generating creative images. In practice, you’ll demo text-to-image + prompt craft + iteration.)*

### Practical items to cover
1. **Prompt structure for reliable images (subject, scene, style, lighting, lens, aspect)**
2. **Academic visuals (figures, diagrams, concept illustrations)**
3. **Professional visuals (flyers, social banners, icons, infographics)**
4. **Health/Law visual ethics (privacy, realism disclaimers, avoiding misleading visuals)**
5. **Iteration patterns (variants, negative constraints, brand consistency)**

### Practical examples

#### 1) Prompt structure
- Copy-ready prompt (universal text-to-image prompt):

  ```text
  Create an image with the following requirements.

  SUBJECT:
  [SUBJECT]

  SCENE/ENVIRONMENT:
  [ENVIRONMENT]

  STYLE:
  [STYLE] (e.g., flat vector, scientific illustration, 3D render, watercolor, photorealistic)

  LIGHTING:
  [LIGHTING] (e.g., soft daylight, studio lighting, high contrast)

  MOOD:
  [MOOD]

  COMPOSITION:
  - Clear focal point
  - Clean background
  - Leave space for text at: [TOP/BOTTOM/LEFT/RIGHT/NONE]

  CONSTRAINTS (important):
  - No logos, watermarks, or brand marks.
  - No identifiable real persons.
  - If medical/legal themed, keep it clearly educational (not misleading).

  OUTPUT:
  - Aspect ratio: [AR] (e.g., 16:9, 1:1, 4:3)
  ```

  >This prompt is a reusable “master template” for text-to-image generation. It forces you to describe the subject, scene, and style clearly, and it adds practical controls like composition and where to leave space for text on a flyer or slide. The constraints (no logos, no identifiable real persons, educational framing for medical/legal themes) reduce common risks and avoid misleading imagery. The final output is a well-structured prompt you can reuse across many image ideas by changing only the bracketed fields.

  ---

- Examples:
  - **Public Health (mosquito lifecycle):**

    ```text
    Create an educational infographic-style illustration.

    SUBJECT:
    Mosquito lifecycle (egg → larva → pupa → adult)

    STYLE:
    Flat vector, high contrast, clean labels

    COMPOSITION:
    4 panels with arrows, minimal text, white background

    CONSTRAINTS:
    - Educational diagram, not photorealistic.
    - No logos.

    OUTPUT:
    Aspect ratio: 16:9
    ```

    >This prompt asks the image model to create a simple educational diagram, not a realistic photo. By specifying a flat-vector style, labeled panels, arrows, and a white background, it produces a classroom-friendly infographic that reads clearly on slides. The constraints help avoid accidental branding and keep the content appropriate for health education. The final output is a 16:9 infographic illustration of the mosquito lifecycle.

    ---

  - **Fluid Dynamics (laminar vs turbulent):**

    ```text
    Create a scientific illustration comparing laminar vs turbulent flow in a pipe.

    STYLE:
    Scientific illustration, clean, textbook-like

    DETAILS:
    - Left side: laminar flow with smooth streamlines
    - Right side: turbulent flow with eddies
    - Use blue/red streamlines to show velocity differences
    - Minimal text (only “Laminar” and “Turbulent”)

    OUTPUT:
    Aspect ratio: 4:3
    ```

    >This prompt asks for a textbook-style scientific comparison image that is easy to teach from. The left/right layout and minimal text make the difference between laminar and turbulent flow visually obvious, while the streamline color hint reinforces the concept. The 4:3 aspect ratio makes it suitable for lecture slides and handouts. The final output is a clean diagram you can use to explain Reynolds number concepts.

    ---

  - **Law (courtroom scene):**

    ```text
    Create a documentary-style courtroom scene in Nigeria.

    STYLE:
    Documentary photography style, soft natural lighting

    CONSTRAINTS:
    - No identifiable faces (faces turned away / blurred / silhouette)
    - No official seals or logos

    OUTPUT:
    Aspect ratio: 16:9
    ```

    >This prompt creates a Nigeria-context courtroom image while protecting privacy and avoiding official-looking marks. By requiring documentary-style lighting but no identifiable faces or seals, it keeps the visual generic and safe for training materials. The wide 16:9 format fits well as a slide background. The final output is a symbolic courtroom scene suitable for legal education or presentations.

    ---

#### 2) Academic visuals
- **Biotech student (CRISPR):**

  ```text
  Create an educational 3-panel diagram explaining CRISPR gene editing.

  STYLE:
  Flat vector, simple icons, classroom-friendly

  PANELS:
  1) Guide RNA targeting DNA
  2) Cas enzyme cutting
  3) Repair/insertion

  OUTPUT:
  Aspect ratio: 16:9
  ```

  >This prompt asks for a simple, classroom-friendly CRISPR diagram rather than a complex scientific figure. The 3-panel structure forces the model to show the process step-by-step (targeting, cutting, repair), which helps learning. A flat-vector style and simple icons keep it readable when projected. The final output is a clear 16:9 educational diagram you can place directly into a slide.

  ---

- **Physics (EM spectrum):**

  ```text
  Create a clean infographic chart of the electromagnetic spectrum.

  STYLE:
  Minimal, chart-like, labeled ranges

  REQUIREMENTS:
  - Label: radio, microwave, infrared, visible, ultraviolet, X-ray, gamma
  - Include wavelength or frequency direction arrow

  OUTPUT:
  Aspect ratio: 16:9
  ```

  >This prompt asks the image model to generate a clean infographic of the electromagnetic spectrum with labeled regions. The chart-like style and direction arrow help students understand order and scale at a glance. By specifying labels up front, you reduce the risk of missing key bands. The final output is a 16:9 spectrum infographic suitable for teaching.

  ---

- **Forestry (sustainable cycle):**

  ```text
  Create an illustration of a sustainable forest management cycle.

  STYLE:
  Flat vector icons, green palette

  REQUIREMENTS:
  - Circular flow with arrows
  - Steps: assessment, planting, monitoring, harvesting, restoration

  OUTPUT:
  Aspect ratio: 1:1
  ```

  >This prompt creates a process-cycle illustration that works well for social posts or square handouts. The “circular flow with arrows” requirement pushes the model to show the management cycle as a loop instead of a list. A green palette and flat icons keep it visually consistent and professional. The final output is a 1:1 sustainable forestry cycle graphic.

  ---

#### 3) Professional visuals
- **Marketing/Social Media:**
  - Product Advertisement Post:

    ```text
    Create a high-impact social media product advertisement.

    SUBJECT:
    A modern smartphone displayed prominently

    SCENE/ENVIRONMENT:
    Clean studio background with soft gradient colors

    STYLE:
    Semi-realistic product illustration, modern ad style

    LIGHTING:
    Studio lighting with clear highlights and shadows

    MOOD:
    Confident, premium, trustworthy

    TEXT IN IMAGE:
    "Experience Speed Like Never Before"

    COMPOSITION:
    - Product slightly centered
    - Headline text at the top
    - Clean background for readability

    CONSTRAINTS:
    - No logos or real brand names
    - Text must be clearly readable
    - No misleading claims

    OUTPUT:
    Aspect ratio: 1:1
    ```

    >This prompt generates a ready-to-post product ad with a built-in headline. It shows how to combine visual hierarchy (product first, text second) while keeping claims generic and safe. The square format is ideal for Instagram and Facebook feeds. Students can easily swap the product type or headline text.

    ---

    For more image-prompt examples, see: <https://ai.google.dev/gemini-api/docs/image-generation#image-generation-prompts>

## 8) Gemini (Dashboards + Data Visualization)

### Practical items to cover
1. **Ask-for-chart prompts (bar/line/pie, pivot summaries)**
2. **Executive KPI dashboards (Finance/Aftersales/Customer Service)**
3. **Audit dashboards (exceptions, anomalies, control failures)**
4. **Health dashboards (clinic activity, outbreak signals)**

### Practical examples

#### 1) Ask-for-chart prompts
- Copy-ready prompt (requires file upload):

  ```text
  You are a data analyst.

  TASK:
  Analyze the dataset I upload and produce a month-by-month summary and a line chart recommendation.

  INPUT:
  [UPLOAD FILE: data.csv]
  Tell me which columns represent date, category, and amount if not obvious.

  OUTPUT:
  - A monthly summary table.
  - A recommended line chart specification (x-axis, y-axis, series).
  - 3 insights in plain English.
  ```

  >This prompt asks the AI to act as a data analyst and summarize an uploaded dataset by month. It also makes the assistant explain how to visualize the result by specifying what should go on the x-axis, y-axis, and series for a line chart. The “tell me which columns…” line prevents mistakes when column names are unclear. The final output is a monthly summary table, a chart recommendation you can build in Excel/Power BI, and three plain-English insights.

  ---

- Copy-ready prompt (pivot + percent change, requires upload):

  ```text
  You are a business intelligence analyst.

  TASK:
  Build a pivot-style summary by category × month, including totals and month-over-month percent change.

  INPUT:
  [UPLOAD FILE: data.csv]
  Expected columns: date, category, amount (or tell me the column names).

  OUTPUT:
  - A table with: category, month, total_amount, mom_percent_change.
  - A short note: which categories are growing/shrinking the fastest.
  ```

  >This prompt asks the AI to act as a BI analyst and produce a pivot-style summary plus month-over-month change. It forces the output into a tidy table so it’s easy to chart and filter later. The percent-change column helps you spot momentum (not just totals), which is useful for executive reviews. The final output is a category-by-month table with MoM change plus a short interpretation of the fastest movers.

  ---

#### 2) Executive KPI dashboards
- **Aftersales:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a dashboard designer.

    TASK:
    Design an Aftersales KPI dashboard from the dataset.

    INPUT:
    [UPLOAD FILE: tickets.csv]
    Expected columns: created_at, closed_at, issue_category, priority, agent

    REQUIRED KPIs:
    - Tickets opened vs closed (by week)
    - SLA breach rate
    - Top 5 issues
    - Average resolution time

    OUTPUT:
    - A dashboard layout (sections + charts).
    - The exact calculations/formulas needed.
    - 5 insights executives care about.
    ```

    >This prompt asks the AI to act as a dashboard designer and propose a complete Aftersales KPI dashboard. It defines the exact KPIs required (volume, SLA breach, issue mix, resolution time) and requests both layout and calculations so the dashboard is buildable, not just conceptual. It also demands executive-level insights so the dashboard tells a story and supports decisions. The final output is a dashboard plan with charts, formulas, and leadership-friendly insights.

    ---

- **Customer Service:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a customer support analytics lead.

    TASK:
    Propose a dashboard by agent.

    INPUT:
    [UPLOAD FILE: tickets.csv]
    Expected columns: agent, created_at, first_response_at (or messages), closed_at, reopened (True/False)
    Optional: csat_score

    METRICS:
    - First response time
    - Resolution time
    - Reopen rate
    - CSAT proxy (define one if csat_score missing)

    OUTPUT:
    - Dashboard layout + formulas + recommended targets.
    ```

    >This prompt asks the AI to act as a customer support analytics lead and design an agent-performance dashboard. It focuses on operational metrics (first response, resolution, reopen rate) and includes target suggestions so managers know what “good” looks like. If CSAT is missing, it asks the assistant to define a reasonable proxy instead of stopping. The final output is a dashboard layout with formulas and recommended targets you can implement.

    ---

- **Finance:**
  - Copy-ready prompt (requires file upload):

    ```text
    You are a finance analyst.

    TASK:
    Build a cashflow dashboard specification.

    INPUT:
    [UPLOAD FILE: cashflow.csv]
    Expected columns: date, amount, type (inflow/outflow), category

    OUTPUT:
    - Weekly inflow/outflow chart spec
    - Burn rate calculation
    - Simple next-month forecast method (explain assumptions)
    - 5 CFO-level insights
    ```

    >This prompt asks the AI to act as a finance analyst and design a cashflow dashboard, not just compute a single metric. It produces a weekly inflow/outflow view, a burn-rate calculation, and a simple forecast approach with assumptions stated clearly. That helps you explain the numbers in meetings and avoid “black box” forecasting. The final output is a dashboard specification plus CFO-level insights you can present.

    ---

#### 3) Audit dashboards
- Copy-ready prompt (audit exceptions dashboard, requires file upload):

  ```text
  You are an audit analytics specialist.

  TASK:
  Create an audit exceptions dashboard plan from the dataset.

  INPUT:
  [UPLOAD FILE: transactions.csv]
  Expected columns: transaction_id, date_time, amount, department, user, vendor, approval_limit

  EXCEPTIONS TO DETECT:
  - Duplicates
  - Weekend postings
  - Split purchases just under approval limits (e.g., multiple transactions to same vendor within 3 days just below limit)

  OUTPUT:
  - Dashboard layout (charts + filters)
  - Rules/logic for each exception
  - An exceptions heatmap by department
  ```

  >This prompt asks the AI to act as an audit analytics specialist and translate transaction data into an exceptions-focused dashboard. It defines the exact exception patterns to look for (duplicates, weekend postings, split purchases near approval limits) so the logic is audit-relevant. The heatmap requirement ensures you can quickly see where risk clusters by department. The final output is a dashboard layout plus clear detection rules you can implement and defend.

  ---

#### 4) Health dashboards
- Copy-ready prompt (cases dashboard, requires file upload):

  ```text
  You are a public health data analyst.

  TASK:
  Create a weekly cases dashboard concept and narrative insights.

  INPUT:
  [UPLOAD FILE: cases.csv]
  Expected columns: facility, date, diagnosis, case_count

  OUTPUT:
  - Dashboard layout (line charts, facility filters, diagnosis filters)
  - Automatic spike detection rule
  - 5 narrative insights for leadership
  ```

  >This prompt asks the AI to act as a public health data analyst and design a weekly cases dashboard with interpretive insights. It includes filters (facility, diagnosis) so leaders can drill down, and it requires a spike-detection rule so unusual increases are highlighted automatically. The narrative insights help you turn charts into a leadership briefing. The final output is a dashboard layout, a spike rule, and five leadership-friendly insights.

  ---

- Copy-ready prompt (vaccination coverage, requires file upload):

  ```text
  You are a monitoring and evaluation (M&E) analyst.

  TASK:
  Build a vaccination coverage dashboard: target vs actual by ward.

  INPUT:
  [UPLOAD FILE: vaccination.csv]
  Expected columns: ward, month, target, actual

  OUTPUT:
  - A table with coverage_percent = actual/target
  - A bar chart spec per ward
  - A list of underperforming wards and possible operational reasons
  ```

  >This prompt asks the AI to act as an M&E analyst and design a vaccination coverage dashboard. It computes a clear coverage percentage (actual/target) and specifies a bar-chart view by ward so gaps are obvious. It also asks for possible operational reasons behind low performance, which helps move from reporting to action planning. The final output is a coverage table, chart specification, and an underperformance list with plausible reasons to investigate.

---

## 9) NotebookLM

### Practical items to cover
1. **Chat with sources (policy PDFs, guidelines, lecture notes)**
2. **Generate study guides, FAQs, MCQs from uploaded sources**
3. **Compare multiple sources (where they agree/disagree)**
4. **Turn sources into briefing notes (1-page memos)**

### Practical examples

#### 1) Chat with sources
- **Medicine:** Copy-ready prompt (requires source upload):

  ```text
  Use only the uploaded sources.

  TASK:
  Summarize first-line management and contraindications for [CONDITION].

  OUTPUT:
  - First-line management (bullets)
  - Contraindications (bullets)
  - Red flags / when to refer (bullets)
  - Source quotes (3–5 short quotes) with page/section references if available
  ```

  >This prompt is designed for NotebookLM’s “chat with sources” workflow and forces grounded answers. By starting with “Use only the uploaded sources,” it prevents the model from pulling in outside information and keeps the summary aligned to your exact guideline/notes. The output structure gives you a quick clinical overview plus safety items (contraindications and red flags). The final output is a bullet summary supported by short quotes from the uploaded documents.

  ---

- **Law:** Copy-ready prompt (requires source upload):

  ```text
  Use only the uploaded sources.

  TASK:
  Extract the elements of the offence for [OFFENCE NAME] and list key precedents.

  OUTPUT:
  - Elements of the offence (numbered)
  - Key cases (case name → principle)
  - Common defenses/exceptions (if mentioned)
  - Source excerpts supporting each point
  ```

  >This prompt asks NotebookLM to extract legal doctrine only from the materials you uploaded. It breaks the answer into elements, key precedents, and defenses/exceptions, which matches how students and practitioners often study offences. The requirement for source excerpts helps you verify each point and quote accurately in notes or briefs. The final output is a structured offence breakdown grounded in your sources.

  ---

- **Audit:** Copy-ready prompt (requires source upload):

  ```text
  Use only the uploaded sources.

  TASK:
  From the policy, list what evidence is required for approvals above [AMOUNT].

  OUTPUT:
  - Evidence checklist (bullets)
  - Who must approve (roles)
  - Typical mistakes and how to avoid them
  - Direct quotes from the policy supporting each requirement
  ```

  >This prompt asks NotebookLM to turn a policy document into an approvals evidence checklist. The “use only the uploaded sources” rule keeps the output compliant with your internal policy instead of generic advice. Including quotes for each requirement makes the checklist auditable and easy to defend during reviews. The final output is a practical evidence checklist plus approver roles and common pitfalls.

  ---

#### 2) Study guides + MCQs
- **Nursing/Midwifery:** Copy-ready prompt (requires source upload):

  ```text
  Use only the uploaded notes.

  TASK:
  Generate 20 exam-style multiple-choice questions.

  REQUIREMENTS:
  - Provide 4 options (A–D).
  - Indicate the correct answer.
  - Provide a 1–2 sentence explanation citing the relevant note section.

  OUTPUT:
  - 20 MCQs in a numbered list.
  ```

  >This prompt asks NotebookLM to generate exam-style MCQs strictly from your uploaded notes. It enforces the standard multiple-choice structure (A–D), provides the correct answer, and requires short explanations that cite the relevant section. That makes the questions teachable and traceable to the source material. The final output is a ready-to-use set of 20 MCQs for practice or revision sessions.

  ---

- **Pharmacy:** Copy-ready prompt (requires source upload):

  ```text
  Use only the uploaded sources.

  TASK:
  Create flashcards in this format:
  Drug/Class | Mechanism | Indications | Contraindications | Common side effects | Counseling points

  OUTPUT:
  - 25 flashcards in a table.
  ```

  >This prompt asks NotebookLM to convert your pharmacy sources into structured flashcards. The fixed columns (mechanism, indications, contraindications, side effects, counseling points) ensure each card is clinically useful and consistent across drugs. Because it uses only uploaded sources, the flashcards stay aligned to your curriculum or guideline set. The final output is a 25-row flashcard table you can paste into a spreadsheet or study app.

  ---

- **Forestry:** Copy-ready prompt (requires source upload):

  ```text
  Use only the uploaded sources.

  TASK:
  Create a study guide on forest ecology.

  OUTPUT:
  - Key terms (20) with short definitions
  - Short-answer questions (10) with model answers
  - Common misconceptions (5)
  ```

  >This prompt asks NotebookLM to produce a complete study package from your forestry sources. It generates key terms for vocabulary building, short-answer questions for exam practice, and common misconceptions to correct typical errors. The structured outputs make it easy to turn into handouts or revision notes. The final output is a study guide that is grounded in the uploaded materials.

  ---

#### 3) Compare sources
- Copy-ready prompt (compare guidelines, requires upload of both):

  ```text
  Use only the uploaded sources.

  TASK:
  Compare Guideline A vs Guideline B on recommended screening intervals for [TOPIC].

  OUTPUT:
  - A comparison table: guideline, recommended interval, target population, rationale, evidence quality notes
  - A short recommendation for practice (with uncertainty notes)
  ```

  >This prompt asks NotebookLM to compare two uploaded guidelines on a specific topic and identify where they agree or differ. The comparison table forces clarity on intervals, populations, and rationale, while the evidence-quality notes help you assess confidence. The recommendation section includes uncertainty, which is important when guidelines conflict. The final output is a structured comparison you can use for teaching or policy discussions.

  ---

- Copy-ready prompt (compare two papers, requires upload of both):

  ```text
  Use only the uploaded sources.

  TASK:
  Compare Paper A vs Paper B.

  OUTPUT:
  - Sample size and population
  - Methods
  - Key findings
  - Limitations
  - Conclusions
  - Which paper is more applicable to Nigeria-context settings and why
  ```

  >This prompt asks NotebookLM to compare two uploaded research papers in a structured way. It focuses on the study design essentials (population, methods, findings, limitations) so you can evaluate quality quickly. The Nigeria-context question encourages practical interpretation rather than abstract summary. The final output is a side-by-side comparison that helps you choose which evidence is more applicable.

  ---

#### 4) Briefing notes
- **Civil servant:** “Turn this policy into: (a) 5-bullet brief for director, (b) 1-paragraph press statement.”

- Copy-ready prompt (requires source upload or paste):

  ```text
  Use only the policy text provided.

  TASK:
  Turn the policy into:
  A) a 5-bullet brief for a Director
  B) a 1-paragraph public press statement (no sensitive details)

  CONSTRAINTS:
  - Keep facts and dates accurate.
  - Use official tone.

  OUTPUT:
  - Section A (5 bullets)
  - Section B (one paragraph)
  ```

  >This prompt asks NotebookLM to turn a policy into two communication products for different audiences. It creates a short internal brief for a Director (high-level, action-oriented) and a public-facing press statement that avoids sensitive details. The constraints protect accuracy (facts/dates) and preserve official tone. The final output is a two-part brief you can use immediately for internal and external communication.

---

## 10) Consensus (evidence-based research answers)

### Practical items to cover
1. **Clinical questions (PICO framing)**
2. **Rapid evidence summaries for policy and teaching**
3. **Finding consensus vs controversy**
4. **Building annotated bibliographies quickly**

### Practical examples

#### 1) PICO framing
- **Clinical Epidemiology:** “In adults with hypertension (P), does DASH diet (I) vs usual diet (C) reduce BP (O)?”
- **Physiotherapy:** “Does telerehab improve outcomes vs in-person rehab?”

- Copy-ready prompt (clinical PICO search):

  ```text
  You are an evidence-based medicine assistant.

  TASK:
  Using Consensus, answer this clinical question and cite the best available studies.

  QUESTION (PICO):
  Population: [PASTE]
  Intervention: [PASTE]
  Comparison: [PASTE]
  Outcome: [PASTE]

  OUTPUT:
  - Short answer (2–3 sentences)
  - Evidence summary (bullets)
  - Key papers (5–10) with 1-line takeaway each
  - Limitations / uncertainty
  ```

  >This prompt asks the AI to use Consensus to answer a clinical question using a PICO structure. The PICO fields help you ask a precise question, which improves search quality and reduces irrelevant papers. The output includes a short answer for quick decisions, then a study-backed summary with key papers and takeaways. The limitations section makes uncertainty explicit instead of overselling conclusions. The final output is an evidence-based answer with citations you can follow up on.

  ---

#### 2) Rapid evidence summaries
- Copy-ready prompt (rapid evidence brief):

  ```text
  You are a public health evidence synthesizer.

  TASK:
  Summarize the evidence for: [TOPIC]

  REQUIREMENTS:
  - Provide a balanced summary (benefits, harms, context).
  - Cite the strongest studies.
  - Provide 3 implementation recommendations for low-resource settings.

  OUTPUT:
  - 1-paragraph summary
  - Evidence bullets (with citations)
  - 3 practical recommendations
  ```

  >This prompt asks the AI to act as a public health evidence synthesizer and produce a rapid evidence brief using Consensus. It requires balance (benefits and harms) and asks for the strongest studies, which keeps the summary evidence-driven. The implementation recommendations are tailored for low-resource settings, which makes the output more realistic for many Nigerian contexts. The final output is a short brief with citations plus three actionable recommendations.

  ---

#### 3) Consensus vs controversy
- Copy-ready prompt (consensus vs controversy):

  ```text
  You are a research methodologist.

  TASK:
  For the question below, report whether there is scientific consensus or significant controversy.

  QUESTION:
  [PASTE HERE]

  OUTPUT:
  - Consensus level (High/Medium/Low) with justification
  - What most studies agree on
  - What studies disagree on and why (population, methods, outcomes)
  - 5–8 key citations
  ```

  >This prompt asks the AI to use Consensus to judge whether a question has broad scientific agreement or meaningful controversy. It forces the assistant to separate what is widely consistent across studies from what varies due to methods, populations, or outcomes. The citations requirement ensures you can trace the reasoning back to real papers. The final output is a consensus rating with justification and a shortlist of key references.

  ---

#### 4) Annotated bibliography
- Copy-ready prompt (annotated bibliography):

  ```text
  You are an academic researcher.

  TASK:
  Create an annotated bibliography on: [TOPIC]

  REQUIREMENTS:
  - 10 papers minimum.
  - Each annotation: 2–3 lines (study type, setting, key finding, limitation).
  - Prefer recent high-quality evidence.

  OUTPUT:
  - Numbered list with citation + annotation.
  ```

  >This prompt asks the AI to act as an academic researcher and build an annotated bibliography using evidence found in Consensus. It enforces a minimum number of papers and keeps each annotation short but useful (study type, setting, key finding, limitation). Preferring recent high-quality evidence helps avoid outdated or weak sources. The final output is a numbered bibliography with citations and concise annotations you can expand into a literature review.

---

## 11) Wispr Flow (voice dictation)

### Practical items to cover
1. **Dictating structured documents (SOAP notes, memos, audit reports)**
2. **Fast email drafting for Aftersales/HR/Finance**
3. **Meeting minutes and action items**
4. **Multilingual/Accent handling + cleanup**

### Practical examples

#### 1) Structured dictation
- **Doctors:** “Dictate clinical encounter → auto-format into SOAP.”
- **Audit:** “Dictate finding: condition, criteria, cause, effect, recommendation.”
- **HSE:** “Dictate incident report: what happened, immediate action, root cause, corrective action.”

- Copy-ready dictation script (Doctors → SOAP):

  ```text
  (READ ALOUD INTO WISPR FLOW)

  Subjective: The patient is a 45-year-old with headache for three days, worse in the evening. No blurred vision. No chest pain.
  Objective: Blood pressure one sixty over one hundred. Pulse ninety. Temperature normal. No focal neurological deficit.
  Assessment: Uncontrolled hypertension, likely contributing to headache.
  Plan: Start lifestyle counseling, prescribe appropriate antihypertensive per local protocol, arrange follow-up in one week, counsel on danger signs.

  (END)
  ```

  >This script is meant to be read aloud into Wispr Flow so the tool can transcribe and keep your structure. Because it uses the SOAP headings (Subjective/Objective/Assessment/Plan), the transcription naturally lands in a clinical note format instead of a messy paragraph. The placeholders are minimal, so it’s easy for beginners to practice dictation without losing clinical meaning. The final outcome is a clean SOAP-style note draft that can be copied into documentation (after clinical review).

  ---

- Copy-ready dictation script (Audit finding → structured):

  ```text
  (READ ALOUD INTO WISPR FLOW)

  Condition: In April, we found 12 payments above the approval threshold processed with only one approver.
  Criteria: Company policy requires two approvals for payments above the threshold.
  Cause: Approval workflow was bypassed due to system access assigned to a single user.
  Effect: Increased risk of unauthorized payments and fraud.
  Recommendation: Enforce two-step approval in the system, review access rights monthly, and retrain finance staff.

  (END)
  ```

  >This script trains users to dictate audit findings in a standard structure (Condition, Criteria, Cause, Effect, Recommendation). Reading it aloud helps Wispr Flow produce a neatly formatted write-up rather than scattered notes. It also models how to include numbers and policy requirements clearly, which improves audit clarity. The final outcome is a structured finding draft you can paste into an audit report and refine.

  ---

- Copy-ready dictation script (HSE incident → structured):

  ```text
  (READ ALOUD INTO WISPR FLOW)

  Incident summary: On Monday at 10 a.m., a staff member slipped near the loading bay due to a wet floor.
  Immediate action: We provided first aid and placed warning signs.
  Root cause: No wet-floor signage during cleaning and poor drainage.
  Corrective actions: Update cleaning procedure, install anti-slip mats, and assign supervisor checks.
  Follow-up: Verify corrective actions within seven days.

  (END)
  ```

  >This script shows how to dictate an HSE incident report in a way that captures the full story (what happened, immediate action, root cause, corrective action, follow-up). The clear headings make the transcription easier to scan and reduce the chance of missing key details like dates and verification timelines. It’s ideal for practicing fast reporting after an incident. The final outcome is a structured incident report draft you can convert into a formal record.

  ---

#### 2) Email drafting
- **Finance:** “Dictate a payment follow-up email with invoice details.”
- **HR:** “Dictate an interview invite + instructions.”
- **Aftersales:** “Dictate resolution summary + next steps.”

- Copy-ready dictation script (Finance payment follow-up):

  ```text
  (READ ALOUD)

  Subject: Payment Reminder for Invoice [INVOICE NUMBER]
  Hello [CUSTOMER NAME], this is a gentle reminder that invoice [INVOICE NUMBER] for [AMOUNT] was due on [DUE DATE].
  Please find payment details here: [PAYMENT LINK OR BANK DETAILS].
  If you have already paid, kindly ignore this message and share proof of payment.
  Thank you.

  (END)
  ```

  >This script is a dictation template for a payment reminder email. Reading it aloud helps you draft professional email copy quickly while still capturing key details (invoice number, amount, due date, payment instructions). The placeholders make it easy to personalize without rewriting the whole message. The final outcome is a clean email draft you can send after double-checking the numbers.

  ---

- Copy-ready dictation script (HR interview invite):

  ```text
  (READ ALOUD)

  Subject: Interview Invitation – [ROLE]
  Dear [CANDIDATE NAME], thank you for your application. We would like to invite you for an interview for the role of [ROLE].
  Date and time: [DATE/TIME]. Location or meeting link: [LOCATION/LINK].
  Please come with a valid ID and copies of your credentials. Kindly confirm your availability.

  (END)
  ```

  >This script helps HR staff dictate a consistent interview invitation without forgetting important logistics. It includes role, date/time, location/link, and required items so candidates get clear instructions. Using a fixed structure also reduces back-and-forth emails. The final outcome is a ready-to-send interview invite draft.

  ---

- Copy-ready dictation script (Aftersales resolution summary):

  ```text
  (READ ALOUD)

  Subject: Resolution Update – Ticket [TICKET ID]
  Hello [CUSTOMER NAME], thank you for your patience. We have resolved your issue regarding [ISSUE SUMMARY].
  What we did: [ACTIONS TAKEN].
  Next steps: [NEXT STEPS].
  If you experience the issue again, reply to this email and we will assist immediately.

  (END)
  ```

  >This script helps aftersales teams dictate a structured resolution update that customers can understand. It separates the summary, what was done, and next steps, which improves clarity and reduces repeat contacts. The placeholders allow quick personalization (ticket ID, issue summary, actions taken). The final outcome is a professional resolution email draft.

  ---

#### 3) Meeting minutes
- Copy-ready dictation script (meeting minutes):

  ```text
  (READ ALOUD)

  Meeting title: Weekly Operations Meeting.
  Date: [DATE]. Attendees: [NAMES].
  Agenda: service backlog, procurement status, finance approvals.
  Decisions: We will prioritize high-severity tickets. Procurement will onboard two new vendors.
  Action items: John to clear twenty tickets by Friday. Mary to submit vendor documents by Wednesday. Finance to finalize payment approvals by Thursday.

  (END)
  ```

  >This script is a fast way to dictate meeting minutes with decisions and action items. The structure (title, date, attendees, agenda, decisions, action items) ensures the transcript is usable immediately as a record. Including owners and deadlines in the action items makes follow-up easier. The final outcome is clean minutes you can paste into email, Teams, or a tracker.

  ---

#### 4) Cleanup
- Copy-ready dictation script (messy → clean demo):

  ```text
  (READ ALOUD EXACTLY, WITH FILLER WORDS)

  So um basically we had like a lot of complaints this week, mostly delivery delays and billing issues, and I think we should maybe, you know, call the vendor and also update the customers more frequently.

  (END)
  ```

  >This script is a teaching demo: you intentionally speak with filler words to show how dictation tools capture natural speech. After transcription, students can practice “cleanup” by removing fillers, tightening sentences, and turning the paragraph into a clearer update. It’s a safe way to learn editing after dictation without sensitive content. The final outcome is a cleaned, professional version of the messy spoken text.

---

## 12) Comet Browser (by Perplexity AI)

### Practical items to cover
1. **Research with citations (medical, legal, academic, market)**
2. **Compare sources and extract contradictions**
3. **Build briefing packs (links + summaries + next actions)**
4. **Fact-checking and claim tracing**

### Practical examples

#### 1) Research with citations
- **Public Health:** “Find latest evidence on malaria prevention strategies in sub-Saharan Africa; cite WHO + 3 recent studies.”
- **Law:** “Research current legal position on contract termination remedies in Nigeria; provide 5 sources and summarize.”
- **Procurement:** “Find standard lead times and best practices for vendor due diligence; cite ISO/procurement guides.”
- **Marketing:** “Find 2026 trends for social media in healthcare education; cite credible sources.”

- Copy-ready prompt (cited research brief):

  ```text
  You are a research assistant using Comet Browser (Perplexity).

  TASK:
  Research the topic below and provide a cited brief.

  TOPIC:
  [PASTE TOPIC HERE]

  REQUIREMENTS:
  - Provide 6–10 credible sources (WHO, government, journals, standards bodies).
  - For each source: 1-line summary + link.
  - Then write a 200–300 word synthesis.
  - Include a “Nigeria relevance” section (3 bullets).

  OUTPUT:
  - Sources list
  - Synthesis
  - Nigeria relevance bullets
  ```

  >This prompt asks Comet Browser (Perplexity) to do web research with citations, not just give opinions. It forces the assistant to collect multiple credible sources, summarize each source in one line, and then write a short synthesis that combines the evidence. The “Nigeria relevance” bullets ensure the brief doesn’t stay generic and helps learners practice contextual interpretation. The final outcome is a cited research brief with links you can verify.

  ---

#### 2) Compare sources
- Copy-ready prompt (compare guidelines):

  ```text
  You are a research analyst.

  TASK:
  Compare these two guidelines and list differences in recommendations.

  INPUT:
  - Guideline A link: [PASTE]
  - Guideline B link: [PASTE]
  - Topic focus: [PASTE]

  OUTPUT:
  - A comparison table.
  - A short recommendation on which guideline to follow in [CONTEXT] and why.
  ```

  >This prompt asks Comet to compare two guideline documents and extract differences in recommendations. By requiring a comparison table, it makes disagreements and scope differences easy to see (who the guideline targets, what intervals/thresholds are used, etc.). The short recommendation section helps you choose what to follow for a specific context while still justifying the choice with sources. The final outcome is a side-by-side comparison plus an actionable recommendation.

  ---

- Copy-ready prompt (vendor comparison table):

  ```text
  You are a procurement analyst.

  TASK:
  Compare Vendor A vs Vendor B product/service specs and summarize differences.

  INPUT:
  - Vendor A link or pasted spec: [PASTE]
  - Vendor B link or pasted spec: [PASTE]

  OUTPUT:
  - Table with: feature/spec, vendor A, vendor B, winner, notes.
  - Recommendation with risks.
  ```

  >This prompt helps procurement teams compare two vendor offerings using sourced specs. It forces a structured table so features and trade-offs are visible, then it asks for a recommendation that includes risks (e.g., warranty gaps, delivery timelines, compliance issues). This reduces “gut-feel” decisions and improves documentation for approvals. The final outcome is a comparison table and a risk-aware recommendation.

  ---

#### 3) Briefing packs
- **Internal Control:** “Create a briefing pack on segregation of duties controls for a medium-sized company: summary, risks, recommended controls, sources.”
- **Civil service:** “Briefing pack on AI policy: opportunities, risks, governance principles.”

- Copy-ready prompt (briefing pack):

  ```text
  You are preparing a briefing pack for senior leadership.

  TOPIC:
  [PASTE TOPIC HERE]

  REQUIREMENTS:
  - 1-page executive summary (bullets)
  - Key risks (5–10)
  - Recommendations (5–10)
  - Implementation checklist (who does what)
  - Sources (6–10 links)

  OUTPUT:
  - Provide the full briefing pack in sections.
  ```

  >This prompt asks Comet to produce a leadership-ready briefing pack, not just a summary. It creates an executive overview, lists risks and recommendations, and adds an implementation checklist so the document can drive action. The sources section ensures the pack is defensible and easy to fact-check. The final outcome is a structured briefing document you can paste into a memo or slide deck.

  ---

#### 4) Fact-checking
- Copy-ready prompt (claim tracing):

  ```text
  You are a fact-checker.

  CLAIM:
  [PASTE CLAIM HERE]

  TASK:
  Trace this claim to the original source.

  REQUIREMENTS:
  - Provide the earliest credible source you can find.
  - Assess study quality (study type, sample size, limitations).
  - State whether the claim is supported, partially supported, or unsupported.

  OUTPUT:
  - Verdict
  - Evidence summary with links
  - Limitations
  ```

  >This prompt teaches claim verification by forcing the assistant to trace a statement back to its original source. It asks for the earliest credible source, a quick quality assessment (study type, sample size, limitations), and a clear verdict (supported/partially/unsupported). This helps learners avoid repeating weak or misleading claims from secondary blogs. The final outcome is a fact-check report with links and limitations.

---

## 13) Bohrium (AI-powered academic research)

### Practical items to cover
1. **Paper discovery (topic → key papers, authors, venues)**
2. **Summarize and extract methods/limitations**
3. **Reproduce a method (code notebooks / computational workflows)**
4. **Research roadmap (what to read next)**

### Practical examples

#### 1) Paper discovery
- **Fluid Dynamics:** “Find seminal + recent papers on turbulence modeling; list 10 must-reads.”
- **Biotech:** “Find key papers on CRISPR delivery methods; compare pros/cons.”
- **Forestry:** “Find research on deforestation monitoring using remote sensing; list methods and datasets.”

- Copy-ready prompt (paper discovery + must-reads):

  ```text
  You are an academic research assistant.

  TASK:
  Find seminal and recent papers on the topic below.

  TOPIC:
  [PASTE TOPIC HERE]

  REQUIREMENTS:
  - 10 must-read papers.
  - For each: citation, year, why it matters (1 line), and link.
  - Then list 5 researchers/authors to follow.

  OUTPUT:
  - Must-read list
  - Authors to follow
  ```

  >This prompt asks Bohrium to find both seminal (foundational) and recent papers on a topic. It forces a “must-read” list with a short explanation of why each paper matters, which helps beginners know what to prioritize. Listing authors to follow makes it easier to track a field over time. The final outcome is a curated reading list with links plus key researchers.

  ---

#### 2) Summaries and extraction
- Copy-ready prompt (paper extraction, requires pasted abstract or uploaded PDF text):

  ```text
  You are a research paper analyst.

  TASK:
  Extract key details from the paper text below.

  OUTPUT FORMAT:
  - Research question:
  - Dataset:
  - Method:
  - Metrics:
  - Key results:
  - Limitations:
  - Future work:

  PAPER TEXT:
  [PASTE HERE]
  ```

  >This prompt asks Bohrium to extract the key components of a paper into a consistent template. It pulls out the research question, dataset, method, metrics, results, limitations, and future work so you can understand a paper quickly without rereading everything. The structured format also helps when building literature review tables. The final outcome is a clear one-page style extraction based on the text you pasted.

  ---

- Copy-ready prompt (theme map from 5 abstracts):

  ```text
  You are a literature review assistant.

  TASK:
  Cluster these 5 abstracts into themes.

  OUTPUT:
  - Theme name → which papers belong → why
  - A short gap analysis: what is missing in the literature

  ABSTRACTS:
  1) [PASTE]
  2) [PASTE]
  3) [PASTE]
  4) [PASTE]
  5) [PASTE]
  ```

  >This prompt asks Bohrium to cluster a small set of abstracts into themes, which is a practical way to start a literature review. It explains why each paper belongs to a theme and then identifies gaps (what’s missing or under-studied). This helps learners move from “a pile of papers” to a structured understanding of a research area. The final outcome is a theme map plus a short gap analysis.

  ---

#### 3) Reproduction workflows
- Copy-ready prompt (repro plan):

  ```text
  You are a reproducibility engineer.

  TASK:
  Create a step-by-step plan to reproduce the results of the paper below.

  REQUIREMENTS:
  - Dependencies and environment
  - Dataset acquisition steps
  - Implementation steps
  - Evaluation metrics and expected outputs
  - Common failure points and troubleshooting

  PAPER:
  [PASTE CITATION + LINK OR ABSTRACT]
  ```

  >This prompt asks Bohrium to plan how to reproduce a study’s results step-by-step. It covers environment/dependencies, dataset acquisition, implementation, evaluation metrics, expected outputs, and troubleshooting—exactly the areas where reproductions usually fail. This is useful for students doing projects or theses with computational methods. The final outcome is a practical reproduction plan you can follow and adapt.

  ---

#### 4) Roadmap
- Copy-ready prompt (reading roadmap):

  ```text
  You are my research supervisor.

  TASK:
  Create a 2-week reading plan for the topic below.

  TOPIC:
  [PASTE HERE]

  REQUIREMENTS:
  - Week 1: foundations + key concepts
  - Week 2: state-of-the-art + open problems
  - Daily plan with 2–3 papers/chapters per day
  - End with: 5 potential research questions/gaps

  OUTPUT:
  - A day-by-day plan.
  ```

  >This prompt asks Bohrium to act like a research supervisor and create a short reading schedule that builds from foundations to current research. The daily structure prevents overwhelm and helps learners progress systematically. Ending with research questions/gaps turns reading into a project direction rather than passive consumption. The final outcome is a 2-week plan with daily readings and potential research questions.

---

## 14) ElevenLabs (voice generation)

### Practical items to cover
1. **Voiceovers for lectures and microlearning**
2. **Role-play simulations (customer service, clinical counseling)**
3. **Multi-accent/multilingual training materials**
4. **Accessibility (audio versions of notes)**

### Practical examples

#### 1) Lecture voiceovers
- **PhD Public Health lecturer:** “Turn lecture notes into a 6-minute audio summary with section breaks.”
- **Law lecturer:** “Create an audio revision pack: definitions + examples + quick quiz.”

- Copy-ready prompt (generate a 6-minute voiceover script, paste notes):

  ```text
  You are a lecture voiceover script writer.

  TASK:
  Turn the notes below into a voiceover script that is about 6 minutes long.

  REQUIREMENTS:
  - Use section breaks with spoken transitions (e.g., “Next, let’s talk about…”).
  - Add 3 recap moments (“Quick recap: …”).
  - Keep tone professional and clear.

  OUTPUT:
  - Return a script formatted for voice reading.

  LECTURE NOTES:
  [PASTE HERE]
  ```

  >This prompt asks the AI to turn lecture notes into a script that sounds natural when read aloud. The section breaks and spoken transitions make the audio easier to follow, and the recap moments reinforce learning for listeners. The “about 6 minutes” requirement helps you control length for microlearning or short lectures. The final outcome is a voiceover-ready script you can paste into ElevenLabs to generate audio.

  ---

- Copy-ready prompt (law revision audio pack):

  ```text
  You are a law tutor.

  TASK:
  Create an audio revision script for the topic below.

  TOPIC:
  [PASTE LAW TOPIC]

  REQUIREMENTS:
  - Definitions (clear)
  - 2 examples
  - A quick quiz: 5 questions (pause after each)
  - Provide answers at the end

  OUTPUT:
  - A single voiceover script.
  ```

  >This prompt creates a structured law revision script designed for audio delivery. It includes definitions, examples, and a built-in quiz with pauses, which makes the audio interactive rather than passive. Answers at the end allow self-checking. The final outcome is one continuous script you can paste into ElevenLabs to create an audio revision pack.

  ---

#### 2) Role-play simulations
- **Customer Service:**
  - “Create 3 voice scripts: angry customer, confused customer, VIP customer. Students practice responses.”
- **Medicine/Nursing:**
  - “Simulate counseling: medication adherence conversation; add empathy, teach-back questions.”

- Copy-ready prompt (customer role-play scripts):

  ```text
  You are a role-play script writer for customer service training.

  TASK:
  Create 3 short customer voice scripts for the same scenario.

  SCENARIO:
  [PASTE ISSUE: e.g., delayed delivery + wrong item]

  CHARACTERS:
  1) Angry customer
  2) Confused customer
  3) VIP customer

  REQUIREMENTS:
  - Each script 30–45 seconds.
  - Include realistic details (order number placeholder, dates placeholder).
  - End each script with a clear request.

  OUTPUT:
  - Provide 3 labeled scripts.
  ```

  >This prompt generates short role-play voice scripts for customer service training. By keeping the scenario constant and changing only the customer type (angry, confused, VIP), learners can practice adapting tone and de-escalation skills. The placeholders (order number, dates) make the scripts feel realistic without using real customer data. The final outcome is three labeled scripts you can voice in ElevenLabs and use for simulations.

  ---

- Copy-ready prompt (clinical counseling role-play):

  ```text
  You are a nursing educator.

  TASK:
  Write a counseling role-play script about medication adherence.

  REQUIREMENTS:
  - Use empathy.
  - Include teach-back questions.
  - Include 3 common patient barriers (cost, side effects, forgetfulness).
  - Keep it 2–3 minutes.

  OUTPUT:
  - Provide a script with Nurse lines and Patient lines.
  ```

  >This prompt creates a realistic nurse–patient counseling conversation focused on medication adherence. It builds empathy, includes teach-back questions to confirm understanding, and incorporates common barriers (cost, side effects, forgetfulness) so the scenario matches real practice. The 2–3 minute target keeps it suitable for classroom role-play or OSCE-style practice. The final outcome is a two-person script you can turn into audio with two voices.

  ---

#### 3) Multi-accent/multilingual
- **Public Health outreach:** “Generate audio PSA in clear English + simplified phrasing; optionally create a second variant in Pidgin-friendly English (clean, respectful).”

- Copy-ready prompt (PSA script):

  ```text
  You are a public health communications officer.

  TASK:
  Write a 45–60 second public service announcement (PSA) script.

  TOPIC:
  [PASTE TOPIC: e.g., handwashing, malaria prevention, antenatal care]

  REQUIREMENTS:
  - Version A: clear English, simple phrasing
  - Version B: Pidgin-friendly English (clean, respectful), optional
  - Include a call to action

  OUTPUT:
  - Provide Version A and Version B.
  ```

  >This prompt asks the AI to write a short public service announcement script that works well as audio. Version A is simple standard English; Version B optionally adapts the same message into respectful, Pidgin-friendly English for broader reach. Including a call to action makes the PSA actionable (what listeners should do next). The final outcome is two script variants you can record or generate in ElevenLabs.

  ---

#### 4) Accessibility
- Copy-ready prompt (PDF → audio script):

  ```text
  You are an accessibility editor.

  TASK:
  Convert the document text below into an audio-friendly script.

  REQUIREMENTS:
  - Add chapter markers as: "Chapter 1:", "Chapter 2:" etc.
  - Use short sentences.
  - Add a 30-second recap at the end.

  OUTPUT:
  - Return a script ready to paste into ElevenLabs.

  DOCUMENT TEXT:
  [PASTE HERE]
  ```

  >This prompt converts written document text into a format that sounds good when spoken. Chapter markers help listeners follow along, short sentences improve clarity, and the recap at the end reinforces key points. This is especially useful for accessibility (audio versions of notes) and for learners who prefer listening. The final outcome is an audio-friendly script ready to paste into ElevenLabs for voice generation.




 -->
