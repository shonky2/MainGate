[Facility Gate Controller](https://github.com/shonky2/MainGate)
======================
Has been adapted for [Resin.io Deployment](https://resin.io)
------
Original Code from [Andrew](https://github.com/andrewshilliday/garage-door-controller)

Monitor and control your gates from the web via a Raspberry Pi.

![Screenshot of Controller Interface](/screenshot/interface.png)

Overview:
------

This project provides software and hardware installation instructions for monitoring and controlling your garage doors remotely (via the web or a smart phone). The software is designed to run on a [Raspberry Pi](www.raspberrypi.org), which is an inexpensive ARM-based computer, and supports:
* Monitoring of the state of the garage doors (indicating whether they are open, closed, opening, or closing)
* Remote control of the garage doors
* Timestamp of last state change for each door
* Logging of all garage door activity

Requirements:
-----

**Hardware**

* [Raspberry Pi 3B +](http://www.raspberrypi.org)
* Micro USB charger (1.5A preferable)
* 8 GB Micro SD Card
* Relay Module, 1 channel per garage door (I used [SainSmart](http://amzn.com/B0057OC6D8 ), but there are [other options](http://amzn.com/B00DIMGFHY) as well)
* [Magnetic Contact Switch](http://amzn.com/B006VK6YLC) (one per garage door)
* [Female-to-Female jumper wires](http://amzn.com/B007XPSVMY)
* 2-conductor electrical wire

Hardware Setup:
------

*Step 1: Install the magnetic contact switches:*

The contact switches are the sensors that the raspberry pi will use to recognize whether the gates are open or shut. You need to install one on each gate so that the switch is *closed* when the gates are closed.  Attach the end without wire hookups to the gate drive wheel itself, and the other end (the one that wires get attached to) to the supporting frame of drive wheel in such a way that they are next to each other when the gate is shut.  There can be some space between them, but they should be close and aligned properly.

![Sample closed contact switch][3]

*Step 2: Install the relays:*

The relays are used to mimic a push button being pressed which will in turn cause your garage doors to open and shut.  Each relay channel is wired to the garage door opener identically to and in parallel with the existing push button wiring.  You'll want to consult your model's manual, or experiment with paper clips, but it should be wired somewhere around here:

![!\[Wiring the garage door opener\]][4]
    
*Step 3: Wiring it all together*

The following diagram illustrates how to wire up a two-door controller.  The program can accommodate fewer or additional garage doors (available GPIO pins permitting).

![enter image description here][5]



Software Installation:
-----

1. **Setup [Resin.io](https://resin.io) account and intialize Raspberry Pi Device.**
    1. [Tutorial](https://docs.resin.io/learn/getting-started/raspberrypi3/python/) Once you get to **Deploy Code** you can refer back to these steps.
  
2.  **Edit `config.json` File **
    
    You'll need one configuration entry for each gate.
    - **name**: The name for the garage door as it will appear on the controller app.
    - **relay_pin**: The GPIO pin connecting the RPi to the relay for that door.
    - **state_pin**: The GPIO pin conneting to the contact switch.
    - **state_pin_closed_value**: The GPIO pin value (0 or 1) that indicates the door is closed. Defaults to 0.
    - **approx_time_to_close**: How long the garage door typically takes to close.
    - **approx_time_to_open**: How long the garage door typically takes to open.

    The **approx_time_to_XXX** options are not crucial. They tell the program when to shift from the opening or closing state to the "open" or "closed" state.  You will not break anything if they are off.  Worst case, you may end up with a slightly odd behavior when closing the gate where it goes from "closing" to "open" (briefly) and then to "closed" when the sensor detects that the gate is actually closed.
  
3. **Clone Code and Move to New Directory of Clone**
    
    ```
    $ git clone https://github.com/shonky2/MainGate.git
    $ cd MainGate
    ```
    
3.  **Add Remote Resin Application Repository**
    
    ```
    $ git remote add resin <USERNAME>@git.resin.io:<USERNAME>/<APPNAME>.git
    ```
    
4.  **Deploy Application to Device**
     
     ```
     $ git push resin master
     ```
    When build is successfully completed you should see this.
    
    
    
5. **Using the Controller Web Service**

    The garage door controller application runs directly from the Raspberry Pi as a web service running on port **3001**. It can be used by directing a web browser (on a PC or mobile device) to **http://[hostname-or-ip-address]:3001/**.  If you want to connect to the Raspberry Pi from outside your home network, you will need to establish port forwarding in your router firewall.  
    
    When the app is open in your web browser, it should display one entry for each garage door configured in your `config.json` file, along with the current status and timestamp from the time the status was last changed.  Click on any entry to open or close the door (each click will behave as if you pressed the garage button once).

Notes:
----------  
Type of relay can change the intended state required

  [1]: http://i.imgur.com/rDx9YIt.png
  [2]: http://i.imgur.com/bfjx9oy.png
  [3]: http://i.imgur.com/vPHx7kF.png
  [4]: http://i.imgur.com/AkNl6FI.jpg
  [5]: http://i.imgur.com/48bpyG0.png
  
