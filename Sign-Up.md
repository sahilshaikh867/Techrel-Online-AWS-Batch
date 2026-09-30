# AWS Account Creation Guide

This comprehensive guide outlines the **two main methods** to create an Amazon Web Services (AWS) account: the standard standalone web sign-up and the multi-account enterprise method via AWS Organizations.

---

## 🛠️ Method 1: The Standalone / Personal Way (Via AWS Website)
Ideal for individuals, learners, or startups creating a primary account eligible for the **AWS Free Tier**.

### Prerequisites
* A unique, valid email address.
* A phone number capable of receiving SMS or voice calls.
* A valid credit or debit card. (AWS will place a temporary **$1 USD hold** to verify the card; it is automatically refunded within 3–5 days).

### Step-by-Step Instructions
1. **Navigate to the Portal:** Open your browser and go to the official [AWS Sign-up Page](https://aws.amazon.com/resources/create-account/). Click **"Create a Free Account"**.
2. **Credential Setup:**
   * Enter your **Root User email address**.
   * Choose an **AWS Account Name** (you can change this later).
   * Click **"Verify email address"**. Retrieve the verification code sent to your inbox, enter it, and proceed.
3. **Set Root Password:** Create a highly secure password for your Root User.
4. **Contact Information:** 
   * Select your account type: **Personal** (for learning/projects) or **Business** (for companies).
   * Fill in your full name, phone number, country, and address. Accept the AWS Customer Agreement.
5. **Billing Details:** Enter your credit or debit card information. 
   * *Note:* Do not worry about immediate charges; you will receive a free tier allowance covering many services for 12 months.
6. **Identity Verification:** Enter a mobile phone number. Select whether you want the verification code via SMS or Voice Call, enter the captcha text, and submit. Input the received PIN code on the webpage.
7. **Support Plan Selection:** Choose the **Basic Support (Free)** tier. Avoid Developer or Business support plans unless you require 24/7 technical assistance and are prepared for monthly fees.
8. **Finalize:** Click **"Complete sign-up"**. Wait a few minutes for AWS to activate your account. You will receive a confirmation email when it is ready.

---

## 🏢 Method 2: The Multi-Account Way (Via AWS Organizations)
Ideal for businesses or advanced users who already own a primary AWS account and want to spin up isolated environments (e.g., Development, Staging, Production) without repeating credit card or phone verification.

### Prerequisites
* An active primary AWS account designated as the **Management Account**.
* [AWS Organizations](https://aws.amazon.com/organizations/) enabled on the management account.
* A unique email address for each new sub-account.

### Step-by-Step Instructions
1. **Log In:** Sign in to the AWS Management Console of your primary management account using Root or appropriate Administrator IAM credentials.
2. **Open Organizations Console:** Search for and select **AWS Organizations** in the top services search bar.
3. **Initiate Creation:** 
   * Click the **"Add an AWS account"** button on the dashboard.
   * Select **"Create an AWS account"**.
4. **Account Details:**
   * **AWS account name:** Give the sub-account a recognizable identifier (e.g., `Company-Dev-Environment`).
   * **Email address:** Enter the unique email. 
     * *Tip:* Use email aliases (e.g., `myemail+awsdev@gmail.com`) to manage notifications in a single inbox.
   * **IAM role name:** Leave this as default (`OrganizationAccountAccessRole`). This grants your main account administrator rights to log into the child account seamlessly.
5. **Submit:** Click **"Create AWS account"**. The process runs asynchronously and typically takes less than 10 seconds.
6. **Accessing the New Account:** 
   * **Via AWS IAM Identity Center (Recommended):** Configure single sign-on to jump directly into the child account.
   * **Via Password Reset:** Go to the standard AWS login panel, select **Root user**, enter the sub-account email address, and click **"Forgot password?"** to generate a local password link.

---

## 🛡️ Critical Post-Creation Next Steps

Regardless of the method chosen, immediately perform these two actions to secure your account and avoid surprise bills:

### 1. Enable Multi-Factor Authentication (MFA)
* Log in as the **Root User**.
* Search for **IAM** (Identity and Access Management) in the top bar.
* Click on **"Add MFA"** on your security status dashboard.
* Link a virtual MFA application like Google Authenticator or Microsoft Authenticator.

### 2. Set Up a Billing Budget Alert
* Search for **AWS Budgets** in the console.
* Click **Create budget** -> Choose **Zero Spend Budget** or **Monthly Cost Budget**.
* Specify a threshold (e.g., **$5 USD**).
* Enter your email address to receive immediate notifications if your projected or actual usage exceeds that cost limit.
