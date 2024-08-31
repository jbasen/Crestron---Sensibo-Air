# Crestron---Sensibo-Air

v1.6 - This version addresses an issue with the amount of messages
the module sends to the Sensibo API, which would go above the limitation
that Sensibo implemented to manage traffic to their API.  The issue is this. 
When you send a command to the Sensibo API based on a user selection the 
API doesn't respond back with information that tells you it has been accepted 
and implemented. So, to keep the outputs of the module in synch with user 
selections I was sending a refresh command right afterwards. 
This doubles the number of calls and if, for example, multiple commands are 
sent in quick progression based on a  user favorite setting, then this 
can cause a lot of commands to be sent in quick sucession which could 
easily run afoul of the API police trying to manage API traffic.  

I've added a new parameter named Manual_Refresh_Required.  When this is 
set to true a refresh command will not be sent after each other command.  
Instead the code will have to manage when to refresh the outputs of the module 
manually.

I also included some additional notes on how to implement the user's ability 
to change the target temperature.  If, for example, this is implemented by 
the user triggering the up/down inputs on an analog increment, then the output 
of the ainc should be run into an abuf with the enable on the abuf tied to a 
"Set" button on a tp.  The user will then be able to use the abuf to change 
the temperature and only after pressing the set button will this new value be 
sent to the module.  This, again, minimizes the number of API calls. 

A user who tested this simply used a oscillator to ping the refresh input
every 5 minutes to catch up the feedback on the module if a user controlled 
the Sensibo with some other method besides the Crestron system. Doing this with
the v1.6 module worked well for him.

v1.5 - Misc bug fixes and enhancements

v1.1 - corrects parsing problems with Fujitsu mini-split systems

This module provides control of a Sensbo Air from a Crestron 3-Series or 
4-Series Processor.  

The first step in using this module is to obtain an API Key.  To accomplish 
this you will need to use your username/password and login to the Sensibo 
web site.  There is a login link in the menu at the bottom of the main 
page of the web site.  You will then be presented with a screen that shows 
you your Sensibo devices.  In the upper left hand corner of the screen is 
a menu icon and selecting it will present you with an option to create an 
API Key.

Next you will need to obtain the ID of your Sensibo Air.  In the lower right
corner of the square that represents you Sensibo Air when you are logged in on
the Sensibo web site is a 3 dot menu icon.  Pressing this displays a drop down
menu.  Press the "Advanced" option on the menu and then choose "Advanced Info".
The UID is the device ID of your Sensibo Air.

Copy the API Key and the UID into the API_Key and Device_ID paramters of this
module. 

Inputs
Initialize                 - Must be pulsed to initilize communications
Refresh                    - Will referesh the status of feedback signals.
                             Shouldn't be needed unless someone controls
                             the Sensibo Air using the buttons on the
                             device or the app.
Device_Off                 - Pulse to turn the device controlled by the Sensibo Air
                             off
Mode_*                     - Sets the device controlled by the Sensibo Air to the
                             specified mode
Fan_Level_*                - Sets the device controlled by the Sensibo Air to the
                             specified fan level
Swing_*                    - Sets the device controlled by the Sensibo Air to the
                             specified swing
Horizontal_Swing_*         - Sets the device controlled by the Sensibo Air to the
                             specified horizontal swing
Light_*                    - Sets the device controlled by the Sensibo Air to the
                             specified light state
Climate_React_On/Off       - Turn on/off climate react mode
Target_Temperature         - Sets the target temperature of the device controlled
                             by the Sensibo Air

Outputs
Device_Off_FB              - Power status of the device controlled by the Sensibo Air
Mode_*_FB                  - Mode status of the device controlled by the Sensibo Air
Fan_Level_*_FB             - Fan level status of the device controlled by the Sensibo Air
Swing_*_FB                 - Swing status of the device controlled by the Sensibo Air
HOrizontal_Swing_*_FB      - Horizontal Swing status of the device controlled by the Sensibo Air
Light_*_FB                 - Light status of the device controlled by the Sensibo Air
Climate_React_*_FB         - Climate React status
Temperature_FB             - Current Room Temperature
Humidity_FB                - Current Room Humidity
Target_Temperature_FB      - Target temperature of the device controlled by the
                             Sensibo Air
Temperature_Unit_FB        - Either C or F depending on whether the Temperature_FB
                             and Target_Temperature_FB are in Celsius or Fahrenheit

Parameters
Device_ID                  - UID of device to be controlled obtained
                             from the Sensibo web site
API_Key                    - API Key from Sensibo web site
Default_Target_Temperature - Target temperature.  Used when system starts up
                             and mini-split in fan mode so temperature can't be
                             read
Default_Temperature_Unit   - C or F.  Used when system starts up and mini-split 
                             in fan mode so temperature can't be read
Debug_Msg_Output           - Where to send debug messages
                             0=None, 1=Console, 2=Error Log, 3=Both 
