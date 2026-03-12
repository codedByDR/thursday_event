# **Persistent Telegram Bot Agent**

A Telegram bot powered by a LangGraph ReAct agent. It features persistent memory across chat sessions and uses a web search tool to answer questions about recent events.

## **Prerequisites**

1. **Telegram Bot Token:**  
   * Open Telegram and search for **@BotFather**.  
   * Send /newbot and follow the prompts to create a new bot.  
   * Copy the HTTP API Token provided.  
2. **Tavily API Key (For Web Search):**  
   * Get a free API key from [Tavily](https://tavily.com/).  
3. **Environment Setup:**  
   * Create a .env file in the root directory:  
     TELEGRAM\_BOT\_TOKEN=your\_telegram\_token  
     OPENAI\_API\_KEY=your\_openai\_api\_key  
     TAVILY\_API\_KEY=your\_tavily\_api\_key

4. **Install Dependencies:**  
   pip install langgraph langchain-openai python-telegram-bot tavily-python python-dotenv

## **Running the Application**

1. Run the script: python telegram\_bot.py  
2. Open Telegram, find your bot, and click **Start**.  
3. Chat with your bot\! Ask it to remember your name, then ask it to search the web for recent news, and finally ask it what your name is to test the memory.

## **telegram\_bot.py**

import os  
import asyncio  
from dotenv import load\_dotenv  
from telegram import Update  
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes

from langchain\_openai import ChatOpenAI  
from langchain\_community.tools.tavily\_search import TavilySearchResults  
from langgraph.prebuilt import create\_react\_agent  
from langgraph.checkpoint.memory import MemorySaver

\# 1\. Load Environment Variables  
load\_dotenv()  
TELEGRAM\_BOT\_TOKEN \= os.getenv("TELEGRAM\_BOT\_TOKEN")

\# 2\. Setup LangGraph Agent with Memory  
llm \= ChatOpenAI(model="gpt-4o-mini", temperature=0)  
search\_tool \= TavilySearchResults(max\_results=2)  
tools \= \[search\_tool\]

\# MemorySaver acts as the checkpointer to remember past interactions  
memory \= MemorySaver()

\# Using a prebuilt ReAct agent for simplicity and power  
agent\_executor \= create\_react\_agent(llm, tools, checkpointer=memory)

\# 3\. Telegram Handlers  
async def start(update: Update, context: ContextTypes.DEFAULT\_TYPE):  
    """Handles the /start command."""  
    await update.message.reply\_text(  
        "Hello\! I am your LangGraph AI Assistant. I can search the web and remember our conversation. How can I help?"  
    )

async def handle\_message(update: Update, context: ContextTypes.DEFAULT\_TYPE):  
    """Processes incoming text messages through the LangGraph agent."""  
    user\_text \= update.message.text  
    chat\_id \= str(update.message.chat\_id) \# Use chat\_id as the unique thread\_id for memory

    \# Send a typing indicator  
    await context.bot.send\_chat\_action(chat\_id=chat\_id, action='typing')

    try:  
        \# Configuration required for memory checkpointer  
        config \= {"configurable": {"thread\_id": chat\_id}}  
          
        \# Invoke the agent asynchronously  
        response \= await agent\_executor.ainvoke(  
            {"messages": \[("user", user\_text)\]},   
            config=config  
        )  
          
        \# The final message is the last one in the messages array  
        final\_reply \= response\["messages"\]\[-1\].content  
        await update.message.reply\_text(final\_reply)

    except Exception as e:  
        print(f"Error: {e}")  
        await update.message.reply\_text("Sorry, I encountered an error processing your request.")

\# 4\. Main Application Loop  
if \_\_name\_\_ \== '\_\_main\_\_':  
    if not TELEGRAM\_BOT\_TOKEN:  
        raise ValueError("No TELEGRAM\_BOT\_TOKEN found in environment variables.")

    print("Starting Telegram Bot...")  
    app \= ApplicationBuilder().token(TELEGRAM\_BOT\_TOKEN).build()

    \# Add handlers  
    app.add\_handler(CommandHandler("start", start))  
    app.add\_handler(MessageHandler(filters.TEXT & \~filters.COMMAND, handle\_message))

    \# Start polling  
    app.run\_polling()  
