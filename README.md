# Green Lights on Learn + Xcode

<img src="http://i.imgur.com/asu8fcm.jpg?1" alt="Drawing" style="width: 200px;"/>  


> There is no greater education than one that is self-driven. -[Neil deGrasse Tyson](https://en.wikipedia.org/wiki/Neil_deGrasse_Tyson)

---

### Quick Fix

If you've come to this page over and over for info, I will include this snippet near the top (to help you out).

```
LOG_PATH=`echo "${BUILD_DIR}" | sed "s/Build\/Products/Logs\/Test/"`
"${SRCROOT}/test_runner.sh" "$LOG_PATH" "${SRCROOT}"
```

I think it helps to have a copy of the `test_runner.sh` file somewhere local on your machine that way you can keep copying and pasting the actual file over to new projects you make.

There are some labs that include tests. Learn might include an additional circle (that can be lit green) that states that you need to "Pass The Tests". When the tests pass within Xcode, the appropriate circle (on learn) should then light up green.

In order for Xcode to be able to communicate with Learn to let Learn know that  all of your tests passed when completing a lab, you need to type the following command in Terminal which will install the file on your machine to make all this happen.

`bash <(curl -s https://raw.githubusercontent.com/flatiron-school/ios-setup/master/install.sh)`

After running the command above in terminal, you should be met with a message that states "You're all set - setup complete!" after going through any necessary steps of it asking you any questions.

![completeSetup](http://i.imgur.com/OBX76qT.png)

You might notice that my terminal looks different than yours, especially that <3 icon I have.

`curl https://raw.githubusercontent.com/flatiron-school/dotfiles/master/.bash_profile -o ~/.bash_profile`

Typing that command in terminal will install a certain file on your machine which is setup to make your terminal application look like mine. I recommend you do this (as it's not just adding a <3 icon) but take note that by running this command it will override any customized bash_profile you might already have on your machine.

Please Note you must reload your bash profile by typing the following in terminal
`source ~/.bash_profile~

---


# Instructions:


* Don't create your `.gitignore` file yet. If you want to know why, scroll down and read the **Important Note** listed below.
* Create an Xcode project. You will want it to include Unit Tests, **NOT** UItests.

![](http://i.imgur.com/Bq5tqhm.png?1)

* You will find the `test_runner.sh` file included with this repo.
* In the folder which includes **YOUR** Xcode project and README.md file for the current lab you're making, drag the `test_runner.sh` file into that folder so it ends up looking like this demo folder I made that represents a lab:
  
![testRunner](http://i.imgur.com/SdBQRfG.png?1)
* If you prefer not to download the `test_runner.sh` file from this repo, you can create an empty `test_runner.sh` file within your directory and copy/paste the following information into that newly made `test_runner.sh` file. **NOTE**: This is NOT required if you dragged the file in (as suggested in the prior bullet point.)

```
#!/bin/bash
set -e
SERVICE_URL='http://ironbroker.flatironschool.com'
SERVICE_ENDPOINT='/e/flatiron_xcpretty'
CURR_DIR="$2"
NETRC=~/.netrc

if [ -f ${NETRC} ]; then
  if grep -q flatiron-push ${NETRC}; then
    GITHUB_USERNAME=`grep -A1 flatiron-push ${NETRC} | grep login | awk '{print $2}'`
    GITHUB_USER_ID=`grep -A2 flatiron-push ${NETRC} | grep password | awk '{print $2}'`
  else
    echo "Please run the iOS setup script before running any tests."
    exit 1
  fi
else
  echo "Please run the iOS setup script before running any tests."
  exit 1
fi

cd "$1"
gunzip -c -S .xcactivitylog `ls -t | grep 'xcactivitylog' | head -n1` | awk '{ gsub("\r", "\n"); print }' > unixfile.txt
TOTAL_COUNT=`tail -n10 unixfile.txt | grep -A1 xctest | grep Executed | awk '{print $2}'`
FAILURE_COUNT=`tail -n10 unixfile.txt | grep -A1 xctest  | grep Executed | awk '{print $5}'`
PASSING_COUNT=`echo "${TOTAL_COUNT} - ${FAILURE_COUNT}" | bc`
REPO_NAME=`echo ${CURR_DIR} | awk -F'/' '{print $NF}'`
cd ${CURR_DIR}


curl -s -H "Content-Type: application/json" -X POST --data "{ \"username\": \"${GITHUB_USERNAME}\", \"github_user_id\": \"${GITHUB_USER_ID}\", \"repo_name\": \"${REPO_NAME}\", \"build\": { \"test_suite\": [{\"framework\": \"xcpretty\", \"formatted_output\": [], \"duration\": 0.0, \"build_output\": []}]}, \"total_count\": ${TOTAL_COUNT}, \"passing_count\": ${PASSING_COUNT}, \"failure_count\": ${FAILURE_COUNT}}" "${SERVICE_URL}${SERVICE_ENDPOINT}"

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

* Select the Test (Debug) section and open up the drop down menu, then select "Post-actions" where your screen should look like this:

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

# Important Note!

The step where you're adding the Post-action script will not persist if you don't do this correctly.

What does that mean?

If you try to add that post-script command within your Xcode project within your directory that _already_ contains a `.gitignore` file, it will **NOT SAVE**. You might close your Xcode, re-open it and see that it _is_ still there, where you might shout "BUT JIM IT DID SAVE!". Yes, it saved to your local Xcode but considering your `.gitignore` file has this lovely line in it:

`xcuserdata/`

That will **prevent** this post-script action from being pushed up to Github. That's because any change to this post-script action (like what we did above) happens within this `xcuserdata/` folder. 

So there are a few ways to handle this.

1. When you create the Xcode project for this lab, don't save it to the folder that contains the `.git` file (meaning the directory that you navigate to to `git add .`, `git commit -m ""` and all that jazz). Save it to your desktop, add this Post-script command to the Xcode project _then_ drag it into the directory which contains the `.git` file. At that point you can enter in all those lovely git commands then push it up.
2. Don't create the `.gitignore` file _until_ you have this project created that includes the post-script action. When that post-script command is added, then `git add .`, `git commit` and then `git push`. At that point.. you should then create the `.gitignore` file.
3. Some other magic you come up with.

![](https://media.giphy.com/media/x7l3pkXcDrKPm/giphy.gif)


If you want to make sure everything is working. Clone down the repo into a different folder somewhere, open Xcode, navigate to the post-script and make sure it's still there. If it's not there, then `gitignore`  is ignoring it. You will need to remove the `gitignore` file (or just the part regarding `xcuserdata/` althoug I haven't tested this out in just removing the `xcuserdata/` part). Then add that post-script action to Xcode, then add, commit and push up the changes to Github. Then re-add the `.gitignore` file. It might still be tracking the xcuserdata info which means you will need to type `git rm -rf yyyyy` those xcuserdata files. `yyyyy` represents the name(s) of those files. You will know it's mistakenly tracking these files if you close Xcode. Type `git status` where you should see everything is up to date after you had added, commited and pushed the changes from the earlier step. Now re-open Xcode. Just click open a file  on the left (don't add anything to it), now close xcode. If you type `git status`, NO files should show as being changed. If it shows a xcuserstate or data type file that needs to be commited, then that's the particular file you need to `git rm -rf`

# Podfile

We use Quick & Nimble for tests.

[Here](https://github.com/learn-co-curriculum/swift-boat/blob/master/swift-boatTests/BoatSpec.swift) is an example of tests written for one of our labs.



<a href='https://learn.co/lessons/iOSTests' data-visibility='hidden'>View this lesson on Learn.co</a>
