# Personalised-chatbot
This document explains the technical details of your chatbot project.

Data Used
We used the DailyDialog dataset. This dataset contains multi-turn conversations.

For each turn in a conversation, the dataset provides:

Dialog: The actual text spoken.

Emotion: A label for the emotion expressed (e.g., happiness, anger).

Act (Intent): A label indicating the communication goal (e.g., question, inform).

How the data was used: The dialogue turns from this dataset are used to prepare examples for fine-tuning the generative model. Information about emotions and intents is conceptually used to instruct the model on how to respond given certain user input and desired persona.

Model Used
The core of the chatbot is a large language model from the Mistral family.
Specifically, we are using:

Base Model: unsloth/Mistral-7B-Instruct-v0.2-bnb-4bit

Instead of fine-tuning the entire large model, which is very resource-intensive, we used LoRA adapters. This technique allows for efficient fine-tuning by only training a small number of new parameters (the adapters) on top of the pre-trained base model. This saves a lot of time and computational resources.

The model generates responses based on the chat history, a selected persona, and the detected mood of the user's last message.

Important: This model will not run without a GPU (Graphics Processing Unit). It requires dedicated graphics memory (VRAM) to load and operate.

Libraries Used
Here are the main Python libraries used in this project:

unsloth: A library that makes it much faster and more memory-efficient to fine-tune and run large language models, especially on consumer GPUs. It's crucial for loading the Mistral model in 4-bit quantized format.

transformers: A foundational library from Hugging Face for working with pre-trained models. unsloth builds on this. We use it for the tokenizer and for model generation.

torch: The PyTorch deep learning framework. unsloth and transformers rely on PyTorch for model computations.

os: A standard Python library for interacting with the operating system, used here to check if the LoRA adapter files exist.

Steps to Run the Project (in Google Colab)
To get your chatbot running with the Mistral model, follow these steps in a Google Colab notebook:

Open a New Colab Notebook: Start a fresh Colab session.

Set GPU Runtime:

Go to Runtime in the Colab menu.

Select Change runtime type.

Under Hardware accelerator, choose GPU. This is mandatory for the model to run. Make sure it's a T4 or better if possible.

Click Save.

Install Necessary Libraries:
Run this command in a Colab cell to install all required libraries:

!pip install unsloth[cu121] transformers torch peft trl accelerate bitsandbytes



Paste the Chatbot Code:
Copy the entire Python code from the "Persona Chatbot (Unified App with Mistral-7B-Instruct-v0.2 & Device Mapping)" Canvas into a new cell in your Colab notebook.

Update LoRA Adapter Path:
In the pasted Python code, find the line LORA_ADAPTER_PATH = "/content/drive/MyDrive/fine_tuned_model".


Run the Code Cell:
Run the cell containing the chatbot code. This will load the model and start the Streamlit application. Colab will provide a link to the Streamlit app after it starts.

Access the Chatbot:
After the cell runs, Colab will provide a public URL (typically via localtunnel if Streamlit is used directly in Colab). Click this URL to open your chatbot interface in a new browser tab.
