#research #cryptography #user-flow
# Account
## Create account 
- Choose **email** and **account password**
- Create **salt** - random 16 bytes
- Create **hash (derived key)** - *Argon2id* with **account password** + **salt**
- Store **salt** and **hash** in database
## Login to account 
- Enter **email** and **account password**
- Retrieve stored **salt** with **email**
- Create **hash (derived key)** - *Argon2id* with **account password** and **salt**
- If **hash** matches **hash** from database
	- Authenticate user

# Vault
## Create vault
- Choose **master password**
- Create **salt** - random 16 bytes
- Create **hash (derived key)** - *Argon2id* with **master password** + **salt**
- Initial empty JSON structure
- Convert JSON to bytes
- Generate random **nonce** - for each encryption
	- For *AES-256* or *ChaCha20*, 12 bytes
	- For *XChaCha20*, 24 bytes
- Create **encrypted blob** with **derived key** and **nonce**
	- Encrypt with *AES-256* or *ChaCha20*
	- Also produces **authentication tag**
- Store **salt**, **encrypted blob**, **nonce**, **auth tag** in database
## Unlock vault
- Enter **master password**
- Retrieve stored **salt**
- Create **hash (derived key)** - *Argon2id* with entered **master password** and **salt**
- Attempt to decrypt **encrypted blob** with **hash (derived key)**
- If decryption is successful
	- Vault is unlocked
## Modify vault
- Add to existing JSON structure
- Convert JSON to bytes
- Create **hash (derived key)** - *Argon2id* with **master password** + **salt**
- Generate *NEW* random **nonce** - for each encryption
- Create new **encrypted blob** using **hash (derived key)** and **nonce**
- Update stored **encrypted blob**, **nonce**, and **auth tag** in database