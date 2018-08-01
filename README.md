[Facility Gate Controller](https://github.com/shonky2/MainGate)
======================
Has been adapted for [Resin.io Deployment](https://resin.io)
------
Original Code from [andrewshilliday](https://github.com/andrewshilliday/garage-door-controller)

Monitor and Control your Gates from the Web via a Raspberry Pi.

![Screenshot of Controller Interface](/screenshot/interface.png)


Overview:
------

I orginally started this journey to enable the ability to remotely open gates at one of our facilities. Only 1 gate is currently installed. The second gate is expected to be installed within 6 months. Once I deployed the credited code above successfully I came across the [Resin.io](https://resin.io) platform I wanted to try adapting it to leverage this very slick platform. This is the resulting software and hardware installation guide for monitoring and controlling your gates remotely (via the web or a smart phone). The software is designed to run on a [Raspberry Pi 3B/+](www.raspberrypi.org), which is an inexpensive ARM-based computer, and supports:
* Monitoring of the State of the Gates
* Remote control of the Gates
* Timestamp of last State Change for each Gate


**Hardware Requirements**
------

* [Raspberry Pi 3B/+](http://www.raspberrypi.org)
* Micro USB charger (2A preferable)
* 8 GB Micro SD Card
* [Crydom Solid State Relay](https://www.digikey.com/product-detail/en/sensata-crydom/DMO063/CC1139-ND/254188) ~$22 This is one that I had laying around. There are cheaper options like [SainSmart](http://amzn.com/B0057OC6D8)
* [Magnetic Contact Switch](http://amzn.com/B006VK6YLC) (one per gate)
* [Female-to-Female jumper wires](http://amzn.com/B007XPSVMY)
* 2-conductor electrical wire


Hardware Setup:
------

**1. Install the Magnetic Contact Switches**

The contact switches are the sensors that the Raspberry Pi will use to recognize whether the gates are open or closed. You need to install one on each gate so that the switch is *closed* when the gates are closed. Attach the side without wires to the gate drive wheel, and the other side with wires to the supporting frame of drive wheel in such a way that they are next to each other when the gate is closed. There can be some space between them, but they should be close and aligned properly.

**Gate Wheel Photo to Come**
![Contact Photo](/screenshot/contact.png)

**2. Install the Relays**

The relays are used to mimic a push button being pressed which will in turn cause your gates to open and close. Each relay channel is wired to the gate push button wiring. You'll want to consult your gate operators manual.
    
**3. Complete the Wiring**

The following diagram illustrates how to wire up a two gate controller.  The program can accommodate fewer or additional gates.

**Updated Wiring Diagram to Come**


Software Installation:
-----

**1. Setup [Resin.io](https://resin.io) account and intialize Raspberry Pi Device.**
    1. [Tutorial](https://docs.resin.io/learn/getting-started/raspberrypi3/python/) Once you get to **Deploy Code** you can refer back to the steps below.
  
**2. Clone Code and Move to New Directory of Clone**
    
    ```
    $ git clone https://github.com/shonky2/MainGate.git
    $ cd MainGate
    ```
    
**3. Edit `config.json` File**
    
    You'll need one configuration entry for each gate.
    - **name**: The name for the garage door as it will appear on the controller app.
    - **relay_pin**: The GPIO pin connecting the RPi to the relay for that door.
    - **state_pin**: The GPIO pin conneting to the contact switch.
    - **state_pin_closed_value**: The GPIO pin value (0 or 1) that indicates the door is closed. Defaults to 0.
    - **approx_time_to_close**: How long the garage door typically takes to close.
    - **approx_time_to_open**: How long the garage door typically takes to open.

    The **approx_time_to_XXX** options are not crucial. They tell the program when to shift from the opening or closing state to the "open" or "closed" state.  You will not break anything if they are off.  Worst case, you may end up with a slightly odd behavior when closing the gate where it goes from "closing" to "open" (briefly) and then to "closed" when the sensor detects that the gate is actually closed.
    
    
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

Troubleshooting:
----------  
Type of relay can change the intended state required

  [1]: http://i.imgur.com/rDx9YIt.png
  [2]: http://i.imgur.com/bfjx9oy.png
  [3]: http://i.imgur.com/vPHx7kF.png
  [4]: http://i.imgur.com/AkNl6FI.jpg
  [5]: http://i.imgur.com/48bpyG0.png
  
