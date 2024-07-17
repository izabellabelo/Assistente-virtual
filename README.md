# Assistente-virtual
assistente virtual chamada Mia que faz pesquisa pesquisa no Google, foi feita no VSCode, utilizando Python

import speech_recognition as sr
import webbrowser
import pyttsx3

def speak(text):
    engine = pyttsx3.init()
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Olá sou a Mia. Me diga algo...")
        audio = recognizer.listen(source)
    
    try:
        query = recognizer.recognize_google(audio, language='pt-BR')
        print(f"Você disse: {query}")
        return query
    except sr.UnknownValueError:
        print("Não entendi o que você disse.")
        return ""
    except sr.RequestError:
        print("Não foi possível obter resultados; erro de conexão.")
        return ""

def search_google(query):
    url = f"https://www.google.com/search?q={query}"
    webbrowser.open(url)

while True:
    query = listen().lower()

    if 'pesquisar' in query:
        speak("O que você gostaria de pesquisar?")
        search_query = listen()
        if search_query:
            search_google(search_query)
            speak(f"Aqui está o resultado da pesquisa para {search_query}.")
        else:
            speak("Desculpe, não entendi o que você queria pesquisar.")
    
    elif 'parar' in query or 'sair' in query:
        speak("Até mais!")
        break
