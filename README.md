# 📊 Market Research Multi-Agent Crew (CrewAI)

An autonomous multi-agent system built using **CrewAI** that takes an AI product idea, performs market, competitor, and customer research, and synthesizes a comprehensive investment-grade business report.

---

## 🤖 How It Works

The system orchestrates **5 specialized AI agents** working in a sequential pipeline:

[Product Idea]

↓

Market Research Specialist       → Evaluates market size, growth trends & industry gaps
↓

Competitive Intelligence Analyst → Analyzes direct/indirect competitors & pricing
↓

Customer Insights Researcher     → Defines customer personas, pain points & acquisition
↓

Product Strategy Advisor        → Outlines MVP features, roadmap & differentiators
↓

Business Analyst                → Synthesizes all reports into a master business document

---

## 🛠 Tech Stack

- **Framework:** CrewAI
- **LLM:** OpenAI (`gpt-4o-mini`)
- **Tools:** `SerperDevTool` (Web Search), `ScrapeWebsiteTool`, `SeleniumScraperTool`
- **Language & Package Manager:** Python 3.12, `uv`

---

## 🚀 Getting Started

### 1. Prerequisites & Installation
Ensure you have Python 3.10+ and `uv` installed:

```bash
# Install CrewAI CLI
uv tool install crewai

# Install dependencies inside project
crewai install
```

###2. Configure Environment Variables
Create a .env file in the project root:
```bash
OPENAI_API_KEY=your_openai_api_key_here
SERPER_API_KEY=your_serper_api_key_here
```

###3. Run the Crew
```bash
crewai run
```

📄 Output
The final synthesized report is saved as a Markdown file in reports/report.md, covering:

Executive Summary & Market Analysis

Competitor Strengths & Weaknesses Matrix

Customer Personas & Pricing Tiers

3-Year ARR Projections & Path to Profitability

👨‍💻 Author
Mustafa Hamid Talukdar

LinkedIn | X
