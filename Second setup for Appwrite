
# Appwrite Different Port Setup with Service

## Table of Contents

- [Appwrite Installation Setup](#appwrite-installation-setup)
- [Appwrite Configuration when Installing](#appwrite-configuration-when-installing)
- [SMTP Service Setup](#smtp-service-setup)
- [Appwrite .env File Configuration for Gmail SMTP Setup](#appwrite-env-file-configuration-for-gmail-smtp-setup)
- [Appwrite SMS Service Setup](#appwrite-sms-service-setup)
- [Logging Provider Service Setup](#logging-provider-service-setup)
- [Setup Logging Provider Service Using Sentry](#setup-logging-provider-service-using-sentry)

---

## Appwrite Installation Setup

Install and configure Appwrite using Docker:

```bash
sudo docker run -it --rm \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    --volume "$(pwd)"/appwrite:/usr/src/code/appwrite:rw \
    --entrypoint="install" \
    appwrite/appwrite:1.4.13
```

## Appwrite Configuration when Installing

Configure Appwrite with the following settings:

- Ports:
  - HTTP: 80 to 21012
  - HTTPS: 443 to 21112

- Key: `DevOps-appwrite`

## SMTP Service Setup

### Appwrite .env File Configuration for Gmail SMTP Setup

Configure Appwrite's `.env` file for Gmail SMTP setup:

1. Enable 2-factor authentication in Gmail after that create an `app password` in Gmail account manager settings and get 'app password'.
2. Edit the `.env` file:
   ```bash
   sudo nano /path/to/appwrite/.env
   ```
3. Update the following parameters in the `.env` file:
   ```yaml
   _APP_SMTP_HOST=smtp.gmail.com
   _APP_SMTP_PORT=465
   _APP_SMTP_SECURE=ssl
   _APP_SMTP_USERNAME=your_Gmail_address@gmail.com
   _APP_SMTP_PASSWORD=your_gmail_app_password
   ```

After that restart Appwrite by using this command:
```bash
docker compose up -d
```

After that check your configuration show in running Appwrite by using this command:
```bash
docker compose exec appwrite vars
```

After that check Appwrite email service running by follow these steps like try forget password or invite team members by using email if you received message in Gmail then your setup done successfully.



## Appwrite SMS Service Setup

To set up SMS functionality in Appwrite using Twilio as the provider, follow these steps:

1. **Sign Up for Twilio**:
   If you haven't already, sign up for a Twilio account at [Twilio's website](https://www.twilio.com/). This will give you access to your Account SID and Auth Token, which you'll need to authenticate with Twilio's API.

2. **Get Twilio Account SID and Auth Token**:
   After signing up, you'll find your Account SID and Auth Token on the Twilio dashboard. These credentials will be used to authenticate your requests to the Twilio API.

3. **Configure SMS in Appwrite**:
   In your Appwrite dashboard, go to the "SMS" section under "Integrations". Click on the "Add Integration" button and select "Twilio". Enter your Twilio Account SID and Auth Token in the respective fields.

4. **Edit Appwrite `.env` File**:
   Update the Appwrite `.env` file with the Twilio configuration:
   ```plaintext
   _APP_SMS_PROVIDER=sms://[ACCOUNT SID]:[AUTH TOKEN]@twilio
   _APP_SMS_FROM=[TWILIO PHONE NUMBER]
   ```
   Replace `[ACCOUNT SID]`, `[AUTH TOKEN]`, and `[TWILIO PHONE NUMBER]` with your actual Twilio Account SID, Auth Token, and Twilio phone number respectively.

   After editing the `.env` file, restart Appwrite using this command:
   ```bash
   docker compose up -d
   ```

5. **Set Up Appwrite SMS Endpoint**:
   Appwrite provides an SMS endpoint that you can use to send SMS messages through Twilio. You can typically access this endpoint using HTTP POST requests. The endpoint URL would look something like this:
   ```
   https://api.appwrite.io/v1/sms/twilio
   ```
   Make sure to replace `https://api.appwrite.io` with the base URL of your Appwrite instance.

6. **Send SMS**:
   Now that you have everything set up, you can start sending SMS messages through Twilio via Appwrite's API. You can do this using any programming language that supports making HTTP requests. Here's an example using cURL:
   ```bash
   curl -X POST \
   -H "Content-Type: application/json" \
   -d '{"phone":"+1234567890", "message":"Hello, this is a test message!"}' \
   https://api.appwrite.io/v1/sms/twilio
   ```

7. **Handle Response**:
   After sending the SMS request, you'll receive a response indicating whether the message was successfully sent or if there was an error. Make sure to handle these responses appropriately in your application.

By following these steps, you should be able to set up SMS functionality in Appwrite using Twilio as the provider.


## Logging Provider Service Setup

### Setup Logging Provider Service Using Sentry

To enable logging errors to Sentry in Appwrite, follow these steps:

1. **Create a Sentry Account**:
   If you haven't already, sign up for a Sentry account at [Sentry's website](https://sentry.io/). You can sign up using your email address or by using a supported authentication provider like GitHub.

2. **Create a Project**:
   After logging in to your Sentry account, create a new project for your application. Go to the Projects tab and click on "Create Project". Follow the prompts to set up your project.

3. **Find your Sentry API Key and Sentry App ID**:
   Once your project is created, go to the Settings tab of your project. Under the Client Keys section, you'll find your Sentry API key. It's labeled as "DSN" (Data Source Name). It typically looks like this:
   ```
   https://your-sentry-project-id@app.sentry.io/12345
   ```
   In this URL, `your-sentry-project-id` is your Sentry App ID.

4. **Set Up Sentry Integration in Appwrite**:
   In your Appwrite dashboard, go to the "Logging" section under "Integrations". Click on "Add Integration" and select "Sentry". Enter your Sentry API key and App ID in the respective fields.

5. **Restart Appwrite**:
   After configuring the Sentry integration, restart your Appwrite server for the changes to take effect.

6. **Verify Sentry Setup**:
   To verify that Appwrite is successfully sending logs to Sentry, you can intentionally trigger an error in your application and check if it appears in your Sentry project's dashboard.

By following these steps, you should be able to set up Sentry as the logging provider for your Appwrite instance.

---

This readme provides instructions for setting up Appwrite with different ports, configuring SMTP and SMS services, and enabling logging to a 3rd party provider using Sentry. Follow the steps outlined above for each setup.
