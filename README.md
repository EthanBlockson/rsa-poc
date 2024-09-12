# RSA encryption guide (short messages <100 characters)

Proof of concept how to secure your messaging with RSA encryption. Using default Mac OS systems console terminal.

## Create user A

Generate private key with custom path

`openssl genpkey -algorithm RSA -out ~/Desktop/public_key.pem -pkeyopt rsa_keygen_bits:4096`

Generate public key from this private key

`openssl rsa -pubout -in ~/Desktop/private_key.pem -out ~/Desktop/public_key.pem`

To view them
`cat private_key_user_A.pem`
`cat public_key_user_A.pem`

## Create user B

Generate private key with custom path

`openssl genpkey -algorithm RSA -out ~/Desktop/private_key_user_B.pem -pkeyopt rsa_keygen_bits:4096`

Generate public key from this private key

`openssl rsa -pubout -in ~/Desktop/private_key_user_B.pem -out ~/Desktop/public_key_user_B.pem`

## Sign the message with private key of user A for user B

`openssl pkeyutl -encrypt -in ~/Desktop/message.txt -out ~/Desktop/encrypted_message.bin -inkey ~/Desktop/public_key_user_B.pem -pubin`

## Receive the message from user A with public key of user B

`openssl pkeyutl -decrypt -in ~/Desktop/encrypted_message.bin -out ~/Desktop/decrypted_message.txt -inkey ~/Desktop/private_key_user_B.pem`

# RSA+AES encryption guide (large messages >100 characters)

For long text, more than a few words or lines.

## Generate AES key

`openssl rand -out ~/Desktop/aes_key.bin 32`

## Encrypt the file using AES key

`openssl enc -aes-256-cbc -salt -pbkdf2 -in ~/Desktop/message.txt -out ~/Desktop/encrypted_message.enc -pass file:/Users/admin/Desktop/aes_key.bin`

## Encrypt AES key with RSA for public key of user B

`openssl pkeyutl -encrypt -in ~/Desktop/aes_key.bin -out ~/Desktop/encrypted_aes_key.bin -inkey ~/Desktop/public_key_user_B.pem -pubin`

## Decrypt AES key with private key of user B

`openssl pkeyutl -decrypt -in ~/Desktop/encrypted_aes_key.bin -out ~/Desktop/decrypted_aes_key.bin -inkey ~/Desktop/private_key_user_B.pem`

## Decrypt the file using AES key

`openssl enc -d -aes-256-cbc -pbkdf2 -in ~/Desktop/encrypted_message.enc -out ~/Desktop/decrypted_message.txt -pass file:/Users/admin/Desktop/decrypted_aes_key.bin`