## create a new bucket

'''
aws s3api create-bucket --bucket acl-learning26 --region us-east-1
'''
## turn off block access for acls
'''
aws s3api put-public-access-block \
    --bucket acl-learning26 \
    --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=true,RestrictPublicBuckets=true"
'''
## change bucket ownership
'''
aws s3api put-bucket-ownership-controls \
    --bucket acl-learning26 \
    --ownership-controls "Rules=[{ObjectOwnership=BucketOwnerPreferred}]"
'''
## change acl to allow for user in another aws account
'''
aws s3api put-bucket-acl \
    --bucket acl-learning26 \
    --grant-read "id=111122223333" \
    --grant-read-acp "id=111122223333"