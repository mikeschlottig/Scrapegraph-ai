# CLAUDE.md - AI Assistant Guide for ScrapeGraphAI

## Project Overview

ScrapeGraphAI is a Python web scraping library that uses LLMs (Large Language Models) and direct graph logic to create intelligent scraping pipelines. It can extract structured information from websites and local documents (XML, HTML, JSON, Markdown, etc.) using natural language prompts.

**Version**: 1.64.0
**License**: MIT
**Python**: >=3.10, <4.0

## Tech Stack

- **Core**: Python 3.10+, LangChain ecosystem
- **LLM Providers**: OpenAI, Ollama, Mistral, AWS Bedrock, Groq, Azure, Gemini
- **Web Scraping**: Playwright, BeautifulSoup4, html2text
- **Search**: DuckDuckGo Search
- **Data Validation**: Pydantic, jsonschema
- **Package Management**: uv (Astral)
- **Build System**: Hatchling
- **Documentation**: Sphinx, ReadTheDocs
- **Code Quality**: Black, Ruff, isort, pylint, mypy

## Project Structure

```
scrapegraphai/           # Main package
  graphs/                # Graph implementations (scraping pipelines)
  nodes/                 # Individual processing nodes
  builders/              # Builder utilities
  docloaders/            # Document loaders
  helpers/               # Helper functions
  integrations/          # Third-party integrations
  models/                # Model configurations
  prompts/               # LLM prompts
  telemetry/             # Usage tracking
  utils/                 # Utilities and logging

examples/                # Usage examples by graph type
  smart_scraper_graph/   # Single page scraping
  search_graph/          # Multi-page search scraping
  script_generator_graph/# Generate scraping scripts
  csv_scraper_graph/     # CSV file scraping
  json_scraper_graph/    # JSON file scraping
  ...

tests/                   # Test suite
  graphs/                # Graph tests
  nodes/                 # Node tests
  utils/                 # Utility tests

docs/                    # Documentation
  source/                # Sphinx source
```

## Key Files

| File | Purpose |
|------|---------|
| `pyproject.toml` | Project configuration, dependencies, tools |
| `scrapegraphai/__init__.py` | Package entry point |
| `scrapegraphai/graphs/smart_scraper_graph.py` | Main single-page scraper |
| `scrapegraphai/graphs/search_graph.py` | Multi-page search scraper |
| `scrapegraphai/graphs/base_graph.py` | Base graph implementation |
| `scrapegraphai/graphs/abstract_graph.py` | Abstract graph class |
| `scrapegraphai/nodes/base_node.py` | Base node implementation |
| `scrapegraphai/nodes/fetch_node.py` | Web page fetching |
| `scrapegraphai/nodes/generate_answer_node.py` | LLM answer generation |
| `Makefile` | Development automation |
| `uv.lock` | Locked dependencies |

## Development Setup

### Prerequisites
- Python 3.10+
- uv package manager

### Quick Start

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Clone and setup
cd Scrapegraph-ai
uv sync

# Install pre-commit hooks
uv run pre-commit install

# Install Playwright browsers (required for web scraping)
playwright install
```

### Docker Setup

```bash
docker build -t scrapegraphai -f Dockerfile .
# or
docker-compose -f docker-compose.yml up
```

## Common Commands

```bash
# Install dependencies
make install
# or: uv sync

# Run linting checks
make lint
# or: uv run ruff check scrapegraphai tests
#     uv run black --check scrapegraphai tests
#     uv run isort --check-only scrapegraphai tests

# Run type checking
make type-check
# or: uv run mypy scrapegraphai tests

# Run tests with coverage
make test
# or: uv run pytest --cov=scrapegraphai --cov-report=xml tests/

# Run pre-commit hooks
make pre-commit
# or: uv run pre-commit run --all-files

# Build package
make build
# or: uv build --no-sources

# Clean build artifacts
make clean
```

## Testing

- **Framework**: pytest with pytest-asyncio
- **Coverage**: pytest-cov
- **Mock**: pytest-mock

### Run Tests
```bash
uv run pytest                              # All tests
uv run pytest tests/graphs/                # Graph tests only
uv run pytest tests/nodes/                 # Node tests only
uv run pytest -x                           # Stop on first failure
uv run pytest --cov=scrapegraphai          # With coverage
```

### Test Structure
- `tests/graphs/` - Graph integration tests
- `tests/nodes/` - Node unit tests
- `tests/utils/` - Utility tests
- `tests/inputs/` - Test input files

## API Structure

### Main Graph Classes
- `SmartScraperGraph` - Single page scraping
- `SmartScraperMultiGraph` - Multi-page scraping
- `SearchGraph` - Search engine results scraping
- `ScriptCreatorGraph` - Generate Python scraping scripts
- `CSVScraperGraph` - CSV file scraping
- `JSONScraperGraph` - JSON file scraping
- `XMLScraperGraph` - XML file scraping
- `DocumentScraperGraph` - Document scraping
- `SpeechGraph` - Extract info + generate audio

### Configuration Pattern
```python
graph_config = {
    "llm": {
        "model": "ollama/llama3.2",  # or "openai/gpt-4o-mini"
        "api_key": "YOUR_KEY",        # for API-based models
        "model_tokens": 8192
    },
    "verbose": True,
    "headless": False,  # Show browser
}

graph = SmartScraperGraph(
    prompt="Extract information about...",
    source="https://example.com",
    config=graph_config
)
result = graph.run()
```

## Code Conventions

### Style
- **Formatter**: Black (line-length: 88)
- **Linter**: Ruff
- **Import sorting**: isort (black profile)
- **Type checking**: mypy (strict mode)

### Graph Pattern
Graphs inherit from `AbstractGraph` and define a workflow of nodes:
```python
from scrapegraphai.graphs import AbstractGraph
from scrapegraphai.nodes import FetchNode, GenerateAnswerNode

class MyGraph(AbstractGraph):
    def _create_graph(self):
        # Define nodes and connections
        pass
```

### Node Pattern
Nodes inherit from `BaseNode`:
```python
from scrapegraphai.nodes import BaseNode

class MyNode(BaseNode):
    def execute(self, state):
        # Process and return updated state
        return state
```

### Commit Messages
Use semantic prefixes:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `style:` - Code style
- `refactor:` - Code changes
- `test:` - Testing
- `perf:` - Performance

## When Making Changes

1. **Follow existing patterns** - Check similar files in `graphs/` or `nodes/`
2. **Add tests** - Create tests in `tests/` directory
3. **Run linting** - Use `make lint` before committing
4. **Type hints** - Add type annotations (mypy strict mode enabled)
5. **Update examples** - Add usage examples in `examples/` if adding new graphs
6. **Pre-commit hooks** - Run `uv run pre-commit run --all-files`

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `OPENAI_API_KEY` | OpenAI API key |
| `ANTHROPIC_API_KEY` | Anthropic API key |
| `GROQ_API_KEY` | Groq API key |
| `AZURE_OPENAI_API_KEY` | Azure OpenAI key |
| `SCRAPEGRAPHAI_TELEMETRY_ENABLED` | Set to `false` to disable telemetry |

## CI/CD

- **Code Quality**: GitHub Actions (`.github/workflows/code-quality.yml`)
  - Ruff, Black, isort, pylint checks
- **Security**: CodeQL scanning (`.github/workflows/codeql.yml`)
- **Dependencies**: Dependency review (`.github/workflows/dependency-review.yml`)
- **Release**: Semantic release (`.github/workflows/release.yml`)

## External Resources

- **Documentation**: https://scrapegraph-ai.readthedocs.io/
- **PyPI**: https://pypi.org/project/scrapegraphai/
- **Discord**: https://discord.gg/gkxQDAjfeX
- **Commercial API**: https://scrapegraphai.com

---

## Recent Changes (v1.62.0 to v1.64.0)

### v1.64.0 (2025-11-06)
- **New Feature**: Configurable timeout for FetchNode
  - Default timeout of 30 seconds for HTTP requests and PDF parsing
  - Timeout propagated to ChromiumLoader via `loader_kwargs`
  - New test file: `tests/test_fetch_node_timeout.py` with comprehensive timeout tests

### v1.63.1 (2025-10-24)
- **Bug Fix**: URL redirect handling

### v1.63.0 (2025-10-22)
- **Feature**: Updated model tokens configuration

---

## Issues & Recommendations

### Critical Issues

1. **Typo in pyproject.toml** (line 121)
   - `pylint-local = "pylint scraperaphai/**/*.py"` should be `scrapegraphai`
   - This causes the pylint-local task to fail

2. **Missing CI task**
   - `.github/workflows/code-quality.yml` references `pylint-score-ci` task (line 35)
   - This task is not defined in `pyproject.toml` - only `pylint-ci` exists
   - CI will fail on the "Check Pylint score" step

3. **Empty test files**
   - `/tests/test_json_scraper_multi_graph.py` - 0 lines
   - `/tests/test_smart_scraper_multi_concat_graph.py` - 0 lines
   - These graphs have no test coverage
   - Note: Testing improved with addition of `tests/test_fetch_node_timeout.py` (v1.64.0)

### Configuration Issues

4. **No test execution in CI**
   - The `code-quality.yml` workflow runs linting but not tests
   - Consider adding a test job to ensure code functionality

5. **requirements.txt misleading**
   - Contains only Sphinx documentation dependencies
   - Main dependencies are in `pyproject.toml`
   - Should be renamed to `requirements-docs.txt` or documented

6. **Contributing guidelines reference non-existent branch**
   - `CONTRIBUTING.md` references `pre/beta` branch
   - Only `main` branch exists in the repository

### Code Quality Issues

7. **Missing version export**
   - `scrapegraphai/__init__.py` doesn't export `__version__`
   - Makes it harder to check installed version programmatically

8. **Dockerfile installs from PyPI**
   - Dockerfile installs package from PyPI, not local source
   - Makes it unsuitable for development/testing

### Documentation Issues

9. **Minimal SECURITY.md**
   - Only contains email contact
   - Should include supported versions, disclosure policy, and security best practices

10. **Missing API documentation**
    - No inline docstrings visible in key files
    - Would improve IDE support and documentation generation

### Recommendations

1. **Fix typo**: Change `scraperaphai` to `scrapegraphai` in pyproject.toml
2. **Add missing task**: Define `pylint-score-ci` in pyproject.toml or update CI workflow
3. **Add tests**: Implement tests for empty test files
4. **Add CI test job**: Include pytest in code-quality workflow
5. **Clarify requirements**: Rename or document requirements.txt purpose
6. **Update CONTRIBUTING.md**: Fix branch name references
7. **Add version export**: Export `__version__` in package init
8. **Improve Dockerfile**: Mount local source for development
9. **Enhance SECURITY.md**: Add comprehensive security policy
10. **Add docstrings**: Document public APIs with Google-style docstrings
