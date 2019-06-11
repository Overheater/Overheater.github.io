---
title: Fault Tolerant Chat 
category: Project
feature_image: "/assets/TolChat.PNG"

---
A basic proof of concept that uses hamming(7,4)for forward error correction
<!-- more -->
Looking into fault tolerance, I found that most forward error correction schemes are incredibly complicated to implement, with many of the scholarly papers outlining them taking around 50 pages to explain the basics.
As such many libraries are available to easily implement systems like [turbo code][[turbocodepage]] them into existing projects. While this is all well and good for productivity, I still wanted to implement some form of error correction code, so I started with one of the first, the [Hamming(7,4)][Hamming] implementation.
started in 1947 and first implemented in 1950, Richard Hamming's error correction system allowed a bit of corruption per 4 bits, which compared to newer, more performant systems may not be amazing, but it still gets the job done. 
<br>
this is a proof of concept system that allows two of the client listening and sending on different ports to send to a man in the middle "corruptor". this corruptor flips a bit in a set percentage of bytes, and can also drop the message completely. If the message is sent successfully, then the corrupted message is sent to the other client. It's the clients job to fix any corruption, as well as ensure the message is successfully sent to the other client.
To do this, I implemented the hamming(7,4) system by doing the following. for each byte, I split it in half then performed hamming(7,4) on both using basic linear algebra to form two seven boolean long arrays, then capped it with a eigth useless bool to pad out the array. This eight boolean long array can then be converted into a byte of data and placed into a byte array for the sent message. after it is corrupted by the man in the middle corruptor (code found [here][FaultyNetwork]), the receiving client does it's half of the work. Using another set of basic linear algebra techniques, the receiving client creates a 3 boolean long array. this boolean array matches a potential binary value of 8, the length of a byte in bits.
if any of these three bools are true, it maps to a binary value, which is position of the bit that is corrupted within the bit. the receiving client then flips this bit, and performs this process on the next byte, then finally attaches the 4 data bits of each byte together to form the original message byte. This process is done for every byte in the message, before finally presenting the corrected message on the receiving clients chat. 

[FaultyNetwork]: https://github.com/jak103/FaultyNetwork
[Hamming]:https://en.wikipedia.org/wiki/Hamming(7,4)
[turbocodepage]: https://en.wikipedia.org/wiki/Turbo_code

<br>
Language:C# <br>
Libraries/Frameworks of note used: Winforms<br>
Github link: https://github.com/Overheater/WinFormsFaultTolNet