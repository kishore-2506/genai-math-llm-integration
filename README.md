# Integration of Mathematical Calculations with a Chat Completion System using LLM Function-Calling

## AIM:
To design and implement a Python function for calculating the volume of a cylinder, and integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

## PROBLEM STATEMENT:
Large Language Models (LLMs) such as GPT are capable of understanding and generating natural language but may not always be reliable when it comes to performing precise mathematical calculations. To ensure accuracy, we integrate external Python functions with the LLM using its function-calling API.  

In this experiment, we implement a function to calculate the **volume of a cylinder** and connect it with the chat completion system, enabling natural language queries like *"What is the volume of a cylinder with radius 6 and height 12?"* to return both natural language answers and exact computed results.

---

## DESIGN STEPS:

### STEP 1:
Import necessary libraries (`openai`, `dotenv`, `json`, `math`) and set up the API key environment.

### STEP 2:
Define a Python function `cylinder_volume(radius, height)` to compute the volume of a cylinder using the formula:
\[
V = \pi r^2 h
\]

### STEP 3:
Register this function schema in the `functions` parameter of the chat completion API.

### STEP 4:
Send a natural language query to the model, allow it to call the function, capture arguments, compute the result, and return the final response.

---
### PROGRAM:
```
import json
import math

def calculate_cylinder_volume(radius, height):
    """Calculate the volume of a cylinder given radius and height."""
    volume = math.pi * (radius ** 2) * height
    return json.dumps({
        "radius": radius,
        "height": height,
        "volume": volume
    })

functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "The radius of the cylinder (in units)"
                },
                "height": {
                    "type": "number",
                    "description": "The height of the cylinder (in units)"
                },
            },
            "required": ["radius", "height"],
        },
    }
]

messages = [
    {
        "role": "user",
        "content": "What is the volume of a cylinder with radius 5 and height 10?"
    }
]

import openai

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",   # or "gpt-4o-mini"
    messages=messages,
    functions=functions
)

args = json.loads(response["choices"][0]["message"]["function_call"]["arguments"])
observation = calculate_cylinder_volume(**args)

messages.append({
    "role": "function",
    "name": "calculate_cylinder_volume",
    "content": observation,
})

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)
print(response["choices"][0]["message"]["content"])
```
### OUTPUT:

<img width="1207" height="32" alt="image" src="https://github.com/user-attachments/assets/3f3d5747-4488-492b-a776-87572043fdd6" />

### RESULT:

Thus, a Python function to calculate the volume of a cylinder was successfully integrated with a Chat Completion system using the LLM function-calling feature. The system can now process natural language queries and provide mathematically precise results with human-like responses.
