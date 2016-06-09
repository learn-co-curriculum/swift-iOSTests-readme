# Green Lights on Learn + Xcode

<img src="http://i.imgur.com/asu8fcm.jpg?1" alt="Drawing" style="width: 200px;"/>  


> There is no greater education than one that is self-driven.

## Steps

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

![first](http://i.imgur.com/kZnlEaM.png)  
-  
![second](http://i.imgur.com/p5HvRmx.png)  
-
![third](http://i.imgur.com/KdG8Clb.png)  
- 
![fourth](http://i.imgur.com/U2s1j38.png)  
-
![fifth](http://i.imgur.com/HmyikHz.png)  
- 
![sixth](http://i.imgur.com/FynWI0R.png)  
-
![seventh](http://i.imgur.com/0OfusZ7.png)  

```
LOG_PATH=`echo "${BUILD_DIR}" | sed "s/Build\/Products/Logs\/Test/"`
"${SRCROOT}/test_runner.sh" "$LOG_PATH" "${SRCROOT}"
```



<a href='https://learn.co/lessons/iOSTests' data-visibility='hidden'>View this lesson on Learn.co</a>
