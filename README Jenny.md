from tkinter import *
import mysql.connector as msc
from tkinter import messagebox
from plyer import notification as noti
from PIL import Image, ImageTk
import datetime
import time
import threading

import pyttsx3
import speech_recognition as sr
import wikipedia
import webbrowser
import os
import smtplib
import requests
from bs4 import BeautifulSoup



converter = pyttsx3.init()
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[2].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishMe():
    hour = datetime.datetime.now().hour
    if 0 <= hour < 12:
        speak("Good Morning!")
    elif 12 <= hour < 17:
        speak("Good Afternoon!")
    else:
        speak("Good Evening!")
    speak("I am Jenny your Virtual Assistant sir. How could I help you")

def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)
    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")
    except Exception as e:
        print("Say that again please...")
        return "None"
    return query.lower()

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('justihateu@gmail.com', '*********')
    server.sendmail('youremail@gmail.com', to, content)
    server.close()
def get_weather(city):
    url = f'https://wttr.in/{city.replace("","+")}?format=3'
    response= requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    weather_info = soup.find('div', class_='weather__info')
    speak(f"The current weather in {city} is: {response.text}")

if __name__ == "__main__":
    wishMe()
    while True:
        query = takeCommand()

        if 'wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            speak(results)
        elif 'weather' in query:
            speak("Please tell me the city name")
            city = takeCommand()
            get_weather(city)
        elif 'open youtube' in query:
            webbrowser.open("https://www.youtube.com")
        elif 'ankit' in query:
            webbrowser.open("https://www.instagram.com/a.a.k.u__/")
        elif 'open google' in query:
            webbrowser.open("https://www.google.com")
        elif 'search vasu' in query:
            webbrowser.open("https://bio.site/ba7_it")
        elif 'basit hussain' in query:
            webbrowser.open("https://www.zoominfo.com/people/Basit/Hussain")
        elif 'my project' in query:
            speak('Core Purpose The main goal of your project is to create an interactive' \
            'application that responds to voice commands and executes various tasks, mimicking a personal assistant. ' \
            'Key Features. ' \
            'The project integrates several Python libraries to provide the following functionalities: ' \
            'Voice Interaction:  It uses pyttsx3 to speak responses back to the user. ' \
            'It uses speech_recognition  to listen to the users voice command (takeCommand). ' \
            'It greets the user appropriately based on the time of day. It can search Wikipedia for a given query and summarize the results. ' \
            'It can open specific websites in a web browser (YouTube, Google, bio.site/ba7_it, zoominfo.com/people/Basit/Hussain')
        elif 'play music' in query:
            music_dir = 'C:\\Users\\Basit\\Music'
            songs = os.listdir(music_dir)
            os.startfile(os.path.join(music_dir, songs[0]))
        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")
        elif 'email' in query:
            try:
                speak("What should I say?")
                content = takeCommand()
                to = "youremail@gmail.com"
                sendEmail(to, content)
                speak("Email has been sent!")
            except Exception as e:
                print(e)
                speak("Sorry my friend. I am not able to send this email")

a.mainloop()
