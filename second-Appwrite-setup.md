
---

# Appwrite Different Port Setup with Service

## Table of Contents

- [Appwrite Installation Setup](#appwrite-installation-setup)
- [Appwrite Configuration when Installing](#appwrite-configuration-when-installing)
- [SMTP Service Setup](#smtp-service-setup)
- [Appwrite .env File Configuration for Gmail SMTP Setup](#appwrite-env-file-configuration-for-gmail-smtp-setup)
- [Appwrite SMS Service Setup](#appwrite-sms-service-setup)
- [Logging Provider Service Setup](#logging-provider-service-setup)
- [Setup Logging Provider Service Using Sentry](#setup-logging-provider-service-using-sentry)

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

1. **Sign Up for Twilio**: If you haven't already, sign up for a Twilio account at [Twilio's website](https://www.twilio.com/).
2. **Get Twilio Account SID and Auth Token**: After signing up, find your Account SID and Auth Token on the Twilio dashboard.
3. **Configure SMS in Appwrite**: In your Appwrite dashboard, go to the "SMS" section under "Integrations" and select "Twilio". Enter your Twilio Account SID and Auth Token.
4. **Set Up Appwrite SMS Endpoint**: Use the provided SMS endpoint URL to send SMS messages through Twilio via Appwrite's API.
5. **Send SMS**: Start sending SMS messages through Twilio via Appwrite's API.
6. **Handle Response**: Handle responses indicating success or errors appropriately in your application.

## Logging Provider Service Setup

To enable logging errors to a 3rd party provider in Appwrite, set the `_APP_LOGGING_PROVIDER` and `_APP_LOGGING_CONFIG` environment variables accordingly.

### Setup Logging Provider Service Using Sentry

To obtain the Sentry API key and Sentry App ID, follow these steps:

1. **Create a Sentry Account**: Sign up for a Sentry account at [Sentry's website](https://sentry.io/).
2. **Create a Project**: Create a new project for your application in Sentry.
3. **Find your Sentry API Key and App ID**: Find your Sentry API key and App ID in the Settings tab of your project.
4. **Use in Appwrite**: Set `_APP_LOGGING_CONFIG` to your Sentry API key and App ID.
5. **Restart Appwrite**: Restart your Appwrite server for the changes to take effect.

---

This readme provides instructions for setting up Appwrite with different ports, configuring SMTP and SMS services, and enabling logging to a 3rd party provider using Sentry. Follow the steps outlined above for each setup.
