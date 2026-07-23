✈️ Intelligent Trip Planner: Multi-Agent Orchestration Workflow

An extensible, production-grade Multi-Agent System built in n8n (low-code/no-code workflow automation) designed to coordinate complex, multi-domain travel itineraries. By splitting responsibility across specialized AI agents, the platform automates flight searches, hotel bookings, activity scheduling, cross-domain constraint validation, and final curation into a seamless trip plan.

📌 Architecture Overview
Traditional trip planning tools suffer from rigid workflows, single-agent reasoning bottlenecks, or fragmented multi-tab browsing. This project addresses these challenges by using a Multi-Agent Orchestration Pattern (v4 Architecture).

                  [ User Input / Chatbot / Telegram ]
                                   │
                                   ▼
                   [ Trip Request Coordinator ]
                       (Validation & Schema)
                                   │
                                   ▼
                       [ Orchestrator / Planner ]
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         ▼                         ▼                         ▼
  [ Flight Agent ]          [ Hotel Agent ]         [ Activity Agent ]
    (SerpAPI / Tool)          (SerpAPI / Tool)       (Tavily / Tools)
         └─────────────────────────┬─────────────────────────┘
                                   │
                                   ▼
                       [ Validator & Summarizer ]
                       (Cross-Domain Consistency)
                                   │
                                   ▼
                     [ Final Formatted Itinerary ]
🤖 Agent Roles & Responsibilities
Agent Name	Role & Objective	Key Input / Output
Trip Request Coordinator	Extracts structured entities (dates, origin, budget) from natural language; returns validated JSON or error messages.	
In: User query string


Out: Structured Trip Request JSON

Orchestrator / Planner	Manages workflow execution and distributes tasks in parallel across domain-specific agents.	
In: Structured Request JSON


Out: Combined Raw Options Data

Flight Agent	Searches real-time flight data, evaluating non-stop vs. layover options, pricing, and scheduling.	
In: Origin, Destination, Dates


Out: 4–6 Round-trip Flight Options

Hotel Agent	Finds accommodations matching budget, duration, star ratings, and preferred locations.	
In: Destination, Check-in/out, Budget


Out: 5–7 Hotel Options

Activity Agent	Searches local attractions, experiences, and seasonal points of interest using search engines.	
In: Destination, Duration, Preferences


Out: 8–12 Categorized Activities

Validator & Summarizer	Evaluates constraints (flight arrival vs. hotel check-in), filters options down to the top 2–3, and curates the final output.	
In: Raw Merged Options


Out: Formatted Final Itinerary

🛠️ Key Features & Technical Tasks
1. Parallel Execution
Domain agents (Flight, Hotel, Activity) run concurrently within n8n sub-workflows to minimize latency and optimize execution time.

2. Cross-Agent Validation & Constraint Reconciliation
Timing Consistency: Verifies flight departure/arrival alignment with hotel check-in/out dates.

Feasibility Checks: Ensures daily activity density avoids schedule overlap or travel fatigue.

Budget Realism: Reconciles combined flight, hotel, and activity estimates against user constraints.

3. Observability & Failure Isolation
Isolated execution blocks allow single-agent retries without restarting the full workflow.

Structured error handling returns clear diagnostic messages to the frontend or user interface.

📋 Data Schemas & Contracts
1. Request Input Contract (Coordinator Output)
JSON
{
  "origin": "Seattle",
  "destination": "Las Vegas",
  "departure_date": "2026-04-01",
  "return_date": "2026-04-04",
  "number_of_travelers": 2,
  "budget": "Moderate",
  "travel_style": "Leisure / Sightseeing",
  "preferences": ["Central Strip hotel", "Top shows", "No early morning flights"],
  "error": false,
  "error_msg": ""
}
2. Standardized Itinerary Output Structure
Trip Overview: High-level summary of dates, traveler counts, and key highlights.

Flight Options: Top 2–3 curated round-trip choices sorted by price/convenience.

Hotel Selections: Top 2–3 recommended stays matching user budget/location requirements.

Daily Itinerary: Balanced daily breakdown of curated local experiences.

🧪 Iterative Prompt Evolution
This project documents the architectural evolution from single-agent setups to multi-agent orchestration:

v1 (Single Agent): Zero-tool internal reasoning; limited accuracy for real-time pricing and schedules.

v2 (Single Agent + SerpAPI): Single prompt handling search tools across all domains; subject to context-window overload and hallucinated constraint checks.

v2.3 (Sequential Single Agent): Enforced execution order but caused slow, serial processing times.

v3 (Basic Multi-Agent): Distributed domain sub-tasks to independent agents executing in parallel.

v4 (Production Multi-Agent): Full coordinator-orchestrator-validator pattern featuring structured schema validation, auto-curation, and silent fault recovery.

⚙️ Getting Started with n8n
Prerequisites
n8n Instance: Self-hosted (Docker) or Cloud.

API Keys Required:

OpenAI / Anthropic API Key (for Agent nodes)

SerpAPI Key (for flight and hotel web searches)

Tavily API Key (for activity and web search aggregation)

Setup Instructions
Import Workflows: Download the workflow JSON files from the /workflows directory and import them into n8n.

Configure Credentials: Set up your API credentials in n8n for LLM providers and search tools.

Set Environment Variables: Configure system time variables ($now) and webhook endpoints.

Trigger Workflow: Test via the included Chatbot UI or Telegram Bot webhook integrations.

🚀 Future Roadmap & Extensions
[ ] Car Rental & Ground Transit Agent: Automatically calculate ground travel times between hotel and scheduled activities.

[ ] Real-Time Weather Integration: Dynamically reschedule outdoor activities based on weather forecasts.

[ ] Visa & Travel Advisory Agent: Automatically pull entry requirements based on traveler nationality and destination.