# Job Skill BD

A Claude Skill for finding recent on-site and hybrid jobs in Bangladesh and remote jobs open to people living in Bangladesh, then preparing a tailored CV and cover letter only for the jobs you choose.

You attach your master CV once. Claude rebuilds its design as a template, so every tailored CV keeps your layout.

## Features

- Searches Bangladeshi job sites, remote job boards, company career boards and large job sites
- Uses the free plan of [Apify](https://apify.com/) for structured job data when it is connected, with web search as the fallback
- Shows remote jobs open to Bangladesh, plus hybrid and on-site jobs based in Bangladesh
- Labels every job as Remote, Hybrid or On-site, with office days when stated
- Sorts jobs by posting date, newest first
- Removes jobs older than 14 days and jobs with no findable posting date
- Opens every link during the search, and removes dead, closed or uncheckable ones
- Creates a CV and cover letter only after you select a job
- Tailors from your own CV facts and reports missing requirements as gaps
- Never submits an application by itself

## How it works

```mermaid
flowchart LR
    A[Master CV and search profile] --> B[Search job sources]
    B --> C[Keep fresh, open, live, eligible jobs]
    C --> D[Job list, newest first]
    D --> E[You pick job numbers]
    E --> F[Tailored CV and cover letter]
    F --> G[You review and apply]
```

## Repository contents

| File | Purpose |
| --- | --- |
| `SKILL.md` | The instructions Claude follows |
| `README.md` | This guide |
| `.gitignore` | Keeps CVs, trackers and secrets out of Git |

## Requirements

- A Claude account with Skills and code execution available, or Claude Code
- A text-based PDF of your CV. Add the Word version too if you have it, for an exact design match.
- Optional: a free Apify account for better job data
- Optional: a Gmail connection for checking application responses

## Installation

### Claude web or desktop app

1. Turn on code execution in **Settings > Capabilities** if it is off.
2. Download `SKILL.md` from this repository.
3. Create a folder named exactly `job-skill-bd` and put `SKILL.md` inside it.
4. Compress that folder into `job-skill-bd.zip`. The ZIP must contain the folder, not a loose file.
5. In Claude, go to **Customize > Skills**, select **+**, then **+ Create skill**, then **Upload a skill**.
6. Upload the ZIP and make sure the Skill is turned on.

GitHub's **Download ZIP** adds the branch name to the folder name, so use the steps above instead of uploading that ZIP directly.

Team and Enterprise users may need an organization owner to allow Skills.

### Claude Code

Clone this repository into your personal skills folder, using the URL from GitHub's **Code** button.

macOS or Linux:

```bash
git clone REPOSITORY_URL ~/.claude/skills/job-skill-bd
```

Windows PowerShell:

```powershell
git clone REPOSITORY_URL "$HOME\.claude\skills\job-skill-bd"
```

## Connect Apify (optional and free)

This Skill uses only [Apify's Free plan](https://apify.com/pricing), which costs $0, needs no credit card, and includes $5 of usage each month. You do not need to pay for anything on Apify. If the credit runs out, Apify stops until the next monthly cycle and the Skill continues with web search.

1. Create a free [Apify account](https://apify.com/).
2. In Claude, look for Apify in the connector directory, or add a custom connector with this URL:

   ```text
   https://mcp.apify.com
   ```

3. Sign in and authorize the connection.

The Skill runs only two low-cost Actors, for Bdjobs and four remote job feeds. It limits a normal search to 150 results, about $0.15 of the free credit at November 2026 prices, and never upgrades your plan or buys credit.

## Usage

### 1. Set up

Start a chat, attach your master CV, and run:

```text
/job-skill-bd search
```

Claude will ask for anything it is missing:

- Target titles, seniority and core skills
- Bangladesh cities you accept for on-site or hybrid work
- Office days per week you accept for hybrid work
- Remote working-hour limits
- Company preferences and deal-breakers
- Expected salary in BDT per month, or USD for remote roles
- Notice period

Skills do not keep your data between chats, so keep your master CV, profile, template files and tracker in a Claude Project.

### 2. Review the job list

Results look like this:

| # | Posted | Job title | Company | Work type | Location and eligibility | Deadline | Fit | Source | Apply link |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 08 Nov 2026 (1 day ago) | Senior Backend Engineer | Example Ltd | Remote | Worldwide | 30 Nov 2026 | 86% | Himalayas | Open |

No CV is created at this stage.

### 3. Choose jobs

Reply with list numbers:

```text
/job-skill-bd apply 1, 4
```

You can also paste a job link:

```text
/job-skill-bd apply https://example.com/jobs/123
```

The first time, Claude shows a rebuilt copy of your master CV. Approve it before tailoring begins.

### 4. Review and apply

For each selected job you receive:

- A CV in your master template
- A cover letter in matching style
- The apply link
- A short list of changes
- Requirements you may not meet

Read both documents, then apply yourself.

### Commands

| Command | What it does |
| --- | --- |
| `/job-skill-bd help` | Shows the commands |
| `/job-skill-bd search` | Returns the job list only |
| `/job-skill-bd apply 1, 4` | Creates materials for selected jobs |
| `/job-skill-bd automate` | Schedules a recurring job list, where scheduling is available |
| `/job-skill-bd status` | Updates application outcomes |

Plain language works too, for example: `Find remote .NET jobs and hybrid or on-site roles in Dhaka`.

## Job sources

| Group | Examples |
| --- | --- |
| Bangladesh | Bdjobs, Chakri.com, Skill.jobs, Shomvob, Prothom Alo Jobs, Kormo Jobs, NRB Jobs, EverJobs, Job.com.bd, Careerjet Bangladesh |
| Remote | We Work Remotely, Remote OK, Himalayas, Remotive, Wellfound, Arc.dev, Jobicy, Working Nomads, Real Work From Anywhere |
| Company boards | Greenhouse, Lever, Ashby, Workable |
| Large boards | Public LinkedIn, Indeed and Glassdoor listings, through web search |

No search can cover every job. Each result list states which sources were searched and which failed.

## Good to know

- **Fewer results from some sites:** LinkedIn and Indeed often block automated page checks. Jobs that cannot be verified are removed, and the list reports how many.
- **Links can still close:** a job may close after the search. The list shows when links were checked.
- **PDF templates are rebuilt:** a PDF cannot be edited reliably. If a font is unavailable, Claude uses the closest match and tells you.
- **Scanned CVs do not work:** use a text-based PDF or Word file.
- **AI can make mistakes:** always review a CV before sending it. You are responsible for what you submit.

## Privacy and responsible use

- Never commit your CV, tracker, API tokens or generated applications. The included `.gitignore` helps, but check `git status` before each commit.
- The Skill never logs in to job sites or uses your account sessions, to avoid putting those accounts at risk.
- Scraping and automation are governed by each website's terms. Review them before enabling a source, and use the data for your own job search.
- Files you share with Claude and connected services are handled under those services' own terms.

## Customize

Edit `SKILL.md` to change:

- The 14-day limit under **Filters**
- The 50% minimum fit score under **Filters**
- Job sites and Actors under **Sources**
- Apify limits under **Using Apify on the Free plan**
- CV rules under **Master CV template**

## Disclaimer

This project is not affiliated with Anthropic, Apify, Bdjobs or any job site named here. Job listings and employer claims are not independently vetted, so watch for scams and never pay to apply.

