<img width="1287" height="687" alt="image" src="https://github.com/user-attachments/assets/c461a891-511e-4344-9892-4813f73c74b3" />### FlyGpt :
My application should be able to interact with the third party flight services, and then retrieve flight details fromt the sites and then also compare that against my preferences such as cheap vs luxury, seats, meals. Based on all of that make a decision and book flights. Here it is expected to AI make decision on behalf of user/customer.

These are called AI Agents. 

### AI Agents
AI Agents are able to interact with third party platforms or websites, gather information and combine that with the memory based on previous conversation, and then interact with LLM which is the real AI here to make a decision.

In nutshell, AI Agent interacts with LLM to process data and make decision.
<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/ea94bc36-4540-4549-b9c9-81559977d55a" />

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/98dd8e53-1e6f-4fd7-9414-c93308980505" />

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/310fd3c6-514c-4886-b15a-e84ec3d25751" />

The Agent relies on AI in the form of LLM to take decision,
	• Agent often interact with AI to know what route to take as part of conditional decision
	• How many times to iterate in a loop, or third party tools to interact with.
	• Or how to process the user input.
AI Agent is like old automation script, except now it can think.

Typical AI Agent workflow looks like this,
	• The user sends the request to AI Agents.
	• The Agent interacts with the LLM , to extract right details. Different user might input differently,
		○ User says "I would like to fly to north london"
"Destination": "London"

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/98fc4be3-1eaf-4852-85ed-1cfc4174a5c1" />

	• Now AI Agents asks which third party tools to use. Now the LLM helps the AI Agents with tools to use. The AI Agent then interacts with airlines and retrieve flight details,

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/4b27af38-fe9f-4584-86e6-4e60ca3731f9" />

	• The AI Agent asks LLM, what do I do next, and the LLM says get the preferences from database.

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/b569a3c9-56f2-4571-8adb-fb9bf7bb0fa4" />

	• With these details, now AI Agents asks the suggestion from LLM. The LLM identifies the right flight to book and sends back information to AI Agent.

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/d478febd-1186-4abb-aaf9-1c0d3c675692" />

	• Now the AI Agents books the flight and sends the user the booked flight details,

<img width="1320" height="681" alt="image" src="https://github.com/user-attachments/assets/d64caa5c-eb30-4167-947e-5db82fa5ddb0" />

This is a flygpt AI Agent written in python,

<img width="672" height="742" alt="image" src="https://github.com/user-attachments/assets/95b7fd0b-6034-4a3a-8b69-e478a7fe752d" />

This is just a simple version of AI Agent. Frameworks like LangChain, LangGraph.

<img width="1315" height="694" alt="image" src="https://github.com/user-attachments/assets/2bae61cd-76b4-4d67-a797-ec46246a5204" />

Tool : A tool allows the agent to interact with another platform. In below, Agent interacts with airlines with tools.

<img width="1315" height="694" alt="image" src="https://github.com/user-attachments/assets/00bbfa7a-2130-4c21-aeb6-6ccb785e98b1" />

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/18f1a224-92f4-45f7-b453-b24d0535c08b" />

Here in fetch_flight_details() function, each tool is called with corresponding query parameters such as origin,  destination and date. This function fetches results from each airline via tool and appends it to result and return that.

In call_tool function, if we see the APIs for each airlines its like, and the API for each airlines varies
www.joyfair.com/api/flights
www.dracair.com/api/flights-list
www.aerogo.com/api/list-flights

Here, this Agent interacts with 3 airlines and all 3 endpoints are different for each airlines. Let say if we have to interact with many airlines and in that case ?

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/76c2b659-d66a-469f-b4ea-5482eea70b94" />

For this issue, AI world provides the solution is MCP. Think MCP as a guide to choose right API and interact with the third party platform. MCP provides the Agents the context that they needs to interact with third party platform.
<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/cdda6e3d-0705-43f4-944f-f71e8a98291a" />

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/8db15912-3aca-49fd-afc6-75e919252dc6" />

### MCP
Let's dive deep that how agents can discover and use MCPs. 

• MCP follow a client-server architecture.
• Agents use the MCP client to interact with the server.
MCP client is embedded within coding agents such as claude code, windsurf, claude code.

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/a882f647-ce4c-4f3a-83e6-d8a9d808271d" />

Here is an example that AI Agent to troubleshoot problem with UI application,
<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/535954f6-eb56-4b90-aa85-b3e5d98b9ace" />

The AI Agent checking the history and finally identified the commit history to never back,
<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/e09d4e83-542d-4e14-9502-1a1d6cb1925e" />


The above doesn’t use an MCP server, while developing front end web application. This doesn’t use an MCP server. While developing frontend web application using the browser, MCP server provides the AI agents access to the browser console logs and html elements that can be used to troubleshoot frontend issues.

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/db5c8e1e-1c35-4f07-a82f-afbd166fc8f6" />

Finally the really use cool usecase - Data engineering. By providing MCP server read only access to our data using Stripe, Big Query and Data studio.

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/127ccad6-776d-46db-9963-0d2965c7709d" />

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/ed83aee9-f826-47b0-b4ca-055e8e7e4488" />

Who builds the MCP servers ?
MCP server can be build by anyone as long as they know how they work. There are company they don’t have MCP servers, communities built MCP servers for it.

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/b9756e83-826a-4af8-917b-1d4488d8b44b" />

What is MCP ?
Model Context Protocol. It defines a set of rules that needs be followed when someone is building MCP server.

<img width="1320" height="731" alt="image" src="https://github.com/user-attachments/assets/4040423e-a697-4771-935a-e52616267915" />

### MCP Architecture :

Consider that going to build MCP Client for a MCP server.

First what is needed, need to look at the API documentation to know everything that the server supports.

<img width="1359" height="757" alt="image" src="https://github.com/user-attachments/assets/43c6d77f-3de7-4f32-9827-a6fcde6f0b97" />

These are known as tools in this MCP world, server must list of all of its capabilities as a list of tools defined in a specific  format

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/1642b632-d29b-467a-a9f4-f5843b5d83b8" />

Each tool should have a description of what it does and then input and output schema. 

Next set of details needed like Refund rules, City Guides, FAQ and these details needed for AI for descision making.  these are known as Resources.
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/96205e97-41bd-42d7-bfb9-d68529e41630" />

Refund rule resources :
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/68524c68-ace2-4bca-9f51-4444c29959f6" />

A resource can point to a url, file, git location as shown below, 

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/218d2d92-078b-4f85-8ccf-ac31e6b0cb3d" />

Let say when the user asks like - "Find me a flight to Bangalore". IT will be given along with apt context to LLM as "You are a travel assistant. When the user asks about flights you must call the 'search_flights' tool with origin, destination, and date, format all datest as YYYY-MM-DD. Be helpful and concise". These are known as prompts.

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/469e416b-90bc-496c-9dc2-2f47c5a79872" />

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/f14cf070-91f0-4c80-bc87-27a7d60f0d25" />

### JSON-RPC
How the MCP Client and Server communicate with each other - via JSON-RPC(2.0) protocol.

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/d867bd28-7d41-45ea-94e6-85e9e81c2670" />

JSON is like key value pair, RPC stands for Remote procedure call and together it defines like how a client can call method/procedure remotely using json and pass parameters and receive a response.

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/97933a6a-6613-400c-a073-e4da091fdf4e" />

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/458f6550-9a0a-417b-913e-2c1d527c7dcd" />

### MCP Server
MCP supports HTTP and STDIO as the standard mechanism for client and server communication.

<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/3f9afdc1-1e6c-4d0a-9a8b-06e59e04908a" />

How to use existing MCP server :

Normally we write code to connect to server via APIs,
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/845243b9-a354-41e8-ae37-83679cb7a734" />

With MCP server, we no longer concerned about API, instead we need to connect to MCP server, and MCP server handling the interaction with API
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/50d51c7d-a81d-4749-8af0-a0edb57af1b9" />

Now the next part where the MCP server is hosted, it's local or remote,

Locally hosted,
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/372239cf-6e49-4841-a03d-c685c1afca0f" />

Remotely hosted,
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/3d5bd0b2-8b90-453d-81e4-636cb5e968b6" />

How to configure MCP server locally :

Mcp.json file for configuring mcp server in IDE
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/31b4041d-8ddc-4610-acf9-e8d6cc6ec94c" />

	• When mcp.json is configured, IDE starts the MCP server in stdio mode. Stdio is the simplest and fastest way for 2 local processes without needing http sockets.
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/a8286ca8-9b9c-42fc-bc43-616e7cea4c15" />

	• When MCP client connects to MCP, need to take care of Authentication, Authorization, Data Privacy, Trust.
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/76f4bdc6-5a1b-45d1-b0f8-4f9ace7f38e2" />

	• Below is the simply MCP client connects to MCP server of joyair.
<img width="1341" height="787" alt="image" src="https://github.com/user-attachments/assets/2c4f0b3a-1836-49c2-824b-6d2cbc3a5915" />

```
from mcp.server.mcpserver import MCPServer

# Create an MCP server
mcp = MCPServer("Flight Booking Server")

@mcp.resource("file://airports")
def get_airports():
    """Get list of available airports"""
    return {
        "LAX": {"name": "Los Angeles International", "city": "Los Angeles"},
        "JFK": {"name": "John F. Kennedy International", "city": "New York"},
        "LHR": {"name": "London Heathrow", "city": "London"}
    }

@mcp.tool()
def search_flights(origin: str, destination: str) -> dict:
    """Search for flights between two airports"""
    return {
        "flights": [
            {"id": "FL123", "origin": origin, "destination": destination, "price": 299},
            {"id": "FL456", "origin": origin, "destination": destination, "price": 399}
        ]
    }

@mcp.tool()
def create_booking(flight_id: str, passenger_name: str) -> dict:
    """Create a flight booking"""
    return {
        "booking_id": f"BK{flight_id[-3:]}",
        "flight_id": flight_id,
        "passenger": passenger_name,
        "status": "confirmed"
    }

@mcp.prompt()
def find_best_flight(budget: float, preferences: str = "economy") -> str:
    """Generate a prompt for finding the best flight within budget"""
    return f"""Please help me find the best flight within a ${budget} budget.
    
My preferences: {preferences}
Please consider:
- Price (must be under ${budget})
- Flight duration  
- Airline reputation
- Departure times
Use the search_flights tool to find available options and provide a recommendation with reasoning."""

@mcp.prompt()
def handle_disruption(original_flight: str, reason: str) -> str:
    """Generate a prompt for handling flight disruptions"""
    return f"""A passenger's flight {original_flight} has been disrupted due to: {reason}
Please help resolve this by:
1. Understanding the passenger's situation
2. Finding alternative flight options using search_flights
3. Providing clear rebooking steps
4. Offering appropriate compensation if applicable
Be empathetic and solution-focused in your response."""

if __name__ == "__main__":
    mcp.run(transport="streamable-http",port=8080) 

```

### Resources : 
	• Resources are the references binders on your desk - airport codes, airline policies, fare rules. You read them to understand the domain.You don’t change them.
		@mcp.resource("file://airports")
		def get_airports():
		    return {"LAX": "Los Angeles", "JFK": "New York"}
		
	• Use a resource when a data is :
		○ Relatively static
		○ Useful as background context
		○ Doesn’t need input parameters.
### Tools :
	• Tools are the systems you log into. - the booking system, the refund system, the rebooking system. You use them to take action on behalf of customers.
		@mcp.tool()
		def search_flights(origin: str, destination: str) -> dict:
		    return {"flights": [...]}
	• Use a tool when:
		○ The AI needs to pass parameters
		○ The action depends on user input
		○ Something gets created, queried dynamically or modified.
		
### Prompts:
	• Prompts are the standard operating procedures - when a flight is cancelled, follow these steps. They guide how you can use the tools.
		@mcp.prompt()
		def find_best_flight(budget: float, preferences: str = "economy") -> str:
		    """Generate a prompt for finding the best flight within budget"""
		    return f"""Please help me find the best flight within a ${budget} budget.
		    
		My preferences: {preferences}
		
		Please consider:
		- Price (must be under ${budget})
		- Flight duration  
		- Airline reputation
		- Departure times
		
		Use the search_flights tool to find available options and provide a recommendation with reasoning."""
		
	• When prompts makes sense,
		○ Repeatable workflow with a specific structure
		○ Guide decision-making with criteria.
Prompt engineering


### Building an MCP server :
<img width="1354" height="715" alt="image" src="https://github.com/user-attachments/assets/0eab0561-cc93-4ae4-8439-40fcaa8743a6" />

Model Context Protocol has certain sdk and these can be used to develop MCP servers/clients. These can be found in https://modelcontextprotocol.io/docs/2026-07-28/sdk

Use the python sdk and develop server.py as below,
Server.py has steps like initialize server, resources, prompts, tool and then finally run the server.
<img width="1388" height="715" alt="image" src="https://github.com/user-attachments/assets/484bfa61-580f-45a4-9e41-caf26f6c7cd4" />

Resources :
With decorator mcp.resources
<img width="1388" height="715" alt="image" src="https://github.com/user-attachments/assets/3e894cf7-092d-433d-a9b2-e2c0e21d4d44" />

Tools : 
With decorator mcp.tool
<img width="1388" height="715" alt="image" src="https://github.com/user-attachments/assets/5a5c1701-41dd-4f2b-9427-4169e36960ae" />

Prompts :
With decorator mcp.prompts
<img width="1388" height="715" alt="image" src="https://github.com/user-attachments/assets/d5380b0d-ef66-43f3-93a0-b59bf01a8182" />

Possible ways to start server :
<img width="1388" height="715" alt="image" src="https://github.com/user-attachments/assets/26e9ab25-697a-4f99-b905-e1669061a596" />

### MCP Inspector :
Before building a mcp client to test with MCP server, you can run MCP inspector to test it.

To run inspector - npx @modelcontextprotocol/inspector
<img width="1388" height="715" alt="image" src="https://github.com/user-attachments/assets/e794e810-885e-435e-bca2-6442f0c3dea8" />

MCP inspector is something like below,
<img width="1715" height="911" alt="image" src="https://github.com/user-attachments/assets/dfd39ba8-4678-4ebf-be5f-750944c73271" />

### Build an MCP server :

Run below commands,
	• # cd /home/mcp-sample
	• # uv init flight-booking-server             ## to create flight-booking-server project folder structure and core configuration files for python project.
	• # cd flight-booking-server
	• # uv add mcp[cli]                                 ## light weight command line interface that allows AI developers and AI coding agents discover, inspect and invoke MCP servers directly from terminal. It's main purpose to solve context window bloat. Mcp-cli replaces heavy upfront load with on-demand, dynamic discovery.

Main.py for server looks as below,
<img width="1360" height="729" alt="image" src="https://github.com/user-attachments/assets/5e3776ac-9834-4232-9590-fdc6d3d6f66f" />

#### STDIO vs HTTP in MCP server :
<img width="1145" height="686" alt="image" src="https://github.com/user-attachments/assets/ff13dfb8-ec30-4143-a71a-d012d311af9b" />

STDIO: STDIO is the native transport for AI coding assistants and local agentic apps. It follows a process-to-process communication model with no network stack involved.
Mechanism: The client spaws the MCP server as a child process. It writes JSON-RPC requests to the server stdin and reads responses from stdout.
<img width="1145" height="686" alt="image" src="https://github.com/user-attachments/assets/916a5e54-1e49-4573-93cd-90cb87a1afd2" />

HTTP : HTTP transport transforms your MCP server into a standard network service. This allows it to handle multiple concurrent clients and integrate with enterprise infrastructure. 
Mechanism : A dual channel approach. Clients send requests via HTTP post, while the server pushes updates and notifications back via Server-Sent-Events(SSE).
<img width="1008" height="726" alt="image" src="https://github.com/user-attachments/assets/98973ada-1685-45eb-b18c-0a6c75d351d0" />

#### Building of a mcp server
<img width="660" height="686" alt="image" src="https://github.com/user-attachments/assets/4a473f08-3a2e-482d-97ac-b05c3299468d" />

# uv init flight-booking-server
# cd flight-booking-server
# uv add "mcp[cli]<2"


built a complete MCP server following official patterns with:
	• Proper project setup using uv and the official MCP package
	• Resources for airport information
	• Tools for flight search and booking
	• Prompts for AI guidance
	• Testing with official MCP development tools
	• Integration with the Roo-Code AI assistant
What you've learned:
	• The official MCP development workflow with uv
	• The FastMCP framework from mcp.server.fastmcp
	• Resource, Tool, and Prompt implementation patterns
	• The MCP Inspector for server testing and validation
	• Real-world MCP server integration with AI assistants
Next steps:
	• Explore HTTP transport: mcp.run(transport="http")
	• Add error handling and input validation
	• Integrate with real flight APIs

In the roo code extension, add the MCP server as below,
<img width="1259" height="726" alt="image" src="https://github.com/user-attachments/assets/a118735d-9001-48e7-8c45-c179067a606d" />

I made the mcp server to run in the backup with nohup command,

cd ~/Work/MLOps/tools/mcp/mcpcourse
nohup uv run python kodekloud/mcp-server/flight-booking-server/server.py > logs/flight-booking.log 2>&1 &

\# See the process
```
# ps aux | grep flight-booking
```
\# Check the logs
```
# tail -f logs/flight-booking.log
```
\# Verify it's listening
```
# curl http://localhost:8080/mcp
```
\# to stop
```
# kill $(ps aux | grep 'flight-booking-server/server.py' | grep -v grep | awk '{print $2}')
```

Use curl with the correct MCP initialization
\# Step 1: Initialize the session
```
# curl -X -I POST http://localhost:8080/mcp \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "initialize",
    "params": {
      "protocolVersion": "2025-03-26",
      "capabilities": {},
      "clientInfo": {"name": "curl-test", "version": "1.0"}
    }
  }'
HTTP/1.1 200 OK
date: Sat, 21 Nov 2026 15:25:47 GMT
server: uvicorn
cache-control: no-cache, no-transform
connection: keep-alive
content-type: text/event-stream
mcp-session-id: 18b5aaa1c6284dddade2084b14e18a81
x-accel-buffering: no
Transfer-Encoding: chunked
```
Copy the session id from above response and pass it in below command,
```
# curl -X POST http://localhost:8080/mcp \
  -H "Content-Type: application/json" \
  -H "Mcp-Session-Id: 18b5aaa1c6284dddade2084b14e18a81" \
  -d '{"jsonrpc":"2.0","id":4,"method":"tools/call","params":{"name":"create_booking","arguments":{"flight_id":"FL123","passenger_name":"Balakrishnan"}}}'
```
event: message
data: {"jsonrpc":"2.0","id":4,"result":{"content":[{"text":"{\n  \"booking_id\": \"BK123\",\n  \"flight_id\": \"FL123\",\n  \"passenger\": \"Balakrishnan\",\n  \"status\": \"confirmed\"\n}","type":"text"}],"isError":false}}


### How to build MCP client :
Lot of agents support MCP clients automatically, the AI Agents mcp.json file has to point to mcp server as shown below,
<img width="1259" height="726" alt="image" src="https://github.com/user-attachments/assets/226e5b92-1ffe-4cbd-9c97-0c3e34c9be30" />

However if you want to build your own AI Agent, in that case you need to build mcp client from scratch.

The below client connects to mcp server with particular port number, list_tools() to list the tools, call_tool to call particular tool with arguments and the resources.

<img width="1259" height="691" alt="image" src="https://github.com/user-attachments/assets/f529f26a-821f-46df-861a-f9146a9fc6f6" />

MCP server has features : Prompts, Resources, Tools
MCP Client has features : Roots, sampling, elicitation

<img width="1259" height="691" alt="image" src="https://github.com/user-attachments/assets/f2c05242-7930-451a-965d-9b2424bd33e1" />

Contexts allows the server to talk back to the clients, may be for giving updates or sharing progress,etc.
Let say the client initiates the booking process, may be long running task, the server might want to periodic updates to the client before the actual response is shared, so for this we use the context, pass the input context.
Context.info()
Context.report_progress()
Context.debug()
<img width="1263" height="687" alt="image" src="https://github.com/user-attachments/assets/ba0cd387-9e18-4715-95e4-0c587b28b75f" />

Roots : MCP provides standard way for clients to expose filesystem "roots" to servers. Roots inform servers about the directories and files the client considers relevant, so that servers can focus their operations accordingly.

Now the next thing is the roots, think of Roots as folders on the client machine that the MCP server is allowed to see or interact with. They are like shared folders and exposed to the servers from your local file system. For security reason, the server shouldnt have access to the entire machine.
so while creating client session pass root argument with the folder that needs to get exposed.

and on the server side, ctx.session.list_roots() to get the list of roots allowed.
<img width="1263" height="687" alt="image" src="https://github.com/user-attachments/assets/d38816ae-584e-4e81-a93d-43fcaf81bccf" />

Sampling : Sampling in MCP allow servers request LLM completions("generations") through a connected MCP client rather than callling an LLM API directly. The server might want to interact with client for some specific reason. For example, the server might send resources to the LLM to summarize it. For such case, the server doesnt interact with the LLM directly and in such case the server will not interact with LLM directly. Because the server may be used by different clients and would want to have control on certain things like LLM Integrations, Token limits, Model selection, etc. The server is purely the logic and tool definitions, resources, lightweight, decoupled from AI.
<img width="1263" height="687" alt="image" src="https://github.com/user-attachments/assets/c877459a-329e-4d1b-a61e-67605e68e982" />

This is done by passing sample llm handler function while creating client session. On the server side, we can send message to the client using ctx.session.create_message()
<img width="1287" height="687" alt="image" src="https://github.com/user-attachments/assets/20cc0b35-5a74-47d5-ad1f-571b75d39764" />

Elicitation : MCP provides a standardized way for servers to request additional information from users through the client during interactions. At times, the server wants to interact with end user to get more information to pick the best flight. the server need additional confirmation that the user is happy for the choice made. in that case, the server can send message using ctx.elicit()

<img width="1287" height="687" alt="image" src="https://github.com/user-attachments/assets/239bddd8-17d4-4d44-9e15-603b4baa3aca" />







