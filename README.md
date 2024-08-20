
# LangChain OpenAI Integration Project

This repository demonstrates the integration of OpenAI's GPT-3.5-turbo model with LangChain for various natural language processing tasks. The project showcases multiple features, including handling multiple queries, utilizing conversational memory, implementing summaries as memory, and using entity-based memory to manage context throughout a conversation.

## Project Overview

The project consists of the following key functionalities:

1. **Multiple Queries:** The script sends multiple queries to the model and processes responses in sequence.
2. **Conversational Memory:** Utilizes conversational memory to remember previous interactions and provide coherent responses across multiple queries.
3. **Summary-Based Memory:** Implements memory that summarizes previous interactions to maintain a concise and relevant conversation history.
4. **Entity-Based Memory:** Uses entity-based memory to remember specific entities mentioned during the conversation, helping the model retain context about those entities.

## Prerequisites

Before running the scripts, ensure that you have the following installed:

- **Python 3.x**
- **Required Python Packages:** Install the necessary packages using pip:
  ```bash
  pip install langchain_openai python-dotenv
  ```

## Setting Up the Environment

### 1. Create a `.env` File

To securely manage your API keys, create a `.env` file in the root directory of this project. This file should contain the following environment variables:

```plaintext
OPENAI_API_KEY=your_openai_api_key_here
```

Replace `your_openai_api_key_here` with your actual OpenAI API key. This key is necessary for authenticating your requests to the OpenAI API.

### 2. Ensure `.env` is Not Included in Version Control

To prevent accidental exposure of your API keys, make sure that the `.env` file is listed in your `.gitignore` file:

```plaintext
.env
```

## How to Run the Scripts

### 1. Multiple Queries

The script sends multiple queries to the model to obtain responses. Here's an example:

```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()

# Set model name and API key
OPENAI_MODEL = "gpt-3.5-turbo"
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# Initialize the model
llm = ChatOpenAI(openai_api_key=OPENAI_API_KEY, model_name=OPENAI_MODEL, temperature=0.3)

# Send multiple queries
query = "What are two common breeds of cats?"
result = llm.invoke(query)
print(result.content)

query = "Which of those shed the least?"
result = llm.invoke(query)
print(result.content)
```

### 2. Conversational Memory

The script leverages a conversational memory to maintain context across multiple queries:

```python
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

# Initialize the model and conversational memory
llm = ChatOpenAI(openai_api_key=OPENAI_API_KEY, model_name=OPENAI_MODEL, temperature=0.3)
buffer = ConversationBufferMemory()
conversation = ConversationChain(llm=llm, verbose=True, memory=buffer)

# Start a conversation
query = "What are two common breeds of cats?"
result = conversation.predict(input=query)
print(result)

query = "Which of those shed the least?"
result = conversation.predict(input=query)
print(result)
```

### 3. Summary-Based Memory

The script demonstrates using summary-based memory to maintain a concise conversation history:

```python
from langchain.chains import ConversationChain
from langchain.memory import ConversationSummaryMemory

# Initialize the models for summary and conversation
summary_llm = ChatOpenAI(openai_api_key=OPENAI_API_KEY, model_name=OPENAI_MODEL, temperature=0.0)
chat_llm = ChatOpenAI(openai_api_key=OPENAI_API_KEY, model_name=OPENAI_MODEL, temperature=0.7)
buffer = ConversationSummaryMemory(llm=summary_llm)
conversation = ConversationChain(llm=chat_llm, memory=buffer, verbose=True)

# Start a conversation with summaries
query = "What are two common breeds of cats?"
result = conversation.predict(input=query)
print(result)

query = "Which of those shed the least?"
result = conversation.predict(input=query)
print(result)

query = "Which one tends to weigh more?"
result = conversation.predict(input=query)
print(result)
```

### 4. Entity-Based Memory

The script demonstrates using entity-based memory to manage context related to specific entities:

```python
from langchain.chains import ConversationChain
from langchain.memory import ConversationEntityMemory

# Initialize the models and memory for entities
entity_llm = ChatOpenAI(openai_api_key=OPENAI_API_KEY, model_name=OPENAI_MODEL, temperature=0.0)
chat_llm = ChatOpenAI(openai_api_key=OPENAI_API_KEY, model_name=OPENAI_MODEL, temperature=0.7)
buffer = ConversationEntityMemory(llm=entity_llm)
conversation = ConversationChain(llm=chat_llm, memory=buffer, verbose=True)

# Start a conversation with entity-based memory
query = "There are two cats up for adoption: Godric is a Maine Coon, and Luna is a Bombay."
result = conversation.predict(input=query)
print(result)

query = "Which of those cats probably has shorter fur?"
result = conversation.predict(input=query)
print(result)

query = "Which cat probably has a calmer temperament?"
result = conversation.predict(input=query)
print(result)
```

## Suggested Improvements

- **Enhanced Error Handling:** Consider adding more robust error handling to manage API errors or timeouts more effectively.
- **User Interface:** Implement a simple user interface (e.g., using Gradio or Streamlit) to allow non-technical users to interact with the project easily.
- **Additional Memory Types:** Explore other types of memory models or configurations to further enhance the conversational experience.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```
