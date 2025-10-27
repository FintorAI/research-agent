# 🔬 Research Agent

**Deep Research Agent for Comprehensive Information Gathering and Report Generation**

[![LangGraph Cloud](https://img.shields.io/badge/LangGraph-Cloud-blue)](https://smith.langchain.com/)
[![GitHub](https://img.shields.io/badge/GitHub-Research-green)](https://github.com/FintorAI/research-agent)

---

## 📋 Overview

This agent conducts thorough research and generates comprehensive, well-structured reports. It uses web search, spawns parallel research subagents, and includes a built-in critique system for quality assurance.

---

## ✨ Features

- ✅ **Deep Research** - Multi-step research with parallel information gathering
- ✅ **Report Generation** - Comprehensive markdown reports with citations
- ✅ **Critique System** - Built-in critique subagent for quality review
- ✅ **Parallel Research** - Spawns multiple subagents for efficient research
- ✅ **Source Citations** - Automatic source tracking and citation
- ✅ **Custom Planning** - Local `planner_prompt.md` for research strategy
- ✅ **Multilingual** - Reports in the same language as the question

---

## 🚀 Quick Start

### Local Development

```bash
cd agents/research

# Create .env file with API keys
cat > .env << 'EOF'
ANTHROPIC_API_KEY=sk-ant-api03-...
TAVILY_API_KEY=tvly-...
EOF

# Start dev server
langgraph dev

# Open http://localhost:2024
```

### GitHub Repository

**Repo**: https://github.com/FintorAI/research-agent

**Deploy Command:**
```bash
git add .
git commit -m "Update agent"
git push origin main
# → LangGraph Cloud auto-deploys!
```

---

## 🔐 Environment Variables

### Required Variables

**1. ANTHROPIC_API_KEY** ✅ Required
- **Purpose**: Claude Sonnet 4.5 model (main agent LLM)
- **Get from**: https://console.anthropic.com/settings/keys
- **Format**: `sk-ant-api03-...`

**2. TAVILY_API_KEY** ✅ Required
- **Purpose**: Web search via Tavily API
- **Get from**: https://www.tavily.com/ (sign up for free)
- **Format**: `tvly-...`

### Setting Variables

**For Local Development:**
```bash
# Create .env in agent directory
ANTHROPIC_API_KEY=sk-ant-api03-your-key
TAVILY_API_KEY=tvly-your-key
```

**For LangGraph Cloud:**
1. Go to: https://smith.langchain.com/deployments
2. Select deployment → Environment Variables
3. Add both keys above

---

## 🔄 Research Workflow

### Phase 1: Question Logging
- Save research question to `question.txt`
- Preserve original query for reference

### Phase 2: Information Gathering
- Break down research into subtopics
- Spawn parallel research subagents
- Use `internet_search` for web queries
- Gather comprehensive information

### Phase 3: Report Writing
- Synthesize findings from all sources
- Write comprehensive report to `final_report.md`
- Include proper markdown structure
- Add source citations

### Phase 4: Critique and Refinement
- Use critique subagent for quality review
- Identify areas for improvement
- Check completeness and accuracy

### Phase 5: Final Revisions
- Refine report based on feedback
- Ensure professional quality
- Verify all citations
- Present final report to user

---

## 🛠️ Available Tools

### Research Tool

**internet_search**
- Web search with Tavily API
- Parameters: query, max_results, topic, include_raw_content
- Topics: "general", "news", "finance"

### Subagents (via task tool)

**research-agent**
- Dedicated researcher for in-depth topics
- Conducts thorough research autonomously
- Returns synthesized findings

**critique-agent**
- Editor for report quality review
- Provides improvement suggestions
- Checks comprehensiveness and structure

### Built-in Tools

- `write_todos` - Plan research phases
- `ls` - List files
- `read_file` - Read drafts and notes
- `write_file` - Create reports
- `edit_file` - Refine reports

---

## 📂 File Structure

```
research/
├── research_agent.py      # Agent implementation
├── planner_prompt.md      # Custom research strategy (editable!)
├── requirements.txt       # Dependencies (copilotagent>=0.1.8)
├── langgraph.json         # LangGraph Cloud config
├── .gitignore             # Python + LangGraph ignores
└── README_CONSOLIDATED.md # This file
```

---

## 🎨 Customization

### Edit Research Strategy

```bash
# Edit the research planning prompt
nano planner_prompt.md

# Commit and push
git add planner_prompt.md
git commit -m "Refine research phases"
git push origin main

# → LangGraph Cloud auto-deploys! ✅
```

### Add Custom Search Tools

```python
# In research_agent.py
from langchain_core.tools import tool

@tool
def academic_search(query: str) -> dict:
    """Search academic databases."""
    # Your search logic
    return results

agent = create_deep_agent(
    agent_type="research",
    tools=[internet_search, academic_search],  # Add custom tool
    planning_prompt=planning_prompt,
)
```

### Custom Subagents

```python
# Add specialized researcher
technical_researcher = {
    "name": "technical-researcher",
    "description": "Expert in technical and scientific research",
    "system_prompt": "You are a technical research specialist...",
    "tools": [internet_search],
}

agent = create_deep_agent(
    agent_type="research",
    subagents=[research_sub_agent, critique_sub_agent, technical_researcher],
)
```

---

## 📝 Report Structure

The agent generates professional reports with:

- **Markdown Format** - Proper headings and structure
- **Comprehensive Content** - Detailed analysis with multiple sections
- **Source Citations** - Numbered citations with URLs
- **Professional Style** - Academic/business quality
- **Multilingual Support** - Reports in question's language

### Example Report Structure

**For Comparison Research:**
1. Introduction
2. Overview of Topic A
3. Overview of Topic B
4. Comparison Analysis
5. Conclusion
6. Sources

**For List Generation:**
1. Table or numbered list with details

**For Topic Overview:**
1. Overview
2. Key Concepts
3. Applications
4. Conclusion
5. Sources

---

## 🌐 LangGraph Cloud Deployment

### Initial Setup

1. **Deploy to GitHub** (already done):
   - Repo: https://github.com/FintorAI/research-agent

2. **Connect to LangGraph Cloud**:
   - Go to: https://smith.langchain.com/deployments
   - Click "+ New Deployment"
   - Select: GitHub → FintorAI/research-agent
   - Branch: main

3. **Configure Environment Variables**:
   - `ANTHROPIC_API_KEY`
   - `TAVILY_API_KEY`

4. **Deploy**:
   - Click "Submit"
   - Wait ~5 minutes
   - Test in playground!

---

## 📊 Dependencies

```txt
copilotagent>=0.1.8      # Core framework (from PyPI)
langchain>=1.0.0         # LangChain framework
langchain-anthropic>=1.0.0  # Claude model
langchain-core>=1.0.0    # LangChain core
langgraph-cli[inmem]     # LangGraph CLI
tavily-python            # Web search API
```

---

## 💡 Usage Example

```python
import os
from typing import Literal
from pathlib import Path
from tavily import TavilyClient
from copilotagent import create_deep_agent

# Initialize Tavily client
tavily_client = TavilyClient(api_key=os.environ["TAVILY_API_KEY"])

# Define search tool
def internet_search(
    query: str,
    max_results: int = 5,
    topic: Literal["general", "news", "finance"] = "general",
    include_raw_content: bool = False,
):
    """Run a web search"""
    return tavily_client.search(
        query,
        max_results=max_results,
        include_raw_content=include_raw_content,
        topic=topic,
    )

# Load custom planning prompt
planning_prompt = Path("planner_prompt.md").read_text()

# Create research agent
agent = create_deep_agent(
    agent_type="research",
    tools=[internet_search],
    planning_prompt=planning_prompt,  # Custom research strategy
    subagents=[research_sub_agent, critique_sub_agent],
)

# Research something
result = agent.invoke({
    "messages": [{"role": "user", "content": "Research climate change impacts on agriculture"}]
})
```

---

## 💡 Tips for Best Results

1. **Be Specific** - Provide clear, specific research questions
2. **Break Down Topics** - Agent works best with focused questions
3. **Specify Language** - Ask in the language you want the report in
4. **Provide Context** - Include relevant constraints or focus areas
5. **Use Parallel Research** - Let agent spawn multiple subagents for complex topics
6. **Iterate** - Use critique feature to refine reports

---

## 🐛 Troubleshooting

### Error: "TAVILY_API_KEY not found"
**Fix**: Add `TAVILY_API_KEY` to `.env` file or LangGraph Cloud dashboard

### Report not in correct language
**Fix**: The agent responds in the same language as the question - verify your question language

### Citations missing
**Fix**: Check that search results include URLs - critique agent will flag this

---

## 📚 Related Documentation

- **Base Package**: https://github.com/FintorAI/copilotBase
- **ITP-Princeton Agent**: https://github.com/FintorAI/itp-princeton-agent
- **DrawDoc-AWM Agent**: https://github.com/FintorAI/drawdoc-awm-agent
- **Tavily API**: https://www.tavily.com/

---

## ✅ Current Status

- ✅ Deployed to GitHub: https://github.com/FintorAI/research-agent
- ✅ Using copilotagent v0.1.8 from PyPI
- ✅ Custom planning prompt: `planner_prompt.md`
- ✅ Web search integrated
- ✅ Auto-deploy on push enabled

---

**Ready for LangGraph Cloud deployment!** 🚀

