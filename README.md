# learn-gpg

Encrypt & decrypt with GPG (GNU Privacy Guard).

## :file_folder: Installation

[Download and install GPG.](https://gnupg.org/download/index.html)

> Note: GPG is already included in most Linux distros.

## :octocat: :arrow_down: Clone repo

Clone this repository and navigate into the directory.

```bash
git clone https://github.com/knqti/learn-gpg
cd ./learn-gpg
```

## :closed_lock_with_key: Create your key pair

```bash
gpg --full-generate-key
```

1. Select algorithm "ECC (sign and encrypt)"
2. Select cryptography "Curve 25519"
3. Select expiration period "0"
4. Enter name
5. Enter email
6. Enter comment
7. Confirm selections

> Note: Use `gpg --generate-key` for default selections.

## :key: Import public key

Import the public key.

```bash
gpg --import knqti_pub_key.gpg
```

Check that it worked.

```bash
gpg --list-keys
```

You should see something like:

```
pub   ed25519 2023-10-27 [SC] [expires: 2025-10-26]
      ABC123DE456FG789HIJ01234KLM567NOP890QRST
uid           [ unknown] Friend's Name <friend@email.com>
sub   cv25519 2023-10-27 [E] [expires: 2025-10-26]
```

### Optional: Validate

By default, imported public keys are set to "unknown" validity level. GPG will just flash warnings each time you use it. 

It's good practice to confirm the public key truly came from who you expected. Contact the public key's owner directly and confirm the public key's fingerprint (in the example above, the fingerprint is the string ending in `P890QRST`).

```bash
gpg --fingerprint <friend@email.com>
```

Once confirmed, you can tell GPG you've validated the public key by signing off on it.

```bash
gpg --sign-key <fingerprint>
```

You should see the validity level set to "full".

### Optional: Set trust level

You can tell GPG how much you trust someone (ie, "I believe this *person* is good at verifing the keys of *others*"). This is useful for a [web of trust.](https://en.wikipedia.org/wiki/Web_of_trust)

```bash
gpg --edit-key <friend@email.com>
trust
<trust_level>
save
```

Trust levels:

- 1 = I don't know or won't say
- 2 = I do NOT trust
- 3 = I trust marginally
- 4 = I trust fully
- 5 = I trust ultimately

## :lock: Encrypt a message

Create a message, sign, and encrypt it.

```bash
echo "your message here" | gpg --sign --encrypt --recipient knqti --output <encrypted_file>.gpg
```

You'll now have a new file `<encrypted_file>.gpg`.

## :key: Prepare your public key

You need to send your public key to your recipient so they can write encrypted messages back to you.

```bash
gpg --export <your_email> > <your_public_key>.gpg
```

## :octocat: :arrow_up: Sync back to the repo

You should now have 4 files in your directory:

1. knqti_pub_key.gpg
2. README.md
2. <encrypted_file>.gpg
3. <your_public_key>.gpg

Push the changes back to the repository.

```bash
git add .
git commit -m "your commit message"
git push
```

Now you and your recipient can exchange encrypted information!
