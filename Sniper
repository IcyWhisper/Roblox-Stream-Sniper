import discord
import requests
import time
import json
import threading

class gen():
    def __init__(self, link, ping, players, max, thumb, name, time):
        self.link = link
        self.ping = ping
        self.players = players
        self.max = max
        self.thumb = thumb
        self.name = name
        self.time = time

    link = ""
    ping = ""
    players = ""
    max = ""
    thumb = ""
    name = ""
    time = ""
    
def sniper(g_id, u_id):
    game_id = g_id

    user_thumb = requests.get(f'https://thumbnails.roblox.com/v1/users/avatar-headshot?userIds={u_id}&size=150x150&format=Png&isCircular=false').json()['data'][0]['imageUrl']

    servers = []

    cursor = ''
    while cursor != None:
        raw_data = requests.get(f"https://games.roblox.com/v1/games/{game_id}/servers/Public?limit=100&cursor={cursor}").json()
        cursor = raw_data['nextPageCursor']
        for i in raw_data['data']:
            servers.append(i)

    def searcher(server):
        print(f"Searching {server['id']} - {t.name}")
        for player in server['playerTokens']:
            data = []
            params = {
                'format': "png",
                'requestId': f"0:{player}:AvatarHeadshot:150x150:png:regular",
                'size': "150x150",
                'targetId': 0,
                'token': player,
                'type': "AvatarHeadShot",
            }
            data.append(params)
            thumb_data = \
            requests.post("https://thumbnails.roblox.com/v1/batch", headers={"Content-Type": "application/json"},
                          data=json.dumps(data)).json()['data'][0]['imageUrl']
            if thumb_data == user_thumb:
                gen.ping = server['ping']
                gen.players = server['playing']
                gen.max = server['maxPlayers']
                gen.link = f"""Roblox.GameLauncher.joinGameInstance({game_id}, '{server['id']}')"""
                thumb_url = requests.get(f"https://thumbnails.roblox.com/v1/places/gameicons?placeIds={game_id}&returnPolicy=PlaceHolder&size=256x256&format=Png&isCircular=false").json()['data'][0]['imageUrl']
                name = requests.get(f"https://users.roblox.com/v1/users/{u_id}").json()['name']
                gen.thumb = thumb_url
                gen.name = name
                break

            time.sleep(.4)

    threads = []

    st = time.time()
    for s in servers:
        t = threading.Thread(target=searcher, args=(s,))
        t.start()
        threads.append(t)
        time.sleep(.5)

    for t in threads:
        t.join()
    e = time.time()
    gen.time = e - st
