# **Autonomous Gmail Action Agent**

A LangGraph-powered Python agent that reads unread emails, analyzes their intent, and autonomously sends replies based on conditional logic.

## **Prerequisites**

1. **Google Cloud Console Setup:**  
   * Create a new project in the [Google Cloud Console](https://console.cloud.google.com/).  
   * Enable the **Gmail API**.  
   * Go to **APIs & Services \> OAuth consent screen**. Configure it as "External" and add your email as a test user.  
   * Go to **Credentials \> Create Credentials \> OAuth client ID** (Application type: Desktop app).  
   * Download the JSON file, rename it to credentials.json, and place it in the root directory of this project.  
2. **Environment Variables:**  
   * Set your LLM API key. This sample uses OpenAI, but you can swap it for Anthropic or Google GenAI.

export OPENAI\_API\_KEY="your-api-key"

3. **Install Dependencies:**  
   pip install langgraph langchain-openai langchain-google-community google-auth-oauthlib google-api-python-client

## **Running the Application**

1. Run the script: python gmail\_agent.py  
2. **First Run Authorization:** A browser window will open asking you to log into your Google account. Grant the required permissions (Read, Send, Modify).  
3. The script will generate a token.json file so you don't have to log in every time.  
4. The agent will fetch the latest unread email, decide if an action is needed, and optionally send a reply.

## **gmail\_agent.py**

import os  
from typing import TypedDict, Annotated, List  
from langgraph.graph import StateGraph, END  
from langchain\_openai import ChatOpenAI  
from langchain\_core.messages import HumanMessage, AIMessage  
from langchain\_google\_community import GmailToolkit  
from langchain\_google\_community.gmail.utils import build\_resource\_service, get\_gmail\_credentials

\# Ensure you have set: os.environ\["OPENAI\_API\_KEY"\] \= "your\_key"

\# 1\. Authenticate and build Gmail Tools  
credentials \= get\_gmail\_credentials(  
    token\_file="token.json",  
    scopes=\["\[https://mail.google.com/\](https://mail.google.com/)"\], \# Full access for reading/sending  
    client\_secrets\_file="credentials.json",  
)  
api\_resource \= build\_resource\_service(credentials=credentials)  
toolkit \= GmailToolkit(api\_resource=api\_resource)  
tools \= toolkit.get\_tools()

\# Extract specific tools we need  
search\_tool \= next(tool for tool in tools if tool.name \== "search\_gmail")  
send\_tool \= next(tool for tool in tools if tool.name \== "create\_gmail\_draft") \# Using draft for safety, swap to 'send\_gmail\_message' for direct sending

\# 2\. Define State  
class AgentState(TypedDict):  
    email\_id: str  
    sender: str  
    subject: str  
    body: str  
    decision: str  
    draft\_response: str

llm \= ChatOpenAI(model="gpt-4o-mini", temperature=0)

\# 3\. Define Nodes  
def fetch\_email(state: AgentState):  
    print("--- FETCHING LATEST UNREAD EMAIL \---")  
    \# Search for latest unread email  
    results \= search\_tool.invoke({"query": "is:unread", "max\_results": 1})  
      
    \# In a real app, you would parse the results list securely.   
    \# For this sample, we mock the parsed extraction.  
    if not results or "No messages found" in str(results):  
        return {"email\_id": None, "subject": "None", "body": "None"}  
      
    \# Simulating parsed email data from the tool's raw string output  
    return {  
        "email\_id": "12345",   
        "sender": "example@domain.com",  
        "subject": "Inquiry about pricing",  
        "body": str(results)  
    }

def analyze\_email(state: AgentState):  
    print(f"--- ANALYZING EMAIL: {state.get('subject')} \---")  
    if not state.get("email\_id"):  
        return {"decision": "IGNORE"}

    prompt \= f"""  
    Analyze this email:  
    Subject: {state\['subject'\]}  
    Body: {state\['body'\]}  
      
    If the email is asking about 'pricing' or 'cost', respond with exactly 'REPLY\_PRICING'.  
    Otherwise, respond with 'IGNORE'.  
    """  
    response \= llm.invoke(\[HumanMessage(content=prompt)\])  
    decision \= response.content.strip()  
    return {"decision": decision}

def draft\_reply(state: AgentState):  
    print("--- DRAFTING/SENDING REPLY \---")  
    reply\_body \= "Hello\! Thank you for inquiring about our pricing. Our standard tier is $99/mo. Let us know if you want to proceed\!"  
      
    \# Invoke the Gmail tool to create a draft (or send)  
    send\_tool.invoke({  
        "to": \[state\["sender"\]\],  
        "subject": f"Re: {state\['subject'\]}",  
        "message": reply\_body  
    })  
    return {"draft\_response": reply\_body}

\# 4\. Conditional Edge Logic  
def route\_action(state: AgentState):  
    if state.get("decision") \== "REPLY\_PRICING":  
        return "draft\_reply"  
    return "end"

\# 5\. Build Graph  
workflow \= StateGraph(AgentState)

workflow.add\_node("fetch\_email", fetch\_email)  
workflow.add\_node("analyze\_email", analyze\_email)  
workflow.add\_node("draft\_reply", draft\_reply)

workflow.set\_entry\_point("fetch\_email")  
workflow.add\_edge("fetch\_email", "analyze\_email")  
workflow.add\_conditional\_edges(  
    "analyze\_email",  
    route\_action,  
    {  
        "draft\_reply": "draft\_reply",  
        "end": END  
    }  
)

app \= workflow.compile()

\# 6\. Run the Agent  
if \_\_name\_\_ \== "\_\_main\_\_":  
    initial\_state \= {"email\_id": "", "sender": "", "subject": "", "body": "", "decision": "", "draft\_response": ""}  
    result \= app.invoke(initial\_state)  
    print("\\nFinal State:", result)  
