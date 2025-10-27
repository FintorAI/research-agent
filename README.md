# Research Agent

Deep Research Agent for comprehensive information gathering and report generation

## Overview

This agent is specialized for conducting thorough research and generating comprehensive reports. It uses web search capabilities to gather information, synthesizes findings, and produces well-structured, detailed reports.

## Features

- **Deep Research**: Conducts thorough multi-step research with parallel information gathering
- **Report Generation**: Creates comprehensive, well-formatted markdown reports
- **Critique System**: Built-in critique subagent for quality assurance
- **Parallel Research**: Spawns multiple research subagents for efficient parallel research
- **Source Citations**: Automatically tracks and cites sources
- **No Default Message**: Requires explicit research questions (allows maximum flexibility)

## Agent Capabilities

### Research Workflow

1. **Question Logging**: Saves the research question for reference
2. **Information Gathering**: Uses research subagents to conduct deep dives
3. **Report Writing**: Synthesizes findings into comprehensive reports
4. **Critique and Refinement**: Uses critique subagent for quality review
5. **Iterative Improvement**: Refines report based on feedback

### Built-in Tools
- `internet_search`: Web search with customizable parameters
- `write_todos`: Plan and track research tasks
- `ls`: List files and directories
- `read_file`: Read source documents and drafts
- `write_file`: Create reports and notes
- `edit_file`: Refine and update reports

### Subagents
- **research-agent**: Dedicated researcher for in-depth topic exploration
- **critique-agent**: Editor for report critique and improvement suggestions

## Usage

### Basic Usage

```python
import os
from typing import Literal
from tavily import TavilyClient
from copilotagent import create_deep_agent

tavily_client = TavilyClient(api_key=os.environ["TAVILY_API_KEY"])

# Web search tool
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

# Create the research agent
agent = create_deep_agent(
    agent_type="research",
    tools=[internet_search],
    system_prompt="Custom research instructions...",
    subagents=[research_subagent, critique_subagent]  # Optional
)

# Research agents require explicit questions (no default message)
result = agent.invoke({
    "messages": [{"role": "user", "content": "Research climate change impacts on agriculture"}]
})
```

### Running the Example

```bash
cd agent/research

# Set your Tavily API key
export TAVILY_API_KEY=your_key_here

# Run the agent (note: this is a standalone script)
python research_agent.py
```

## Configuration

The agent is configured with:
- **Agent Type**: `research`
- **Default Model**: Claude Sonnet 4.5
- **Planning**: Research-specific planning prompts
- **Default Message**: None (requires explicit input)
- **Required Tool**: `internet_search` (you must provide this)

## Report Structure

The research agent generates reports with:

- **Markdown Format**: Proper headings and structure
- **Comprehensive Content**: Detailed analysis with multiple sections
- **Source Citations**: All sources numbered and listed
- **Professional Style**: Academic/business report quality
- **Multilingual Support**: Reports in the same language as the question

## Workflow Example

When you invoke the research agent, it will:

1. **Save the research question** to `question.txt`
2. **Break down the research** into subtopics
3. **Launch research subagents** in parallel for each subtopic
4. **Synthesize findings** from all research tasks
5. **Write comprehensive report** to `final_report.md`
6. **Request critique** from critique subagent
7. **Refine the report** based on feedback
8. **Present final report** to user

## Research Strategies

The agent can handle different research types:

### Comparison Research
```
User: "Compare electric cars vs hydrogen fuel cell vehicles"
```
Structure: Intro → Overview A → Overview B → Comparison → Conclusion

### List Generation
```
User: "List the top 10 AI companies in 2024"
```
Structure: Table or numbered list with details for each item

### Topic Overview
```
User: "Explain quantum computing"
```
Structure: Overview → Key Concepts → Applications → Conclusion

### Deep Dive
```
User: "Research the economic impact of remote work"
```
Structure: Multiple sections covering different aspects with detailed analysis

## Requirements

- Python 3.11+
- copilotagent package (from PyPI)
- tavily-python (for web search)
- Tavily API key (get one at https://www.tavily.com/)

See `requirements.txt` for full list of dependencies.

## Environment Variables

```bash
export TAVILY_API_KEY=your_tavily_api_key
export ANTHROPIC_API_KEY=your_anthropic_api_key  # For Claude
```

## Integration

This agent can be integrated into:
- Research automation systems
- Content generation pipelines
- Market research platforms
- Competitive intelligence tools
- Academic research assistance
- News monitoring systems

## Customization

You can customize the research agent by:

### Adding Custom Search Tools
```python
def custom_search(query: str) -> dict:
    # Your custom search implementation
    pass

agent = create_deep_agent(
    agent_type="research",
    tools=[custom_search],
)
```

### Custom Subagents
```python
custom_research_subagent = {
    "name": "specialist-researcher",
    "description": "Expert in technical research",
    "system_prompt": "You are a technical research specialist...",
    "tools": [internet_search],
}

agent = create_deep_agent(
    agent_type="research",
    subagents=[custom_research_subagent],
)
```

### Custom Report Instructions
```python
custom_instructions = """
You are a research agent specialized in financial research.
Always include:
- Market data with specific numbers
- Competitor analysis
- Financial metrics and KPIs
"""

agent = create_deep_agent(
    agent_type="research",
    system_prompt=custom_instructions,
)
```

## Tips for Best Results

1. **Be Specific**: Provide clear, specific research questions
2. **Break Down Complex Topics**: The agent works best with focused questions
3. **Specify Language**: Ask in the language you want the report in
4. **Provide Context**: Include relevant context or constraints in your question
5. **Use Parallel Research**: For multi-faceted topics, let the agent spawn multiple subagents
6. **Iterate**: Use the critique feature to refine reports

## Support

For questions or issues with the Research agent, refer to the main CopilotAgent documentation.

See the full implementation in `research_agent.py` for detailed examples of research instructions and subagent configuration.

