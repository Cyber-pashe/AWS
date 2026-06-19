## create bucket

aws s3 mb s3://encryption-pashe

## create a file

aws s3 cp index.txt s3://encryption-pashe

## put object with encryption of kms(aws automatically creates the kms key and the defines the policy, to make it custom you have create your own and define your policies)

aws s3api put-object \
  --bucket encryption-pashe \
  --key kms.txt \
  --body index.txt \
  --server-side-encryption "aws:kms"

## put object with encryption of sse-c
'''we create The Key Material: You must provide a 256-bit (32-byte), base64-encoded binary key. S3 will not accept a plain text password like "mysecretkey".
The Key MD5 Hash: To protect against corruption or transmission errors, S3 requires you to provide the MD5 truncated 128-bit hash of the key material, also base64-encoded.

# 1. Generate a random 32-byte key encoded in base64
export Pashekey=$(openssl rand -base64 32)
echo $Pashekey

# 2. Calculate the MD5 hash of that raw binary key material
export MY_MD5=$(echo -n "$Pashekey" | openssl base64 -d | openssl md5 -binary | openssl base64)
'''

aws s3api put-object --bucket encryption-pashe --key kms.txt --body index.txt --sse-customer-algorithm "AES256" --sse-customer-key "$Pashekey" --sse-customer-key-md5 "$MY_MD5"

aws s3api put-object --bucket encryption-pashe --key kms.txt --body index.txt --server-side-encryption-c-algorithm "AES256" --server-side-encryption-c-key "$Pashekey" --server-side-encryption-c-key-md5 "$MY_MD5"

### Put Object with SSE-C via aws s3
# https://catalog.us-east-1.prod.workshops.aws/workshops/aad9ff1e-b607-45bc-893f-121ea5224f24/en-US/s3/serverside/ssec

openssl rand -out ssec.key 32

aws s3 cp index.txt s3://encryption-pashe/ssec.txt \
  --sse-c AES256 \
  --sse-c-key fileb://ssec.key