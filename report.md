# Security Assessment Report

## Table of contents
<ol>
  <li><a href="#summary-of-findings">Summary of findings</a></li>
  <li><a href="#type-of-tools-used">Type of tools used</a></li>
  <li><a href="#potential-threats-and-impact">Potential Threats and Impact</a></li>
  <li><a href="#cia-triad">CIA Triad</a></li>
  <li><a href="#owasp-recommendations">OWASP recommendations</a></li>
  <li><a href="#executive-summary">Executive Summary</a></li>
</ol>

## Summary of findings
Of the findings discovered during our assessment, 5 were considered High risks, 4 Moderate risks, 2 Low risks. 

- Total: 11 risks

## Type of tools used
Developer Tools (Intergrated on most web broswer)
Burp Suite Community Edition

## Potential Threats and Impact
| Threat Origination Category      | Type Identifier |
| ----------- | ----------- |
| Threats launched purposefully      | P       |
| Threats created by unintentional human or machine   | U        |
| Threats caused by environmental agents or disruptions | E |

| Likelihood | Description |
| ------------- | ----------- |
| Low | Low chance to expolit and impact to the system. |
| Moderate | Moderate chance that a threat could exploit a vulnerability and cause loss to the system or its data. |
| High | There is a high chance that a threat could exploit a vulnerability and cause loss to the system or its data. |

| ID | Threat Name | Type Identifier | Description |  Image  | Likelihood  | 
| ----------- | ----------- | ----------- | ----------- | ----------- | --------- | 
| T1 | Internal Error Leak | P, U | Internal Error are exposed to the end user. They can be used to find vulnability in the system | ![Link](https://i.imgur.com/iR6MFR8.png) | Low |
| T2 | Limited Password Field | U | Password Field is limited to 10 chararters and restricts the use of special charaters | | Moderate | 
| T3 | Denined Of Service risk | P, U | Attacker can lock someone account by request to reset the password | | Moderate | 
| T4 | Vote API don't validate userID | P | Attacker can vote on behalf of other user | ![image](https://i.imgur.com/RATfy5G.png) | High |  
| T5 | Vote API don't check for repeat comments | P | Attacker can vote multiple times (Front end won't allow) | | Moderate |
| T6 | Response header expose internal system | P | Attacker can use the infomation to find vulnability (X-AspNet-Version, X-AspNetMvc-Version, X-Powered-By) | ![image](https://i.imgur.com/onESMcg.png) | Low |
| T7 | Server only check cookie for admin permission | P | Attacker can change IsAdmin cookie to true to gain Admin | ![image](https://i.imgur.com/yUQ0bDu.png) | High |  
| T8 | Not encrypt password | E, P | If the system compromised, the password can be reuse on others server | | High | 
| T9 | Directory leak | P | robots.txt disclose about /secret/admin and /api/admin/users path | ![image](https://i.imgur.com/itQBOBz.png) | Moderate |
| T10 | Clickjacking | P | Attacker can use Clickjacking to steal user infomation  | | High |
| T11 | XSRF Attack | P | Attacker can change any one firstname, lastname and admin status | ![image](https://i.imgur.com/2lK6HQx.png) | High |


## CIA Triad
| ID | Confidentiality | Integrity | Availability |  
| ----------- | :-----------: | :-----------: | :-----------: | 
| T1 | :white_check_mark:  | | |
| T2 | :white_check_mark: | | |
| T3 | | | :white_check_mark: |
| T4 | | :white_check_mark: | |
| T5 | | :white_check_mark: | :white_check_mark: |
| T6 | :white_check_mark: | | |
| T7 | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| T8 | :white_check_mark: | | |
| T9 | :white_check_mark: | | |
| T10 | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| T11 | | :white_check_mark: | |

## OWASP Recommendations
Reference: [OWASP Quick Reference Guide V2](https://owasp.org/www-pdf-archive/OWASP_SCP_Quick_Reference_Guide_v2.pdf)

- Input Validation: 
    
    - Conduct all data validation on a trusted system
    - Identify all data sources and classify them into trusted and untrusted. Validate all data from untrusted sources (e.g., Databases, file streams, etc.)
    - Specify proper character sets, such as UTF-8, for all sources of input

- Authentication and Password Management:

    - Enforce password complexity requirements established by policy or regulation (e.g., requiring the use of alphabetic as well as numeric and/or special characters)
    -  Enforce password length requirements established by policy or regulation. Eight characters is commonly used, but 16 is better or consider the use of multi-word pass phrases
    - If your application manages a credential store, it should ensure that only cryptographically strong oneway salted hashes of passwords are stored and that the table/file that stores the passwords and keys is write-able only by the application. 
    - Password hashing must be implemented on a trusted system.

- Error Handling and Logging:

    - Do not disclose sensitive information in error responses, including system details, session identifiers or account information
    - Use a cryptographic hash function to validate log entry integrity

## Executive Summary

| ID | Solutions | Reference |
| -- | --------- | --------- |
| T1 |  Hide error log from users  | [Stackoverflow Article](https://stackoverflow.com/questions/8824581/how-to-disable-net-event-log-warnings/8825230#8825230) 
| T2 | Increase maximum, set minimum limit chararters of password and accept special chararters | | 
| T3 | Reset password won't lock down user password until the link in email is click and processed | [OWASP Cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html) |
| T4 & T5 & T11 | Validate in backend UserId with logged in user |  |
| T6 | Remove headers | [Guide by Solarwinds](https://support.solarwinds.com/SuccessCenter/s/article/Disable-the-IIS-web-banner-and-other-IIS-headers-in-the-Orion-Platform?language=en_US)
| T7 | Validate in backend to make user that user is Admin | |
| T8 | Only store hashed and salted password | [OWASP Cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)|
| T9 | Config server to limit access to that directory | [Tenable](https://www.tenable.com/plugins/nessus/10302)
| T10 | Use X-Frame-Options header to prevent the browser from loading the frame | [OWASP Cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html) |  
