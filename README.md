![CanaryTail](https://github.com/canarytail/standard/blob/master/canarytail-logo.png?raw=true)


# Standardized Model for Warrant Canaries
---
A proposal for standardizing the model for warrant canaries to facilitate scalable adoption by enabling automated monitoring and validation.

*This proposal was originally created by [carrotcypher](http://keybase.io/carrotcypher) for consideration of CanaryWatch and CalyxInstitute and contains external research graciously made publicly available by the CalyxInstitute.*

---

## Warrant Canaries
### Introduction

The objective of this document is to present the case for standardized criteria in warrant canaries as a means of simplifying the process of creating, distributing, monitoring, and administrating *​warrant canaries*.

Warrant canaries introduce several issues as have been witnessed from organizations that employed or maintained them improperly. This standard seeks to resolve those issues through a proper standardized model of generation, administration, and distribution, with variance allowed only inside the boundaries of a defined protocol. Those issues are:

* Confusion attributed to non-standardized language
* Loss of access leading to misinterpretation
* Imperfect generation, maintenance, and distribution methods frustrating the overall maintenance process
* Inability to automate the monitoring and reasonable interpretation of canary health
* Reliance on centralized generation and maintenance (single point of failure)
* Reliance on centralized distribution (single point of failure)

---

### Scalability
Thanks in no small part due to the valiant efforts of CanaryWatch (Calyx, EFF, Freedom of the Press Foundation, NYU Law, and the Berkman Center), adoption of warrant canaries — at least in the beginning — was explosive, gaining much traction on the internet as a viable means to combat unconstitutional and unethical intervention from governments through the use of NSLs, gag orders, and their international equivalents.


![Search frequency for the term “Warrant canary” since 2007](https://github.com/canarywatch/standard/blob/master/interest-over-time.png?raw=true "Search frequency for the term “Warrant canary” since 2007")

 ¹ ​ _Search frequency for the term “Warrant canary” since 2007_

While adoption had continued to increase over time, the inability to automate verification and maintenance of those canaries has proven too troublesome for many parties to continue employing them, and without a standardized framework and language for the canaries themselves, not only has it made automation impossible, but misinterpretation is frequently common. Upon discontinuation of the original CanaryWatch project, the Electronic Frontier Foundation was quoted as saying [*"the fact that canaries are non-standard makes it difficult to automatically monitor them for changes or takedowns"*](https://www.eff.org/deeplinks/2016/05/canary-watch-one-year-later).

---

### Standardizing the language

To resolve these issues, we propose a standardization of the very language of the canaries themselves. By standardizing the elements of these warrant canary statements
through codification, one can construct a human and machine readable warrant canary that can be programmatically verified. To consider what language should be standardized, we need to first define the purpose of the canary and under what conditions it is considered failed.

In general, canaries are designed to fail when:

* Warrants have been issued for searching, seizing, or monitoring data
* NSLs, gag orders, or other court orders have been given to hide, conceal, or otherwise withhold information from the public
* Backdoors have been planted to spy on and trap users
* Raids have been conducted on hardware and operating premises
* Integrity of access keys and leadership of a company is no longer sound or trustworthy

In the case of gag orders, some make it unlawful to reveal the exact number of occurrences of requests for data for example, but do allow for an organization to reveal a broad range (e.g. “anywhere from  0  to 1,000”). This combined with a general statement that there has never been a request, one may deduce whether or not a request has in fact been made, and to a certain degree, how many times.

---

### Codification and parameters

Codes are loosely named after their threat and response category:

CODE | | DESCRIPTION
--- | --- | ---
WAR | ........ | Warrants
GAG | ........ | Gag orders
SUBP | ........ | Subpoenas
TRAP | ........ | Trap and trace orders
CEASE | ........ | Court order to cease operations
DURESS | ........ | Coercion, blackmail, or otherwise operating under duress
RAID | ........ | Raids with high confidence nothing containing useful data was seized
SEIZE | ........ | Raids with low confidence nothing containing useful data was seized
XCRED | ........ | Compromised credentials
XOPERS | ........ | Compromised operations
SEPPU | ........ | Seppuku pledge²
---
*² ​A Seppuku pledge is a statement of intent to shut down and cease operations of a company or project, wipe all devices, and destroy all files in the event of an unsurpassable order to operate under the supervision, control, or influence of a malicious entity.*

---

### Construction

Construction of the canary is done as JSON array with multiple options for verification rated by security. Among the fields provided is the ​VERSION​ of the canary codes map used, the date of ​RELEASE​, the date it EXPIRES, and optionally a cryptographically verifiable proof of ​FRESHNESS​ as the latest block hash of the bitcoin blockchain. *This is not to imply that any blockchain technology is required or involved in the canary process, it is merely used as a dating mechanism and can be replaced with other methods such as news story headlines that couldn't have been guessed ahead of time.*


#### Low security

The canary can be authenticated to the original source by providing the ​DOMAIN​ it will be served at. This is a less secure method of authenticity as anyone with publishing access to the domain can modify the canary.


#### Medium security

Authentication of the canary is done via ECDSA (​ Curve25519 ​) cryptographic signature of the squashed JSON array by the public key provided inside as the variable ​PUBKEY​.


#### High security

The ECDSA (​ Curve25519 ​) ​PUBKEY​ can be a split-key created by n-of-p in order to ensure multiple parties consent to updates of the canary.



#### Format

Valid Fields:

`DOMAIN` - specifies the address (if any) tied to the entity. This is used for simplistic TLS/SSL certificate verification of authenticity as well.

`PUBKEY` - specifies the public key (if any) tied to the entity. This is used for signing the canary itself to prove authenticity.

`NEWPUBKEY` - specifies the expected replacement public key (if any) tied to the entity for any future signatures. This is used when signing parties change or a key needs to be updated for whatever reason.

`PANICKEY` - specifies the public key (if any) that can trigger the canary simply by a future canary being signed by it. This is used when the party wishes to end the canary for whatever reason (e.g. business closing, compromised, lost keys to main signing key).

`NEWPANICKEY` - specifies the replacement panic public key (if any) tied to the entity for any future signatures. This is used when signing parties change or a key needs to be updated for whatever reason.

`VERSION` - specifies the protocol version (different protocols may behave differently or contain different codes)

`RELEASE` - specifies when the canary was signed

`EXPIRY` - specifies when the canary should expire

`FRESHNESS` - specifies a recent block hash from the bitcoin blockchain. This is used to provide evidence that the canary was not signed before said block was mined.

`CODES` - specifies an array of the codes that apply to this canary. Missing codes indicate triggered canary.




Usage example:

    {
    "CANARY": {
        "DOMAIN": "cryptanalys.is",
        "PUBKEY": "Y4PsjneDjxckrgibojs38VDWFBTyvlVtTQR3Z9RTRw0=",
        "NEWPUBKEY": "",
        "PANICKEY": "",
        "NEWPANICKEY": "",
        "VERSION": "0.1",
        "RELEASE": "2020-09-30T19:17:40+09:00",
        "EXPIRY": "2030-09-30T19:17:40+09:00",
        "FRESHNESS": "00000000000000000008b4177c1302331aa546635d65306d35c97a8b9bb3461d",
        "CODES": [
            "SEPPU": "none",
            "GAG": "none",
            "TRAP": "none",
            "DURESS": "none",
            "XCRED": "none",
            "XOPERS": "none",
            "WAR": "none",
            "SUBP": "none",
            "CEASE": "none",
            "RAID": "none",
            "SEIZE": "none"
        ]
    },
    "SIGNATURE": "Eajg00cYjeP1Tt//6moo2eMpfHnrd8HZnOqY1XcawoqgwBrF4pIQ984kxN1uLQQHaKfStoWJTM5MKhR5aujTBA=="
   }

Using the canary above, one may programmatically determine that the owner of the PUBKEY​ ​Y4PsjneDjxckrgibojs38VDWFBTyvlVtTQR3Z9RTRw0=​ is confident that:

* they have not received warrants or gag orders of any kind
* they have not been asked to spy on their customers or users or install backdoors
* their premises were not raided with any noteworthy seizures
* their operational integrity and credentials are intact
* they will be able to cease operations if forced to violate the trust of their users
* they are safer if they update their key to a new one

If a canary is signed by the pubkey listed under ​PANICKEY​, it means that whichever party was trusted with the split keys to sign with that key has done so with the intent of forcibly causing the canary to fail before it can expire naturally or through change of contents.

---

### Trust model for signing

Canaries are cryptographically signed to ensure the integrity of the data and to verify the origin of it. There are two basic types of signing models canary authors can choose from:

---

*  **Single party signing** — To generate a canary where the sole authorship and authority to sign it is in the hands of a single entity, one may use a single party signing key. This trust model carries a significant risk of loss and inevitable canary failure, as well as the potential for a malicious entity to seize the key from the owner and sign canaries on their behalf. Signing keys using single party signing should only be done if your threat model deems it necessary.

* **Split key signing** — Distributing the trust to multiple parties, especially geographically distant and in multiple jurisdictions, significantly reduces the likelihood of a canary’s key being seized or completely lost. By splitting the key into several parts, one may share those parts with trusted individuals. Then after convincing the parties that the conditions of the canary have not changed, parties involved may sign the canary as being valid.

Split keys are generated using two data points: the number of total participants you want to share with, and the minimum number of parties required to come together to sign. The ratio of required parties to involved parties should reflect your unique situation. A ratio of ​ 2-of-3 suggests that all parties are moderately trustworthy, have high availability for signing, and it is unlikely that two of the parties will both lose their keys or become compromised. A ratio of ​ 3-of-5 suggests that there is high trust and confidence in the relationship with all parties involved, but equally high likelihood they will lose their key, be unavailable in time for signing, or become compromised.

---


### Automated validation

Client or server software connecting to the distributed database network can download and verify canaries independently, alerting when one has failed. Pending integration by either a plugin or browser’s developer, this functionality could also be automatically performed inside a browser plugin, similarly to how an invalid certificate returns a broken lock symbol for expired or invalid certs.

---

### Decentralized distribution / storing history

To combat censorship and circumvention and to provide a comparable history of previously published canaries, the distribution of canaries can done through a decentralized file sharing system such as IPFS, TahoeLAFS, Torrents, or similar mechanism. With multiple public entities storing their own mirrors of the distributed database of canaries with version changes, submission of canaries in a timely manner is not easily thwarted by censorship tactics, and automated validation can be performed on the published canary.

The distribution of canaries is currently outside the scope of this standard, but is taken into consideration when designing it to be scalable. Future implementations of code may include distribution mechanisms as well.

---

This standard is currently being developing into a PoC CLI client located at https://github.com/canarytail/client.
