This repository contains a simplified end-to-end data pipeline for daily loan collection call campaigns. The goal is to fetch daily CSV data dumps, clean and validate the data, transform it by computing key performance metrics for each agent, and report the results both as a CSV file and a Slack-style summary.

## Features
- **Data Ingestion:** Load `call_logs.csv`, `agent_roster.csv`, and `disposition_summary.csv` via pandas.
- **Validation & Cleaning:**
  - Check for required columns and data types.
  - Flag and drop duplicate records.
  - Identify and handle missing values in key fields.
  - Normalize text fields (e.g., call status).
  - Filter out invalid durations.
- **Join Logic:**
  - Left-join of call logs with disposition summaries and agent rosters.
  - Logging of merge indicators to surface any mismatches.
- **Feature Engineering:**
  - **Total Calls Made**
  - **Unique Loans Contacted**
  - **Connect Rate** (completed calls / total calls)
  - **Average Call Duration** (in minutes)
  - **Presence** flag (based on login time)
- **Reporting:**
  - Saves `agent_performance_summary.csv` with one row per agent per day.
  - Prints a Slack-style summary for the latest date in the data.

## Prerequisites
- Python 3.8 or higher
- pandas
- numpy (optional)
- python-dotenv (optional)

## Installation
1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/dpdzero-data-ops.git
   cd dpdzero-data-ops
   ```
2. (Optional) Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate   # macOS/Linux
   venv\Scripts\activate    # Windows
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Directory Structure
```
project_root/
├── data/
│   ├── call_logs.csv
│   ├── agent_roster.csv
│   └── disposition_summary.csv
├── notebooks/
│   └── run_pipeline.ipynb   # Jupyter notebook with full pipeline code
├── scripts/
│   └── run_pipeline.py      # Modular CLI script (optional)
├── README.md
└── requirements.txt
```

## Usage
### Jupyter Notebook
1. Open the notebook:
   ```bash
   jupyter notebook notebooks/run_pipeline.ipynb
   ```
2. Adjust file paths at the top of the notebook if needed.
3. Run all cells to:
   - Load, validate, and clean data
   - Merge datasets
   - Compute metrics
   - Save `agent_performance_summary.csv`
   - Print Slack-style summary and preview results

### CLI Script
If using the optional CLI script:
```bash
python scripts/run_pipeline.py \
  --call-logs data/call_logs.csv \
  --roster data/agent_roster.csv \
  --dispo data/disposition_summary.csv \
  --out agent_performance_summary.csv
```

## Reporting
After running the pipeline, find the output CSV at:
```
agent_performance_summary.csv
```
And a Slack-style summary printed to stdout, e.g.:
```
📊 *Agent Summary for 2025-04-28*
• *Top Performer:* Ravi Sharma (98% connect rate)
• *Total Active Agents:* 45
• *Average Duration:* 6.5 min
```

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for enhancements and bug fixes.

## License
This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

