#research #cryptography #database
## Proposed user collection
```JSON
{
	"id": "<UUID>",
	"email": "<string | null>",  // null if no account made
	"salt": "<base64 | null>",  // null if no account made
	"hash": "<base64 | null>",  // null if no account made
	...  // display name, etc...
},
```
## Proposed vault collection
```JSON
{
	"id": "<UUID>",
	"user_id": "<UUID | null>",  // null if no account made
	"salt": "<base64>",  // for deriving encryption key from master password
	"encrypted_blob": "<base64>",  // encrypted vault contents
	"nonce": "<base64>",  // for authenticated encryption
	"auth_tag": "<base64>"  // authentication tag from AEAD cipher
	...  // vault name, colour, etc...
},
