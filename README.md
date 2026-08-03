# The Planet’s Prestige — Email Investigation

This repository documents my investigation of **The Planet’s Prestige**, an email investigation challenge from Blue Team Labs Online completed as part of the MYDFIR Forge.

## Project Information

**Platform:** [Blue Team Labs Online](https://blueteamlabs.online/home/challenge/the-planets-prestige-e5beb8e545)<br>
**Project Type:** Email Investigation<br>
**Status:** In Progress

## Objective

The objective of this investigation is to analyze the provided email evidence, identify suspicious activity and indicators of compromise, and determine what occurred based on the available artifacts.

## Scenario

CoCanDa has experienced a series of unexplained citizen abductions, causing widespread unrest across the planet. After government and military leaders met to address the crisis, the Planetary President learned that his daughter had also disappeared.

Despite an extensive search, investigators received no ransom demand, communication, or useful information about the missing individuals. Three days after the President’s daughter disappeared, a CoCanDa representative serving as an Army Major on Earth received a suspicious email.

The investigation begins with an analysis of that email to determine whether it contains information connected to the disappearances.

## Tools Used

- Notepad++
- CyberChef

## Investigation Process

### 1. Initial Review

The provided evidence was an `.eml` file named `A Hope to CoCanDa.eml`. I opened the file in Notepad++ to review the raw email contents, including the headers, MIME structure, encoded message body, and attachment information.

The email was identified as a multipart MIME message containing a Base64-encoded plain-text body.

<img width="717" height="407" alt="01-email-mime-base64-body" src="https://github.com/user-attachments/assets/49f7a908-5032-43d3-ba86-5afa6b6850d8" />

The Base64-encoded message body was copied into CyberChef and decoded using the `From Base64` operation.

<img width="2034" height="806" alt="02-cyberchef-decoded-email-body" src="https://github.com/user-attachments/assets/25667a4b-6f0a-4b83-a0c3-0a22251b761c" />

The decoded message contained a ransom demand and instructed the recipient to solve an attached puzzle for further information. The phrase `Don't Trust Your Eyes` appeared to be an important clue for analyzing the attachment.

#### Initial Email Details
> **Note:** Potentially malicious domains, URLs, email addresses, and IP addresses have been defanged to prevent accidental access.

- **Filename:** `A Hope to CoCanDa.eml`
- **Sender:** `billjobs[@]microapple[.]com`
- **Recipient:** `themajoronearth@gmail.com`
- **Subject:** `A Hope to CoCanDa`
- **Date:** `Tue, 26 Jan 2021 01:41:18 -0500 (EST)`
- **Reply-To:** `negeja3921[@]pashter[.]com`
- **Return-Path:** `billjobs[@]microapple[.]com`
- **Message-ID:** `20210126064118.1993E221F8@localhost`
- **Attachment:** `PuzzleToCoCanDa.pdf`
- **Sending IP:** `93[.]99[.]104[.]210`
- **SPF Result:** Fail

#### Initial Observations

- The Reply-To address does not match the visible sender address.
- SPF failed because the sending IP was not authorized to send mail for `microapple[.]com`.
- The email contains a Base64-encoded PDF attachment.

### 2. Email Header Analysis


<!--

Review items such as:

* From
* Reply-To
* Return-Path
* Received headers
* Sending IP address
* Message ID
* SPF results
* DKIM results
* DMARC results

### 3. Link and Domain Analysis

Document any domains or URLs discovered during the investigation.

Include:

* Domain names
* Full URLs
* Redirects
* Registration information
* Reputation results
* Suspicious characteristics

### 4. Attachment Analysis

Document any attachments examined during the investigation.

Include:

* Filename
* File type
* File size
* File hash
* Analysis results
* Suspicious behavior or characteristics

Do not open potentially malicious files directly on your main computer.

### 5. Findings

Summarize the important evidence discovered during the investigation.

* Finding 1
* Finding 2
* Finding 3

## Indicators of Compromise

| Indicator     | Type                                      | Description                  |
| ------------- | ----------------------------------------- | ---------------------------- |
| Add indicator | Email, IP, domain, URL, hash, or filename | Explain why it is suspicious |

## Timeline

| Time          | Event                  |
| ------------- | ---------------------- |
| Add timestamp | Describe what occurred |

## MITRE ATT&CK Mapping

Only include techniques that are supported by the evidence.

| Technique        | Name               | Evidence                                          |
| ---------------- | ------------------ | ------------------------------------------------- |
| Add technique ID | Add technique name | Explain how the evidence relates to the technique |

## Conclusion

Provide your final assessment of the investigation.

Explain:

* Whether the email was malicious, suspicious, or legitimate
* What the suspected attacker attempted to accomplish
* Which evidence supports your conclusion
* Whether further investigation or escalation would be required

## Recommended Actions

Document the containment and remediation actions you would recommend.

Examples may include:

* Block identified domains, URLs, or IP addresses
* Remove the email from affected mailboxes
* Reset compromised account credentials
* Review account sign-in activity
* Isolate affected systems
* Scan attachments and endpoints
* Notify affected users
* Update detection rules

Only include recommendations relevant to your findings.

## Skills Practiced

* Email and phishing analysis
* Email-header analysis
* Indicator identification
* Evidence collection
* Investigation documentation
* Threat analysis
* MITRE ATT&CK mapping
* Remediation planning

## Screenshots

Supporting screenshots will be included throughout the investigation where appropriate.

Screenshots should demonstrate the investigative process without exposing protected challenge answers, flags, or unnecessary sensitive information.

## Disclaimer

This project was completed for educational and professional-development purposes.

The challenge belongs to Blue Team Labs Online. This repository documents my investigative process and is not intended to distribute protected challenge answers or flags.

-->
