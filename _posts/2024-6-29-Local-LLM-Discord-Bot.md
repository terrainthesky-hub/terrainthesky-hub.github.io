So I decided to make a discord bot that uses a locally running Large Language Model. In this case, I used gemma2:9b, a lightweight, low parameter LLM that can be
run with limited VRAM (llama70b needs at least 120 GB of VRAM to run at a reasonable speed). I installed it using [Ollama](https://ollama.com/). Once you've downloaded and installed Ollama,
pull the LLM model you want in your terminal with a similar command.

    ollama pull gemma2:9b

After, type this to run it locally.

    ollama run gemma2

Now you can run the model locally and send it messages:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/22f44025-6a81-4213-bb84-d45560706bd2)

Once you've done that it's time to set up Discord. You're going to want to go to [Discord Applications and log in](https://discord.com/developers/applications).
Once there, select "New Application" button.

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/54a7f256-278d-4a0f-a14e-f0e727fe4b45)


Click on bot on the left tab and set up the following options. Name your bot, mine is LocalGemma2Bot. 

Turn on Oauth2 Code Grant

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/96ecda14-dc27-48b9-a878-58f861e9d83a)

Then set Message Content Intent to True:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/9a65a6b1-f708-4ec7-84de-05837a644c11)


Now go to OAuth2 on the left tab. And under URL Generator Select "bot".

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/18b0beba-bea3-4cb1-8420-464e1b7f2240)


Under Bot Permissions select Send Messages and Read Message History.

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/07fe2f32-78c0-42d2-bc70-af0f84138e52)



Copy the generated URL at the bottom and go to that webpage. Now you can add the bot to a server:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/1199fde3-3750-4d64-9c0d-d1ba29f1292e)

You'll want to take note of the bot token on the bot page on from the left tab to save for later. Now you're ready to write the Python code. Create a bot.py file, and here is code for the defaulted Ollama gemma2 settings:

    import os
    from dotenv import load_dotenv
    import json
    import discord
    import requests
    
    load_dotenv()
    
    intents = discord.Intents.default()
    intents.message_content = True
    client = discord.Client(intents=intents)
    
    DISCORD_TOKEN = os.getenv('DISCORD_TOKEN')
    OLLAMA_API_URL = os.getenv('OLLAMA_API_URL')
    
    @client.event
    async def on_message(message):
        if message.author == client.user:
            return
    
        if message.content.startswith('!ask '):
            query = message.content[5:]
            
            payload = {
                'model': 'gemma2',
                'prompt': query
            }
            
            response = requests.post(OLLAMA_API_URL, json=payload)
            
            if response.status_code == 200:
                try:
                    json_objects = response.text.split('\n')
                    responses = []
                    for obj in json_objects:
                        if obj.strip():
                            parsed_obj = json.loads(obj)
                            responses.append(parsed_obj['response'])
                    
                    final_response = ''.join(responses)
                    await message.channel.send(final_response)
                except ValueError as e:
                    await message.channel.send("Sorry, I couldn't parse the response from the LLM.")
            else:
                await message.channel.send("Sorry, I couldn't get a response from the LLM.")
    
    client.run(DISCORD_TOKEN)

This script will send your discord bot's query to your local machine where it will return the local LLM's series of tokens, combine and clean those json token responses,
and send them to the channel.

For proper security, set up a .env file, and grab your discord bot token from the discord webpage under the bot tab and enter this into your .env:

      DISCORD_TOKEN=YOUR_DISCORD_TOKEN_HERE

Also add your Ollama_Api_Url variable to the .env file as well, but you'll want to generate your own port:

      OLLAMA_API_URL=http://localhost:YOUR_PORT_NUMBER_HERE/api/generate
      
Once you've created this .env file and bot.py file, create a pip environment and install the following dependencies.

      pip install requests
      pip install discord
      pip install python-dotenv

Now you're ready to run your bot from the terminal by running the bot.py Python script. Use !ask MESSAGE_HERE in discord to query the bot.

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/f413b6d2-994c-48f1-9167-72df26d460dc)




