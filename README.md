# 🔍 GitHub User & Repository Analytics — London

> **TDS Project 1** · Scraping, cleaning, and analyzing GitHub data via the GitHub REST API

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)
![GitHub API](https://img.shields.io/badge/GitHub-REST%20API%20v3-181717?style=flat-square&logo=github&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Findings](#-key-findings)
- [Repository Structure](#-repository-structure)
- [Data Dictionary](#-data-dictionary)
- [Methodology](#-methodology)
- [Getting Started](#-getting-started)
- [Analysis Highlights](#-analysis-highlights)
- [Actionable Recommendations](#-actionable-recommendations)

---

## 🧭 Overview

This project collects data on **GitHub users based in London with 500+ followers** and their public repositories, then performs exploratory data analysis to uncover patterns in language popularity, developer activity, and open-source contribution trends.

The scraper uses the **GitHub REST API v3** with full rate-limit handling and outputs clean, analysis-ready CSV datasets.

---

## 💡 Key Findings

1. **JavaScript dominates open-source** — JS repositories have accumulated **over 460,000 stars** in total, far outpacing all other languages, confirming its continued dominance in the open-source ecosystem.

2. **Go is a rising star** — While newer than JS, Go repos show a disproportionately high star-to-repo ratio, signaling strong community interest in the language for systems and backend projects.

3. **Follower count ≠ repository count** — Some of the highest-follower developers maintain fewer than 10 repositories, suggesting influence is driven more by project quality and community engagement than raw output volume.

---

## 📁 Repository Structure

```
Tools-In-Data-Science-Project-1/
│
├── Scrapper.py                  # GitHub API scraper with rate-limit handling
├── TDS_Project_1_Solution.ipynb # Full EDA notebook with visualizations
├── users.csv                    # Cleaned user data (500+ follower Londoners)
├── repositories.csv             # Up to 500 repos per user, sorted by push date
└── README.md                    # You are here
```

---

## 📊 Data Dictionary

### `users.csv`

| Column | Type | Description |
|---|---|---|
| `login` | string | GitHub username |
| `name` | string | Full display name |
| `company` | string | Employer (cleaned: stripped `@`, uppercased) |
| `location` | string | Self-reported location |
| `email` | string | Public email address |
| `hireable` | boolean | Whether user is open to work |
| `bio` | string | Profile biography |
| `public_repos` | int | Number of public repositories |
| `followers` | int | Follower count |
| `following` | int | Following count |
| `created_at` | datetime | Account creation date (ISO 8601) |

### `repositories.csv`

| Column | Type | Description |
|---|---|---|
| `login` | string | Owner's GitHub username |
| `full_name` | string | `owner/repo` format |
| `created_at` | datetime | Repository creation date |
| `stargazers_count` | int | Total stars |
| `watchers_count` | int | Total watchers |
| `language` | string | Primary language |
| `has_projects` | boolean | GitHub Projects enabled |
| `has_wiki` | boolean | Wiki enabled |
| `license_name` | string | SPDX license key (e.g., `mit`, `apache-2.0`) |

---

## 🔬 Methodology

### Data Collection

The `GitHubScraper` class handles all API interaction:

- **User Search** — Queries `location:London followers:>=500` via the `/search/users` endpoint, paginated at 100 results/page.
- **User Details** — Fetches full profile data for each user via `/users/{login}`.
- **Repositories** — Retrieves up to **500 most recently pushed repos** per user via `/users/{login}/repos`.

### Data Cleaning

- Company names are stripped of leading `@` symbols and converted to **uppercase** for consistency.
- `null` values in string fields are replaced with empty strings `""`.
- `hireable` defaults to `False` when not set.

### Rate Limit Handling

The scraper automatically detects **HTTP 403** responses, reads the `X-RateLimit-Reset` header, and sleeps until the window resets — no manual intervention needed.

```python
elif response.status_code == 403:
    reset_time = int(response.headers.get('X-RateLimit-Reset', 0))
    sleep_time = max(reset_time - time.time(), 0) + 1
    time.sleep(sleep_time)
```

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install requests pandas
```

### Running the Scraper

```bash
python Scrapper.py
```

You will be prompted for your **GitHub Personal Access Token**. Generate one at [github.com/settings/tokens](https://github.com/settings/tokens) — only `public_repo` read scope is needed.

> ⚠️ **Note:** Scraping a large number of users will consume GitHub API rate-limit quota (5,000 requests/hour for authenticated users). The scraper handles this automatically.

### Running the Analysis

Open the notebook in Jupyter:

```bash
jupyter notebook TDS_Project_1_Solution.ipynb
```

---

## 📈 Analysis Highlights

| Metric | Value |
|---|---|
| Total users scraped | 500+ followers in London |
| Most starred language | **JavaScript** (460,000+ stars) |
| Fastest-growing language | **Go** |
| Data collection method | GitHub REST API v3 |
| Max repos per user | 500 (sorted by most recently pushed) |

---

## ✅ Actionable Recommendations

- **New project?** Build in **JavaScript** for maximum reach and contributor potential, or **Go** if performance and systems-level work is the goal.
- **Hiring signal:** A large number of London-based developers with 500+ followers have `hireable: true` — this dataset is a useful sourcing tool.
- **Open-source strategy:** High-follower developers with fewer repos tend to have highly starred single projects. Focus on depth over breadth when building a public portfolio.

---

## 👤 Author

**Aman Mani Tiwari**
[github.com/AmanManiTiwari](https://github.com/AmanManiTiwari)

---

*Data collected via the GitHub API. This project is for educational purposes as part of the Tools in Data Science course.*
