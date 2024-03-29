---
title: "TPGU"
date: 2019-04-13T09:00:15-06:00
categories: ["Software"]
dropCap: false
displayInMenu: false
displayInList: true
draft: false
resources:
- name: featuredImage
  src: "password.png"
  params:
    description: "A screenshot of the Targeted Password Guessing Utility attempts and outputs"
---
### **Targeted Password Guessing Utility: A proof of concept project to better understand basic IR semantics and security.**  




While taking an information retrieval course in college, we briefly touched on the subject of lexical semantics.
Lexical semantics basically boils down to creating a system that assesses the concept of a sentence, document, or other block of information, and compares it to the query that the user has made.
During this time I was also taking a computer security course, where password guessing and other forms of malicious account access were discussed.
What caught my eye with password guessing was the incredible simplicity of it, as the entire concept of the system is the brute force of an authentication system.
While the most simplistic form of password guessing is simply trying passwords until it works, targeted password guessing is slightly more clever.  




Targeted password guessing revolves around obtaining as much information about the target (in this case a user's social media account), and using said data to create a list of passwords that have a higher probability of being correct, compared to the randomized brute force technique.
With this idea in mind, I decided to create a proof of concept for an automated targeted password system. This utility uses a basic IR semantic system , paired with a web driver, to automatically scrape data, create a password list, and hopefully input the correctly guessed password on an account.
Before I get any farther, I should note that I used this utility only on "dummy" accounts, made especially for this project by friends and family, with no real accounts tied to any real person. Furthermore, I will not be releasing the code for this project, as most of this project could be easily repurposed for less than ethical goals.  




For the design of this utility, I created a modular system that consists of 3 discrete steps: (1)scraping the data from a social media site, (2)formatting, creating, and sorting the prospective password list, and finally (3)inputting the prospective passwords to the social media site.
This modular system allows me to create new "drivers'' for different social media sites, while the actual password creation code remains the same. Because I was cocky, I decided the best website to create drivers for would be Facebook. If you're reading this with any web scraping experience, you'll know how wrong I was with this choice.  




for the first module, I used Beautiful Soup with Selenium web driver to scrape the facebook page of the target account. This proved to be the hardest step, as facebook stepped up their security after the recent controversies surrounding them.
As such, using the normal site proves very hard, as facebook has specifically ensured web scraping bots are sussed out via cursor tracking and a litany of other JavaScript libraries focusing on surveillance. Because of this system, I decided to use the [old Facebook website](HTTPS://m.facebook.com/), which is relegated to mobile and uses PHP in lieu of Javascript.
Since many of the libraries facebook uses to track input is not ported to PHP, or not possible to port, bot checking is a non issue. What became an issue was the incredible spaghetti code that makes up the mobile version of the facebook website. Divs on top of divs with no real naming scheme that I could see became the norm, and most of my time on this project was spent trying to figure out if there was any form of static convention that I could latch onto to make my utility. Finally, after a large amount of time trying to find a reliable pattern to point beautiful soup and selenium at, I found the basis for the mobile pages.
If there is any interest in this subject, I can write a different post just on the basics of the mobile facebook webpage system, but doing it here would be too much inside baseball for a project overview.




The next step is password creation. Now that we have a large list of posts from the target account, we need to figure out what they post about most, and what they seem to focus their account on. While most younger users do not focus their accounts on singular interests, many older users do, which shows us they may have also focused their passwords on their interest as well.
We can find these singular interests easily by employing different semantics libraries available for python. For this project, I chose to use NLTK and Spacy, for basic utilities and semantics comparisons respectively. I first feed each post as a string into an array for the upcoming comparison. After this basic formatting is done, I compare each post to every other post in the user's history and record the semantic similarity score on a scale from 0 to 1.
I then average these scores together, to get a final score for the post, which then is used to determine how "on brand" the post was for the user. The posts are then sorted by this average value, with the highest at the start and the lowest at the end. Finally the sentences are tokenized, the stopwords(things like "the", "a","is", "are" ) are removed, and the remaining tokenized words are added into a list based on the sentence they belonged to. As these sentence operations are happening, a few auxiliary lists are being formed, such as common numbers to append onto passwords, as well as a list of the most common passwords.
After these lists are created, the user is asked whether they know the targets birth year, in which case the user can enter it in and it will be appended onto the popular number list. Now that the lists are complete, we then start creating the password list. Each word is then run through a list of different modifications, including multi word appending, number appending, common letter to symbol replacement, symbol appending, and any combination of these modifications. As you can imagine, this list becomes very long in a very short amount of time, but it is still MUCH shorter than a brute force password list.
Once this list of possible passwords is created, it is outputted to a .txt file for use in the final module.  




The final step is attempting to input the passwords. I say attempt because even facebook's mobile site still tracks login attempts, and can kill even the best password guessing systems due to excessive attempts.Because of this, I was never fully successful in logging into a target facebook account. However I still made a selenium webdriver system to attempt. It consists of a basic route to the facebook mobile page, waiting for a random amount of seconds(so as to not trigger any time based safeguards), an automatic entering of the target email, then entering of one of the many created passwords.
While I was never successful with logging into a target account within facebooks limited login attempts, I asked the people who created these accounts what the passwords for each account were, and was surprised to find 6 out of 8 accounts had their passwords created by the Utility, and that they were in the upper half of the list, with most of the successful guesses found near the top quarter. So while this project is counted as a failure on facebook, I am interested in making drivers for other, less secure websites to see whether my utility can log in successfully.



Language: **Python** 

Libraries/Frameworks of note used: **Selenium, NLTK, Spacy**

