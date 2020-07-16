# KPOP

KPOP is short for Proof of Public Knowledge


Inspired by the fact that twitter is often used by journalists to post hashes of documents to prove
they had knowledge of something at a specific date.

However, posting hashes has now been banned on twitter, due to the recent bitcoin hack.

Funnily enough Bitcoin is often also used for the usecase of proving public knowledge.  However you have to pay transaction fees
to get information in the block.

Instead, we decide to store the proofs inside CT logs.  Which will record the fact when the certificate
was signed until eternity in a tamper-proof way. Each proof of public knowledge (with is the hash of a document) will
be stored as a SAN in a letsencrypt certificate.    proofs are batched in certificates by the hour to reduce load on letsencrypt's service.

the SANs will point to the CT log entry in question; making it easy to shortlink to the proof of knowledge. 

