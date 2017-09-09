### Securing Automated Decryption

New Cryptography and Techniques

Date: Sun 5th Feb
Time: 13:00
Speaker: Nathaniel McCallum

Key escrows using Diffe-Helman Exchange and McCallum-Relyea Exchange.

Generate two different keys before both encryption and decryption phases, the mix the keys together to perform the decryption.

So even if you listen to HTTPS traffic, you can never find the keys.

Server that implements McCallum-Relyea Exchange is available on GitHub. It's called **Tang**. 2000 requests per second, singe-threaded server. Good performance.

**Clevis** is an example client also on GitHub. 

Secrets as a service API -- Custodia

TODO: Jose, Clevis and Tang -- check them out. As well as Luksmeta.

TODO: look into Shamir secret sharing