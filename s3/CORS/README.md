## create website 1

## create a bucket
aws s3 mb s3://cors-example-pashe
## change block public access
aws s3api put-public-access-block \
    --bucket cors-example-pashe \
    --public-access-block-configuration "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=false,RestrictPublicBuckets=false"
## create block policy
aws s3api put-bucket-policy --bucket cors-example-pashe --policy file://pashe.json
## turn on static website hosting
aws s3api put-bucket-website --bucket cors-example-pashe --website-configuration file://website.json
## apply index.html file and include resource that would cross origin
aws s3 cp index.html s3://cors-example-pashe
## view the website and see if index.html is there
http://cors-example-pashe.s3-website.us-east-1.amazonaws.com

## create website 2

## create a bucket
aws s3 mb s3://cors-example-pashe2

## change block public access
aws s3api put-public-access-block \
    --bucket cors-example-pashe2 \
    --public-access-block-configuration "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=false,RestrictPublicBuckets=false"
## create block policy
aws s3api put-bucket-policy --bucket cors-example-pashe2 --policy file://pashe2.json
## turn on static website hosting
aws s3api put-bucket-website --bucket cors-example-pashe2 --website-configuration file://website.json
## apply index.html file and include resource that would cross origin
aws s3 cp pashe.js s3://cors-example-pashe2

## create api gateway with mock response and then test the endpoint
curl -X POST \
  -H "Content-Type: application/json" \
  https://bpjucf5az2.execute-api.us-east-1.amazonaws.com/pashe/pashe
## set CORS on our bucket

aws s3api put-bucket-cors --bucket cors-example-pashe --cors-configuration file://cors.json


