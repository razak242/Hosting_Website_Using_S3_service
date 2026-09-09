# 🛒 Fruitables - Static Website Hosting on AWS S3
> A production-ready, highly available static website hosting implementation for **Fruitables (Organic Veggies & Fruits eCommerce Template)** hosted on **Amazon Web Services (AWS) Simple Storage Service (S3)**.

---

## 📌 Project Overview

This repository demonstrates the step-by-step process of configuring, deploying, and serving a modern, responsive static web application on the cloud using **AWS S3 Static Website Hosting**.

### Why AWS S3 for Static Website Hosting?
* **High Availability & Durability:** 99.999999999% (11 9's) data durability.
* **Cost Efficiency:** Extremely low cost with pay-as-you-go pricing and zero server management.
* **Scalability:** Automatically scales to handle traffic spikes from zero to millions of requests without manual provisioning.
* **Performance:** Fast asset delivery with low latency when combined with AWS global infrastructure.

---

## 🏗️ Architecture & Deployment Flow

```
+------------------+       Upload Files        +-------------------------+
| Local Repository | ------------------------> |      AWS S3 Bucket      |
| (HTML/CSS/JS/Img)|                           | (mybucket24p31a0561)    |
+------------------+                           +-------------------------+
                                                            |
                                             Configure Static Hosting
                                             & Public Read Permissions
                                                            v
+------------------+       HTTP Request        +-------------------------+
|   End User /     | <======================= |   S3 Website Endpoint   |
|   Web Browser    |        HTML/Assets        |  (us-east-1.amazonaws)  |
+------------------+                           +-------------------------+
```

---

## 🚀 Step-by-Step AWS S3 Deployment Process

Here is the complete walkthrough with verified process screenshots from the AWS Management Console and live deployment tests.

---

### Step 1: Create an Amazon S3 Bucket

1. Sign in to the **AWS Management Console** and open the **Amazon S3** console.
2. Select **Create bucket**.
3. Configure the bucket details:
   - **Bucket name:** `mybucket24p31a0561` *(must be globally unique across AWS)*
   - **AWS Region:** `US East (N. Virginia) us-east-1`
4. Under **Block Public Access settings for this bucket**:
   - Uncheck **Block *all* public access**.
   - Acknowledge that the objects will become public when policies are applied.
5. Click **Create bucket**.

![Step 1 - S3 Bucket Creation](Process_clips/Screenshot%202026-09-05%20154046.png)
*Figure 1: S3 Bucket `mybucket24p31a0561` created in the `us-east-1` region.*

---

### Step 2: Upload Website Source Files & Asset Directories

1. Navigate into the newly created bucket (`mybucket24p31a0561`).
2. Click the **Upload** button.
3. Select and upload all required static website files and folders:
   - Root HTML files: `index.html`, `shop.html`, `shop-detail.html`, `cart.html`, `chackout.html`, `contact.html`, `404.html`
   - Assets & styling: `css/`, `scss/`, `js/`, `lib/`, `img/`
4. Monitor the upload progress in real-time.

![Step 2 - Uploading Website Files](Process_clips/Screenshot%202026-09-05%20153151.png)
*Figure 2: Real-time progress bar tracking the upload of files and folders into S3.*

> **Asset Processing Note:** All subfolders (including SCSS components, Bootstrap dependencies, and vendor libraries) are preserved with their original directory hierarchy.
> 
> ![Step 2b - Asset Upload Details](Process_clips/Screenshot%202026-09-05%20153208.png)
> *Figure 2b: Detailed status check during asset and SCSS stylesheet upload.*

---

### Step 3: Verification of Successful Upload

1. Wait for the upload queue to complete.
2. Check the green **Upload succeeded** notification banner.
3. Confirm that all items succeeded with **0 failed files**.

![Step 3 - Upload Succeeded](Process_clips/Screenshot%202026-09-05%20153408.png)
*Figure 3: AWS S3 console confirmation showing 100% upload success for all libraries and scripts.*

---

### Step 4: Verify Bucket Objects Structure in AWS Console

1. In the bucket **Objects** tab, verify that all root-level files and directories are present.
2. Ensure file types and storage classes are properly mapped (e.g., HTML, CSS, JavaScript, Standard Storage Class).

![Step 4 - S3 Bucket Objects](Process_clips/Screenshot%202026-09-05%20154430.png)
*Figure 4: Bucket inventory showing root objects (`index.html`, `cart.html`, `contact.html`, `css/`, `img/`, `js/`, etc.).*

---

### Step 5: Enable Static Website Hosting & Public Access

1. Open the bucket **Properties** tab.
2. Scroll to the bottom to find **Static website hosting** and click **Edit**:
   - Choose **Enable**.
   - **Hosting type:** Host a static website.
   - **Index document:** `index.html`
   - **Error document:** `404.html`
   - Click **Save changes**.
3. Open the bucket **Permissions** tab:
   - Under **Bucket Policy**, click **Edit** and paste the JSON policy below to grant public read permissions (`s3:GetObject`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mybucket24p31a0561/*"
    }
  ]
}
```

4. Locate the **Bucket website endpoint** URL generated by AWS:
   ```
   https://mybucket24p31a0561.s3.us-east-1.amazonaws.com/index.html
   ```
5. Open the URL in a web browser to verify the live landing page.

![Step 5 - Live Website Homepage](Process_clips/Screenshot%202026-09-05%20154137.png)
*Figure 5: Live Fruitables landing page running directly from the AWS S3 endpoint.*

---

### Step 6: Multi-Page Navigation & Live Testing

1. Test links and internal page navigation across the hosted site.
2. Navigate to `shop.html` (`/shop.html`) to ensure secondary pages, product listings, category filters, and CSS/JS animations work seamlessly without broken paths.

![Step 6 - Live Shop Page Verification](Process_clips/Screenshot%202026-09-05%20154215.png)
*Figure 6: Live Fresh Fruits Shop page confirming functional routing and asset rendering on AWS.*

---

## 📂 Project Structure

```
cloud_repo_1/
│
├── Process_clips/                 # AWS deployment process screenshots
│   ├── Screenshot 2026-09-05 154046.png  # Bucket creation
│   ├── Screenshot 2026-09-05 153151.png  # Upload in progress
│   ├── Screenshot 2026-09-05 153208.png  # Subfolder upload
│   ├── Screenshot 2026-09-05 153408.png  # Upload completion
│   ├── Screenshot 2026-09-05 154430.png  # S3 bucket objects
│   ├── Screenshot 2026-09-05 154137.png  # Live homepage
│   └── Screenshot 2026-09-05 154215.png  # Live shop page
│
├── css/                           # Compiled CSS styles (Bootstrap, responsive)
├── img/                           # Website image assets
├── js/                            # Interactive scripts and plugins
├── lib/                           # Vendor libraries (Lightbox, OwlCarousel, Waypoints)
├── scss/                          # Source SASS/SCSS files
│
├── 404.html                       # Error document for broken routes
├── cart.html                      # Shopping cart page
├── chackout.html                  # Checkout page
├── contact.html                   # Contact form page
├── index.html                     # Main entry index document
├── shop.html                      # Product catalog page
├── shop-detail.html               # Product details page
├── testimonial.html               # Customer testimonials page
│
├── LICENSE.txt                    # Template license
├── READ-ME.txt                    # Source template attribution
└── README.md                      # Documentation & deployment guide
```

---

## 🛠️ Technology Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Cloud Hosting** | AWS S3 (Simple Storage Service) | Serverless static web hosting & object storage |
| **Markup** | HTML5 | Semantic structure and SEO-optimized web pages |
| **Styling** | CSS3 / SCSS / Bootstrap 5 | Modern, mobile-first responsive layout and design |
| **Interactivity** | JavaScript (ES6) / jQuery | Client-side dynamics, carousel controls, search |
| **Components** | Owl Carousel, Easing, Waypoints | Carousels, smooth scrolling, and view triggers |

---

## 🔒 Security Best Practices for S3 Hosting

* **Least Privilege Access:** Only `s3:GetObject` permission is allowed publicly; write, delete, and list operations remain restricted.
* **Custom Error Document:** `404.html` is configured to catch missing resources and keep users within the site interface.
* **Optional HTTPS / CDN Integration:** For custom domain names and SSL/TLS certificates, pair this bucket with **Amazon CloudFront** and **AWS Route 53**.

---

## 📜 Credits & License

* **Website Template:** [Fruitables - Vegetable Website Template](https://htmlcodex.com/vegetable-website-template) by [HTML Codex](https://htmlcodex.com)
* **License:** Licensed under [HTML Codex License](https://htmlcodex.com/license)
* **Cloud Architecture & Deployment:** Hosted via Amazon Web Services (AWS)
