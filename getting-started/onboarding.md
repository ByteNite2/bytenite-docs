---
icon: shoe-prints
---

# Onboarding

Get all you need to begin running workloads on ByteNite and doing some Byte-magic!

***

## 👤 Create an account

1. **Get an access code**\
   Currently, our platform is in beta and you'll need an access code to create an account. If you have one, use it when required during the sign-up process. Otherwise, you can [Request an Access Code](https://bytenite.com/get-access).
2. **Sign up on Computing Platform**\
   To create an account on ByteNite, follow this link and fill out the form with your contact info and your access code: [Sign Up for ByteNite](https://idp.bytenite.com/login?auth_mode=signup).



***

## 💳 Add a payment method

Having an account is sufficient for creating apps and start playing around with ByteNite. However, once your credits are over you’ll need an active billing account to keep launching jobs. Follow the steps below to add a payment method to your account and activate it.

1. **Access your Billing page**\
   After registering and logging in, go to [https://app.bytenite.com/billing](https://app.bytenite.com/billing) or navigate to the **Billing** page on the sidebar.
2. **Add a payment method**\
   Locate the Payment Info card an navigate to the Customer Portal. Follow the steps to add a payment method to your account.

NOTE: Your payment information will be stored for manual top-ups as well as automatic top-ups triggered by the end of the current billing cycle. Make sure you have enough funds to cover your previous billing cycle's costs to avoid service interruptions.



***

## 🪙 Redeem ByteChips

If you have a coupon, you can redeem it on your billing page to add ByteChips to your balance.

1. **Access your Billing page**: \
   After registering and logging in, go to [https://app.bytenite.com/billing](https://app.bytenite.com/billing) or navigate to the **Billing** page on the sidebar.

* **Redeem a coupon**: \
  Go to the Account Balance card and click on "**Redeem**". Enter your coupon code in the corresponding field and complete the process. Ensure the amount has been added to your balance; refresh the page if needed.

We'd love to get you started with free credits to test our platform. If you haven't received free ByteChips upon signing up, [send us a message,](https://bytenite.com/info) and we'll grant you some.



***

## 🔐 Get an API key

For programmatic access to our API, you'll need an API key linked to your account. You can have multiple keys, each meant for a specific integration. Follow these steps to get a key:

1. **Access your profile**\
   After registering and logging in, go to [https://app.bytenite.com/profile](https://app.bytenite.com/profile) or click on your profile avatar located in the top right corner of the webpage.
2. **Create a new API key**\
   On your profile page, locate and click the '**New API Key**' button or link.
3. **Configure your API key settings**\
   Provide a descriptive name for your API key, choose its validity duration, and enter the confirmation code sent to your email. After filling out the necessary details, click the 'Generate API Key' button.
4. **Copy your API key**\
   Once generated, immediately copy your API key and securely store it. Treat your API key with the same security precautions as a password.

{% hint style="warning" %}
**One-time API key access**

Please note that for security reasons your API key will only be visible and copyable right after you've generated it. Be sure to copy and securely store it immediately.

_Note: The "key ID" is merely an identifier and differs from the actual key._
{% endhint %}

6. **Managing your API keys**\
   If your key is no longer needed or is compromised, return to the API section in your profile. Find the key and click 'Revoke' to invalidate it.



***

## 🔑 Get an access token

An access token is required to authenticate all requests to the ByteNite API, except for OTP and access token requests.&#x20;

To request an access token, send a request to the [#access\_token](../api-reference/authentication-api/access-token.md#access_token "mention") endpoint using your API key.

Access tokens have a default duration of 3600 seconds (i.e., 1  hour). Once expired, you must request a new one.&#x20;



***

## 🛠️ Set up development tools

Here are some recommended tools to get started with ByteNite's[#product-and-services](how-it-works.md#product-and-services "mention").

1. **Download & set up ByteNite Dev CLI:**

{% content-ref url="../sdk/bytenite-dev-cli.md" %}
[bytenite-dev-cli.md](../sdk/bytenite-dev-cli.md)
{% endcontent-ref %}

2. **Set up ByteNite API:**

<details>

<summary><strong>Create a Postman collection from ByteNite's OAS</strong></summary>

Postman is a popular API development tool that simplifies API testing, debugging, and collaboration. It allows developers to organize API requests into collections for easy management and execution. With Postman, you can quickly import ByteNite's OpenAPI Specifications (OAS), generate pre-formatted API requests, and start interacting with our APIs without the need to write code.

Below is a setp-by-step guide to set up Postman and load ByteNite's API specification:

1. **Launch Postman**\
   Download [Postman](https://www.postman.com/) on your device or access the web app from your browser. Create a free account and log in to start using the platform.
2. **Import ByteNite's OpenAPI Specification**
   * Click the **"Import"** button at the top-left corner of the Postman interface.
   * In the **Import modal**, switch to the **"Link"** tab.
   * Paste one of ByteNite's Swagger JSON links into the URL field:
     * **Jobs API**: [`https://api.bytenite.com/v1/customer/docs/swagger.json`](https://api.bytenite.com/v1/customer/docs/swagger.json)
     * **Auth API**: [`https://api.bytenite.com/v1/auth/docs/swagger.json`](https://api.bytenite.com/v1/customer/docs/swagger.json)
     * **Dev API**: [`https://api.bytenite.com/v1/dev/docs/swagger.json`](https://api.bytenite.com/v1/customer/docs/swagger.json)
   * Click **"Continue"**.
3. **Confirm and Create the Collection**
   * Postman will analyze the Swagger JSON and prompt you to confirm the import.
   * Ensure one of the options "Postman Collection" or "OpenAPI 2.0 with a Postman Collection" are selected.
   * Click **"Import"** to generate the collection.
   * Repeat for each Swagger JSON file.
4. **Review the Imported Collection**
   * Navigate to the **Collections** tab in Postman.
   * Locate the newly created collections (`ByteNite jobs API`, `ByteNite auth API`, `ByteNite dev API`).
   * Expand the collection to see the organized endpoints and methods.
5. **Configure Environment Variables (Optional)**\
   Environment variables in Postman are useful for storing values like API keys, names, and IDs that you frequently use in your requests. By using environment variables, you can manage these values centrally and reuse them across multiple requests.
6. **Test the API**
   * Select an endpoint from the collection.
   * Add the necessary headers, body, or parameters as specified in ByteNite's documentation.
   * Click **"Send"** to execute the request and view the response.
   * That’s it! You’ve successfully set up ByteNite’s API collections in Postman.&#x20;

**Additional Tips**

* **Authentication**: ByteNite APIs require authentication (Bearer tokens). Set up the required auth details under the **Authorization** tab for the collection or individual requests.
* **Collaboration**: Share the imported collection with your team by clicking **"Share Collection"**.
* **Documentation**: Use Postman’s **Documentation** feature to add notes or examples to requests for easy reference.

</details>



