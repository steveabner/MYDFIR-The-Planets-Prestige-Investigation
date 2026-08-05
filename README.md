# The Planet’s Prestige — Email Investigation

This repository documents my investigation of **The Planet’s Prestige**, an email investigation challenge from Blue Team Labs Online completed as part of the MYDFIR Forge.

## Investigation Report

A shorter, formal version of this investigation is available for SOC leads and managers.

[View the formal investigation report](Report.md)

## Project Information

**Platform:** [Blue Team Labs Online](https://blueteamlabs.online/home/challenge/the-planets-prestige-e5beb8e545)<br>
**Project Type:** Email Investigation<br>
**Status:** Completed

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
- HxD
- LibreOffice Calc
- ExifTool

## Investigation Process

### 1. Initial Review

The provided evidence was an `.eml` file named `A Hope to CoCanDa.eml`. I opened the file in Notepad++ to review the raw email contents, including the headers, MIME structure, encoded message body, and attachment information.

The email was identified as a multipart MIME message containing a Base64-encoded plain-text body. After decoding the body in CyberChef, I identified a ransom demand and instructions to solve an attached puzzle.

<details>
<summary><strong>▶ Click to expand: MIME structure and Base64 analysis</strong></summary>

<br>

The raw email showed a multipart MIME structure and a Base64-encoded plain-text body.

<img width="717" height="407" alt="Raw MIME structure and Base64-encoded email body" src="https://github.com/user-attachments/assets/49f7a908-5032-43d3-ba86-5afa6b6850d8" />

The Base64-encoded message body was copied into CyberChef and decoded using the `From Base64` operation.

<img width="2034" height="806" alt="CyberChef decoding the Base64 email body" src="https://github.com/user-attachments/assets/25667a4b-6f0a-4b83-a0c3-0a22251b761c" />

</details>

### 2. Email Header Analysis

The email failed SPF authentication because the sending IP address `93[.]99[.]104[.]210` was not authorized to send email on behalf of `microapple[.]com`.

The visible sender was `billjobs[@]microapple[.]com`, while the Reply-To address was `negeja3921[@]pashter[.]com`. This meant replies would be directed to a different domain.

<details>
<summary><strong>▶ Click to expand: Email-header evidence</strong></summary>

<br>

<img width="839" height="488" alt="Email authentication results and Reply-To mismatch" src="https://github.com/user-attachments/assets/4bb8746d-3fa9-49b6-8845-778990279247" />

</details>

### 3. Attachment Analysis

> **Safety Note:** All attachment analysis and file extraction were performed inside an isolated Windows virtual machine.

The raw email contained an attachment named `PuzzleToCoCanDa.pdf`. Its MIME headers declared it as an `application/pdf`, and its contents were encoded using Base64.

#### Attachment Findings

- **Declared filename:** `PuzzleToCoCanDa.pdf`
- **Declared content type:** `application/pdf`
- **Transfer encoding:** Base64
- **Observed file signature:** `50 4B 03 04`
- **Identified file type:** ZIP-based archive
- **Finding:** The attachment’s actual file type did not match its `.pdf` extension or declared MIME type.

This mismatch showed that the attachment was presented as a PDF even though its file signature identified it as a ZIP-based archive. Because the file type did not match the declared extension, I continued the analysis inside an isolated Windows virtual machine.

<details>
<summary><strong>▶ Click to view attachment MIME data</strong></summary>

<br>

The raw email showed that the attachment was labeled as a PDF and encoded using Base64.

<img width="684" height="284" alt="Attachment MIME headers and Base64 content" src="https://github.com/user-attachments/assets/dd119098-8680-4fa1-a8e4-7226c73f07f9" />

</details>

<details>
<summary><strong>▶ Click to expand: File-signature verification</strong></summary>

<br>

I copied the Base64-encoded attachment data into CyberChef and used the `From Base64` operation followed by `To Hex` to examine the file's first four bytes.

<img width="1294" height="723" alt="CyberChef displaying the attachment file signature" src="https://github.com/user-attachments/assets/df7567e2-f5bb-4b23-93c9-53148c2f7063" />

The decoded file began with: `50 4B 03 04`

Gary Kessler’s file-signature reference identifies this as a ZIP-based archive signature.

<img width="1035" height="677" alt="ZIP file signature reference" src="https://github.com/user-attachments/assets/fde67fb7-7a42-4409-9f4a-eb8c439b922d" />

For comparison, a typical PDF begins with: `25 50 44 46` which represents `%PDF`.

<img width="1039" height="473" alt="PDF file signature reference" src="https://github.com/user-attachments/assets/801da4ed-f6c6-48d2-800f-4e331c635258" /> 

</details>

#### Archive Extraction

After confirming that the attachment was a ZIP-based archive, I removed the `To Hex` operation from CyberChef and saved the decoded output as `email_attachment.zip`.

The archive contained a folder named `PuzzleToCoCanDa`. Initial review showed two visible files, `DaughtersCrown` and `GoodJobMajor`. After enabling **Hidden items** in File Explorer, I identified a third file named `Money.xlsx`.

The files were not opened directly. Their actual file types and contents still needed to be verified.

<details>
<summary><strong>▶ Click to expand: Archive extraction steps</strong></summary>

<br>

The decoded Base64 output was saved as `email_attachment.zip`.

<img width="1514" height="843" alt="Saving the decoded archive from CyberChef" src="https://github.com/user-attachments/assets/e5c79729-4e82-4a7a-a6ff-a381e37ce7ba" />

The archive was extracted inside the virtual machine.

<img width="1214" height="828" alt="09-email-attachment-zip-extraction" src="https://github.com/user-attachments/assets/54681136-87b6-446d-8cfe-7389e166005c" />

The extracted archive contained a folder named `PuzzleToCoCanDa`.

<img width="1215" height="736" alt="Extracted PuzzleToCoCanDa folder" src="https://github.com/user-attachments/assets/d13cd895-e272-4922-a0b5-3e1a2f1d3a68" />

Inside the folder were the files `DaughtersCrown` and `GoodJobMajor`.

<img width="1270" height="774" alt="Files extracted from the archive" src="https://github.com/user-attachments/assets/29f8aa77-a594-4bf8-aee7-21810d2c1a6a" />

#### Hidden File Discovery

While reviewing the extracted folder, I enabled **Hidden items** in Windows File Explorer. This revealed an additional file named `Money.xlsx` that was not visible by default.

<img width="1247" height="734" alt="12-hidden-money-xlsx-discovered" src="https://github.com/user-attachments/assets/5e30c064-e303-4bd4-ab78-e35937b88125" />

The extracted folder therefore contained three files:

- `DaughtersCrown`
- `GoodJobMajor`
- `Money.xlsx` — hidden

The hidden status of `Money.xlsx` increased its relevance to the investigation and warranted further file-type and content analysis.

</details>

### 4. Extracted File Analysis

#### DaughtersCrown

The extracted file `DaughtersCrown` did not include a file extension, so I opened it in HxD to inspect its file signature before attempting to open it.

The first four bytes were `FF D8 FF E0`.

According to Gary Kessler’s file-signature reference, `FF D8 FF E0` is associated with a standard JPEG/JFIF image.

<details>
<summary><strong>▶ Click to expand: DaughtersCrown file-signature verification</strong></summary>

<br>

The file was inspected in HxD, where the first four bytes were identified as `FF D8 FF E0`.

<img width="836" height="676" alt="13-daughterscrown-file-signature-hxd" src="https://github.com/user-attachments/assets/3fe18e81-ff81-4923-b605-9ddbdfb5ef15" />

The signature was then compared with Gary Kessler’s file-signature reference and matched the standard JPEG/JFIF signature.

<img width="1096" height="561" alt="14-jpeg-file-signature-reference" src="https://github.com/user-attachments/assets/bcf692e8-ab3d-4973-9a54-5fcfe609f7d9" />

</details>

After verifying the file type, I renamed the file from `DaughtersCrown` to `DaughtersCrown.jpeg`.

<details>
<summary><strong>▶ Click to expand: Renamed JPEG file</strong></summary>

<br>

<img width="894" height="452" alt="15-daughterscrown-renamed" src="https://github.com/user-attachments/assets/d10e9c9c-9f1a-4c3c-a374-680ce21e9061" />

</details>

After confirming the file type, I opened the image inside the isolated Windows virtual machine. The image displayed a crown graphic.

<details>
<summary><strong>▶ Click to expand: DaughtersCrown image</strong></summary>

<br>

<img width="1171" height="881" alt="16-daughterscrown-image" src="https://github.com/user-attachments/assets/bc938d70-2fe3-4523-b670-6ef273c4d1e4" />

</details>

#### DaughtersCrown Findings

- **Original filename:** `DaughtersCrown`
- **Original extension:** None
- **Observed file signature:** `FF D8 FF E0`
- **Identified file type:** JPEG/JFIF image
- **Renamed file:** `DaughtersCrown.jpeg`
- **Observed content:** Image of a crown

#### GoodJobMajor

The extracted file `GoodJobMajor` did not include a file extension, so I opened it in HxD to inspect its file signature before attempting to open it.

The first four bytes were `25 50 44 46`.

According to Gary Kessler’s file-signature reference, `25 50 44 46` is associated with a PDF file.

<details>
<summary><strong>▶ Click to expand: GoodJobMajor file-signature verification</strong></summary>

<br>

The file was inspected in HxD, where the first four bytes were identified as `25 50 44 46`.

<img width="840" height="473" alt="17-goodjobmajor-file-signature-hxd" src="https://github.com/user-attachments/assets/5f1fac7c-f149-472e-b141-fcc5ff257e16" />

The signature was then compared with Gary Kessler’s file-signature reference and matched the standard PDF signature.

<img width="1088" height="437" alt="18-pdf-file-signature-reference" src="https://github.com/user-attachments/assets/96af82ef-707a-4850-ae6f-005c0a264ebe" />

</details>

After verifying the file type, I renamed the file from `GoodJobMajor` to `GoodJobMajor.pdf`.

<details>
<summary><strong>▶ Click to expand: Renamed PDF file</strong></summary>

<br>

<img width="914" height="441" alt="19-goodjobmajor-renamed-pdf" src="https://github.com/user-attachments/assets/e1578a11-0380-40aa-85bb-a154c2ac768c" />

</details>

After confirming the file type, I opened the PDF inside the isolated Windows virtual machine. The document stated that the CoCanDians were safe, referenced the `DaughtersCrown` file as proof, and indicated that the location to send 1 Billion CoCanDs was in `Money.xlsx`.

<details>
<summary><strong>▶ Click to expand: GoodJobMajor PDF content</strong></summary>

<br>

<img width="812" height="578" alt="20-goodjobmajor-pdf-content" src="https://github.com/user-attachments/assets/050c2afe-7349-4d78-b57c-fb256acb0962" />

</details>

#### GoodJobMajor Findings

- **Original filename:** `GoodJobMajor`
- **Original extension:** None
- **Observed file signature:** `25 50 44 46`
- **Identified file type:** PDF
- **Renamed file:** `GoodJobMajor.pdf`
- **Observed content:** Message indicating the CoCanDians are safe, referencing `DaughtersCrown` as proof, and directing the ransom location to `Money.xlsx`

#### Metadata Analysis

I used ExifTool to inspect the metadata of `GoodJobMajor.pdf`. The PDF’s `Author` field contained the first and last name requested in the investigation.

<details>
<summary><strong>⚠️ Click to reveal retired-lab answer and metadata evidence</strong></summary>

<br>

<img width="976" height="475" alt="27-goodjobmajor-exiftool-metadata" src="https://github.com/user-attachments/assets/b2462301-a0f9-47ea-9c6a-f700da84f3b7" />

#### Metadata Findings

- **Analyzed file:** `GoodJobMajor.pdf`
- **Author:** `Pestero Negeja`
- **Producer:** `Skia/PDF m90`
- **Page count:** 1

The author metadata identified `Pestero Negeja` as the likely creator of the PDF and provided the name requested in the investigation.

</details>

#### Metadata Findings

- **Analyzed file:** `GoodJobMajor.pdf`
- **Author:** `Pestero Negeja`
- **Producer:** `Skia/PDF m90`
- **Page count:** 1

The author metadata identified Pestero Negeja as the likely document creator.

#### Money.xlsx

The hidden file `Money.xlsx` already included a file extension, but I still inspected it in HxD to verify that the file signature matched the declared spreadsheet format.

The first four bytes were `50 4B 03 04`.

According to Gary Kessler’s file-signature reference, this signature is associated with Microsoft Office Open XML files, including `.xlsx` spreadsheets.

<details>
<summary><strong>▶ Click to expand: Money.xlsx file-signature verification</strong></summary>

<br>

The file was inspected in HxD, where the first four bytes were identified as `50 4B 03 04`.

<img width="836" height="376" alt="21-money-xlsx-file-signature-hxd" src="https://github.com/user-attachments/assets/b456e69a-04f0-471e-80c7-edf2967bd6ff" />

The signature was compared with Gary Kessler’s file-signature reference and matched the Microsoft Office Open XML format used by `.xlsx` files.

<img width="1097" height="542" alt="22-xlsx-file-signature-reference" src="https://github.com/user-attachments/assets/7fd1a2ec-f280-40ce-850d-2d5a3311fb28" />

</details>

After confirming the file type, I opened `Money.xlsx` using LibreOffice Calc inside the isolated Windows virtual machine.

> **Analysis Note:** In a production environment, a disposable cloud-based file viewer or dedicated malware-analysis sandbox would be preferable for inspecting an untrusted spreadsheet. For this lab, I used LibreOffice Calc inside an isolated Windows virtual machine.

The first worksheet contained a message stating that the earlier information was false and that the attackers intended to begin a war with the CoCanDians.

<details>
<summary><strong>▶ Click to expand: Money.xlsx first worksheet</strong></summary>

<br>

<img width="1255" height="744" alt="23-money-xlsx-sheet1-message" src="https://github.com/user-attachments/assets/cd080d57-ea1e-4b93-a5df-57f70d80ab6e" />

</details>

A second visible worksheet, named `Sheet3`, initially appeared empty. After clearing the cell formatting, I discovered text that had been concealed by its formatting.

<details>
<summary><strong>▶ Click to expand: Hidden text discovery in Sheet3</strong></summary>

<br>

The worksheet initially appeared blank.

<img width="1258" height="745" alt="24-money-xlsx-sheet3-blank" src="https://github.com/user-attachments/assets/fb8f1323-81ca-4362-b230-4ebe73a5a1cb" />

After clearing the formatting, an encoded string became visible.

<img width="1254" height="747" alt="25-money-xlsx-sheet3-hidden-text-revealed" src="https://github.com/user-attachments/assets/9d139727-93b7-4fe1-9aad-ca05cbae7bec" />

</details>

#### Base64 Decoding

The encoded string revealed in `Sheet3` was copied into CyberChef and decoded using the `From Base64` operation.

<details>
<summary><strong>▶ Click to expand: Base64 decoding of the concealed Sheet3 text</strong></summary>

<br>

<img width="1420" height="637" alt="26-money-xlsx-sheet3-base64-decoded" src="https://github.com/user-attachments/assets/77ecce47-9dfb-4b9f-97dd-b662c72c1b0c" />

</details>

The decoded message revealed the following location: `The Martian Colony, Beside Interplanetary Spaceport.`

This finding identified the location referenced in `GoodJobMajor.pdf` and provided the next lead in the investigation.

#### Money.xlsx Findings

- **Filename:** `Money.xlsx`
- **Observed file signature:** `50 4B 03 04`
- **Verified file type:** Microsoft Excel Open XML spreadsheet
- **Visible worksheets:** `Sheet1` and `Sheet3`
- **Sheet1 content:** Message claiming the previous information was false and threatening war with the CoCanDians
- **Sheet3 content:** Base64-encoded text concealed through cell formatting
- **Decoded location:** `The Martian Colony, Beside Interplanetary Spaceport.`

### 5. Findings Summary

The investigation began with a suspicious `.eml` file containing a ransom message and an attachment presented as `PuzzleToCoCanDa.pdf`.

Analysis of the email headers identified several suspicious indicators:

- The message failed SPF authentication.
- The visible sender and Reply-To addresses used different domains.
- The attachment was Base64-encoded and presented as a PDF.

File-signature analysis showed that the attachment was actually a ZIP-based archive. After decoding and extracting the archive inside an isolated Windows virtual machine, three files were discovered:

- `DaughtersCrown`
- `GoodJobMajor`
- `Money.xlsx`, which was hidden

Further analysis identified the files as:

- `DaughtersCrown` — JPEG image showing a crown
- `GoodJobMajor` — PDF containing additional instructions
- `Money.xlsx` — Excel spreadsheet containing a hidden Base64-encoded message

The concealed Base64 content in `Money.xlsx` decoded to:

`The Martian Colony, Beside Interplanetary Spaceport.`

This location represented the final lead uncovered during the investigation.

## Conclusion

The email was malicious and used sender spoofing, failed email authentication, misleading file extensions, hidden files, and encoded content to conceal the attacker’s instructions.

By analyzing the raw email, decoding Base64 content, verifying file signatures, extracting the disguised archive, and examining the recovered files, I identified the location provided by the attacker:

`The Martian Colony, Beside Interplanetary Spaceport.`

The findings were validated against the Blue Team Labs Online challenge, and the investigation was completed successfully.

## Challenge Completion

The investigation findings were successfully validated, and **The Planet’s Prestige** challenge was completed on August 4, 2026.

<img width="802" height="616" alt="28-the-planets-prestige-completion" src="https://github.com/user-attachments/assets/759b9dd7-9f24-4304-8225-30a8c37135c0" />

