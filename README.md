# test_Improvado
Technical Exam for Improvado.io

## Overview
This Make.com automation streamlines the onboarding process by automatically assigning a workspace email using the new hire’s full name and setting their primary email group based on their role. All data is pulled from a Google Sheet.

Deliverables:
Share your Google Sheet (with view access).
https://docs.google.com/spreadsheets/d/1Vuq3_wMFkuloT4o2RzXgQ42dipi5sKMlAZxML5K13B0/edit?usp=sharing

Share your automation:
If Make/n8n: Public link to the scenario blueprint (if possible) AND screenshots of the setup.
A brief README file (text or markdown) explaining:
Your choice of tools.
	Make.com
	- Google Sheets
	- Google Gemini 1.5 Flash
  
A quick overview of how your automation works.


How the AI suggestion for the Google Group is implemented or would be prompted.
> "Suggest a Google Group email for a new hire with the role: {{ Role }}. Keep it concise like 'sales-team@improvado.io'. Make sure it always has @improvado.io at the end."
> With parameters:
	-	 "temperature": 0.7,
	- "maxOutputTokens": 150

Any assumptions made or quick notes on potential improvements if you had more time.
- Prevent duplicates
- Send onboarding message to the user on Slack


# Why Make.com?


# Explanation

---

## ✅ STEP-BY-STEP GUIDE

### 🔧 TOOLS USED

* **Make.com**
* **Google Sheets**
* **HTTP Module (for OpenAI API call or simulated response)**
* **Text Aggregator or Compose a String modules**


---

### 🧱 STEP 1: Prepare the Google Sheet

Create a Google Sheet with these columns:

| Full Name   | Role                  |
| ----------- | --------------------- |
| John Smith  | Software Engineer     |
| Anna Chen   | Sales Rep             |
| Maya Kapoor | Marketing Coordinator |

**Share it with Make with “Editor” access** to allow reading changes.

---

### Setup Make.com Scenario

**Modules Used:**

1. Google Sheets → **Watch Rows**
2. Text → **Split text / Text Parser** (to break down Full Name)
3. **Text aggregator or Compose a String** (to generate email)
4. HTTP → **AI Prompt (OpenAI or simulated)**
5. Text tools → **Create welcome message + IT summary**
6. (Optional) Gmail/Slack → send notifications

---

### Step-By-Step Module Configuration
#### 1. Google Sheets > Watch New Rows
<img width="361" alt="Screen Shot 2025-05-22 at 8 49 18" src="https://github.com/user-attachments/assets/67b2acae-3bac-4928-998b-acf63127602e" />
> The limit needs to be 1 as it needs to work with the data individually.


* **Trigger:** Watch New Rows
  

#### **2. Split Full Name**

* Use a “Text Parser” to extract First and Last name.
* This helps generate the email: `firstname.lastname@improvado.io`

#### **3. Generate Company Email**

* Use "Set Variable" or "Text Aggregator"

```plaintext
{{lowercase(replace(fullName; " "; "."))}}@improvado.io
```

* E.g. John Smith → `john.smith@improvado.io`


---

#### **4. AI Google Group Suggestion (Simulated or Real)**

**Option A – Real OpenAI Call (Use HTTP module):**

* Method: `POST`
* URL: `https://api.openai.com/v1/chat/completions`
* Headers:

```json
Authorization: Bearer YOUR_OPENAI_API_KEY
Content-Type: application/json
```

* Body (Raw JSON):

```json
{
  "model": "gpt-4",
  "messages": [
    {
      "role": "user",
      "content": "Suggest a Google Group email for a new hire with the role: {{Role}}. Keep it concise like 'sales-team@improvado.io'."
    }
  ],
  "temperature": 0.5
}
```

**Option B – Simulate AI (Cheaper/Faster)**

Use **Router** + Text tools for simple keyword matching:

* Role contains:

  * “Sales” → `sales-team@improvado.io`
  * “Engineer” → `eng-all@improvado.io`
  * “Marketing” → `marketing@improvado.io`

💡 **Prompt you'd use if you were calling OpenAI:**

> *“Given a job role, suggest a primary Google Group email they should belong to. The response should only include the group email. Role: Software Engineer → Response: [eng-all@improvado.io](mailto:eng-all@improvado.io)”*

---

#### **5. Create Welcome Message**

Use “Compose a string” module:

```plaintext
Welcome {{Full Name}}! We're excited to have you join as a {{Role}}. Your new email is {{Email}}. Feel free to reach out if you have any questions.
```

---

#### **6. Draft IT Summary**

Another “Compose a String”:

```plaintext
New Hire Alert:

- Name: {{Full Name}} # Harry Kane
- Role: {{Role}} # Sales Rep
- Email: {{Email}} #harry.kane@improvado.io
- Google Group: {{Suggested Group}} 
```

---

#### **7. (Optional) Output:**

You can:

* Send the welcome message to Slack, Gmail, or even Telegram.
* Add the summary to a Google Doc or forward it via email.

---

## 📂 DELIVERABLES PREPARATION

* **Share Google Sheet**: Add view access and share the link.
* **Make.com Scenario**:

  * Go to your scenario
  * Click **... → Share → Create public link**
* **Screenshots**: Capture:

  * The full scenario
  * Each module config (especially HTTP/Email generation)
* **README (Markdown format)**: Include:

---

## 🧾 README OVERVIEW (example content)

### ✅ Why I Chose Make.com

* **Visual interface** = fast setup and debugging
* **Easy Google Sheets integration**
* **HTTP module** = easy to call OpenAI or simulate logic
* **AI optional** = fallback with conditions if not available

### 🔍 How It Works

1. Triggers on new row in Google Sheets
2. Parses name and role
3. Creates email
4. Uses AI (or rules) to suggest a Google Group
5. Drafts messages
6. Sends alerts or stores info

### 🤖 AI Prompt Used

> "Suggest a Google Group email for a role like 'Software Engineer'. Use concise format like '[eng-all@improvado.io](mailto:eng-all@improvado.io)'."

### ⚠️ Assumptions

* AI access available or role names are simple
* Emails are generated from full name, no duplicates handling yet
* Scenario runs once per new row; batch processing not included

---

## ✅ PROS & CONS OF MAKE.COM FOR THIS TASK

### ✅ Pros

* Easy drag-and-drop interface
* Fast prototyping
* Rich integrations (Sheets, HTTP, text tools)
* Visual debugging

### ⚠️ Cons

* API call limits depending on plan
* Limited AI formatting unless controlled
* Doesn’t handle advanced logic as well as code (e.g. duplicate email checks)

#IMPROVEMENTS
