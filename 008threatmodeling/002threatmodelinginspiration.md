# Inspiration for threat modelling
Sometimes, when working on step 4, you may find it difficult to come up with threats by just looking at the diagram you just created. Many tools, methodologies, and other aids have been created over the years that may help you in such a situation. 

# Mnemonics
Well-known threat-modeling mnemonics include STRIDE and LINDDUN. Each letter of the mnemonic refers to a certain threat category that may help you in structuring your thoughts. 

## STRIDE
Now that you have your model, it's time to investigate it for threats. Gather your team and start brainstorming about potential threats. Write them down, do not come up with actions yet, simply focus on the threats. Some teams may find it hard to come up with threats, or need a way to bring in more structure. The mnemonic `STRIDE` can help you by providing 6 threat categories:
* **S**poofing: impersonating something or someone else
* **T**ampering: modifying data or code
* **R**epudiation: claiming not to have performed an action
* **I**nformation disclosure: exposing information to someone not authorized to see it
* **D**enial of service: denying or degrading service to users
* **E**levation of privilege: gain capabilities without proper authorization

Usually not every category is applicable to every component in your DFD. For example, you cannot modify a human (well, unless you are a surgeon perhaps, but that's out of scope), and so tampering is not applicable to users:

|                 | S  |T   |R   |I    |D  |E  |
|  -------------  |--  |--  |--  |--   |-- |-- |
| External entity | X  |    | X  |     |   |   |
| Process         | X  | X  | X  | X   | X |   |
| Data store      |    | X  | ?! | X   | X |   |
| Data flow       |    | X  |    | X   | X |   |

_Note that the '?!' symbol in the repudiation column for data stores refers to data stores acting as audit logs. Normal data stores are not usually subject to repudiation attacks, but data stores containing audit logs may. For example, if an attacker manipulates an audit log he or she may afterwards claim not having performed a certain action. Since the audit logs have been altered, all proof is lost._

Threats can be identified for each category. For example, the following threats would fit in the 'tampering' category:
* an attacker can replay data without detection because your code does not provide timestamps or sequence numbers
* an attacker can write to a data store your code relies on
* an attacker can alter information in a data store because it has open permissions
* an attacker can change parameters over a trust boundary (e.g. SQL injection from a web application to the server in the data center)

In the end, you should end up with a categorized list of threats for each component in your DFD. Some more advanced threat modelers may create a traceability matrix, which is basically the same but contains more information about the identified threats.

Do not overthink the categorization of threats. Sometimes a threat may fit in multiple categories, that does not matter. **The categorization is a means to an end, not the end itself**. The end goal is to come up with a list of threats, regardless their category. 

## LINDDUN
While STRIDE focuses on attacks violating the cyber security principles, it does not say a lot about the threats that may exist to the privacy of users. For that, another mnemonic was introduced: `LINDDUN`. While this is outside the scope of this course, feel free to check their website (https://www.linddun.org/linddun) should you ever want to do a threat modeling session that focuses on finding privacy threats. 

# Games
## Adam Shostack's elevation of privilege card game
Adam Shostack, a well-known threat modeling guru, has created a game called 'Elevation of Privilege' that builds upon the STRIDE mnemonic and gamifies the threat modeling experience. I advise you to check it out [here](https://github.com/adamshostack/eop).
For people that know the game 'wiezen', it should be easy to get acquainted with the rules:

* You start with a threat model (diagram) and deal the cards to 3-6 players. You’ll also want to assign someone to take notes.
* Play starts with the 3 of Tampering. The player with that card reads it out, and explains how the threat on the card might apply to the system you’re building. If they can provide a credible threat for which no mitigation is present, they get a point. A credible threat here is one for which you’d file a bug.
* Play proceeds clockwise until each player has had a chance to play a card. Each player needs to play in suit if they have a card in suit.
* When each player has played, the highest numbered card played wins. [Ace is high] The player who won gets a point for the hand, and gets to lead the next hand, including picking the suit that leads that next hand.
* If a player doesn’t have a card in the hand that was lead, they may play any card. Elevation of Privilege cards are 'trumps' ('troef') that beat any other suit. Only the suit lead or Elevation of Privilege can win the hand.

So there are two ways to get a point:

1. by applying the threat that's on the card when playing the card => threat + vulnerable component must be recorded in the notes;
2. by taking the trick ('slag') => this must also be recorded in the notes;

The person taking notes can use the following template: https://github.com/adamshostack/eop/blob/master/EoP_Score%20Card.pdf . **It is important to document each identified threat**. 

# Source attribution
Some parts of this page are based on [Elevation of Privilege (EoP) Threat Modeling Card Game](https://www.microsoft.com/en-us/download/details.aspx?id=20303) by Microsoft, which is licensed under [CC-BY-SA 3](https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License).