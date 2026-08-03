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

- Windows 11 Virtual Machine
- Notepad++
- CyberChef
- Gary Kessler's File Signature Database

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
- The email contains a Base64-encoded attachment presented as a PDF file.

### 2. Email Header Analysis

The email headers showed that the message failed SPF authentication. The sending IP address `93[.]99[.]104[.]210` was not authorized to send email on behalf of `microapple[.]com`.

The visible sender was `billjobs[@]microapple[.]com`, while the Reply-To address was `negeja3921[@]pashter[.]com`, indicating that replies would be redirected to a different domain.

<img width="839" height="488" alt="03-email-header-authentication-results" src="https://github.com/user-attachments/assets/4bb8746d-3fa9-49b6-8845-778990279247" />

### 3. Attachment Analysis
> **Safety Note:** All attachment analysis and file extraction were performed inside an isolated Windows virtual machine.

The raw email contained an attachment named `PuzzleToCoCanDa.pdf`. Its MIME headers declared the file as an `application/pdf`, and the attachment content was encoded using Base64.

<img width="684" height="284" alt="04-attachment-mime-base64" src="https://github.com/user-attachments/assets/dd119098-8680-4fa1-a8e4-7226c73f07f9" />

---

To verify the attachment's true file type, I copied the Base64-encoded attachment data into CyberChef. I then used the `From Base64` operation followed by `To Hex` to examine the file's first four bytes.

<img width="1294" height="723" alt="05-attachment-file-signature-cyberchef" src="https://github.com/user-attachments/assets/df7567e2-f5bb-4b23-93c9-53148c2f7063" />

---

The decoded file began with the hexadecimal signature: `50 4B 03 04`

According to Gary Kessler’s file-signature reference, `50 4B 03 04` is associated with ZIP and other ZIP-based archive formats.

<img width="1035" height="677" alt="06-zip-file-signature-reference" src="https://github.com/user-attachments/assets/fde67fb7-7a42-4409-9f4a-eb8c439b922d" />

---

For comparison, a typical PDF file begins with the hexadecimal signature: `25 50 44 46` which represents `%PDF`.

<img width="1039" height="473" alt="07-pdf-file-signature-reference" src="https://github.com/user-attachments/assets/801da4ed-f6c6-48d2-800f-4e331c635258" />

#### Attachment Findings

- **Declared filename:** `PuzzleToCoCanDa.pdf`
- **Declared content type:** `application/pdf`
- **Transfer encoding:** Base64
- **Observed file signature:** `50 4B 03 04`
- **Identified file type:** ZIP-based archive
- **Finding:** The attachment’s actual file type does not match its `.pdf` extension or declared MIME type.

This mismatch showed that the attachment was presented as a PDF even though its file signature identified it as a ZIP-based archive. Because the file type did not match the declared extension, I continued the analysis inside an isolated Windows virtual machine and extracted the archive for further inspection.

#### Archive Extraction

After confirming that the attachment was a ZIP-based archive, I returned to CyberChef, removed the `To Hex` operation, and saved the decoded Base64 output as `email_attachment.zip`.

> **Safety Note:** All attachment analysis and file extraction were performed inside an isolated Windows virtual machine.

<img width="1514" height="843" alt="08-cyberchef-save-decoded-archive" src="https://github.com/user-attachments/assets/e5c79729-4e82-4a7a-a6ff-a381e37ce7ba" />

---

The downloaded archive was then extracted inside the virtual machine.

<img width="1214" height="828" alt="09-email-attachment-zip-extraction" src="https://github.com/user-attachments/assets/a8252f4f-cd7c-4357-ae32-8bcf8c87169f" />

---

The extracted archive contained a folder named `PuzzleToCoCanDa`.

<img width="1215" height="736" alt="10-extracted-puzzle-folder" src="https://github.com/user-attachments/assets/d13cd895-e272-4922-a0b5-3e1a2f1d3a68" />

---

Inside the folder, I identified two files:

- `DaughtersCrown`
- `GoodJobMajor`

<img width="1270" height="774" alt="11-extracted-archive-files" src="https://github.com/user-attachments/assets/29f8aa77-a594-4bf8-aee7-21810d2c1a6a" />

---

At this stage, the files were not opened directly. Their actual file types and contents still needed to be verified before further analysis.



<!--

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
