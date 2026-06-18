## create a bucket

'''md
aws s3 mb  s3://checksum-pashe-223
'''

## creating a file to do checksum
'''
echo "Pashe!" > myfile.txt
'''

## getting the checksum of the file

'''
md5sum myfile.txt
## 7a71e50f8b0f314e46186d2771f82a5f  myfile.txt
'''

## uploading file and getting its etag

'''
aws s3 cp myfile.txt s3://checksum-pashe-223/myfile.txt
aws s3api head-object --bucket checksum-pashe-223 --key myfile.txt
## "ETag": "\"7a71e50f8b0f314e46186d2771f82a5f\"",
'''
## uploading another file with a different checksum
'''
sudo apt install rhash -y
rhash --crc32 --simple myfile.txt
## 1fa75af3  myfile.txt
