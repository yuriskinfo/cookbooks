
[Get a list of all the buckets under user account](#ee1)  
[Recursively list contents of a given bucket _yurisk.info_](#ee2)  
[Recursively list contents of a given bucket printing sizes in a friendly format](#ee3)  
[List contents of a bucket, add summary for number of objects and their total size](#ee4)  
[Get access-list associated with the bucket provided you have permissions to do so on this bucket](#ee5)  
[Create bucket in default region specified in configuration file](#ee6)  
[Create the bucket in a given region](#ee7)  
[Delete an empty bucket, if it is not empty fails with the error `BucketNotEmpty`](#ee8)  
[Force deleting non empty bucket provided that objects in it have not been versioned](#ee9)  
[Simulate removing without deleting anything](#ee10)  
[Delete recursively contents of a bucket, bucket itself is not deleted](#ee11)  
[Delete recursively all files in a bucket except JPEG and PNG](#ee12)  
[Delete only files with a given extension (here JPG)](#ee13)  
[Delete an empty bucket](#ee14)  
[Set the existing S3 bucket _yurisk.info_ for hosting the website with the default served document being index.html and the error page for any 4xx response error codes being error.html](#ee15)  
[Make sure HTML pages of the bucket are accessible by anyone for reading](#ee16)  
[Verify the website configuration](#ee17)  
[Delete website configuration from a bucket (doesn't delete any objects iside the bucket)](#ee18)    
[Upload local file _index.html_ to the bucket _yurisk.info_ at the location _/tag/nmap/_ and set website redirection to the _http://yurisk.info_, also set ACL to `public-read`](#ee19)    
[Create expiring link/hot-link to the object in s3 bucket, pre-sign link](##20)  








<a name="ee1"></a>  
### Get a list of all the buckets under user account

```bash
 aws s3 ls
```

<a name="ee2"></a>  
### Recursively list contents of a given bucket
The bucket here is _yurisk.info_.  
```bash
 aws s3 ls yurisk.info --recursive
```


<a name="ee3"></a>  
### Recursively list contents of a given bucket printing sizes in a friendly format

```bash
 aws s3 ls yurisk.com --recursive --human
```


<a name="ee4"></a>  
### List contents of a bucket, add summary for number of objects and their total size

```bash
aws s3 ls yurisk.info --summarize --human --recursive
```

<a name="ee5"></a>  
### Get access-list associated with the bucket provided you have permissions to do so on this bucket

```bash
aws s3api get-bucket-acl --bucket yurisk.info
```


<a name="ee6"></a>  
### Create bucket in default region specified in configuration file

```bash
aws s3 mb s3://testbucket
```

<a name="ee7"></a>  
### Create the bucket in a given region
```bash
aws s3 mb s3://testbucket --region us-east-1
```



<a name="ee8"></a>  
### Delete an empty bucket, if it is not empty fails with the error `BucketNotEmpty`
```bash
aws s3 rb yurisk.info
```

<a name="ee9"></a>  
### Force deleting non empty bucket provided that objects in it have not been versioned
```bash
aws s3 rb s3://yurisk.com --force
```


<a name="ee10"></a>  
### Simulate removing without deleting anything
```bash
aws s3 rm s3://yurisk.info --dryrun --rec
```

<a name="ee11"></a>  
### Delete recursively contents of a bucket, bucket itself is not deleted
```bash
aws s3 rm s3://yurisk.info --recursive
```

<a name="ee12"></a>  
### Delete recursively all files in a bucket except JPEG and PNG
```bash
aws s3 rm s3://yurisk.info --recursive --exclude "*.jpg" --exclude "*.png" 
```

<a name="ee13"></a>  
### Delete only files with a given extension (here JPG)
```bash
aws s3 rm s3://yurisk.info --recursive --exclude "*" --include "*.jpg"
```

<a name="ee14"></a>  
### Delete an empty bucket
```bash
aws s3api delete-bucket --bucket yurisk.info
```


<a name="ee15"></a>  
### Set the existing S3 bucket _yurisk.info_ for hosting the website with the default served document being _index.html_ and the error page for any 4xx response error codes being _error.html_
```bash
aws s3 website s3://yurisk.info/ --index-document index.html --error-document error.html
```

<a name="ee16"></a>  
### Make sure HTML pages of the bucket are accessible by anyone for reading
```bash
aws s3api get-bucket-acl --bucket yurisk.info
```


<a name="ee17"></a>  
### Verify the website configuration
```bash
aws s3api get-bucket-website --bucket yurisk.info
```

<a name="ee18"></a>  
### Delete website configuration from a bucket (doesn't delete any objects inside the bucket)
```bash
aws s3api delete-bucket-website --bucket yurisk.info
```



<a name="ee19"></a>  
### Upload local file _index.html_ to the bucket _yurisk.info_ at the location _/tag/nmap/_ and set website redirection to the _http://yurisk.info_, also set ACL to `public-read`
```bash
aws s3api put-object --bucket yurisk.info --key /tag/nmap/index.html --website-redirect-location http://yurisk.info --body C:/Users/yurisk.info/tag/nmap/index.html --acl public-read
```


<a name="ee20"></a>  
### Create expiring link/hot-link to the object in s3 bucket, pre-sign link

```bash
aws s3 presign s3://yurisk.info/download.me --expires-in 259200 --profile         awsadminprofile --region eu-west-1
```
Here:  
`download.me` - object in S3 to create download link to. NOTE: You don't have to  make this object public in any way, still, anyone with the link will have read    access to it.  
`--expires-in 259200` - Expiration time starting from now in **seconds**/ Here it is set to 3 days. If this parameter is absent, the default expiration is 3600     seconds or 1 hour.  
`--region <name>` - region location of the object, if it is different from the    default one set in your AWS profile.   
`--profile` - optional. Only needed if you ahve multiple AWS IAM user profiles    configured on the host.  

Output:  
```bash
ttps://yurisk.info.s3.amazonaws.com/download.me?                                  AWSAccessKeyId=AKIA2QEA3PKXP5TYM2GO&Signature=0WU7257sOAy9odrh6Fs88d0Vp94%3D&Expires=1599498917   
```

The `expires` here is epoch time when the link expires.  
