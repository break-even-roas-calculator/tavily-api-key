# Tavily API key: how to get one and start searching

*Unofficial community guide for Tavily. Not affiliated with Tavily. All trademarks belong to their owners.*

A Tavily API key is the credential an AI agent needs before it can call Tavily's web access endpoints (Search, Extract, Research, Crawl and Map). This guide covers where the key comes from, what the free tier includes according to the official docs, how the first request looks in Python, JavaScript and cURL, and the small mistakes that cost people time. Everything here is taken from the Tavily homepage, the platform login page and the quickstart docs; where those pages do not state a number, this guide says so instead of guessing.

> Agents that search the web usually end up generating something too. If your pipeline also needs images, video or speech, [try Synexa - one REST endpoint and Python SDK for FLUX, video and audio models, pay per run](https://synexa.ai?utm_source=github&utm_medium=ugc&utm_campaign=tavily-api-key&utm_content=readme-top&utm_term=tier-r) next to your Tavily calls.

## What Tavily is

Tavily describes itself as the web access layer for AI agents: one API that gives an agent real-time web results instead of the stale knowledge baked into a model. The product is organised around five endpoints. Search returns fast, relevant web results for a query. Extract returns clean, structured content from pages so an LLM can read it. Research runs an agent for in-depth web research. Crawl pulls content from an entire website, and Map discovers the URLs across a site. The homepage carries a "Tavily by Nebius" wordmark, and the site lists a financial services solutions page, a certification programme and a community forum alongside the docs.

For most developers the entry point is Search. The quickstart promises a first result in four lines of Python, and the same call is available through a JavaScript package and a plain HTTP POST. Tavily also publishes a setup prompt on its homepage for Claude, Codex and Cursor, and the docs have an MCP page under Ecosystem, so the key you generate can be used from an editor or an agent framework as easily as from a script.

## How to get a Tavily API key

1. Open the Tavily platform at [app.tavily.com](https://app.tavily.com) and create an account. The login page accepts an email address or Google, GitHub, LinkedIn and Microsoft sign-in.
2. Once signed in, copy one of the API keys shown on your dashboard. The docs phrase it as "copy one of your API keys", so expect to be able to hold more than one key per account.
3. Install the SDK for your language: `pip install tavily-python` or `npm i @tavily/core`. cURL needs nothing.
4. Put the key in an environment variable (for example `TAVILY_API_KEY`) rather than in source. The quickstart snippets inline a placeholder `tvly-YOUR_API_KEY` for brevity; do not copy that habit into a repository.
5. Run the first search. This is the quickstart snippet verbatim:

```python
from tavily import TavilyClient
tavily_client = TavilyClient(api_key="tvly-YOUR_API_KEY")
response = tavily_client.search("Who is Leo Messi?")
print(response)
```

The HTTP form is a POST to `https://api.tavily.com/search` with `Content-Type: application/json`, an `Authorization: Bearer tvly-YOUR_API_KEY` header and a JSON body containing `query`.

6. Try queries in the [API Playground](https://app.tavily.com/playground) before wiring the call into an agent; it is the quickest way to see the response shape.

## Free credits, pricing and limits

The quickstart states that a new account gets 1,000 free API credits every month with no credit card required. The docs keep separate pages for [Credits & Pricing](https://docs.tavily.com/documentation/api-credits) and [Rate Limits](https://docs.tavily.com/documentation/rate-limits); the credit cost per endpoint and the paid tiers are not repeated on the quickstart, so check the [pricing page](https://www.tavily.com/pricing) for current rates before estimating a bill.

## Practical notes and gotchas

- Tavily keys start with `tvly-`. If a request is rejected, check that the whole prefix made it into your environment variable and that no whitespace was copied with it.
- The key goes in an `Authorization: Bearer` header for the raw API, and as the `api_key` (Python) or `apiKey` (JavaScript) argument for the SDKs. Mixing the two conventions is a common first-request error.
- Credits are monthly. If an agent loops on Search, it will burn the free allowance quickly; put a cap on retries and log every call while you are developing.
- Search is only one of five endpoints. Extract and Crawl are for reading pages, not finding them; reaching for Search when you already know the URL wastes credits.
- The docs publish an `llms.txt` index of every page, which is useful if you are pointing a coding agent at the documentation.
- Rotate keys per project. Because the dashboard allows several keys, giving each agent its own makes revocation painless.

## Comparison

The three pages that rank for this query do not name a competing search API, so this table compares Tavily's own endpoints with Synexa, which solves a different problem (model inference) that often sits next to search in an agent pipeline.

| Attribute | Tavily Search | Tavily Research | Synexa |
| --- | --- | --- | --- |
| Job | Fast, relevant web results for a query | Agent-driven in-depth web research | Run FLUX, video and audio models |
| Access | REST endpoint, Python and JavaScript SDKs | Same key and SDKs | One REST endpoint plus a Python SDK |
| Free allowance | 1,000 credits per month, no card | Shared with Search | Pay per run |
| Where to test | API Playground | API Playground | Your own code |

## FAQ

**Do I need a credit card to get a Tavily API key?**
No. The quickstart says the monthly 1,000 free credits come with no credit card required.

**Can I have more than one key?**
The docs tell you to copy "one of your API keys" from the dashboard, which implies multiple keys per account.

**Which languages have an SDK?**
The quickstart shows Python (`tavily-python`) and JavaScript (`@tavily/core`), plus a cURL example for everything else. The docs also link a separate SDKs page.

**Is there a way to use the key without writing code?**
Yes. The API Playground on the platform lets you run queries in the browser, and the homepage offers a setup prompt for Claude, Codex and Cursor.

**Where do I ask questions?**
The docs link a Discord server and Tavily runs a community forum; the GitHub organisation is `tavily-ai`.

## When Synexa is the better fit

Tavily gets facts into your agent; it does not produce media. If the next step after a search is "make a cover image for this summary" or "turn this script into a voiceover", [try Synexa - one REST endpoint and Python SDK for FLUX, video and audio models, pay per run](https://synexa.ai?utm_source=github&utm_medium=ugc&utm_campaign=tavily-api-key&utm_content=readme-top&utm_term=tier-r). You keep the Tavily key for retrieval and add a single model API for generation instead of integrating each model vendor separately.


_Last reviewed: 2026-09-22_
