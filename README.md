🚀 Memory Management – Sessions (ADK)

Kaggle 5-Day Agents Course – Day 3

This repository contains code and explanations for building stateful AI agents using the Google Agent Development Kit (ADK).
You’ll learn how sessions work, how to persist conversations, and how to optimize context using compaction.

🌟 Features Covered
✔ Session Management

Understanding Sessions & Events

Stateful conversations with context retention

InMemorySessionService vs DatabaseSessionService

✔ Persistent Conversations

Saving session history using SQLite

Resuming conversations after restart

Inspecting stored events

✔ Context Compaction

Automatically summarizing long histories

Reducing cost and improving performance

Configuring compaction interval & overlap

✔ Session State

Storing structured key-value data

Custom tools to save & retrieve user profile

Cross-session state behavior

📦 Installation

For local development:

pip install google-adk

🔑 API Key Setup

Create a Gemini API Key from Google AI Studio

Add it to Kaggle Secrets as GOOGLE_API_KEY

Authenticate:

from kaggle_secrets import UserSecretsClient
import os

os.environ["GOOGLE_API_KEY"] = UserSecretsClient().get_secret("GOOGLE_API_KEY")

🧠 Core Components
Importing ADK Modules
from google.adk.agents import Agent, LlmAgent
from google.adk.sessions import InMemorySessionService, DatabaseSessionService
from google.adk.runners import Runner
from google.adk.models.google_llm import Gemini
from google.adk.apps.app import App, EventsCompactionConfig

⚙️ Creating a Stateful Agent (In-Memory)
session_service = InMemorySessionService()
agent = Agent(
    model=Gemini(model="gemini-2.5-flash-lite"),
    name="text_chat_bot"
)
runner = Runner(agent=agent, app_name="default", session_service=session_service)

🗄 Persistent Sessions (SQLite)
db_url = "sqlite:///my_agent_data.db"
session_service = DatabaseSessionService(db_url=db_url)

runner = Runner(
    agent=agent,
    app_name="default",
    session_service=session_service
)

🧩 Context Compaction
app = App(
    name="research_app",
    root_agent=agent,
    events_compaction_config=EventsCompactionConfig(
        compaction_interval=3,
        overlap_size=1
    )
)

🤝 Session State Tools
Save user info:
def save_userinfo(tool_context, user_name, country):
    tool_context.state["user:name"] = user_name
    tool_context.state["user:country"] = country
    return {"status": "success"}

Retrieve user info:
def retrieve_userinfo(tool_context):
    return {
        "user_name": tool_context.state.get("user:name"),
        "country": tool_context.state.get("user:country")
    }


Attach tools to agent:

agent = LlmAgent(
    model=Gemini(model="gemini-2.5-flash-lite"),
    name="text_chat_bot",
    tools=[save_userinfo, retrieve_userinfo]
)

📊 Key Learnings

How LLMs maintain short-term memory using sessions

Persist conversations using databases

Improve performance with context compaction

Store structured user data using session state

Build richer, more personalized AI agents

📚 Next Steps

Continue to Memory Management – Part 2 to learn:

Long-term memory

Memory extraction

Personalized agent behavior

Sharing knowledge across sessions
