☁️ In this project, we will be developing a text narrator using Amazon Polly. A piece of text (book, article, newsletter) will be uploaded in an Amazon S3 bucket and converted to speech. The voice, pitch and speed parmeters can be adjusted ☁️
Steps to be performed 👩‍💻

In the next few lessons, we'll be going through the following steps.

    Exploring Amazon Polly
    Creating an IAM role
    Creating an S3 Bucket
    Writing Lambda function code
    Checking the output of Amazon Polly

Services Used 🛠

    Amazon Polly: Converts text to life like speech with customizable features.
    AWS Management Console: Manages accounts and configures Amazon Polly.
    AWS IAM: Ensures secure access by managing user permissions.

Estimated Time & Cost ⚙️

    This project is estimated to take about 20-30 minutes
    Cost: Free (When using the AWS Free Tier)

➡️ Architectural Diagram

This it the architectural diagram for the project:

![0001](https://github.com/user-attachments/assets/2b6f61de-c728-44b5-a731-8abf9851a301)
What is Amazon Polly?

Amazon Polly is a service provided by AWS that enables developers to generate human-like speech from text.

    Text-to-Speech Conversion: Amazon Polly turns written text into spoken words. So, you can type something, and Amazon Polly will say it out loud in a natural-sounding voice.
    Realistic Speech: The speech created by Amazon Polly sounds like a real person speaking, not robotic or unnatural. It's great for making computerized voices sound more human-like.
    Options to Customize: You can change how the speech sounds. For example, you can make it faster or slower, adjust the pitch (high or low), and even pick different accents or languages.
    Supports Many Languages and Voices: Amazon Polly can speak in lots of different languages and with different voices. Whether you want a man or a woman, or someone with a British or American accent, you have options.
    SSML Support for Control: You can use special codes called SSML to control exactly how the speech sounds. This allows for things like emphasizing certain words, adding pauses, or changing the tone of voice.

Trying out Amazon Polly

    Login to your AWS management console and search for Amazon Polly on the search bar.
![4B](https://github.com/user-attachments/assets/c056bf30-853c-4b44-984b-1dc6b1b0de3e)
![0003](https://github.com/user-attachments/assets/20d314ee-e859-419d-9dbb-eaa44dd07fb1)
Amazon Polly provides different engines according to the requirements for the sounding of the audio and the content length.
To explain it in easy terms:
- Neural Engine: Used for lifelike and expressive speech or for natural-sounding interactions.
- Standard Engine: Provides good quality speech suitable for most applications.
- Long Form Engine: Optimized for longer texts like articles or books that provides a good quality throughout the content.

Amazon Polly also provides different languages to translate and different voice tones. So you can use a voice tone according to the need in your application.
![4D](https://github.com/user-attachments/assets/4ae6652b-4e69-448f-afc7-d69e6ea5b5bc)
    Choose the language and voice model, type the content you want to be translated in the audio form in the input text box.

    You can listen to the output using the Listen button present on the top right or you can even download the audio or save it in a S3 bucket.

We have seen a quick demo on how Amazon Polly works. In the next few sections we will explore on how to call the Polly service using a serverless function.
Creating an IAM role

    In this project, we will try to access the Amazon Polly service and store the audio output in a S3 bucket using a Lambda function.
    For that we need an IAM role with suitable policies attached to it.

    From your AWS management console, search for IAM from the search bar.
    ![0004](https://github.com/user-attachments/assets/89d4554a-fe94-44e1-a43a-0e8df8dc4ea6)
    Navigate to Access Management → Roles → Create Role.

Select AWS service as the trusted entity type and Lambda as your use case. Click on Next.
![0005](https://github.com/user-attachments/assets/99dc2a4e-410f-4d5a-aec9-3d4b4963cbab)
From the list of permission policies, choose the following policies :

    AmazonPollyFullAccess
    AmazonS3FullAccess
    AWSLambdaBasicExecutionRole

Click on Next.
Provide a suitable name and description for the IAM role and click on Create role.
![0006](https://github.com/user-attachments/assets/6320066f-3367-4b7a-9714-3325e548c394)
Creating a S3 bucket

    From your AWS management console, search for S3 from the search bar.
    Click on Create bucket.
    
![0007](https://github.com/user-attachments/assets/9728e42f-1158-4638-ad8a-731ef57e15c7)
Give a suitable name for the S3 bucket, keep the rest of the configurations as it is and click on Create bucket.
![0008](https://github.com/user-attachments/assets/fe7b1f98-866d-4e3d-8268-e8690f54b10a)

![00013](https://github.com/user-attachments/assets/8f401093-728a-479a-a558-51edd73bd277)
    The audio files from generated by Amazon Polly would be stored in this bucket with the help of AWS Lambda.

Great work keeping up so far! In the next step we will create a Lambda function through which we will access the Amazon Polly service and store the audio output in the created S3 bucket.
Creating a Lambda function

    From your AWS management console, navigate to Lambda from the search bar. 
    ![00010](https://github.com/user-attachments/assets/1bcb3e96-33d5-42f0-93cf-e59c2e53f148)

    Click on Create function.
Give an appropriate name to the Lambda function and choose Node.js 16.x as the runtime environment.
Toggle the option ‘Change default configuration role’ and check ‘Use existing role’.
Choose the role that you created earlier, leave the rest of configurations as default and click Next.

Rename your ‘index.mjs’ file to ‘index.js’.
We're using Amazon's tools (AWS SDK) to talk to two services: Polly (for making speech from text) and S3 (for storing files).
We're writing a function that AWS will run for us whenever something happens. It's like a little program that waits for a signal to start working.
9. When the function gets a message with some text, we're going to make it into speech. We decide how the speech will sound and what format it should be in 
10. We send the text to Polly and ask it to turn it into speech. Polly does its magic and gives us back the speech as data.

11. We then save this speech in our S3 storage.
12. We make a message saying the speech has been saved successfully with its special name in our storage. If something goes wrong, there is an error message.

 Complete Lambda Function

Your complete Lambda function code will look like this.
14. Deploy the code changes by clicking on Deploy.

Checking the ouput

We have completed with the code configuration of the Lambda function, let’s test the function out by creating a test event.

    Click on Test and configure a test event for your Lambda function.

    Provide a name for your test configuration and in the Event JSON, provide the text you want to be converted to audio in the form:
    ![00014](https://github.com/user-attachments/assets/1627f110-e98c-4f94-b202-76c03f0afa06)

    3. Leave the rest of configurations as default and click ‘Save’.

4. Click on Test button again to invoke the test event.

5. Check the output.
![00012](https://github.com/user-attachments/assets/59164844-3628-4d15-8495-bf61f42137b1)
6. You can access the audio file by checking it in the previously created S3 bucket and downloading it.
   ![00013](https://github.com/user-attachments/assets/a9025007-e650-494a-a06a-dd152a94f641)

   


   
























