# Investigation Report: The Planet’s Prestige

## Findings

| Field | Finding |
|---|---|
| **Email Date/Time** | January 26, 2021, at 1:41:18 AM EST |
| **Subject** | `A Hope to CoCanDa` |
| **Sender** | `billjobs@microapple[.]com` |
| **Reply-To** | `negeja3921@pashter[.]com` |
| **Recipient** | `themajoronearth@gmail.com` |
| **Sending Host** | `emkei[.]cz` |
| **Sending IP** | `93[.]99[.]104[.]210` |
| **SPF Result** | Fail |
| **Attachment Name** | `PuzzleToCoCanDa.pdf` |
| **Actual Attachment Type** | ZIP archive |
| **PDF Author Metadata** | `Pestero Negeja` |
| **Recovered Location** | `The Martian Colony, Beside Interplanetary Spaceport` |

> **Metadata Note:** Document metadata can be changed and does not confirm a person’s identity by itself.

### Additional Findings

- The sender and Reply-To addresses used different domains.
- The Received header showed the message coming from `emkei[.]cz` at `93[.]99[.]104[.]210`.
- The SPF check failed, meaning the sending server was not authorized to send email for the claimed sender domain.
- The email body and attachment were Base64 encoded.
- The attachment was presented as a PDF, but its file signature was `50 4B 03 04`, which identified it as a ZIP archive.
- The archive contained two visible files and one hidden spreadsheet:
  - `DaughtersCrown`
  - `GoodJobMajor`
  - `Money.xlsx`
- `DaughtersCrown` was identified as a JPEG image.
- `GoodJobMajor` was identified as a PDF containing additional instructions.
- `Money.xlsx` contained concealed Base64 text that revealed the final location.

## Investigation Summary

On January 26, 2021, at 1:41:18 AM EST, an email titled **“A Hope to CoCanDa”** was sent to `themajoronearth@gmail.com`.

The message claimed responsibility for the disappearance of several CoCanDians and the Planetary President’s daughter. It included a ransom demand and directed the recipient to review an attached file.

The email headers raised several red flags. The SPF check failed, the sender and Reply-To addresses used different domains, and the Received header showed the message coming from `emkei[.]cz` at `93[.]99[.]104[.]210`.

Further analysis showed that the attachment named `PuzzleToCoCanDa.pdf` was actually a ZIP archive based on its file signature. The attachment was decoded and extracted inside an isolated Windows 11 virtual machine.

The archive contained three files: a JPEG image, a PDF with additional instructions, and a hidden spreadsheet. The spreadsheet included concealed Base64 text that decoded to:

> **The Martian Colony, Beside Interplanetary Spaceport**

Overall, the challenge demonstrated how phishing emails can use misleading file extensions, encoded content, hidden files, and concealed data to make an investigation more difficult.

## Who, What, When, Where, Why, and How

### Who

The email targeted the Army Major associated with `themajoronearth@gmail.com`.

The sender used `billjobs@microapple[.]com`, while replies were directed to `negeja3921@pashter[.]com`.

The Author metadata field in `GoodJobMajor.pdf` identified `Pestero Negeja` as the likely document creator. However, metadata can be changed and does not confirm identity by itself.

### What

A simulated phishing email containing an encoded ransom message and a misleading attachment was sent to the recipient.

The attachment appeared to be a PDF but was actually a ZIP archive containing several files with additional instructions and concealed information.

### When

The email was dated:

> **January 26, 2021, at 1:41:18 AM EST**

Because this was a training scenario, the available evidence could not confirm whether the activity continued after the email was sent.

### Where

The email was delivered to:

`themajoronearth@gmail.com`

The email originated from:

- **Host:** `emkei[.]cz`
- **IP address:** `93[.]99[.]104[.]210`

The concealed message identified the location as:

> **The Martian Colony, Beside Interplanetary Spaceport**

### Why

The email initially presented the incident as a ransom-related abduction. Evidence recovered from `Money.xlsx` indicated that the ransom demand was misleading and that the sender’s stated objective was to provoke conflict with the CoCanDians.

### How

The activity used several techniques to conceal information and mislead the recipient:

- Failed SPF authentication
- Sender and Reply-To domain mismatch
- A sending system identifying itself as `localhost`
- Base64-encoded email content
- Base64-encoded attachment data
- A ZIP archive presented as a PDF attachment
- Files stored without extensions
- A hidden spreadsheet
- Text concealed through spreadsheet formatting
- Base64-encoded location information
- Document metadata containing creator information

## Recommendations

1. Search the email environment for messages containing the same sender, Reply-To address, subject, attachment name, sending host, sending IP, or related domains.

2. Quarantine or remove any matching messages from affected mailboxes.

3. Block or monitor the identified indicators, including the sender address, Reply-To address, domains, sending host, and sending IP.

4. Review the recipient’s account and computer for evidence that the attachment was opened, extracted, or executed.

5. Use email-security controls to flag SPF failures, sender and Reply-To mismatches, suspicious sending hosts, and unusual attachments.

6. Use automated email-security tools to safely inspect suspicious attachments and verify their true file types before delivery.

7. Preserve the original email and extracted files as evidence for further investigation.

8. Remind users to report unexpected attachments, ransom messages, or emails that pressure them to open files or follow unusual instructions.
