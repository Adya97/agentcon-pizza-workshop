# Adding Agent Instructions  

In the previous chapter, you created your first basic agent and started a conversation with it.  
Now, we’ll take a step further by learning about **system prompts** and why they’re essential for shaping your agent’s behavior.  


## What Is a System Prompt?  

A system prompt is a set of **instructions** you provide to the model when creating an agent.  
Think of it as the **personality and rulebook** for your agent: it defines how the agent should respond, what tone it should use, and what limitations it should follow.  

Without a system prompt, your agent may respond in a generic way. By adding clear instructions, you can tailor it to your needs.  

### System prompts:  

- Ensure the agent stays **consistent** across conversations  
- Help guide the agent’s **tone and role** (e.g., friendly teacher, strict code reviewer, technical support bot)  
- Reduce the risk of the agent giving **irrelevant or off-topic answers**  
- Allow you to **encode rules** the agent must follow (e.g., "always answer in JSON")  


## Adding Instructions to Your Agent  

When creating an agent, you can pass the `instructions` parameter.  
Here’s an example:  

```python
agent = project_client.agents.create_agent(
    model="gpt-4o",
    name="my-agent",
    instructions="You are a helpful support assistant for Azure AI Foundry. Always provide concise, step-by-step answers."
)
print(f"Created agent with system prompt, ID: {agent.id}")
```

Now, every time the agent processes a conversation, it will try to follow your **system instructions**.  


## Using an External Instructions File  

Instead of hardcoding instructions in your Python script, it’s often better to store them in a **separate text file**.  
This makes them easier to edit and maintain.  

First, create a file called **`instructions.txt`** with the following content:  

```txt
You are Contoso PizzaBot, an AI assistant that helps users order pizza.

Your primary role is to assist users in ordering pizza, checking menus, and tracking order status.

## guidelines
When interacting with users, follow these guidelines:
1. Be friendly, helpful, and concise in your responses.
1. When users want to order pizza, make sure to gather all necessary information (pizza type, options).
1. Contoso Pizza has stores in multiple locations. Before making an order, check to see if the user has specified the store to order from. 
   If they have not, assume they are ordering from the San Francisco, USA store.
1. Your tools will provide prices in USD. 
   When providing prices to the user, convert to the currency appropriate to the store the user is ordering from.
1. Your tools will provide pickup times in UTC. 
   When providing pickup times to the user, convert to the time zone appropriate to the store the user is ordering from.
1. When users ask about the menu, provide the available options clearly. List at most 5 menu entries at a time, and ask the user if they'd like to hear more.
1. If users ask about order status, help them check using their order ID.
1. If you're uncertain about any information, ask clarifying questions.
1. Always confirm orders before placing them to ensure accuracy.
1. Do not talk about anything else then Pizza
1. If you do not have a UserId and Name, always start with requesting that.

## Tools & Data Access
- Use the **Contoso Pizza Store Information Vector Store** to search get information about stores, like address and opening times.
    - **Tool:** `file_search`
    - Only return information found in the vector store or uploaded files.
    - If the information is ambiguous or not found, ask the user for clarification.

## Response
You will interact with users primarily through voice, so your responses should be natural, short and conversational. 
1. **Only use plain text**
2. No emoticons, No markup, No markdown, No html, only plain text.
3. Use short and conversational language.

When customers ask about how much pizza they need for a group, use the pizza calculator function to provide helpful recommendations based on the number of people and their appetite level.
```


## Modifying the Agent Code  

Now, update your `agent.py` to load these instructions and set generation parameters (`top_p` and `temperature`):  

```python
agent = project_client.agents.create_agent(
    model="gpt-4o",
    name="pizza-bot",
    instructions=open("instructions.txt").read(),
    top_p=0.7,
    temperature=0.7,
)
print(f"Created agent with system prompt, ID: {agent.id}")
```

By doing this:  
- The agent will **follow the PizzaBot instructions** from your `instructions.txt`.  
- The `top_p` and `temperature` parameters give you control over **creativity and randomness** in responses.  


## Run the Agent  

Try out the Agent:  

```shell
python agent.py
```

Try modifying your `instructions.txt` and rerun the agent. You’ll see how the system instructions directly influence the personality and behavior of the agent.  

You can now chat with your agent directly in the terminal. Type `exit` or `quit` to stop the conversation.  




## Recap  

In this chapter, you have:  

- Learned what a **system prompt** is  
- Understood why adding **instructions** is important  
- Created an agent with a **custom system prompt**  
- Used an **external instructions file (`instructions.txt`)**  
- Experimented with **generation settings** (`top_p` and `temperature`)  