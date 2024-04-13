# Automated-SOC
![The flow control diagram](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/soc%20automation%20flowchart.jpg)

## Introduction:
Having tried my hand with a basic, entirely Azure based SOC simulation, I decided to take up a challenge a notch higher; SOC automation utilizing everything open source! This project is still underway, but so far, I have configured an automated workflow to send me a notification through an email for any usage of Mimikatz on the only machine present in the environment, a windows VM.


## Tech stack:
- Wazuh as the EDR.
- Shuffle as the driver for the workflow.
- TheHive as the SIEM/Security Incident Response.

  ## Alert:
  ![Mimikatz alert](https://github.com/Hammad-AlQuraishi/mediumImages/blob/main/SOCAutomation/mimikatz%20alert.png)
