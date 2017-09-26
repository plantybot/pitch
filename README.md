## Plantybot

An Urbanfarming bot that you can chat with. Make fresh, organic produces for your family, right in your home. Let plantybot take care of maintaining your farm.

### Why Plantybot?

Plantybot brings a maintainable aquaponics mini farm into your house and you can monitor the plant growth by chatting with it from FB messenger.

### What's aquaponics?

Aquaponics is a farming technique which contains fishes in freshwater and soil-less irrigation bed, which combines into a self sustaining ecosystem. The water from the fish tank is pumped into the irrigation bed periodically, which is drained back into the fish tank. This keeps the water pure, oxygenated, and nutrient rich for the plants to absorb.

<iframe src="https://player.vimeo.com/video/141252002" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/141252002">Open Source Aquaponic Greenhouse</a> from <a href="https://vimeo.com/opensourceecology">Open Source Ecology</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

### How is it implemented?

We are making some additions to the basic aquaponics framework, like:

```markdown

1. Adding artificial lights (LED strips)
2. Sensors drive the system. 
Temperature, pH, water level, 
lighting on the irrigation bed etc are monitored
3. Based on the sensing info, 
actuators irrigate the bed, 
or the fishes are fed
4. The information collected are periodically 
updated to a server through a Web-API
5. A chatbot service (messenger based) is connected 
to this API and it is used to notify the user 
about the health and productivity of your farm
6. Other notifications like when the 
fish food storage has to be refilled, 
health of fish tank, 
emergency alerts etc are also sent
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

### In depth Information for Techies

#### software architecture - Backend

![software](https://c1.staticflickr.com/5/4004/4653088356_194d56a59a_b.jpg)

Python Flask is a great microwebframework that makes it easy to implement [webhooks](https://webhooks.pbworks.com/w/page/13385124/FrontPage) and a [RESTful web API](https://blog.miguelgrinberg.com/post/designing-a-restful-api-with-python-and-flask)

The webhooks are used by the hardware for updating the server with its observations (like temp, humidity etc). 

The Web API is used by the Chatbot service to interact with the database and fetch latest information about someone's farm

#### Hosting

[AWS](https://console.aws.amazon.com) is used for hosting.

We can use AWS Lambda which is a serverless architecture for hosting the flask app. Ref: https://github.com/Miserlou/flask-zappa

[AWS RDS](https://aws.amazon.com/rds/) can be used to host a POSTGRE-SQL database, which works easily with flask.
(We need to design the database tables and figure what are the information we will be saving on to it)

#### AI

We need to create intents (stories) in Wit.ai (or LEX) for each of the intents we come up with. Then we need to connect that to the flask app. 

A [tutorial](https://github.com/joeycharlesworth/Autonomyx-bot/wiki/Wit.Ai-Tutorital) on Wit.ai

#### Microsoft Bot Framework

We have to write POST/GET endpoints in flask for botframework and the app to be connected. Here is something I worked on previously, (its easier to explain with my code) https://github.com/joeycharlesworth/Autonomyx-bot/blob/master/app.py

### Hardware prototyping - Raspberry Pi

![RPi](https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Raspberry_Pi_B%2B_rev_1.2.svg/2000px-Raspberry_Pi_B%2B_rev_1.2.svg.png)

On Raspberry pi side, we have four main parts:

1. Sensing
2. Connecting and updating server
3. The default irrigation routine (which can be interrupted by the user through the chatbot side)
4. Actuators

#### Sensing

We can start with light over agri-bed, fish-tank water-level and fish-tank water temperature sensing

(need updates here, fork and create pull requests)

#### Connecting and updating server

RPi, has to collect data, use the Wifi module to connect to the WebHooks and update them regularly (say once every 10mins)


(need updates here, fork and create pull requests)

#### Default farming algorithm

If unsupervised by the user, the bot should be able to take care of the farming bed with good default algo. So it should be able to irrigate the farm periodically and feed the fish in appropriate intervals. The user should however be given control over changing the frequency of irrigation/amt of water supplied, force feed fishes too

(need updates here, fork and create pull requests)

#### actuators

1. A pump is used along with a motor control module (L298N) to start and stop pump at appropriate times.
2. We need a mechanism to feed small amounts of food to fishes from a storage container 
(need updates here, fork and create pull requests)

### Conclusion

I have put kinda all the info I have about the solution. Please fork the repo, create pull requests, or you can even directly edit this README.md to update info. A pull request helps in all of us reviewing a change, which I feel might help improve the doc/site.

Also for any updates create an issue [here](https://github.com/plantybot/pitch/issues). Create issues for TODOs also, like I am creating an issue for us having a pair programming session where we can explore how to create a flask based messenger bot (a sample bot)
