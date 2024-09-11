# RSA encryption guide

Proof of concept how to secure your messaging with RSA encryption. Using default Mac OS systems console terminal.

## Create user A

Generate private key with custom path

`openssl genpkey -algorithm RSA -out ~/Desktop/private_key_user_A.pem -pkeyopt rsa_keygen_bits:4096`

Generate public key from this private key

`openssl rsa -pubout -in ~/Desktop/private_key_user_A.pem -out ~/Desktop/public_key_user_A.pem`

## Create user B

Generate private key with custom path

`openssl genpkey -algorithm RSA -out ~/Desktop/private_key_user_B.pem -pkeyopt rsa_keygen_bits:4096`

Generate public key from this private key

`openssl rsa -pubout -in ~/Desktop/private_key_user_B.pem -out ~/Desktop/public_key_user_B.pem`

## Sign the message with private key of user A for user B

`openssl pkeyutl -encrypt -in ~/Desktop/message.txt -out ~/Desktop/encrypted_message.bin -inkey ~/Desktop/public_key_user_B.pem -pubin`

## Receive the message from user A with public key of user B

`openssl pkeyutl -decrypt -in ~/Desktop/encrypted_message.bin -out ~/Desktop/decrypted_message.txt -inkey ~/Desktop/private_key_user_B.pem`