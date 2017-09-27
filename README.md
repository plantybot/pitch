## Plantybot

An Urbanfarming bot that you can chat with. Make fresh, organic produces for your family, right in your home. Let plantybot take care of maintaining your farm.

### Why Plantybot?

Plantybot brings a maintainable aquaponics mini farm into your house and you can monitor the plant growth by chatting with it from FB messenger.

### What's aquaponics?

Aquaponics is a farming technique which contains fishes in freshwater and soil-less irrigation bed, which combines into a self sustaining ecosystem. The water from the fish tank is pumped into the irrigation bed periodically, which is drained back into the fish tank. This keeps the water pure, oxygenated, and nutrient rich for the plants to absorb.

<iframe src="https://player.vimeo.com/video/141252002" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/141252002">Open Source Aquaponic Greenhouse</a> from <a href="https://vimeo.com/opensourceecology">Open Source Ecology</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

### How is it implemented?

The main blocks to the implementation are:

1. The farming setup
2. The sensors and hardware setup
3. A Webservice (API)
4. Chatbot NLP engine
5. A chat client (FB Messenger)

These blocks are connected in such a way that a user could maintain their farm
by interacting with an AI chatbot, getting status updates of the farm and requesting
changes to farm parameters.

#### Farmbed setup
Aquaponics farming setup would contain:

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
![basic setup](https://upload.wikimedia.org/wikipedia/commons/b/b1/Systeme_FLOOD%26DRAIN_573px.jpg)
*A basic setup*

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

### Hardware prototyping - Raspberry Pi

![RPi](https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Raspberry_Pi_B%2B_rev_1.2.svg/2000px-Raspberry_Pi_B%2B_rev_1.2.svg.png)

We intend to develop a ROS based solution on Raspberry Pi for automating the desired, intended aquaponics system on our PlantyBot. The higher level architecture of the system is depicted in the following diagram.

![sysArch](images/highlevelarch.jpg?raw=true "System Architecture")

The state information of the system as read by relevant sensors equipped in the system is passed onto a Reasoner. The Reasoner periodically runs algorithms to take decisions on irrigation routines with the collected data. The Reasoner then connects to a Chatbot Application over the web. The Chatbot communicates directly with the User, thereby giving the status of the farm, taking instructions from the user and regularly initiating processes to perform health checkups on the system. The Reasoner along with the Chatbot application forms the core A.I. of the system. The A.I. is backed up by the sensor suite and the actuation system directly controlled through the Raspberry Pi microcomputer. This A.I. based control system represents the overall architecture of the PlantyBot.

On Raspberry pi side, we have four main parts:

1. Sensing
2. Connecting and updating server
3. The default irrigation routine (which can be interrupted by the user through the chatbot side)
4. Actuators

#### Sensors

1. __Light intensity Sensor__ to measure the amount of light over the minifarm
2. __Temperature sensor__ to measure the temperature of water in fish tank
3. __pH or Conductivity sensor__ to measure the pH levels of water
4. __water level sensor__ to measure the amount of water in the system
5. __fish feed sensor__ to measure the amount of fish food in the automatic fish feeder

#### Connecting and updating server

RPi, has to collect data, use the Wifi module to connect to the WebHooks of the plantybot webservice and update them regularly (say once every 10mins)

(need updates here, fork and create pull requests)

#### Default farming algorithm

If unsupervised by the user, the bot should be able to take care of the farming bed with good default algo. So it should be able to irrigate the farm periodically and feed the fish in appropriate intervals. The user should however be given control over changing the frequency of irrigation/amt of water supplied, force feed fishes too

(need updates here, fork and create pull requests)

#### Actuators

1. __Water pump__ to circulate water from fish tank to the irrigation bed
2. __Automatic fish feeder__ feeds fishes in fish tank
3. __LED Lamp__ is used to illuminate the minifarm
4. __temperature control__ maintaining ideal water temperature. A heater-cooler is used.

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

#### AI - NLP platform

We need to create and train intents in API.AI (or LEX) for each of the intents we come up with. Then we need to connect that to the flask app.

A [tutorial](https://github.com/joeycharlesworth/Autonomyx-bot/wiki/Wit.Ai-Tutorital) on Wit.ai

#### Microsoft Bot Framework

We have to write POST/GET endpoints in flask for botframework and the app to be connected. Here is something I worked on previously, (its easier to explain with my code) https://github.com/joeycharlesworth/Autonomyx-bot/blob/master/app.py

#### High level architecture of Chatbot

![chatbot_architecture](images/REST_API.png)

### Conclusion

I have put kinda all the info I have about the solution. Please fork the repo, create pull requests, or you can even directly edit this README.md to update info. A pull request helps in all of us reviewing a change, which I feel might help improve the doc/site.

Also for any updates create an issue [here](https://github.com/plantybot/pitch/issues). Create issues for TODOs also, like I am creating an issue for us having a pair programming session where we can explore how to create a flask based messenger bot (a sample bot)
