# Automated-SOC
![The flow control diagram](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/soc%20automation%20flowchart.jpg)

## Introduction:
Having tried my hand with a basic, entirely Azure based SOC simulation, I decided to take up a challenge a notch higher; SOC automation utilizing everything open source! This project is still underway, but so far, I have configured an automated workflow to send me a notification through an email for any usage of Mimikatz on the only machine present in the environment, a windows VM.


## Tech stack:
- Wazuh as the EDR.
- Shuffle as the driver for the workflow.
- TheHive as the SIEM/Security Incident Response.

---

  ## Alert Life-Cycle:
  #### 1. Alert gets created in one of the endpoints:
  
  ![Mimikatz alert](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/mimikatz%20alert.png)

  #### 2. Wazuh direct the alert towards TheHive after IOC enrichment utilizing the VirusTotal API:

  ![Workflow uptil the Hive](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/workflow%20uptil%20theHive.png)

  #### 3. The alert finally reaches TheHive:

  ![Alert in TheHive](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/TheHive%20alerts%20showing%20up..png)


---

## Troubleshooting:

### 1. Virustotal not receiving alerts properly:

Now, triggering a mimikatz alert and then analyzing the workflow reveals that, even though the hash was captured and sent successfully, VirusTotal did not receive it correctly. There's something wrong with he GET request being sent to VirusTotal which we will try to figure out now.

The first source of debugging that I would utilize in such a case is to look up the [api](https://docs.virustotal.com/reference/file-info). Make sure that the correct url with the correct endpoints are being utilized, and as shown below, they were:

![Virustotal debugging 1](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/virustotal%20debugging%202.png)

It turns out, i wasn't selecting the right output from the shuffle node. I should've been selecting the "list" options so that the output of the shuffle node in the middle is properly parsed and only the SHA256 sum is provided to the VirusTotal node:

![https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/virustotal%20debugging%203.png](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/virustotal%20debugging%203.png)

### 2. Incorrect JSON request being sent to TheHive:
As is the case with open source, things may break in very subtle manners without the developers realizing. As I was trying to use use TheHive's application in my shuffle workflow to send the data in JSON format to my actual TheHive's dashboard, the POST request being generated wasn't properly parsed and hence kept on returning status 400. Turns out, whenever I would use a parameter in my arguments, I have to put a space character between the text and the actual argument starting with a "$" otherwise the string doesn't get parsed properly. I was also missing a comma between the "flag" parameter and the "pap" parameter which, once added, was fixed.

![TheHive debugging 1](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/TheHive%20debugging.png)

After the fix:

![TheHive debugging 2](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/TheHive%20debugging2.png)


