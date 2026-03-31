# AI-Marketing-Content-Agent-N8N

An automated **n8n marketing content generation workflow** that collects content requirements through a form, generates AI-powered content, converts the output into a Google Doc, makes it publicly accessible, and sends the final document link to the user via email. It supports both **long-form content** and **post-style content** depending on the selected content type. :contentReference[oaicite:1]{index=1}

---

## Features

- Form-based content request submission
- Supports multiple content types such as:
  - Blog
  - Article
  - Post
  - LinkedIn Post
  - Instagram Caption
  - Twitter/X Thread Post
- AI-generated marketing content using **OpenRouter**
- Separate prompt logic for:
  - Long-form content
  - Post-style content
- Automatic Markdown-to-Google-Doc conversion via external API
- Automatically sets Google Doc permission to **public reader**
- Sends final content link to requester through **Gmail**
- Supports SEO inputs, audience targeting, tone, voice, hashtags, and format preferences :contentReference[oaicite:2]{index=2}

---

## Workflow Overview

This workflow follows the process below:

1. **User submits a form**
2. Workflow checks the selected content type
3. If the type is **not "Post"**, it uses the long-form content generation chain
4. If the type is **"Post"**, it uses the post-focused content generation chain
5. AI content is generated using an **OpenRouter Chat Model**
6. Generated text is sent to an external **md2doc** endpoint to create a document
7. Google Drive permission is updated so the file can be viewed publicly
8. A Gmail notification is sent to the user with the final document link :contentReference[oaicite:3]{index=3}

---

## Nodes Used

### 1. Form Trigger
The workflow starts with an **n8n Form Trigger** called **On form submission**.

It collects the following details from the user:

- Primary Topic
- Email
- Select Type
- Target Audience
- SEO Main Keyword
- SEO Supporting Keyword
- Geographic Focus
- Competitor or Reference Links
- Content Style Tone
- Content Style Length
- Content Style Voice
- Format Preference
- Must-Include Points
- Hashtags
- Words to Avoid
- Goal :contentReference[oaicite:4]{index=4}

---

### 2. If Node
An **If** node checks whether the selected type contains `"Post"` or not.

- **True branch** → Long-form content flow
- **False branch** → Post content flow :contentReference[oaicite:5]{index=5}

---

### 3. AI Content Generation
The workflow uses two separate **Basic LLM Chain** nodes:

#### Long-form chain
Used for:
- Blog
- Article
- Other longer structured content

This prompt instructs the model to generate:
- exact word count
- selected tone and voice
- SEO-focused structure
- headings and subheadings
- scannable formatting
- conclusion with CTA :contentReference[oaicite:6]{index=6}

#### Post chain
Used for shorter post-style content, especially social-style output.

This prompt focuses on:
- concise persuasive writing
- headline and subheadings
- bullet points when needed
- platform-friendly formatting
- CTA at the end :contentReference[oaicite:7]{index=7}

---

### 4. OpenRouter Chat Model
The workflow connects LLM chains with **OpenRouter**.

One branch explicitly uses:

- `qwen/qwen-vl-plus`

The second OpenRouter model node is also connected for post generation. :contentReference[oaicite:8]{index=8}

---

### 5. HTTP Request to md2doc
After content generation, the result is sent to:

`https://md2doc.n8n.aemalsayer.com`

This converts the generated markdown/text into a document, using the **Primary Topic** as the file name. :contentReference[oaicite:9]{index=9}

---

### 6. Google Drive Permission Update
A second HTTP request is made to the Google Drive API to set document permissions as:

- role: `reader`
- type: `anyone`

This makes the generated document publicly accessible through a shareable link. :contentReference[oaicite:10]{index=10}

---

### 7. Gmail Notification
Finally, the workflow sends an HTML email to the user’s submitted email address.

The email includes:
- a confirmation that content was created
- a button to access the article/post
- a branded closing from **Systemapic** :contentReference[oaicite:11]{index=11}

---

## Form Inputs Supported

| Field | Purpose |
|---|---|
| Primary Topic | Main content topic |
| Email | Recipient email for final output |
| Select Type | Content format selection |
| Target Audience | Defines intended audience |
| SEO Main Keyword | Primary keyword target |
| SEO Supporting Keyword | Supporting SEO keyword |
| Geographic Focus | Regional targeting |
| Competitor or Reference Links | Inspiration or competitor research |
| Content Style Tone | Tone selection |
| Content Style Length | Word count |
| Content Style Voice | Narrative voice |
| Format Preference | Content structure preference |
| Any Must-Include Points | Required elements |
| Hashtags | Optional hashtags |
| Words to Avoid | Exclusion list |
| Goal | Intended call to action | :contentReference[oaicite:12]{index=12}

---

## Requirements

To run this workflow successfully, you need:

- **n8n**
- **OpenRouter API credentials**
- **Google Docs OAuth2 credentials**
- **Gmail OAuth2 credentials**
- Access to the external document conversion endpoint:
  - `md2doc.n8n.aemalsayer.com` :contentReference[oaicite:13]{index=13}

---

## Credentials Used

This workflow depends on the following credentials:

- **OpenRouter account**
- **Google Docs account**
- **Gmail account** :contentReference[oaicite:14]{index=14}

---

## Setup Instructions

1. Import the JSON workflow into n8n
2. Connect and validate all required credentials
3. Confirm the external `md2doc` endpoint is reachable
4. Activate the workflow
5. Open the n8n form URL
6. Submit a test request
7. Check whether:
   - content is generated correctly
   - Google Doc is created
   - file permission is updated
   - email is delivered successfully :contentReference[oaicite:15]{index=15}

---

## Example Use Cases

- Automated blog generation for marketing teams
- Lead magnet or campaign content creation
- LinkedIn content generation
- Instagram caption drafting
- SEO article drafting
- Client-facing content request automation :contentReference[oaicite:16]{index=16}

---

## Notes

- The workflow contains two separate generation paths to better handle different content styles
- Google Drive permissions are automatically changed to public reader access
- Email delivery depends on valid Gmail OAuth configuration
- Output quality depends on prompt design, model performance, and user input quality :contentReference[oaicite:17]{index=17}

---

## Suggested Improvements

You can improve this workflow further by adding:

- validation for missing required SEO inputs
- different prompts for LinkedIn, Instagram, and Twitter/X specifically
- retry logic for HTTP requests
- internal logging and error notifications
- support for saving outputs in Google Drive folders
- webhook response with final document URL
- moderation or quality check step before sending email

---

## Author

Built as an **n8n AI automation workflow** for marketing content creation and delivery.

---

## License

You can add your preferred license here, such as:

- MIT
- Apache-2.0
- Proprietary
