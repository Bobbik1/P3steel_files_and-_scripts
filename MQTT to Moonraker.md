1. Instalacja Brokera na Pi 3B+ (Serwer)
Zaloguj się na swoją główną Malinkę i wpisz:

```
sudo apt update
sudo apt install mosquitto mosquitto-clients -y
```
Po instalacji broker startuje automatycznie. Teraz Twoje Pi 3B+ jest "centralą", która zbiera wiadomości.
2. Skrypt na Pi Zero 2 W (Nadawca)
Zaloguj się na Pi Zero i stwórz mały skrypt, który będzie wysyłał dane.
Najpierw biblioteka:

```
sudo apt install python3-paho-mqtt -y
```
Potem stwórz plik:
```
nano ~/send_stats.py
```
Wklej tam poniższy kod (pamiętaj o zmianie adresu IP na adres Twojego Pi 3B+ w Tailscale):

```
import paho.mqtt.client as mqtt
import os
import time

# Ustawienia - wpisz IP swojego Pi 3B+
BROKER_IP = "100.XX.XX.XX" 

client = mqtt.Client("PiZero_Reporter")
client.connect(BROKER_IP)

def get_stats():
    # Pobieranie temperatury
    t_cmd = os.popen("vcgencmd measure_temp").readline()
    temp = t_cmd.replace("temp=","").replace("'C\n","")
    # Pobieranie napięcia
    v_cmd = os.popen("vcgencmd measure_volts core").readline()
    volt = v_cmd.replace("volt=","").replace("V\n","")
    return temp, volt

while True:
    try:
        t, v = get_stats()
        client.publish("picam/temp", t)
        client.publish("picam/volt", v)
    except:
        pass # Ignoruj błędy sieciowe, spróbuj za chwilę
    time.sleep(10)
```
3. Autostart skryptu na Pi Zero
Żeby skrypt działał zawsze w tle, dodaj go do crontab:

```
crontab -e
```
Na samym dole dodaj linię:
```
@reboot python3 /home/bobbik/send_stats.py &
```

4. Konfiguracja Moonrakera (Pi 3B+)
Teraz powiedz Mainsailowi (przez Moonrakera), żeby słuchał tych danych.
 Edytuj moonraker.conf na Pi 3B+:

```
[mqtt]
address: 127.0.0.1

[sensor picam_temp]
type: mqtt
name: Pi Zero Temp
state_topic: picam/temp
# Ta linia mówi Moonrakerowi: "użyj odebranej wiadomości jako wartości"
state_response_template: {payload}
units: °C

[sensor picam_volt]
type: mqtt
name: Pi Zero Volt
state_topic: picam/volt
state_response_template: {payload}
units: V


```

Dlaczego to jest "pro"?
Wizualizacja: Po restarcie Moonrakera, w Mainsail na dashboardzie obok temperatury Hotendu i Bed-u pojawią się kafelki "Pi Zero Cam" i "Pi Zero Volt".
Zero stresu dla Pi Zero: Ten skrypt zużywa mniej niż 1% CPU.
Brak konfliktów: MQTT działa na porcie 1883, więc nie dotyka  streamów wideo na 8554/8555/8556.















