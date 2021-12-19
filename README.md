# Eva-Ai

import speech_recognition as sr
from selenium import webdriver
from requests_html import HTMLSession
import playsound
import os
import random
import time 
from datetime import datetime
import pytz
from gtts import gTTS
from time import ctime, localtime
import webbrowser
import pywhatkit as kit
 

  
r = sr.Recognizer()


def speech_record(ask = False):
    with sr.Microphone() as micro:
        if ask:
            speak(ask)
        audio = r.listen(micro)
        audio_data=" " 
        try:
            audio_data = r.recognize_google(audio)
            speak(audio_data)
            print(audio_data)

        except sr.UnknownValueError:
            print("unknown audio")
            speak("unknown audio")
        
        except sr.RequestError:
            print("sorry, my speech serive is down")
            speak("sorry, my speech serive is down")

        return audio_data

def there_exists(terms):
    for term in terms:
        if term in data:
            return True
def respond(audio_data):
    #if "what is your name" in audio_data:
    if there_exists(["what is your name", "what's your name","tell me your name" ]):
        speak("hello my name is Eva ")

    if there_exists(["what are you"]):
        speak("i am a digital voice assistant that uses voice recognition, language processing algorithms, and voice synthesis to listen to specific voice commands and return relevant information or perform specific functions as requested by the user.")

    if there_exists(["who is your creator"]):
        creator = {"Anuj Mahto"}
        speak(creator)
    
    if there_exists(["what is the time", "what's the time","what time is it"]):
        tz_IN = pytz.timezone('Asia/Kolkata') 
        datetime_IN = datetime.now(tz_IN)
        time_IN = datetime_IN.strftime("%H:%M")
        speak(time_IN)
        speak("Done")

    if there_exists(["what's the time in America", "what's the time in America"]):
        tz_NY = pytz.timezone('America/New_York') 
        datetime_NY = datetime.now(tz_NY)
        time_NY = datetime_NY.strftime("%H:%M")
        speak("the time in America is " + time_NY)
        speak("Done")
        
    if there_exists(["what's the time in Europe", "what is the time in Europe"]):
        tz_EU = pytz.timezone("Europe/london")
        datetime_EU = datetime.now(tz_EU)
        time_EU = datetime_EU.strftime("%H:%M")
        speak("the time in europe is " + time_EU)
        speak("Done")
        
    if "search" in audio_data: 
        s = HTMLSession()
        record = speech_record("what do u want to search for??")
        url = "http://google.com/search?q=" + record
        r = s.get(url)
        webbrowser.get().open(url)
        print("here's what i found online for " + record)
        #inform = r.html.find("div.FLP8od" , first = True).text
        speak("here's what i found online for " + record )
        #speak(inform)
        speak("Done")

    if "location" in audio_data:
        location = speech_record("which area u want to search for??")
        url = "http://google.nl/maps/place/" + location + "/&amp; "
        webbrowser.get().open(url)
        #print("here's the " + location)
        speak("here's the " + location)
        speak("Done") 

    if "play" in audio_data:
        play = speech_record("what do u want to play")
        print(play)
        url = play
        kit.playonyt(url)

    if there_exists(["stock price" , "what's the stock price of"]):
        search_term = data.lower().split(" of ")[-1].strip() #strip removes whitespace after/before a term in string
        s = HTMLSession()

        url = f"https://www.google.com/search?q=stock+price+{search_term}"

        r = s.get(url)

        price = r.html.find("div.PZPZlf span.IsqQVc.NprOob.wT3VGc" , first = True).text
        unit = r.html.find("div.PZPZlf span.knFDje", first = True).text
        speak(f"the stock price of {search_term} is {price} {unit}")

            
    if there_exists(["weather", "temperature"]):
        s = HTMLSession()
        search = data.lower().split(" ")[-1].strip()
        url = f"https://www.google.com/search?q=weather+{search}"
        r = s.get(url)

        temp = r.html.find("span#wob_tm" , first= True).text
        print(temp)

        unit = r.html.find("div.vk_bk.wob-unit span.wob_t", first=True).text
        print(unit)

        forecast = r.html.find("div.VQF4g ", first=True).find("span#wob_dc" , first=True).text
        print(forecast)
        print(f"the temperature of {search} is {temp}{unit} and its going to be {forecast}")
        speak(f"the temperature of {search} is {temp}{unit} and its going to be {forecast}")

    if "exit" in audio_data:
        speak("exiting.....")
        exit()


def speak(audio_string):
    tts = gTTS(text=audio_string , lang= "en")
    rand = random.randint(1, 100000000000000)
    audio_file = "audio--" + str(rand) + ".mp3"
    tts.save(audio_file)
    playsound.playsound(audio_file)
    print(audio_string)
    os.remove(audio_file)



#time.sleep(0)

speak("how can i help you??")

while True:
   data = speech_record()
   print(data)
   respond(data)

