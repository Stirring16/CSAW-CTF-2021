# Contact Us
Veronica sent a message to her client via their website's Contact Us page. Can you find the message?

Author: moat, Pacific Northwest National Laboratory

`Hint:` Some of you with eagle eyes may have noticed another flag hiding in the packet capture: flag{$$L_d3crypt3d}. The author had a browser tab open when capturing packets for the challenge and was surfing the net for flags because that's what you do when you're a challenge author. Moreover knowing this flag will not help you get closer to the solve. There's no limit to the number of attempts to submit this particular flag so nobody was affected. We do regret any time you lost though. Also, there's a server associated with the challenge but it's not necessary to connect to it to solve the challenge. We're therefore taking the server offline to streamline your effort with this task. Good luck!

[ContactUs.pcap](https://drive.google.com/file/d/1au3DOiBRVv7wLhprQdPmdyJqVS3JQct2/view?usp=sharing)

[sslkeyfile.txt](https://github.com/Stirring16/CSAW-CTF-2021/files/7166793/sslkeyfile.txt)

## Using Wireshark 

We have two files: `ContactUs.pcap` and `sslkeyfile.txt`

So we can use `sslkeyfile.txt` to decrypt the pcap by choosing:

`Edit -> Preferences -> Protocols -> TLS -> (Pre)-Master-Secret log filename` and click browse and select the `sslkeyfile.txt` file

![image](https://user-images.githubusercontent.com/62060867/133372018-fbc918e6-cf6f-4300-8280-75ab46dd3ab8.png)

Wireshark is now configured to decrypt TLS data.

Then, to reduce the starting size we filtered by `ip.src == 192.168.198.135` since this was the private IP that had the most outbound connections and find the decrypted flag in the packet capture


![image](https://user-images.githubusercontent.com/62060867/133373014-b4bd6637-99b3-46c2-98d3-a337787188fd.png)

So we got the flag


