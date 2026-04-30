import requests
import time
import random

server = "http://192.168.56.101:8080"

while True:
  try:
    r = requests.get(server, timeout=5)
    print(f"Beacon sent: {r.status_code}")
  except Exception as e:
    print(f"Error: {e}")
  time.sleep(random.randint(8,15))
