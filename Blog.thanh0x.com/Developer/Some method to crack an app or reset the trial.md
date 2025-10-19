### Manipulate trial (Easy)
#blog-layout 
##### Reset trial
Simply remove the file which store the data to know when will the timer expired
Sample:
+ Revenue Cat
+ IntelliJ IDE
##### Adjust expired date
Before opening the app, adjust your local date time to a future in a day. So for example the app let you have 7 days of trial. When change the time to 30 days later and open the app, it trigger the app to think that the date of expired is 37 (30+7) later. Then you revert your local time and enjoy extended trial time
Sample: I can not explicitly mentioned it as there is no available reference for it. But it tried it with one of the app I'm using [[All the apps I am using]]
### Network manipulation (medium)
##### One Time check
Mock Json one time and it upgraded to the premium user and be there forever.
```Sample code of MITM
```
##### Every day check
Mock json and have always on to return valid verify reponse each time the app request to the server

### Manipulate the code (medium-hard-super hard)
We got to this step when all above is not possible. For some reason
+ The file are not found to delete, so they store the trial date some where which we cannot found
+ The network reponse have a hash or token which verify client/server. Which we can not mock
##### Inject the code
Quite easy and usually works on Windows, Android or Jailbroken IPhone
##### Reverse Engineering and recompile with modified logic (Hard or SuperHard)
Depends on the platform and depend on how the code have been obfuscated.
ANd some of the app put the logic into a deeper level (virtual space)