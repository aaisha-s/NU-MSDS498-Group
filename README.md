# NU-MSDS498-Group

AWS Rekognition: pre-trained image recognition model. The tool can be tested and demoed using AWS's own demo tool - 
https://us-east-2.console.aws.amazon.com/rekognition/home?region=us-east-2#/label-detection

However, I wanted to go through the testing and analyze the labels in a code based method. For this reason I used functionality through the boto3 package to call label and face recognition. High level steps and code snippets are below. Check the attached images and Jupyter Notebook to dive deeper into the example. 

https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rekognition.html

Steps:
1. Set up account in AWS IAM with AmazonRekognitionFullAccess access
2. Download Access Key csv for above account
3. Install boto3 and csv where your Jupyter Notebook will be
4. Read in secret access:
```
with open('rekog_demo_secret_credentials.csv', 'r') as input:
  next(input) #skip first row since it's just column names
  reader = csv.reader(input)
  for line in reader:
    access_key_id = line[2]
    secret_access_key = line[3]
 ```
5. If you have a locally saved image, convert it to bytes. If you have an image in a S3 bucket, that image will be converted by AWS services
```
with open(choc_chip_cookie_img, 'rb') as choc_chip_cookie:
    choc_chip_cookie_bytes = choc_chip_cookie.read()
```
6. Pass the bytes variable to the ```detect_labels``` method. You can add additional parameters such as ```MaxLabels``` or ```MinConfidence``` based on the criteria you'd like the function take into account. For more information on styling and variables, check the boto3 documentation
https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rekognition.html#Rekognition.Client.detect_labels
```
choc_chip_response = client.detect_labels(
    Image={'Bytes': choc_chip_cookie_bytes},
    MaxLabels=10)
 ```
 7. Lastly, take the ```response``` variable and check the labels detected along with their confidence levels
 ```
 print('Detected labels:' )
 for label in choc_chip_response['Labels']:
    print(label['Name'] + '\t' + str(round(label['Confidence'], 4)))
```

#### Similar steps and formatting included for the ```detect_faces``` method in the Jupyter Notebook
