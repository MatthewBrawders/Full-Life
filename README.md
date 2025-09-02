# Full-Life -- Smart Profile & Event Node Scheduler

Full-Life is a **profile and event management platform** that lets users
create rich personal profiles, schedule events with natural language,
and view AI-enriched recommendations.\
It combines **React (Vite)** for the frontend, a modular **FastAPI**
backend, and **MongoDB** for persistence, all deployed via **Docker
Compose** with optional NGINX for production.

------------------------------------------------------------------------

## Features

-   **Profiles** -- Create and manage user profiles with name, bio, and
    preferences.\
-   **Event Nodes** -- Store events with title, description, location,
    categories, and user-specified or AI-parsed dates.\
-   **Natural Language Queries** -- Enter prompts like *"I want an event
    this weekend"*; the system extracts and normalizes dates.\
-   **Geolocation-Aware** -- Frontend passes latitude and longitude from
    browser geolocation to personalize results.\
-   **Event Controller** -- Integrates with:
    -   **SerpAPI** for event data\
    -   **OpenWeather** for location/weather enrichment\
    -   **OpenAI** for summarization, categorization, and natural
        language parsing\
-   **Event Manager** -- Delegates event data to repository for database
    writes and retrieval.\
-   **Repository Layer** -- Handles MongoDB reads/writes, grouping
    results for efficient queries.\
-   **Clean Architecture** -- Clear separation between controllers,
    managers, and repositories.

------------------------------------------------------------------------

## Tech Stack

-   **Frontend**: React 18 + Vite (modern SPA with local storage +
    geolocation)\
-   **Backend**: FastAPI (async endpoints, Pydantic models,
    Twisted/async integration)\
-   **Database**: MongoDB (event + profile collections)\
-   **APIs**: SerpAPI, OpenWeather, OpenAI GPT models\
-   **Deployment**: Docker Compose (frontend, backend, MongoDB; NGINX
    for prod)

------------------------------------------------------------------------

## Architecture

``` mermaid
flowchart TD
    Browser[React Frontend] -->|HTTP /api| Nginx[(NGINX Reverse Proxy)]
    Nginx --> FastAPI[FastAPI Backend]
    FastAPI --> Controller[Event Controller]
    Controller -->|fetch + enrich| APIs[(SerpAPI / OpenWeather / OpenAI)]
    Controller --> Manager[Event Manager]
    Manager --> Repo[Event Node Repository]
    Repo --> Mongo[(MongoDB)]
    FastAPI --> FrontendResp[(Responses: events, summaries, categories)]
```

------------------------------------------------------------------------

## Example Workflow

1.  User opens the React SPA, geolocation stored in local storage.\
2.  User enters a prompt: *"Find concerts near me next Friday."*\
3.  Frontend calls `/query` → FastAPI → Event Controller.\
4.  Controller uses OpenAI to parse date/place, SerpAPI for event data,
    OpenWeather for enrichment.\
5.  Event Manager delegates storage to Repository, which writes results
    into MongoDB.\
6.  Frontend displays enriched event list, categorized and summarized.

------------------------------------------------------------------------

## Development

### Prerequisites

-   Docker & Docker Compose
-   Node.js (for local frontend dev)
-   Python 3.10+ (for backend dev)

### Run Locally

``` bash
# start all services
./dev up

# rebuild after changes
./dev build

# stop
./dev down
```

Frontend will be available at `http://localhost:5173` (dev) or
`http://localhost` (prod).\
Backend runs at `http://localhost:8000/api`.

------------------------------------------------------------------------

## Environment Variables

Create a `.env` file with:

``` bash
MONGO_URI=mongodb://mongo:27017
MONGO_DB_NAME=full_life
OPENAI_API_KEY=sk-...
SERPAPI_KEY=...
OPENWEATHER_KEY=...
EVENT_CONTROLLER_URL=http://event-controller:8000
VITE_API_BASE_URL=/api
```

------------------------------------------------------------------------

## Future Plans

-   Calendar view & reminders
