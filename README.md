# KPOP

KPOP is short for Proof of Public Knowledge

< Insert a cool logo of the word `KPOP` in big bold letters here, but there's a mirror that in the reflection reads POPK which is short for Proof of Public Knowledge > 

<Insert KPOP Stan Videos here>

Inspired by the fact that twitter is often used by journalists , whisteblowers and activists to post hashes of documents to prove
they had knowledge of something at a specific date.

However, posting hashes has now been banned on twitter, due to the recent bitcoin hack.

Funnily enough Bitcoin is often also used for the usecase of proving public knowledge.  However you have to pay transaction fees
to get information in the block.

Instead, we decide to store the proofs inside [Certificate Transparency](https://www.certificate-transparency.org/) logs.  Which will record the fact when the certificate
was signed until eternity in a tamper-proof way. Each proof of public knowledge (with is the hash of a document) will
be stored as a SAN in a letsencrypt certificate.    proofs are batched in certificates by the hour to reduce load on letsencrypt's service.

the SANs will point to the CT log entry in question; making it easy to shortlink to the proof of knowledge. 

## Specification

* Every batch's `CN` is `<YYYY>-<DD>-<MM>-<hh>.kpop.sh`
* The batch's _first_ certificate Not-Before date is between   `hh:00:00` and `hh:59:00`
* The SAN list is in the form of `[0-9a-f]{64}.kpop.sh` denoting the `sha256sum`
* Amount of batches per day is at the discretion of `kpop.sh` to accomodate changes in the ToS of Letsencrypt
* `https://<hash>.crt.sh` will stay reachable and will have its certificate renewed using cerbot
* however it will serve a 301 redirect to the `crt.sh` page of the _first_ certificate issued for that SANs batch
* The user clicking on `https://<hash>.kpop.sh` can look at the `Not-Before` date of the first certificate in `crt.sh` to obtain the proof that the person
submitting the hash knew it at `Not-Before`


## Example

I disclosed on `2020-07-16T20:47:38+02:00` that I know of the following text:

```
hello
```

My proof:

https://5891b5b522d5df086d0ff0b110fbd9d21bb4fc7163af34d08286a2e846f6be03.kpop.sh
