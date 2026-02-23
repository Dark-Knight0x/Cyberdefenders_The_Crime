
# The Crime Lab - Android Forensics Investigation

## Overview

This lab focuses on Android endpoint forensics using ALEAPP to analyze extracted mobile artifacts. 



The objective was to reconstruct the victim’s financial situation, movements, communications, and travel plans through systematic forensic analysis.

`Category: Endpoint Forensics` 

`Tools Used:  DB Browser for SQLite  `
Link Assignment : [the-crime](URL(https://cyberdefenders.org/blueteam-ctf-challenges/the-crime/)")

---

## Investigation Summary

Using ALEAPP and manual SQLite database analysis, we examined application artifacts, call logs, messages, Discord conversations, and location-related data to uncover critical evidence.

---

## Questions & Findings

### Q1: Trading Application SHA256

The victim was heavily involved in trading activities and accumulated significant debt. 

By analyzing installed applications and extracted APK artifacts, we identified the primary trading application used on the device.

**SHA256 Hash:**



```
4f168a772350f283a1c49e78c1548d7c2c6c05106d8b9feb825fdc3466e9df3c
```
#### Answer :

<img width="1003" height="201" alt="image" src="https://github.com/user-attachments/assets/1edd2430-20c6-47e5-9c7a-67aae198b56f" />


To determine the trading application used by the victim, we navigated to the `/data/app` directory within the extracted Android file system. This directory contains installed third-party applications.

Among the installed applications, we identified `ticno.olymptrade`, which is associated with a trading platform. Another application present was Discord; however, it was unrelated to the trading activity described in the case scenario.

<img width="993" height="174" alt="image" src="https://github.com/user-attachments/assets/a27dc676-58cf-4f32-b731-943e3d43193c" />


We then navigated into the directory of `ticno.olymptrade` to locate the corresponding APK file. Inside the folder, we found the file `base.apk`, which represents the primary installation package of the application.

After extracting the `base.apk` file, we calculated its SHA256 hash value to identify the exact application version used by the victim.

The resulting SHA256 hash was:
<img width="1222" height="80" alt="image" src="https://github.com/user-attachments/assets/362db679-1aa8-4c3b-b87b-abd499fcad33" />





---

### Q2: Debt Amount

Call logs and related artifacts indicated repeated incoming calls from a creditor.

The total amount owed by the victim was:

**250000**

#### Answer

To verify the exact debt amount, we examined the mmssms.db file, which stores SMS and MMS data on the device.

<img width="2558" height="459" alt="image" src="https://github.com/user-attachments/assets/ff7bc18b-bdc6-4736-b99b-cf8a016db500" />


The mmssms.db database contains:

📩 SMS messages (text messages)
🖼 MMS messages (multimedia messages such as images or files)
🧵 Conversation threads
📞 Sender and recipient phone numbers
⏱ Timestamps
📦 Message content

Upon reviewing the sms table, we identified a message stating:

`"It's time for you to pay back the money you owe me, but you're not picking up my calls. You better think twice about not paying, because it won't end well for you. Prepare the sum of 250,000 EGP, and I'll expect your call within an hour at most."`

This message clearly confirms that the total amount owed by the victim was 250,000 EGP





---

### Q3: Creditor's Name

Analysis of communication records revealed the name of the individual to whom the victim owed money:

**Shady Wahab**


#### Answer

<img width="1888" height="189" alt="image" src="https://github.com/user-attachments/assets/4cbdbea5-2e4a-404c-8d6c-a0c7ac6412e2" />


From the previous analysis of the mmssms.db database, we identified that the threatening message was associated with sender ID 6 in the SMS table.

To determine the real identity behind this ID, we examined the device’s contacts database: `contacts2.db`.

The `contacts2.db` file stores contact information from the device, including:
👤 Contact names
📞 Phone numbers

<img width="1885" height="649" alt="image" src="https://github.com/user-attachments/assets/774f1fcc-5ac1-492b-8147-c0d2ace6ab75" />


By correlating the sender ID from the SMS database with the contact information stored in contacts2.db, we confirmed that the creditor demanding repayment was Shady Wahab.



---

### Q4: Location on September 20, 2023

Location-related artifacts and booking information showed that on September 20, 2023, the victim was at:

**The Nile Ritz-Carlton**

#### Answer 
Initially, we examined location-related data in the following directories:
```
/data/data/com.google.android.apps.maps
/data/data/com.google.android.gms
```
However, no direct or conclusive evidence of the victim’s exact location on the specified date was found in these directories.

Since GPS logs and related artifacts did not provide a definitive answer, we shifted our focus to alternative sources of evidence.


During this analysis, we discovered a screenshot taken by the victim. The image clearly showed the location displayed on the device at that time.

We then examined system snapshot data located at:

`/data/system_ce/0/snapshots/`

<img width="1440" height="3120" alt="image" src="https://github.com/user-attachments/assets/0560f71f-391d-4d15-bb86-9ef248db42d5" />



---

### Q5: Intended Travel Destination

Flight-related data stored on the device revealed the victim’s planned destination:

**Las Vegas**

#### Answer 

During the investigation, we first examined Gmail data located at:

`/data/data/com.google.android.gm/databases`

However, no flight information was found there.

We then checked the device’s download directory:

`/data/media/0/Download`

In this folder, we discovered an image containing the victim’s flight details, which clearly showed a booking to Las Vegas.

<img width="1421" height="461" alt="image" src="https://github.com/user-attachments/assets/033d04ba-22c3-4b9e-a268-6e140c808b31" />






---

### Q6: Discord Meeting Location

Discord conversation analysis showed the victim had arranged to meet a friend at:

**The Mob Museum**

#### Answer 

Initially, we examined the directory:

`/data/data/com.discord/databases`

However, no stored message history was found there, as Discord primarily stores conversations on its servers rather than locally on the device.

Further analysis led us to the following path:

`/data/data/com.discord/files/kv-storage/@account.665825323065016370`

Inside this directory, we identified a file named a, which is a SQLite database used as local storage for that specific Discord account.

Upon examining this database, we discovered cached data containing a reference to:

The Mob Museum
<img width="1882" height="202" alt="image" src="https://github.com/user-attachments/assets/5119093f-a148-4a3a-97c5-7c745a69d63d" />


---

## Skills Demonstrated

- Android artifact analysis using ALEAPP
- SQLite database examination
- Call log and communication analysis
- Location data interpretation
- Timeline reconstruction
- Digital evidence correlation

---

## Conclusion

This investigation demonstrates how mobile forensic analysis can reconstruct financial distress, communication patterns, travel plans, and meeting arrangements from Android device artifacts.

The case highlights the importance of systematic artifact examination and correlation across multiple data sources.
