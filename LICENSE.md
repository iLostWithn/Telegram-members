from telethon.sync import TelegramClient
from telethon.tl.functions.channels import GetParticipantsRequest
from telethon.tl.types import ChannelParticipantsSearch

api_id = 123456  # غيّرها لـ API ID حقك
api_hash = 'your_api_hash'  # غيّرها لـ API HASH حقك
phone = '+9665xxxxxxxx'  # رقمك

client = TelegramClient('session_name', api_id, api_hash)
client.start(phone)

group_username = 'GroupUserNameHere'  # بدون @

group = client.get_entity(group_username)

all_participants = []
offset_user = 0
limit_user = 100

while True:
    participants = client(GetParticipantsRequest(
        group, ChannelParticipantsSearch(''), offset_user, limit_user, hash=0))
    if not participants.users:
        break
    all_participants.extend(participants.users)
    offset_user += len(participants.users)

for user in all_participants:
    if user.username:
        print('@' + user.username)
