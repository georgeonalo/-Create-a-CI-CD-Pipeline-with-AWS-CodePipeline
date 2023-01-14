# Create a CI/CD Pipeline with AWS CodePipeline

![image](https://user-images.githubusercontent.com/115881685/212470830-e7195a87-06fe-4881-aa01-bc458052b964.png)

In this project, I will go over how to create a CI/CD pipeline with AWS CodePipeline. CI/CD stands for continuous integration and continuous deployment. Implementing a pipeline makes it a series of steps to automate the process of delivering new versions of software. The steps are; to build the code, run tests, and safely deploy. I will be using Github for the source code and an S3 bucket for the deploy stage.


## Prerequisites

* AWS account

* Github account

## Step 1


First, we will create an S3 bucket to host a static website. Navigate to S3 in the AWS console. Click on Buckets > Create bucket. Here you will give your bucket a unique name, choose the region, and indicate that you want the bucket to be public by unchecking Block all public access.

![image](https://user-images.githubusercontent.com/115881685/212471095-b362bbc3-8605-44b2-8093-55ea5731f349.png)

Click Create bucket. Now that your bucket is created we will configure it to host a static website. We can do so by navigating to the Properties tab in the bucket.

![cicd](https://user-images.githubusercontent.com/115881685/212471263-4f5f437e-9a96-4234-a5a5-5eba389bd857.png)


Scroll all the way down to Static website hosting. Click on Edit and Enable static website hosting. Then type in index.html and error.html as indicated and click Save. Now move over to the Permissions tab. Here we will add a bucket policy to allow public read access. Click on the Edit bucket policy and input the JSON code below.

![cicd1](https://user-images.githubusercontent.com/115881685/212471277-6eff55f4-7d56-448c-bfdc-e9ecf4e5b96f.png)

Be sure to change the ARN to your bucket with “/*” after the ARN. Click Save. Your bucket is now publicly accessible.

![cicd2](https://user-images.githubusercontent.com/115881685/212471313-de570c80-13af-4c30-a717-1c800eb27623.png)

## Step 2

For the next step navigate to CodePipeline > Create pipeline. Name your pipeline and click Next. On the Add source stage you will want to choose GitHub (Version 2) and connect to your GitHub account. You will be prompted to enter your GitHub account username and password.

![image](https://user-images.githubusercontent.com/115881685/212471363-e7da7008-1ecd-423b-9504-8a3a1a4a6fbe.png)

Authorize the connector. You will then be asked which repositories AWS can connect to. Choose accordingly, the repo of your choosing, and click Install > Connect. You will be brought back to CodePipeline. Here you will choose your repo and branch name.

![cicd3](https://user-images.githubusercontent.com/115881685/212471413-ae6584e7-5536-4e6d-87f1-aaaafe43da0f.png)

## Step 3


Next, you are asked to choose a builder, indicate AWS CodeBuild. Choose your Region and click create a project, this will bring you to CodeBuild. In CodeBuild I used the following configurations:

* Managed Image

* Standard runtime

* aws/codebuild/standard:5.0

* Always use the latest image for this runtime version

* Linux

* Create a New Service Role

Then click on Continue to CodePipeline. Back in CodePipeline, you should find that your project was successfully built in CodeBuild.

![image](https://user-images.githubusercontent.com/115881685/212471621-a3eee7ad-e2fb-483d-b291-55007b7cf5c0.png)

## Step 4
On to the deploy stage! We will be deploying to the S3 bucket you just created. Choose S3 and your bucket.

![cicd4](https://user-images.githubusercontent.com/115881685/212471660-002b8585-8ec6-455c-83a3-56111fb2d4aa.png)

Click Next to review the stages and if it all looks good then click on Create pipeline. When it is done deploying your screen should look like this.

![CICD5](https://user-images.githubusercontent.com/115881685/212471680-88e93e27-da1b-40fe-9889-7d2d9bb45a3e.png)
![cicd 9](https://user-images.githubusercontent.com/115881685/212471720-9e132fc7-12fc-4968-bde7-11e9d3e706a3.png)

Now that the code is deployed to S3 we can check by grabbing the S3 website endpoint. Found in the properties tab of your S3 bucket, scroll all the way to the bottom of the page.

![cicd 11](https://user-images.githubusercontent.com/115881685/212471749-4505b851-ca9b-4c0d-8a26-db0d8161d1f6.png)

Copy and paste the URL in your browser…

![cicd6(1)](https://user-images.githubusercontent.com/115881685/212471777-a3392e35-2a39-469a-aac9-bd6bd7e13970.png)

And behold the static website! That we are able to view due to the read permissions given to the public.

## Step 5
To verify that the Code Pipeline is triggered I will make a change to the code in GitHub. The pipeline should automatically trigger and make the changes. Let’s test it out! I will edit the HTML file and commit the changes.

![cicd7](https://user-images.githubusercontent.com/115881685/212471827-791ce9ee-440e-48a0-8b56-38f14f9b5ffb.png)

Checking on the Pipeline back in the AWS console you can actually see the Pipeline at work. It is automatically taking in the changes and executing them.

![cicd6](https://user-images.githubusercontent.com/115881685/212471941-cfda26c7-25d8-4306-9fc5-24724844c4f1.png)

When refreshing the static website the changes I made to the file should appear.

![cicd8](https://user-images.githubusercontent.com/115881685/212471975-5007b3ad-8ceb-4e21-9bd3-f2d8965b3af5.png)

And it worked! The CodePipeline was triggered and updated on the website. All using GitHub, CodePipeline, CodeBuild, and S3.























