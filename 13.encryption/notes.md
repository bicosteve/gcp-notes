# Encryption

- data in the cloud is supposed to be secured.
- this is where encryption comes in

## Date States

1. Data at rest - stored on device or backup. eg data on disk, database, backups and archives.
2. Data in motion - being transfered across a network. also called data in transit eg application talking to a database, copying data from on-premise to cloud storage. Two types of data in motion;
   - In and out of cloud (from internet); should really secure the data while in transit
   - withing cloud; can control everything hence security is not a major factor.
3. Data in use - active data processed in a non-persistent state e.g data in your RAM.

## Encryption

- if you store data as is, unauthorized entity can get access to it.
- losing an unencrypted hard disk will be catastrophic.
- first law of security is defense in depth.
- enterprises should encrypt all the data they have.
- should not only encrypt data at rest but also data on transit.
  There are two ways on encrypting data;
  a) Symmetric Key Encryption;
  the algorithm uses the same key for encryption and decryption.
  key factor 1 is choosing the right encryption algorithm.
  key factor 2 is how to secure the encryption key.
  key factor 3 is how to share the encryption key.

  b) Assymetric Key Encryption;
  uses two keys, public and private key.
  also called public key cryptography.
  you encrypt data with public key and decrypt with private key.
  you can share the public key with everybody and keep the private key with you.

## Cloud KMS

- it is the gcp service to create and manage cryptographic keys (symmetric and assymetric).
- KMS - Key Management Service
- you can control their use in your applications and GCP Services.
- kms provide an api to encrypt, decrypt, or sign data.
- use existing cryptographic keys created on premises.
- kms integrate with almost gcp services that need data encryption like google managed key-no configuration required, customer-managed key - use key from KMS, customer-supplied key - provide your own key
- to get started, you must create 'key ring' which is like key holder we ordinarily use in daily life.
- create key rings with; gcloud kms keyring create <KEYRING_NAME> --location us
- create a crypto key; gcloud kms keys create <CRYPTOKEY_NAME> --location us --keyring <KEYRING_NAME> --purpose encryption
- create key ring on the console by going to security -> key management -> create key ring
- create key ring -> bix-tuts-keys
- create key bix-first-key
- attach it to a VM/or any other services by granting the vm instance permission to use the key.
