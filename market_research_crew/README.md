# Market Research Crew

A sequential multi-agent system built with [CrewAI](https://crewai.com). Give it one AI product idea and it researches the market, competitors, and customers, defines a product strategy, and writes an investment-memo style report to `reports/report.md`.

## How It Works

Five agents run one after another. Each task receives the outputs of all earlier tasks as context, so later agents build on earlier findings.

```
main.py (product_idea)
        |
        v
1. Market Research Specialist        -> market size, trends, regulation, tech readiness
        v
2. Competitive Intelligence Analyst  -> competitors, pricing, gaps, threats
        v
3. Customer Insights Researcher      -> segments, personas, pain points, channels
        v
4. Product Strategy Advisor          -> MVP features, differentiation, roadmap, KPIs
        v
5. Business Analyst                  -> pricing, financials, GTM, risks, Go/No-Go
        |
        v
reports/report.md
```

| # | Agent | Task output | Target length |
|---|-------|-------------|---------------|
| 1 | Market Research Specialist | Market research report | 1500-2000 words |
| 2 | Competitive Intelligence Analyst | Competitor profiles and comparison matrix | 2000-2500 words |
| 3 | Customer Insights Researcher | Personas, pain points, journey maps | 2000-2500 words |
| 4 | Product Strategy Advisor | Strategy, feature matrix, 12-month roadmap | 2500-3000 words |
| 5 | Business Analyst | Final investment memo (saved to file) | 3000-4000 words |

Only the final task writes a file. Intermediate outputs are passed in memory.

## Tools

All agents share the same toolkit:

- `SerperDevTool` - web search (needs `SERPER_API_KEY`)
- `ScrapeWebsiteTool` - static page scraping
- `SeleniumScrapingTool` - scraping of JavaScript-rendered pages (needs a local Chrome browser)

## Project Structure

```
market_research_crew/
├── knowledge/
│   └── user_preference.txt        # placeholder, not used by the crew
├── reports/
│   └── report.md                  # final output
├── src/market_research_crew/
│   ├── config/
│   │   ├── agents.yaml            # role, goal, backstory per agent
│   │   └── tasks.yaml             # task descriptions, expected output, agent
│   ├── tools/
│   │   └── custom_tool.py         # template stub, not used
│   ├── crew.py                    # wires agents, tasks, tools, process
│   └── main.py                    # entry point, holds product_idea
├── pyproject.toml
└── uv.lock
```

## Requirements

- Python >= 3.10 and < 3.14
- [uv](https://docs.astral.sh/uv/)
- OpenAI API key
- Serper API key
- Google Chrome (for Selenium)

## Setup

```bash
# 1. Install uv and the CrewAI CLI
pip install uv
uv tool install crewai

# 2. Install project dependencies (run inside market_research_crew/)
crewai install
```

Create a `.env` file in the `market_research_crew/` folder:

```bash
OPENAI_API_KEY=your_openai_api_key
SERPER_API_KEY=your_serper_api_key
```

Optional: set the model explicitly, for example `MODEL=gpt-4o-mini`. The code does not set an LLM itself, so CrewAI falls back to its environment default.

## Run

1. Open `src/market_research_crew/main.py` and edit the idea:

```python
inputs = {
    "product_idea": "Your AI product idea here"
}
```

2. Start the crew from the `market_research_crew/` folder:

```bash
crewai run
```

3. Read the result in `reports/report.md`. Each run overwrites the previous report, so rename or copy it first if you want to keep it.

## Output

The report is a Markdown investment memo containing:

- Executive summary and Go / No-Go / Conditional recommendation
- Pricing tiers and rationale
- Revenue model, unit economics, 3-year ARR projection, break-even
- Go-to-market plan
- Resource requirements and funding ask
- Risk matrix
- Investment thesis and next steps

## Customizing

| To change | Edit |
|-----------|------|
| Product idea | `main.py` |
| Agent role, goal, backstory | `config/agents.yaml` |
| Task instructions, output format, length | `config/tasks.yaml` |
| Tools per agent, task order, context, process | `crew.py` |
| Output file path | `output_file` in `business_analyst_task` in `crew.py` |

## Known Limitations

- The product idea is hardcoded; there is no CLI input yet.
- Figures and citations come from LLM output over web search. Verify any number before relying on it. Projections (ARR, CAC, churn) are assumptions, not researched data.
- The pipeline is linear: no retries, no review loop, and one failed task stops the run.
- All agents share every tool, and the final report may be long enough to raise token cost.
- `knowledge/user_preference.txt` and `tools/custom_tool.py` are template leftovers and have no effect.
- Selenium depends on a working local Chrome setup.

## Troubleshooting

| Problem | Likely cause and fix |
|---------|----------------------|
| Authentication or API key error | Check `.env` exists in the project folder and keys are valid |
| Search returns nothing | `SERPER_API_KEY` missing or out of credits |
| Selenium errors | Install or update Chrome; confirm `webdriver-manager` can download a driver |
| `crewai: command not found` | Run `uv tool install crewai` and restart the terminal |
| Run stops mid-way | Check API rate limits; rerun, as there is no resume |

## Author

Mustafa Hamid Talukdar
