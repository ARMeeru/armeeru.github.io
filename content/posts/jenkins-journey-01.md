---
title: "Jenkins and Slack - Why I Started Pursuing This"
date: 2023-03-26T21:31:34+06:00
draft: false
tags: ["devops", "jenkins"]
---
### Backstory
My frustration with my Internet service provider (ISP) has been growing recently. Although I can understand brief periods of inaccessibility, my IP address frequently changes for unknown reasons. For various reasons, I do not appreciate sudden IP changes. However, it is frustrating that their CS team needs the exact time the IP address was changed in order to determine the cause.

That's why I've settled on using tools like Jenkins and Slack. Since I've only just begun using these tools, you can assume that any mistakes I make will be those of a complete noob.
### Goal
My intention was to have a script run on a recurrent basis, and if my desired IP address didn't match the one my ISP gave me, I wanted to be alerted on Slack.
### Prerequisite
- Jenkins
- Slack (and also [Slack Notification](https://plugins.jenkins.io/slack/) plugin for the alert)
### Setting Up Jenkins Job
1. Create a new item as a **Freestyle project**
2. From Build Steps section, add build step as execute shell
3. Add the path of the executable shell file (we'll come back to this later)![Build Steps](https://ptpimg.me/7757e6.png)
4. Give it a test run and it should succeed if everything is alright.
### Some More Personal Preference For Jenkins
1. You can add cron-like expression that defines a schedule for a Jenkins job to run![Build Triggers](https://ptpimg.me/ra3cg9.png)
	- This specific expression means to run the job every hour at a random minute
2. You can check **Add timestamps to the Console Output** on **Build Environment** section
### Script I Was Working With

    #!/bin/bash
    
      
    
    # Define your preferred IP address
    
    preferred_ip="192.X40.2X3.12X"
    
      
    
    # Get your ISP provided IP address from a website
    
    isp_ip=$(curl -s https://api.ipify.org)
    
      
    
    # Check if ISP provided IP matches your preferred IP
    
    if [[ $isp_ip == $preferred_ip ]]; then
    
    echo  "IP match found: ISP provided IP ($isp_ip) matches preferred IP ($preferred_ip)"
    
    exit  0
    
    else
    
    echo  "IP match not found: ISP provided IP ($isp_ip) does not match preferred IP ($preferred_ip)"
    
    exit  1
    
    fi
   

 A simple script which you need to make executable with `chmod +x /path/to/script.sh` so that Jenkins can run it.
 ### Slack Integration
 1. Make sure you have installed the [plugin](https://plugins.jenkins.io/slack/)
 2. Go to **Manage Jenkins**>**Configure System**
 3. Make sure your Jenkins CI is installed on the desired slack space where you'll get a token
 4. A Slack section should appeared which can be configured accordingly![Jenkins Slack Config](https://ptpimg.me/9dn2ou.png)
 ### Setting Up Alert For Slack
 1. Now go back to the job that was initially created
 2. Scroll down to **Post-build Actions** and choose **Slack Notifications** from the dropdown
 3. Check your parameter, for my case, it is **Notify Every Failure**
 4. And now I have specific time of failure![Slack Alert](https://ptpimg.me/632fsb.png)
 ### Conclusion
 Indeed, Jenkins is a delight to me. I'm hoping to pick up some helpful information. Interested in studying with me? Drop me a line. This is how I spend my weekends, wasting time.
