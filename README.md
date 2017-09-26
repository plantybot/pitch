## Plantybot

An Urbanfarming bot that you can chat with.

### Why Plantybot?

Plantybot brings a maintainable aquaponics mini farm into your house and you can monitor the plant growth by chatting with it from FB messenger.

### What's aquaponics?

Aquaponics is a farming technique which contains fishes in freshwater and soil-less irrigation bed, which combines into a self sustaining ecosystem. The water from the fish tank is pumped into the irrigation bed periodically, which is drained back into the fish tank. This keeps the water pure, oxygenated, and nutrient rich for the plants to absorb.

![asd](https://www.youtube.com/watch?v=gk6UTFQEOVU)

<iframe width="420" height="315"
src="https://www.youtube.com/watch?v=gk6UTFQEOVU">
</iframe> 

### How is it implemented?

We are making some additions to the basic aquaponics framework, like:

```markdown

1. Adding artificial lights (LED strips)
2. Sensors drive the system. Temperature, pH, water level, lighting on the irrigation bed etc are monitored
3. Based on the sensing info, actuators irrigate the bed, or the fishes are fed
4. The information collected are periodically updated to a server through a Web-API
5. A chatbot service (messenger based) is connected to this API and it is used to notify the user about the health and productivity of your farm
6. Other notifications like when the fish food storage has to be refilled, health of fish tank, emergency alerts etc are also sent
```

### Chatbot architecture


#### How to make a messenger bot?

![messenger-bot](https://blog.hartleybrody.com/wp-content/uploads/2016/06/facebook-chatbot.png)

Reference: 

1. https://blog.hartleybrody.com/fb-messenger-bot/
2. https://www.fullstackpython.com/blog/build-first-slack-bot-python.html (this one for slack, we can use Microsoft Bot Framework to make this for FB Messenger)
3. https://developers.facebook.com/docs/messenger-platform/getting-started/quick-start/ (FB's docs on making a messenger app)

#### How to make the bot AI?

One of the basic concept of making a chatbot into a AI chatbot, is making the bot understand user **intent**.

For example when the user starts a conversation with the bot by saying "Hi", the bot should understand that the intent is `greeting` and create a response like "Hey there {username}". Some common intents that make our bot sound human:

```markdown
1. greeting (e.g. user says "Good Morning")
2. help (e.g. user says "Can you help me?")
3. status (e.g. user says "how's it going?", "how are things in my farm?" etc)
4. customercare (e.g. user says "Can I talk to some support?")
5. name (e.g. user asks  plantybot for its name "what's your name?")
6. location (e.g. user asks plantybot "Where do you live?")

Please create pull requests and add more intents based on what you think the bot needs
```

There are many web services which serve as the bots AI engine. For example https://wit.ai is facebooks Chatbot AI engine, that is free even for commercial use. Other options are Lex (Amazons), IBM Watson, LUIS (microsofts).

My suggestion is to either go for Wit or Lex, as they are both showcasing awesome results.


