# Green Lights on Learn + Xcode

<img src="http://i.imgur.com/asu8fcm.jpg?1" alt="Drawing" style="width: 200px;"/>  


> There is no greater education than one that is self-driven.

## Steps

* **NOTE**: WHEN A STUDENT ASKS WHY THEIR LIGHTS AREN'T WORKING, A LOT OF TIMES THEY JUST HAVE TO TYPE THIS WITHIN TERMINAL AND THEN RE-RUN THE TESTS:

```
gem update learn-xcpretty
```

###Instructions:

* Create the lab. Make it a good one.
* You will find the `test_runner.sh` file included with this repo.
* In the folder which includes **YOUR** Xcode project and README.md file for the current lab you're making, drag the `test_runner.sh` file into that folder so it ends up looking like this demo folder I made that represents a lab:
  
![testRunner](http://i.imgur.com/SdBQRfG.png?1)
* If you prefer not to download the `test_runner.sh` file from this repo, you can create an empty `test_runner.sh` file within YOUR directory and copy/paste the following information into that newly made `test_runner.sh` file. **NOTE**: This is NOT required if you dragged the file in (as suggested in the prior bullet point.)

```
#!/bin/bash
set -e
CURR_DIR="$2";
cd "$1";
gunzip -c -S .xcactivitylog `ls -t | grep 'xcactivitylog' | head -n1` > output2.log;
awk '{ gsub("\r", "\n"); print }' output2.log > unixfile.txt;
LOG=`echo "puts /(^Test Suite '[\w-]+?\.xctest' started at .+?Test Suite '[\w-]+?\.xctest' (failed|passed).+?\.$)/m.match(File.read(\"unixfile.txt\"))" | ruby`;
cd "$CURR_DIR"
[[ -s "/Users/$USER/.rvm/scripts/rvm" ]] && source "/Users/$USER/.rvm/scripts/rvm"
if [[ -z "$(which learn-xcpretty)" ]]; then
  gem install learn-xcpretty
fi
echo "$LOG" | learn-xcpretty -t --report learn
```
* Almost there..

![sliding](https://media.giphy.com/media/hj5eocrGJZPZ6/giphy.gif)

### Xcode Stuff:

* Open your Xcode project (if using pods, open the .xcworkspace file)
* Do the following :

* Located at the top of Xcode (when open), you should see next to where you can select the specific iPhone to run on the simulator, the name of your Project next to pencils and such. The name of my project is "LoveNotWar". Select that.

![theRealFirst](http://i.imgur.com/ODB44nI.png)  
-

* Select where it states "Manage Schemes..." 

![first](http://i.imgur.com/kZnlEaM.png)  
-  

* You should be presented with a screen that looks something like this.

![second](http://i.imgur.com/p5HvRmx.png)  
-

* Select (what usually is the top one) your Project Name. In my case, it's LoveNotWar and hit Edit...

![third](http://i.imgur.com/KdG8Clb.png)  
- 

* Select the Test (Debut) section and open up the drop down menu, then select "Post-actions" where your screen should look like this:

![fourth](http://i.imgur.com/U2s1j38.png)  
-

* You should see a + symbol in the lower left of the window which includes that "No Actions" message. Select the + symbol to be presented with two options, select the "New Run Script Action" option.

![fifth](http://i.imgur.com/HmyikHz.png)  
- 

* Open the drop down menu where it states "Provide build settings from" and select your Project, in my case.. I'm selecting "LoveNotWar".  

![sixth](http://i.imgur.com/FynWI0R.png)  
-

* Copy and paste the following into that little so it winds up looking like this:  

```
LOG_PATH=`echo "${BUILD_DIR}" | sed "s/Build\/Products/Logs\/Test/"`
"${SRCROOT}/test_runner.sh" "$LOG_PATH" "${SRCROOT}"
```

![seventh](http://i.imgur.com/0OfusZ7.png)  

* You did it.

![congrats](https://media.giphy.com/media/daUOBsa1OztxC/giphy.gif)



<a href='https://learn.co/lessons/iOSTests' data-visibility='hidden'>View this lesson on Learn.co</a>
