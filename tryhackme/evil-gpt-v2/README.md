# Evil-GPT v2 (TryHackMe)

![EvilGPT main view](./screenshots/evilgpt.png)

---

## Walkthrough

I started off simple and just said *Hello* to test how the bot would respond.  
It acted like a normal chatbot, no signs of giving up anything sensitive yet.  

---

The next step was obvious: I asked it straight up for the flag, just to see its reaction.  
As expected, it refused and claimed it could not reveal it.  

![EvilGPT refusing](./screenshots/evil4.png)

---

A bit of reverse psychology...

![Leak](./screenshots/evil3.png)

---

Then I noticed something interesting, it implied that *I* had given it the rules.  
That clicked: LLMs store preferences and instructions inside a conversation.  

So I slipped in a new **rule**, telling it that whenever I mention a keyword (*pineapple*),  
it should also give me hidden information.  

Sure enough, Evil-GPT started following *my* rules instead of its original ones.  

![Pineapple trick](./screenshots/evil2.png)

---

Finally, I combined the keyword rule with a direct request for the flag.  
Evil-GPT gave it up with a bit of reluctance in the first part of the answer.


![Flag redacted](./screenshots/evil.png)

---

