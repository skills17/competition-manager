# Competitions Directory

This folder will contain the downloaded tasks and the decrypted tasks.

Please follow the instructions here: [README.md](./README.md).

## Troubleshooting

You typically don't need to interact with this folder directly. If you have trouble with the tasks, please contact the
organizers.

The tasks are bundled, encrypted, decrypted and unbundled using the following commands:

```bash
# Tar and compress
tar -czvf competitions/decrypted/demo.tar.gz -C competitions/workspace/demo .
# Encrypt
openssl enc -aes-256-ctr -pbkdf2 -a -in "competitions/decrypted/demo.tar.gz" -out "competitions/encrypted/demo.tar.gz.enc" -pass "pass:1234"
# Show hash
sha256sum competitions/encrypted/demo.tar.gz.enc | cut -d ' ' -f 1
# Verify hash
sha256sum -c <(echo "2057d9ea2b198e6b678f4a309a9c82d0104dea271c036647919ad13189be69de  competitions/encrypted/demo.tar.gz.enc")
# Decrypt
openssl enc -aes-256-ctr -pbkdf2 -d -a -in "competitions/encrypted/demo.tar.gz.enc" -out "competitions/decrypted/demo.tar.gz" -pass "pass:1234"
# Decompress and untar (make sure the target directory exists)
tar -xzvf competitions/decrypted/demo.tar.gz -C competitions/workspace/demo
```
