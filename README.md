# Securely store Proofs of Prior Knowledge using KPOP

KPOP is short for Proof of Prior Knowledge

< Insert a cool logo of the word `KPOP` in big bold letters here, but there's
a mirror that in the reflection reads POPK which is short for Proof of Public
Knowledge >


As a journalists, whistleblower or activist you want to prove the fact that
you knew of something at a specific date. A common way to do this is to share
the hash of your publication on a public platform like Twitter but this
requires you to trust these platforms to not alter content or actively censor
you.

Since 15 July 2020, Twitter has blocked people of posting content that looks
like hashes, to limit the fallout of the [2020 Twitter bitcoin
scam](https://en.wikipedia.org/wiki/2020_Twitter_bitcoin_scam), making the
platform demonstrably unreliable for these purposes. This inspired us to come
up with a more reliably secure way to solve this problem.

Ironically, Bitcoin is a rather useful piece of technology in this regard.
People can put arbitrary data in blocks, that once mined are immutable. This
information is stored in a Merkle tree for efficient retrieval and
verification. This allows you to claim knowledge of certain information at a
specific time, and record that fact securely.

One downside of Bitcoin is that you will have to pay a hefty transaction fee
in order to store information. KPOP also stores its hashes in a distributed
Merkle tree, but one that is not associated with cryptocurrencies. We add
your hashes in certificates that we issue every few hours through Letsencrypt
and publish those certificatres to Certificate Transparency, a distributed
Merkle tree of issued certificates.

Once you have published the hash on our platform, you will get a shareable
link that you can use to prove the hash was published at a specific date and
time. Once you reveal your document, people can check at the trustworthiness
of the document by looking up its entry in Certificate Transparency.

the SANs will point to the CT log entry in question; making it easy to
shortlink to the proof of knowledge.

## Specification

* Certificates are published in batches. Each batch containing several `SAN`s with hashes. This is done to accomodate rate-limits
* Every batch's `CN` is `<YYYY>-<DD>-<MM>-<hh>.kpop.sh`
* The batch's _first_ certificate Not-Before date is between   `hh:00:00` and `hh:59:00`
* The SAN list is in the form of `[0-9a-f]{64}.kpop.sh` denoting the `sha256sum`
* Amount of batches per day is at the discretion of `kpop.sh` to accomodate changes in the ToS of Letsencrypt
* `https://<hash>.crt.sh` will stay reachable and will have its certificate renewed using cerbot
* however it will serve a 301 redirect to the `crt.sh` page of the _first_ certificate issued for that SANs batch
* The user clicking on `https://<hash>.kpop.sh` can look at the `Not-Before` date of the first certificate in `crt.sh` to obtain the proof that the person
submitting the hash knew it at `Not-Before`


## Example


[![asciicast](https://asciinema.org/a/ubAYXKlXuBLkmzfqNkf9ID80j.svg)](https://asciinema.org/a/ubAYXKlXuBLkmzfqNkf9ID80j)



## Related work

This is basically the same as https://wiki.mozilla.org/Security/Binary_Transparency
