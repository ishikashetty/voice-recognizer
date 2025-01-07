# voice-recognizer
import speech_recognition as sr
import pyttsx3

# Initialize text-to-speech engine
engine = pyttsx3.init()
engine.setProperty('rate', 150)  # Speed of speech
engine.setProperty('volume', 1)  # Volume level

# Function to convert text to speech
def speak(text):
    engine.say(text)
    engine.runAndWait()

# Function to recognize speech from microphone
def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    
    try:
        command = recognizer.recognize_google(audio)
        print(f"You said: {command}")
        return command.lower()
    except sr.UnknownValueError:
        speak("Sorry, I couldn't understand that.")
        return ""
    except sr.RequestError:
        speak("There seems to be an issue with the service.")
        return ""

# Function to calculate BMI
def calculate_bmi(weight, height):
    bmi = weight / (height ** 2)
    return round(bmi, 2)

# Main assistant loop
def assistant():
    speak("Hello, I am your voice assistant. I can help you calculate your BMI. Please tell me your weight in kilograms.")
    try:
        weight = float(listen())
        speak("Now, please tell me your height in meters.")
        height = float(listen())
        bmi = calculate_bmi(weight, height)
        speak(f"Your BMI is {bmi}")
        print(f"Your BMI is {bmi}")
    except ValueError:
        speak("I couldn't understand the values you provided. Please try again.")

if __name__ == "__main__":
    assistant()
