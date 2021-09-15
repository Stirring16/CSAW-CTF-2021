# Tripping Breakers

Attached is a forensics capture of an HMI (human machine interface) containing scheduled tasks, registry hives, and user profile of an operator account. There is a scheduled task that executed in April 2021 that tripped various breakers by sending DNP3 messages. We would like your help clarifying some information. What was the IP address of the substation_c, and how many total breakers were tripped by this scheduled task? Flag format: flag{IP-Address:# of breakers}. For example if substation_c's IP address was 192.168.1.2 and there were 45 total breakers tripped, the flag would be flag{192.168.1.2:45}.

`Hint:` The challenge prompt asks for the number of breakers that were tripped by the task. To clarify, it refers to breakers that were tripped for all substations, not just substation_c.

[hmi_host_data.zip](https://github.com/Stirring16/CSAW-CTF-2021/files/7169473/hmi_host_data.zip)

## Solution

In the .zip file we have:

- `host\scheduled_tasks.csv` contains scheduled tasks
- `host\Registry\SOFTWARE_ROOT.json` contains registry hives
- `host\operator` contains the User-Profile folder of a Windows account

![image](https://user-images.githubusercontent.com/62060867/133422918-308da882-6121-42d9-b37a-f88b643f7d13.png)

