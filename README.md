# 🎤 ElderlyVoice: AI-Powered & Custom Voice Medication Reminders

**ElderlyVoice** is a complete, serverless medication reminder system designed to help seniors stay on track with their medication schedules. This application allows users to send personalized reminders via **SMS** and **email**. What makes ElderlyVoice unique is its dual-voice functionality: reminders can be delivered using a warm, life-like **AI-generated voice** (powered by AWS Polly) or a **custom voice recording** uploaded by a family member, adding a personal and comforting touch.

The entire system is built on a serverless architecture using AWS Lambda, ensuring scalability, cost-efficiency, and easy maintenance.

---

## ✨ Key Features

* **Dual Voice Options:**
    * **AI-Powered Voices:** Choose from multiple voice personalities (Gentle, Caring, Professional, Encouraging) to generate a high-quality, neural voice reminder.
    * **Custom Voice Upload:** Users can record their own voice message directly in the browser or upload an existing audio file. This allows for truly personal and comforting reminders from loved ones.
* **Multi-Channel Delivery:** Reminders are sent simultaneously via:
    * **SMS:** A text message with the reminder details.
    * **Email:** An HTML email containing the reminder text and an attached audio file of the voice message.
* **Comprehensive Reminder Details:** Include the senior's name, medication name, dosage, and special instructions.
* **Flexible Scheduling:**
    * **Send Now:** For immediate reminders.
    * **Schedule for Later:** Plan reminders for a future date and time.
* **Interactive Frontend:**
    * **Live Message Preview:** See a preview of the reminder message as you type.
    * **In-Browser Voice Recording:** A user-friendly interface to record, play back, and re-record custom voice messages.
    * **Audio Visualizer:** A visual representation of the audio being recorded.
* **Serverless & Scalable:** Built entirely on AWS Lambda, SES, SNS, and Polly, the application is cost-effective and can handle a high volume of reminders without the need to manage servers.

---

## 🛠️ Tech Stack

* **Frontend:** HTML, CSS, JavaScript (with no external libraries)
* **Backend:** Node.js (running on AWS Lambda)
* **AWS Services:**
    * **AWS Lambda:** For serverless compute.
    * **Amazon Polly:** For text-to-speech synthesis (AI voices).
    * **Amazon SES (Simple Email Service):** For sending emails with audio attachments.
    * **Amazon SNS (Simple Notification Service):** For sending SMS messages.
    * **API Gateway (Lambda Function URL):** To expose the Lambda function as a public endpoint.

---

## 🚀 How It Works

1.  **User Fills Out the Form:** A user (e.g., a family member) enters the senior's information, medication details, and a delivery schedule into the web interface.
2.  **Voice Selection:**
    * If an **AI Voice** is chosen, the frontend sends the text and voice preference to the Lambda function.
    * If a **Custom Voice** is chosen, the user can either record a new message in the browser or upload an audio file. The audio is then converted to a Base64 string and sent along with the form data.
3.  **Lambda Function is Triggered:** The frontend sends a JSON payload to the AWS Lambda function URL.
4.  **Voice Processing:**
    * For **AI voices**, the Lambda function calls **Amazon Polly** to generate an MP3 audio stream from the reminder text.
    * For **custom voices**, the Lambda function decodes the Base64 audio data into a buffer.
5.  **Sending Notifications:**
    * **Amazon SNS** is used to send an SMS reminder to the senior's phone number.
    * **Amazon SES** is used to send an email. This email is formatted as a `multipart/mixed` MIME type, allowing the generated (or uploaded) audio file to be included as an attachment.
6.  **Confirmation:** The Lambda function returns a success or failure message to the frontend, which is then displayed to the user.

---

## 📦 Setup and Deployment

To deploy this project, you will need an AWS account.

### 1. **AWS Lambda Function**

1.  **Create a Lambda Function:**
    * Go to the AWS Lambda console and create a new function.
    * Choose **Author from scratch**.
    * Use a **Node.js** runtime.
    * For the architecture, select **x86\_64**.
2.  **Configure Permissions:**
    * In the function's configuration, go to **Permissions**.
    * Edit the execution role to attach the following AWS managed policies:
        * `AmazonPollyFullAccess`
        * `AmazonSNSFullAccess`
        * `AmazonSESFullAccess`
3.  **Add Code:**
    * Copy the code from `index.js` into the Lambda function's code editor.
    * Click **Deploy**.
4.  **Enable Function URL:**
    * In the function's configuration, go to **Function URL**.
    * Create a new function URL with **Auth type** set to **NONE**.
    * Enable **CORS** and configure it to allow requests from the domain where you'll host the `index.html` file (or `*` for testing).

### 2. **Amazon SES (Simple Email Service)**

* **Verify Email Address:** In the SES console, you must verify the email address you'll be using in the `From:` field of the emails (`chilukalagayathri@gmail.com` in the provided code).
* **Request Production Access:** By default, SES is in a sandbox environment. You'll need to request production access to send emails to unverified email addresses.

### 3. **Frontend (`index.html`)**

1.  **Update Lambda URL:** Open `index.html` and find the `LAMBDA_URL` constant. Replace the placeholder URL with the Function URL you created for your Lambda function.
2.  **Host the File:** You can open `index.html` directly in your browser for local testing, or host it on a web server, AWS S3, or any static site hosting service.

---

## Usage

1.  Navigate to the hosted `index.html` page.
2.  Fill in the senior's details and medication information.
3.  Choose a voice personality or select "Custom Voice" to record or upload your own.
4.  Select a delivery schedule.
5.  Click "Send Complete Reminder" to dispatch the notifications.
