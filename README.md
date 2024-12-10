# Implementation-of-Chatbot-using-NLP

import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import WordNetLemmatizer

nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')

def preprocess_text(text):
    tokens = word_tokenize(text.lower())
    stop_words = set(stopwords.words("english"))
    lemmatizer = WordNetLemmatizer()
    filtered_tokens = [lemmatizer.lemmatize(word) for word in tokens if word not in stop_words]
    return filtered_tokens

user_input = "Hello, how can you help me today?"
processed_input = preprocess_text(user_input)
print(processed_input)

intents = {
    "greeting": ["Hello! How can I assist you today?", "Hi there!"],
    "goodbye": ["Goodbye! Have a nice day.", "Bye! Take care!"],
    "help": ["I'm here to help! What do you need assistance with?"],
    "default": ["I'm sorry, I didn't understand that. Can you rephrase?"]
}

def identify_intent(user_input):
    keywords = {
        "greeting": ["hello", "hi", "hey"],
        "goodbye": ["bye", "goodbye", "exit"],
        "help": ["help", "assist", "support"]
    }
    for intent, words in keywords.items():
        if any(word in user_input for word in words):
            return intent
    return "default"

intent = identify_intent(processed_input)
print(intent)

def generate_response(intent):
    return intents.get(intent, intents["default"])[0]

response = generate_response(intent)
print(response)

def chatbot():
    print("Chatbot: Hi there! Type 'exit' to end the conversation.")
    while True:
        user_input = input("You: ")
        if user_input.lower() in ["exit", "quit", "bye"]:
            print("Chatbot: Goodbye!")
            break
        processed_input = preprocess_text(user_input)
        intent = identify_intent(processed_input)
        response = generate_response(intent)
        print(f"Chatbot: {response}")

chatbot()
