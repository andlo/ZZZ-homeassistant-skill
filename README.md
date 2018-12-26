# Home Assistant Skill for Mycroft

## THIS IS A COPY OF ORIGINAL WORK FROM @btotharye

## About 
Home Assistant is a open source project that lets you control all your smart devices in a single easy to use UI.  This skill talks to Home Assistant's REST API and controls devices and entities you have configured.  Control your lights, garage door, thermostats and more.

## Examples 
* "Hey Mycroft, turn on office light"
* "Hey Mycroft, turn off bedroom lights"
* "Hey Mycroft, turn on the AC (AC is a switch object in Home Assistant)"

based off the original code from https://github.com/BongoEADGC6/mycroft-home-assistant, spun off my own version since they seem to be inactive.

See https://geekedoutsolutions.com/using-home-assistant-with-mycroft-ai-for-home-automation for a general overview of Mycroft AI and Home Assistant

# Testing Against Demo Home Assistant
You can use the following information to test against a hosted demo of home assistant:
```
host: hademo.btotharye.com
token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJiNzdhOWNlMmZkNzU0MWM1OTZkMWQ5ZTZlMWVkNzdmZSIsImlhdCI6MTUzOTQ0NjM1MywiZXhwIjoxODU0ODA2MzUzfQ.ILrZhuw0vsryPcj0pK8PlSVtRTF_u6VEfdA_j7PlAG8
```
Put the above in the home.mycroft.ai site for the skill after installing it and you can now test against a demo home instance.  You can see the UI page by going to http://hademo.btotharye.com:8123 with user `demo` and pw `demo` make sure the SSL options are unchecked.

# Updates
Now confirmed working for mycroft 18.8.1. 

Now using the new Long-Lived Access Tokens instead of legacy api password. 

Make sure you have your home assistant settings filled out on home.mycroft.ai.

This is a skill to add [Home Assistant](https://home-assistant.io) support to
[Mycroft](https://mycroft.ai). Currently is supports turning on and off several
entity types (`light`, `switch`, `scene`, `climate`, `groups` and `input_boolean`).

## Support/Help
You can always find me in the mycroft mattermost channel here, https://chat.mycroft.ai

If you want to join my official discord where I will have a chatbot helper and more you can join via https://discord.gg/Xnn89dB

If you like this and want to say thanks check out my [Patreon](https://www.patreon.com/Geekedoutsol) I also include special posts here for my Patrons.

## Donate

If you liked this project, you can donate to support it and new features and also help me justify spending more time on this project.

You can donate directly via my [Blog](https://geekedoutsolutions.com/donations/geeked-out-donation) or any of the other options listed below.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=brianhh1230%40gmail%2ecom&lc=US&item_name=Geeked%20Out%20Solutions&no_note=0&cn=Add%20special%20instructions%20to%20the%20seller%3a&no_shipping=1&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted)

Or become a [Patreon](https://www.patreon.com/Geekedoutsol) - I offer special posts and tutorials to my Patreons that aren't public.

## Installation
Should be able to install this now via just saying `Hey Mycroft, install skill home assistant` it will then confirm if you want to install it and say yes and you are good to go.

Can also be installed via `msm install https://github.com/btotharye/mycroft-homeassistant.git`

## Configuration
This skill utilizes the skillsettings.json file which allows you to configure this skill via home.mycroft.ai after a few minutes of having the skill installed you should see something like below in the https://home.mycroft.ai/#/skill location:

Fill this out with your appropriate home assistant information and hit save.

You create the Long-Lived Access Token on the user profile page

![Screenshot](screenshot.JPG?raw=true)

###  Enabling using the conversation component as Fallback

Home-Assistant [supports basic speech based communication](https://www.home-assistant.io/components/conversation/).
When enabling the setting `Enable conversation component as fallback` on home.mycroft.ai, sentences that were not parsed
by any skill before (based on matching keywords) will be passed to this conversation component at the local Home-Assistant server.
Like this, Mycroft will answer default and custom sentences specified in Home-Assistant.

## Usage

Say something like "Hey Mycroft, turn on living room lights". Currently available commands
are "turn on" and "turn off". Matching to Home Assistant entity names is done by scanning
the HA API and looking for the closest matching friendly name. The matching is fuzzy (thanks
to the `fuzzywuzzy` module) so it should find the right entity most of the time, even if Mycroft
didn't quite get what you said.  I have further expanded this to also look at groups as well as lights.  This way if you say turn on the office light, it will do the group and not just 1 light, this can easily be modified to your preference by just removing group's from the fuzzy logic in the code.


Example Code:
So in the code in this section you can just remove group, etc to your liking for the lighting.  I will eventually set this up as variables you set in your config file soon.

```
def handle_lighting_intent(self, message):
        entity = message.data["Entity"]
        action = message.data["Action"]
        LOGGER.debug("Entity: %s" % entity)
        LOGGER.debug("Action: %s" % action)
        ha_entity = self.ha.find_entity(entity, ['group','light', 'switch', 'scene', 'input_boolean'])
        if ha_entity is None:
            #self.speak("Sorry, I can't find the Home Assistant entity %s" % entity)
            self.speak_dialog('homeassistant.device.unknown', data={"dev_name": ha_entity['dev_name']})
            return
        ha_data = {'entity_id': ha_entity['id']}
```

## TODO
 * Script intents processing
 * New intent for opening/closing cover entities
 * New intent for locking/unlocking lock entities (with added security?)
 * New intent for thermostat values, raising, etc.
 * New intent to handle multimedia/kodi

## In Development
* Increasing and Decreasing Climate controls

## Contributing

All contributions welcome:

 * Fork
 * Write code
 * Submit merge request

## Credits 
@btotharye

## Category
**IoT**

## Tags
#homeautomation
#iot
#homeassistant
#smarthome
#hue
#smartbulb
#light
#lighting
#lights
#nest
#temperature
#thermostat
#lifx

## Licence

See [`LICENCE`](https://gitlab.com/robconnolly/mycroft-home-assistant/blob/master/LICENSE).
