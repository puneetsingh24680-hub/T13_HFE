# Multi-agent Ethics Simulator

Team 13 (HFE) course project in **Agents & Game Theory**. Four classical decision-making models (expected utility,
Sen-inspired welfare, Simon's satisficing, prospect theory) face the same Moral Machine-style dilemmas. Each dilemma is
varied one feature at a time, and every decision and explanation is checked against the rule that produced it.

Everything runs locally and offline. Free OpenRouter language models can optionally play the same four agents within the
free quota.

## Quick start

```bash
./start.sh            # macOS / Linux: creates .venv on first run
```

```powershell
./start.ps1           # Windows PowerShell
```

Open <http://127.0.0.1:8000/decision> for the **Decision Lab** or <http://127.0.0.1:8000/> for the earlier **Before You
Decide** lab. Interactive API docs are at `/docs`. Set `PORT` to use another port.

| Command | Does |
|---|---|
| `make install` | Create `.venv` and install runtime and test dependencies |
| `make run` | Start the server (`start.sh`) |
| `make test` | Run the full test suite offline |
| `make evaluate` | Regenerate `results/decision_lab.md` and `.json` |

Requires Python 3.10 or newer. No GPU, model download, Node build or API key is needed.

## The two labs

| Lab | Question | Docs |
|---|---|---|
| **Decision Lab** (`/decision`) | When a dilemma is varied in a controlled way, how do four decision models differ, and do their decisions and explanations follow their rules? | [docs/decision-lab.md](docs/decision-lab.md) |
| **Before You Decide** (`/`) | Do ethical frameworks differ in what they need to know before deciding, under a question budget? | [docs/before-you-decide.md](docs/before-you-decide.md) |

## Results at a glance

From [results/decision_lab.md](results/decision_lab.md):

- 0 invariance violations across 99 dilemmas; every factor an agent reads changes at least one of its decisions.
- All templated explanations pass the five-item audit.
- The four agents agree on 58 of 99 dilemmas. Prospect theory shows an omission bias, satisficing is sensitive to lane
  order, and only the welfare agent follows the Moral Machine age and pedestrian preferences in both lane positions.
- LLM pilot: a free model playing all four agents on the 8 base dilemmas matches the rules 24 of 32 times, but almost none
  of its explanations pass the full audit (§5 of the results).

## Optional LLM agents

Put `OPENROUTER_API_KEY=...` in a git-ignored `.env` file. One request covers all four agents on a dilemma. Requests are
counted before sending and cached. Failing models cool down, and the 8 committed real recordings replay when there is no key or quota.
See [docs/llm-agents.md](docs/llm-agents.md).

## Documentation

| Document | Contents |
|---|---|
| [docs/decision-lab.md](docs/decision-lab.md) | Dilemma format, variants, the four agents, evaluation methods, API |
| [docs/llm-agents.md](docs/llm-agents.md) | OpenRouter setup, free-quota strategy, recordings, model-comparison details |
| [docs/architecture.md](docs/architecture.md) | Modules, design rules, storage, tests |
| [docs/before-you-decide.md](docs/before-you-decide.md) | The information-seeking lab |
| [docs/report.md](docs/report.md) | Final report draft |
| [docs/presentation.md](docs/presentation.md) | Six-slide outline and demo script |
| [docs/history/](docs/history/) | Original plan, proposal feedback, earlier presentation and implementation log |

## Repository layout

```text
ethics_lab/            FastAPI app, Before You Decide engine, OpenRouter adapter, SQLite store
ethics_lab/decision/   Decision Lab: dilemmas, agents, evaluation, human directions, API, LLM agents
static/                Frontend for both labs (vanilla JS, no build)
tests/                 Offline pytest suite, plus optional Playwright browser checks
tools/                 Evaluation, LLM recording and OpenRouter probe scripts
results/               Generated Decision Lab results
recordings/            Real LLM runs for offline replay
docs/                  Documentation
```

## Contributors

Five team members completed the project. Files are listed under the member responsible for them in the team's file
assignment.

| Member | GitHub | Contribution | Files |
|---|---|---|---|
| [Debanshu Ghosh](#debanshu-ghosh) | [debanshughosh009](https://github.com/debanshughosh009) | Decision agents, Before You Decide engine, project setup | 12 |
| [Girisha](#girisha) | [Girisha-coder](https://github.com/Girisha-coder) | Scenarios, human-preference benchmark, architecture and report docs | 13 |
| [Priyanshu](#priyanshu) | [PriyanshuMahlawat](https://github.com/PriyanshuMahlawat) | Evaluation, Decision Lab API, OpenRouter and LLM agents | 12 |
| [Puneet Singh](#puneet-singh) | [puneetsingh24680-hub](https://github.com/puneetsingh24680-hub) | Dilemmas, run journal, evaluation runner, LLM recordings, final integration | 12 |
| [Rohan Hasharth Amathaluri](#rohan-hasharth-amathaluri) | [Rohanharsh163](https://github.com/Rohanharsh163) | FastAPI app, frontend for both labs, UI tests, presentation | 13 |

### Debanshu Ghosh

- **Decision agents:** the four decision models (expected utility, Sen-inspired welfare, Simon's satisficing and prospect theory).
- **Traceable decisions:** each agent returns its choice, a numeric trace, the factors it read and an explanation built from that trace.
- **Before You Decide engine:** reference policies that plan which questions to ask using observations only, never the hidden world.
- **Model comparison storage:** independent model attempts are saved separately from the reference journal, with history and export.
- **Project setup and testing:** the Makefile, runtime dependencies, LLM agent guide, original plan, and tests for the agents and engine.

| File | Contribution |
|---|---|
| `ethics_lab/decision/agents.py` | The four deterministic decision-theory agents |
| `ethics_lab/decision/__init__.py` | Decision Lab package |
| `ethics_lab/engine.py` | Reference policies for the Before You Decide lab |
| `ethics_lab/comparisons.py` | Storage for independent model attempts |
| `ethics_lab/__init__.py` | Main package |
| `tests/test_decision_agents.py` | Tests for the decision agents |
| `tests/test_engine.py` | Tests for the reference engine |
| `docs/llm-agents.md` | Guide to the optional LLM agents |
| `docs/history/plan.md` | Original project plan |
| `Makefile` | Install, run, test and evaluate commands |
| `requirements.txt` | Runtime dependencies |
| `.gitignore` | Files kept out of version control |

### Girisha

- **Scenario catalog:** the 8 pilot cases for Before You Decide, along with their hidden facts and 3 ethical frameworks.
- **Human-preference benchmark:** 6 preference directions from the Moral Machine experiment, tested with the favoured group in each lane.
- **Project documentation:** the architecture guide, Decision Lab guide, final report draft, project overview and implementation record.
- **API and browser testing:** tests for the main API and an end-to-end browser check that saves QA screenshots.
- **Results and configuration:** the evaluation results data, the test configuration and the Windows start script.

| File | Contribution |
|---|---|
| `ethics_lab/scenarios.py` | Authored pilot cases for the Before You Decide lab |
| `ethics_lab/decision/human.py` | Human-preference benchmark from the Moral Machine experiment |
| `results/decision_lab.json` | Decision Lab evaluation results as data |
| `tests/test_api.py` | Tests for the main API |
| `tests/browser_smoke.py` | End-to-end browser check |
| `docs/architecture.md` | System architecture |
| `docs/decision-lab.md` | Decision Lab documentation |
| `docs/report.md` | Final report draft |
| `docs/history/progress.md` | Implementation record |
| `docs/history/proposal-feedback.md` | Proposal summary and teacher feedback |
| `README.md` | Project overview |
| `pytest.ini` | Test configuration |
| `start.ps1` | Windows start script |

### Priyanshu

- **Evaluation system:** decision consistency checks, the explanation audit, human-preference alignment and agreement between agents.
- **Decision Lab API:** HTTP routes for the catalog, individual dilemmas, the evaluation report and the language-model runs.
- **Language-model agents:** one batched request per dilemma, with daily and per-minute limits, caching and cooldowns for failing models.
- **OpenRouter integration:** free-model filtering, connection status, response parsing and a live probe that never calls paid models.
- **Recording and testing:** a tool that records real model runs for offline replay, the evaluation report, and tests for the API, evaluation and LLM agents.

| File | Contribution |
|---|---|
| `ethics_lab/decision/evaluation.py` | Decision consistency, explanation faithfulness and human-preference comparison |
| `ethics_lab/decision/api.py` | HTTP routes for the Decision Lab |
| `ethics_lab/decision/llm.py` | Optional language-model agents built for free OpenRouter limits |
| `ethics_lab/decision/llm_api.py` | HTTP routes for the language-model agents |
| `ethics_lab/openrouter.py` | Bounded OpenRouter adapters |
| `tools/check_openrouter.py` | Live OpenRouter probe |
| `tools/record_llm.py` | Records real model runs for offline replay |
| `tests/test_decision_api.py` | Tests for the Decision Lab API |
| `tests/test_decision_evaluation.py` | Tests for the evaluation |
| `tests/test_decision_llm.py` | Tests for the language-model agents |
| `results/decision_lab.md` | Decision Lab evaluation report |
| `docs/before-you-decide.md` | Before You Decide lab documentation |

### Puneet Singh

- **Dilemma set:** two-lane Moral Machine-style dilemmas, with 8 base cases expanded into 91 variants that each change one feature.
- **Run journal:** a local SQLite store with atomic edits and replayable events.
- **Evaluation runner:** a script that regenerates all Decision Lab results offline, plus 8 recorded model runs for replay without an API key.
- **Model comparison frontend:** the model selection, connection status, result cards and history, plus a read-only browser check of saved results.
- **Final integration:** restored `ethics_lab/app.py`, corrected the import in `ethics_lab/comparisons.py`, and uploaded the missing project files.

| File | Contribution |
|---|---|
| `ethics_lab/decision/dilemmas.py` | Two-lane dilemmas and one-feature controlled variants |
| `ethics_lab/store.py` | Local run journal with replayable events |
| `static/models.js` | Model comparison frontend |
| `tools/run_evaluation.py` | Regenerates the Decision Lab evaluation |
| `tools/view_model_result.py` | Read-only browser check of a saved model result |
| `tests/test_decision_dilemmas.py` | Tests for the dilemmas and variants |
| `tests/test_planning.py` | Analytical challenge cases for the planners |
| `recordings/decision_llm.json` | Recorded language-model runs |
| `recordings/README.md` | Guide to the recordings |
| `results/decision_llm.json` | Language-model pilot results |
| `.env.example` | Example environment configuration |
| `requirements-dev.txt` | Development dependencies |

### Rohan Hasharth Amathaluri

- **Application server:** the FastAPI app that serves both labs, with routes for runs, reveals, replay, benchmarks, comparisons and exports.
- **Before You Decide interface:** the main page, its frontend logic and the shared stylesheet.
- **Decision Lab interface:** the page, styles and logic for its dilemmas, evaluation and LLM tabs.
- **Interface testing:** tests for the Decision Lab page and model comparison, plus UI checks that use fixture model outputs.
- **Presentation and launch:** the final presentation outline and demo script, the earlier demonstration notes and the macOS/Linux start script.

| File | Contribution |
|---|---|
| `ethics_lab/app.py` | FastAPI application server |
| `static/index.html` | Before You Decide page |
| `static/app.js` | Before You Decide frontend logic |
| `static/styles.css` | Shared stylesheet |
| `static/decision.html` | Decision Lab page |
| `static/decision.js` | Decision Lab frontend logic |
| `static/decision.css` | Decision Lab styles |
| `tests/browser_models.py` | UI checks with fixture model outputs |
| `tests/test_decision_page.py` | Tests for the Decision Lab page and its assets |
| `tests/test_models.py` | Tests for the model comparison feature |
| `docs/presentation.md` | Final presentation outline and demo script |
| `docs/history/presentation-before-you-decide.md` | Earlier professor demonstration notes |
| `start.sh` | macOS and Linux start script |

## References

Main sources: Awad et al., "The Moral Machine experiment", *Nature* 2018; von Neumann and Morgenstern 1944; Sen,
"Rational Fools", 1977; Simon 1955; Kahneman and Tversky 1979 and 1992. The full list is in
[docs/report.md](docs/report.md#references).
