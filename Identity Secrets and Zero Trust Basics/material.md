# Identity Secrets and Zero Trust

## Authentication and Authorisation 
Authentication asks "Who are you ?" and checks the answer : a password, a code from your phone, a key.
Authorisation asks "What is this person allowed to do ?" and checks that against a policy. 

A system can get the first right and the second badly wrong. The classic VPN failure is exactly that: strong authentication at the door, and then once inside, authorization is "whatever the network lets you reach", which is usually far too much.

## What a Secret is ?
A secret is a string that lets its holder do something: log in, call an API, decrypt data. 
Types : 
1. Password
2. API token (or API key)
3. SSH key pair
4. Servuce account token 

A token is just as good as a password and often better for an attacker: it does not go through MFA, it is rarely rotated, and it is usually copied around between files, chat windows and terminals. Treat every token as if it were the account's password, because for a program it is.

## How Keys work, without the math 
A key pair is two numbers with a special relationship. You keep one (the private key) and publish the other (the public key). With the private key you can make a signature over any piece of data; with the public key, anyone can check that signature, but nobody can make a new one.

When you SSH to a machine that has your public key, your client signs a piece of data that is unique to this one connection (it is tied to the session the two sides just set up, so it is useless anywhere else). The server checks the signature against your public key. If it checks out, only the holder of the private key could have made it, so the server knows it is you. Your private key never left your laptop, nothing reusable went over the wire, and there is nothing on the server worth stealing (a public key is public). Some older explanations describe this as the server encrypting a challenge for you to decrypt; modern SSH keys such as ed25519 sign rather than decrypt. The difference between hashing, encrypting and signing, with hands-on exercises, is on Hashing, encryption and keys.

Compare that to a password, which goes over the wire every time, can be guessed, can be reused from another site's breach, and is only as strong as the person who chose it. This is why the droplets here accept keys only: there is no password to guess, so the endless password-guessing traffic that hits every internet-facing SSH server has nothing to work with.

## Why secrets leak ?
Almost never by being cracked. Usually:

1. Committed to a git repository. A .env file, a config with a token in it, a script with a hardcoded password. Even if deleted in the next commit, it is in the history forever and automated bots scan GitHub for exactly these patterns within minutes of a push.
2. Pasted into chat, a ticket or a screenshot. The terminal screenshot with the token visible in the command line.
3. Printed in a log. A build job that echoes its environment; an error message that includes the request headers.
4. Left in shell history. export TOKEN=... typed at the prompt is saved to ~/.bash_history.
5. Stored somewhere readable on the machine. A world-readable config file; a token in cloud-init user-data that any process on the VM can read from the metadata endpoint. This project has exactly that last problem with the connector tokens, says so in the Threat model, and lists the fix as a follow-up rather than pretending.
5. Never rotated. A token from 2021 that still works means every leak since 2021 still works too.
Every control in this project's secrets design is aimed at one of those bullets.

## The fix : a secrets manager and the discipline around it
