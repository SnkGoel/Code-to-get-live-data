import pandas as pd
from bs4 import BeautifulSoup as bs
import requests
import time
import random

# Open Telegram and search for the BotFather bot.

# Start a chat with the BotFather and type "/newbot" to create a new bot.

# Follow the prompts to choose a name and username for your bot. The username must end in "bot".

# Once you've created your bot, the BotFather will provide you with a token. Keep this token safe, as it will be used to authenticate your bot when you interact with the Telegram API.

bot_token = 'Telegram bot token'
chat_id = 'chat i.d.'

turl = f'https://api.telegram.org/bot{bot_token}/sendMessage'
murl = 'https://www.moneycontrol.com/news/business/economy/'
lastUpdate = ''

response = requests.get(murl)
html_c = response.content
soup = bs(html_c, 'html.parser')
script_tag = soup.find("ul",{"id":'cagetory'})
dic = []
for title in script_tag.find_all('h2'):
    message = title.get_text(strip=True)
    dic.append(message)
    if(len(lastUpdate)==0):
        lastUpdate = message
    
dic = dic[::-1]
for str in dic:
    print(str)
    params = {'chat_id': chat_id, 'text': str}
    response = requests.post(turl, json=params)
    print(response.content)
    time.sleep(2)
    
print(lastUpdate, "INITIAL UPDATES DONE")
while True:
    r1 = random.randint(10*60,20*60)
    response = requests.get(murl)
    html_c = response.content
    soup = bs(html_c, 'html.parser')
    script_tag = soup.find("ul",{"id":'cagetory'})
    message =  script_tag.find_all('h2')[0].get_text(strip=True)
    if(message!=lastUpdate):
        print(message)
        params = {'chat_id': chat_id, 'text': message}
        response = requests.post(turl, json=params)
        print(response.content)
    
    time.sleep(r1)
