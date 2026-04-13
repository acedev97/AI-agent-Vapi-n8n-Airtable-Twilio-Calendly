# 📞 VAPI Voice AI Agent - Automated Lead Calling System

[![n8n](https://img.shields.io/badge/n8n-Automation-orange)](https://n8n.io/)
[![VAPI](https://img.shields.io/badge/VAPI-Voice%20AI-blue)](https://vapi.ai/)
[![Airtable](https://img.shields.io/badge/Airtable-Database-brightgreen)](https://airtable.com/)
[![Automated](https://img.shields.io/badge/Fully-Automated-success)](https://github.com)

> **Intelligent voice AI agent that automatically calls leads from Airtable, conducts personalized conversations, and sends appointment booking links via email and phone. Built with VAPI, n8n, and Calendly integration.**

Set it and forget it—your AI agent calls prospects every 4 minutes during business hours, qualifies leads, and books appointments automatically.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Workflow Breakdown](#-workflow-breakdown)
- [VAPI Assistant Setup](#-vapi-assistant-setup)
- [Customization](#-customization)
- [Troubleshooting](#-troubleshooting)
- [Best Practices](#-best-practices)
- [License](#-license)

---

## 🎯 Overview

**VAPI Voice AI Agent** is a sophisticated automated calling system that leverages AI to conduct natural phone conversations with leads. Perfect for real estate, financial services, insurance, or any business that needs to qualify and nurture leads at scale.

### What Makes This Special?

- 🤖 **AI-Powered Calls** - VAPI conducts human-like conversations
- 📊 **Airtable Integration** - Manages leads and tracks call status
- ⏰ **Scheduled Automation** - Calls leads every 4 minutes during business hours
- 📧 **Multi-Channel Follow-up** - Email + phone with Calendly links
- 🎯 **Smart Lead Routing** - Handles phone validation and status updates
- 📅 **Appointment Booking** - Direct integration with Calendly
- 💬 **Personalized Messaging** - Uses prospect data for customized outreach
- 🔄 **Status Tracking** - Automatic updates: To Do → Done/Number not Found

### Who Is This For?

- 🏠 **Real Estate Agents** - Call property leads automatically
- 💼 **Financial Advisors** - Reach retirement planning prospects
- 🏥 **Insurance Agents** - Follow up on policy inquiries
- 📞 **Sales Teams** - Qualify leads at scale
- 🎯 **Lead Generation Agencies** - Service multiple clients
- 💰 **Loan Officers** - Connect with mortgage applicants

---

## ✨ Key Features

### 📞 **Automated Voice Calling (VAPI)**
- AI-powered natural conversations
- Personalized with prospect data (name, company, industry, job title)
- Handles objections and questions
- Books appointments during the call
- Records call outcomes

### 📊 **Airtable CRM Integration**
- Searches for leads with "To Do" status
- Extracts prospect information
- Updates call status automatically
- Tracks campaign progress
- Manages lead pipeline

### ⏰ **Smart Scheduling**
- Runs every 4 minutes during business hours (11 AM - 4 PM)
- Configurable cron schedule
- Timezone-aware (America/Chicago)
- Prevents after-hours calls
- Batch processing capability

### 📧 **Multi-Channel Follow-Up**
- **Phone/SMS** via Twilio
- **Email** via Gmail
- Calendly booking links
- Personalized messages
- Branded templates

### 🎯 **Lead Validation**
- Phone number verification
- Missing number handling
- Status updates (Done/Number not Found)
- Data formatting and cleaning
- Error recovery

### 📅 **Calendly Integration** (Optional)
- Check availability in real-time
- Book appointments automatically
- Send confirmation emails
- Sync with calendar
- Reduce no-shows

### 🔔 **Webhook Response Handling**
- Receives VAPI call outcomes
- Triggers follow-up actions
- Tool call integration
- Real-time processing
- Error handling

---

## 🔄 How It Works

### User Experience (Prospect's View)

```
11:05 AM - Phone rings
Prospect: "Hello?"

AI Agent: "Hi John! This is Sarah from Protect Fortunes. 
          I'm calling about retirement planning strategies for 
          business owners like yourself at ABC Company. 
          Do you have 2 minutes to chat?"

[Natural conversation ensues]

AI Agent: "I'd love to send you our Calendly link to schedule 
          a proper consultation. What's the best way to reach 
          you—email or text?"

Prospect: "Email works!"

[Call ends]

11:07 AM - Email arrives with Calendly link
11:07 AM - Phone message with same link
```

### Business Owner Experience

```
Step 1: Add leads to Airtable with "To Do" status
Step 2: Activate n8n workflow
Step 3: AI agent calls leads automatically every 4 minutes
Step 4: Review call outcomes in Airtable
Step 5: Check booked appointments in Calendly
```

### Technical Flow

```
Schedule Trigger (Every 4 minutes)
     ↓
Search Airtable (Status = "To Do", Limit: 1)
     ↓
Format Lead Data
├─ Name: firstName + lastName
├─ Phone: +[Number]
├─ Email: emailAddress
├─ Company: companyName
├─ Industry: businessIndustry
└─ Job Title: jobTitle
     ↓
Validate Phone Number
├─ If Empty → Mark "Number not Found"
└─ If Exists → Continue
     ↓
Initiate VAPI Call
├─ Assistant ID: Your VAPI assistant
├─ Phone Number ID: Your VAPI number
├─ Customer Data: name, number, email
└─ Variables: prospect_name, company, industry, email, title
     ↓
[Call in Progress - VAPI handles conversation]
     ↓
Update Airtable Status → "Done"
     ↓
[If prospect requests booking during call]
     ↓
Webhook Receives Tool Call
     ↓
Send Phone/SMS (Twilio)
     ↓
Send Email (Gmail)
     ↓
Return Response to VAPI
     ↓
[Optional: Book directly in Calendly]
```

---

## 🏗️ System Architecture

### Complete System Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    AIRTABLE CRM                              │
│                  (Lead Database)                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Leads Table:                                               │
│  ┌──────────────────────────────────────────────┐          │
│  │ ID │ Name │ Email │ Phone │ Company │ Status │          │
│  ├──────────────────────────────────────────────┤          │
│  │ 1  │ John │ j@co  │ +1... │ ABC     │ To Do  │          │
│  │ 2  │ Mary │ m@co  │ +1... │ XYZ     │ To Do  │          │
│  │ 3  │ Bob  │ b@co  │       │ DEF     │ To Do  │          │
│  └──────────────────────────────────────────────┘          │
│                                                              │
│  Additional Fields:                                         │
│  • jobTitle, businessIndustry, location, country            │
│  • linkedInURL, websiteURL, seniority                      │
│  • milestone, reasoning, postUrl                           │
└───────────────────────┬─────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                 N8N WORKFLOW ENGINE                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │      Schedule Trigger                        │          │
│  │      (Every 4 minutes, 11 AM - 4 PM)         │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │      Search Airtable Records                 │          │
│  │      • Filter: Call = "To Do"                │          │
│  │      • Limit: 1 lead at a time               │          │
│  │      • Fields: name, email, phone, company   │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │   Format & Extract Data (Python)             │          │
│  │                                               │          │
│  │   • Combine firstName + lastName             │          │
│  │   • Add "+" to phone number                  │          │
│  │   • Extract email, company, industry         │          │
│  │   • Format job title                         │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │   Validate Phone Number (If Node)            │          │
│  │                                               │          │
│  │   IF phoneNumber == "":                      │          │
│  │     → Update Status: "Number not Found"      │          │
│  │   ELSE:                                       │          │
│  │     → Continue to VAPI Call                  │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                    VAPI VOICE AI                             │
│               (AI Calling Platform)                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │      Initiate Call (HTTP Request)            │          │
│  │                                               │          │
│  │  POST https://api.vapi.ai/call               │          │
│  │  Headers:                                     │          │
│  │    Authorization: Bearer [YOUR_TOKEN]        │          │
│  │                                               │          │
│  │  Body:                                        │          │
│  │  {                                            │          │
│  │    "assistantId": "...",                     │          │
│  │    "phoneNumberId": "...",                   │          │
│  │    "customers": [{                           │          │
│  │      "number": "+1234567890",                │          │
│  │      "name": "John Doe",                     │          │
│  │      "email": "john@example.com"             │          │
│  │    }],                                        │          │
│  │    "assistantOverrides": {                   │          │
│  │      "variableValues": {                     │          │
│  │        "prospect_name": "John",              │          │
│  │        "company_name": "ABC Corp",           │          │
│  │        "industry": "Technology",             │          │
│  │        "email": "john@example.com",          │          │
│  │        "number": "+1234567890",              │          │
│  │        "prospect_title": "CEO"               │          │
│  │      }                                        │          │
│  │    }                                          │          │
│  │  }                                            │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │    AI Conducts Phone Call                    │          │
│  │                                               │          │
│  │  • Natural conversation                      │          │
│  │  • Uses prospect variables                   │          │
│  │  • Handles objections                        │          │
│  │  • Offers appointment booking                │          │
│  │  • Records call outcome                      │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│              N8N - UPDATE STATUS                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │   Update Airtable Record                     │          │
│  │   • Match by ID                              │          │
│  │   • Set Call status: "Done"                  │          │
│  │   • Timestamp update                         │          │
│  └──────────────────────────────────────────────┘          │
│                                                              │
└─────────────────────────────────────────────────────────────┘

        ↓ (If prospect requests booking during call)

┌─────────────────────────────────────────────────────────────┐
│              VAPI WEBHOOK RESPONSE                           │
│          (Tool Call: Book Appointment)                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │   Webhook Trigger                            │          │
│  │   POST /Calendly_tool                        │          │
│  │                                               │          │
│  │   Receives:                                   │          │
│  │   • Tool call ID                             │          │
│  │   • Function: book_appointment               │          │
│  │   • Arguments: preferred_timeslot            │          │
│  │   • Customer: name, email, number            │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        ↓
        ┌───────────────┴───────────────┐
        ↓                               ↓
┌──────────────────┐          ┌──────────────────┐
│  TWILIO (SMS)    │          │   GMAIL (Email)  │
├──────────────────┤          ├──────────────────┤
│                  │          │                  │
│ Send phone:   │          │ Send Email:      │
│                  │          │                  │
│ "Hello John,     │          │ Subject: Schedule│
│ Here's your      │          │ Your Appointment │
│ Calendly link:   │          │                  │
│ [LINK]"          │          │ Body: Calendly   │
│                  │          │ link + details   │
└──────────────────┘          └──────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│              RESPOND TO VAPI                                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Return to VAPI:                                            │
│  {                                                           │
│    "results": [{                                            │
│      "toolCallId": "...",                                   │
│      "result": "📧 An email and a phone message have    │
│                 been sent with the Calendly link."          │
│    }]                                                        │
│  }                                                           │
└─────────────────────────────────────────────────────────────┘
                        ↓
              [AI continues conversation]
              "Great! Check your email and text 
               for the booking link."
```

### Optional: Calendly Direct Booking (Disabled Nodes)

```
┌─────────────────────────────────────────────────────────────┐
│            CALENDLY INTEGRATION (ADVANCED)                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  If enabled, this flow checks availability and books:       │
│                                                              │
│  1. Get User URI (GET /users/me)                            │
│  2. Get Event Types (GET /event_types)                      │
│  3. Get Schedule UUID (GET /user_availability_schedules)    │
│  4. Check Availability (GET /user_busy_times)               │
│  5. If Available → Create Invitee (POST /invitees)          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| **Automation** | n8n | Workflow orchestration |
| **Voice AI** | VAPI | AI-powered phone calls |
| **CRM** | Airtable | Lead database and tracking |
| **SMS/phone** | Twilio | Multi-channel messaging |
| **Email** | Gmail | Email follow-ups |
| **Scheduling** | Calendly | Appointment booking |
| **Programming** | Python (n8n Code node) | Data formatting |
| **API** | HTTP Request nodes | Service integration |

---

## 📦 Prerequisites

### Required Accounts & API Keys

| Service | Required? | Purpose | Cost |
|---------|-----------|---------|------|
| **n8n** | ✅ Yes | Run workflows | Free (self-hosted) or $20/mo |
| **VAPI** | ✅ Yes | AI voice calls | $0.05-0.10/min |
| **Airtable** | ✅ Yes | Lead database | Free or $20/mo (Pro) |
| **Twilio** | ✅ Yes | phone/SMS | Pay-per-message (~$0.005-0.02) |
| **Gmail** | ✅ Yes | Email sending | Free |
| **Calendly** | ⚪ Optional | Appointment booking | Free or $10/mo |

### API Keys & Credentials Needed

- ✅ VAPI API Token (Bearer token)
- ✅ VAPI Assistant ID
- ✅ VAPI Phone Number ID
- ✅ Airtable Personal Access Token
- ✅ Airtable Base ID & Table ID
- ✅ Twilio Account SID & Auth Token
- ✅ Gmail OAuth2 credentials
- ⚪ Calendly API Key (if using direct booking)

### Technical Requirements

- n8n instance (v1.0+)
- Airtable base with lead data
- VAPI assistant configured
- Phone number from VAPI
- Stable internet connection

---

## 🚀 Installation

### Step 1: Download Workflow

1. Download `Protect_fortunes_voice_agent.json` from repository
2. Save to your computer

### Step 2: Import to n8n

1. Open your n8n instance
2. Click **"Workflows"** → **"Import from File"**
3. Select `Protect_fortunes_voice_agent.json`
4. Click **"Import"**
5. Workflow appears in canvas

---

### Step 3: Set Up Airtable Base

1. Go to https://airtable.com/
2. Create new base: **"Leads Database"**
3. Create table: **"Sheet1"**

**Required Fields (columns):**
```
• firstName (Single line text)
• lastName (Single line text)
• emailAddress (Email)
• Number (Phone number) - store without "+"
• companyName (Single line text)
• businessIndustry (Single select)
• jobTitle (Single line text)
• Call (Single select: To Do, Done, Number not Found)
```

**Optional Fields:**
```
• linkedInURL
• seniority
• location
• country
• websiteURL
• milestone
• reasoning
• postUrl
• emailStatus
```

4. Add sample leads with "To Do" status
5. Copy Base ID and Table ID from URL:
```
https://airtable.com/[BASE_ID]/[TABLE_ID]
```

---

### Step 4: Create VAPI Assistant

1. Go to https://vapi.ai/
2. Sign up / log in
3. Click **"Assistants"** → **"Create New"**

**Configure Assistant:**

**Name:** Real Estate Lead Caller

**First Message:**
```
Hi {{prospect_name}}! This is Sarah from [Your Company]. 
I'm calling about [your service]. Do you have a quick minute to chat?
```

**System Prompt:**
```
You are a professional [real estate/financial] advisor calling to 
qualify leads and book appointments. 

Prospect Information:
- Name: {{prospect_name}}
- Company: {{company_name}}
- Industry: {{industry}}
- Title: {{prospect_title}}

Your goals:
1. Introduce yourself warmly
2. Reference their company and industry
3. Explain your service briefly
4. Gauge interest
5. Offer to send a Calendly booking link
6. If they want to book, call the book_appointment function

Be conversational, professional, and respectful of their time.
If they're not interested, thank them and end the call politely.
```

**Add Function (Tool Call):**

Name: `book_appointment`

Description: `Books an appointment by sending Calendly link via email and phone`

Parameters:
```json
{
  "type": "object",
  "properties": {
    "preferred_timeslot": {
      "type": "string",
      "description": "The date/time prospect wants to meet (ISO format)"
    }
  },
  "required": ["preferred_timeslot"]
}
```

**Save Assistant** and copy the **Assistant ID**

---

### Step 5: Get VAPI Phone Number

1. In VAPI dashboard, go to **"Phone Numbers"**
2. Purchase or select existing number
3. Copy the **Phone Number ID**

---

### Step 6: Configure n8n Workflow Nodes

#### Update: Schedule Trigger

Current: Every 4 minutes, 11 AM - 4 PM (America/Chicago)

**Cron Expression:** `*/4 11-16 * * *`

To change:
- Every 10 minutes: `*/10 11-16 * * *`
- Every hour: `0 11-16 * * *`
- Custom hours: Modify `11-16` to your range

---

#### Update: Search records (Airtable)

1. Click on **"Search records"** node
2. Add Airtable credential
3. Select your Base
4. Select your Table
5. Update filter: `{Call}='To Do'`
6. Limit: 1 (one call at a time)

---

#### Update: Start Marketing Call (VAPI)

1. Click on **"Start Marketing Call (VAPI)"** node
2. Update URL (leave as is): `https://api.vapi.ai/call`
3. Update Headers:
   - Authorization: `Bearer YOUR_VAPI_TOKEN`

4. Update JSON Body - replace IDs:

**Before:**
```javascript
{
  "assistantId": "4a5ef345-13ad-4a4f-af44-90b55168c8a8",
  "phoneNumberId": "4d0f9789-1e6d-4990-bac0-6bda6db3e6c9",
  ...
}
```

**After:**
```javascript
{
  "assistantId": "YOUR_ASSISTANT_ID",
  "phoneNumberId": "YOUR_PHONE_NUMBER_ID",
  ...
}
```

---

#### Update: Send phone/SMS (Twilio)

1. Click **"Send an SMS/MMS/phone message"** node
2. Add Twilio credential:
   - Account SID
   - Auth Token
3. Update **From** number: Your Twilio phone number
4. Customize message template

---

#### Update: Send Email (Gmail)

1. Click **"Send a message"** node
2. Add Gmail OAuth2 credential
3. Authorize access
4. Customize email template:

```
Subject: Schedule Your Appointment with [Your Company]

Hello {{ prospect_name }},

I'd like to invite you to book a convenient time for a consultation...

👉 https://calendly.com/your-link/meeting

Looking forward to connecting,
[Your Name]
📧 contact@yourcompany.com
🌐 www.yourcompany.com
```

---

#### Update: Webhook (VAPI Response)

1. Click **"VAPI Call Response Webhook"** node
2. Copy the **Production URL**
3. Save for VAPI configuration

**Example URL:**
```
https://your-n8n.app.n8n.cloud/webhook/Calendly_tool
```

---

#### Update: Airtable Update Nodes

**For "Create or update a record":**
- Select same Airtable credential
- Same Base and Table
- Updates Call status to "Done"

**For "Update record":**
- Same credential, Base, Table
- Updates Call status to "Number not Found"

---

### Step 7: Connect VAPI Webhook

1. In VAPI dashboard, go to your Assistant
2. Find **"Function Calls"** or **"Server URL"** section
3. Paste your n8n webhook URL
4. Save configuration

---

### Step 8: Test the Workflow

**Important: Start with test numbers first!**

1. Add test lead to Airtable:
   ```
   firstName: Test
   lastName: User
   emailAddress: your@email.com
   Number: YOUR_PHONE_NUMBER (without +)
   companyName: Test Co
   businessIndustry: Technology
   jobTitle: Manager
   Call: To Do
   ```

2. **Execute workflow manually:**
   - Click **"Execute Workflow"** button
   - Watch nodes light up
   - VAPI should call your test number

3. **Answer the call:**
   - AI should introduce itself
   - Use your prospect information
   - Offer appointment booking

4. **Request booking link during call:**
   - Say "Yes, send me the link"
   - Check email and phone for messages

5. **Verify Airtable update:**
   - Call status should change to "Done"

---

## ⚙️ Configuration

### Customize Calling Schedule

**Current: Every 4 minutes, 11 AM - 4 PM**

**Cron Format:** `minute hour day month weekday`

**Common Schedules:**

**Every 5 minutes, 9 AM - 5 PM:**
```
*/5 9-17 * * *
```

**Every 10 minutes, weekdays only:**
```
*/10 9-17 * * 1-5
```

**Every 30 minutes:**
```
*/30 * * * *
```

**Specific times (9 AM, 12 PM, 3 PM):**
```
0 9,12,15 * * *
```

---

### Adjust Timezone

1. Click workflow settings (gear icon)
2. Find **Timezone** setting
3. Current: `America/Chicago`
4. Change to your timezone:
   ```
   America/New_York
   America/Los_Angeles
   Europe/London
   Asia/Dubai
   ```

---

### Modify Call Limit

Currently calls 1 lead per execution.

To call multiple leads:

1. Find **"Search records"** node
2. Change Limit from `1` to desired number
3. The workflow will process one at a time automatically

---

### Customize VAPI Script

**Update First Message:**
```
Hi {{prospect_name}}! This is [Your Name] with [Your Company]. 
I noticed you're [job title] at {{company_name}}. 
I'd love to chat about [your offer]. Got 2 minutes?
```

**Update System Prompt - Real Estate Example:**
```
You are a real estate agent calling property leads.

Prospect Info:
- Name: {{prospect_name}}
- Current Location: [from data]
- Looking for: [property type]

Script:
1. Warm introduction
2. Reference their property interest
3. Ask about timeline and budget
4. Highlight 2-3 relevant listings
5. Offer virtual tour or in-person showing
6. Book appointment if interested

Be enthusiastic but not pushy. Focus on helping them find their dream home.
```

---

### Add More Variables to VAPI

In **"Start Marketing Call"** node, add to `variableValues`:

```javascript
"assistantOverrides": {
  "variableValues": {
    "prospect_name": "...",
    "company_name": "...",
    "industry": "...",
    "email": "...",
    "number": "...",
    "prospect_title": "...",
    // ADD MORE:
    "location": "{{ $json.location }}",
    "linkedin": "{{ $json.linkedInURL }}",
    "website": "{{ $json.websiteURL }}"
  }
}
```

Then use in VAPI: `{{location}}`, `{{linkedin}}`, etc.

---

## 📖 Workflow Breakdown

### Node-by-Node Explanation

**1. Schedule Trigger**
- Runs every 4 minutes
- Time: 11 AM - 4 PM (configurable)
- Starts entire workflow automatically

**2. Search records (Airtable)**
- Searches for leads with Call = "To Do"
- Returns 1 lead per execution
- Extracts all relevant fields

**3. Format and Extract only Required data (Python)**
```python
# Combines firstName + lastName
dict['name'] = item.json.get('firstName', '') + " " + item.json.get('lastName', '')

# Adds + to phone number
phone = item.json.get("Number", "")
dict['phoneNumber'] = '+' + phone if phone else ""

# Extracts other fields
dict['jobTitle'] = item.json.get('jobTitle', '')
dict['email'] = item.json.get('emailAddress', '')
dict['industry'] = item.json.get('businessIndustry', '')
dict['company'] = item.json.get('companyName', '')
```

**4. If (Phone Validation)**
- Checks if phone number is empty
- True: Updates Airtable → "Number not Found"
- False: Continues to VAPI call

**5. Start Marketing Call (VAPI)**
- Initiates AI phone call
- Passes customer data and variables
- Returns call ID and status

**6. Create or update a record (Airtable)**
- Updates lead status to "Done"
- Matches by record ID
- Timestamps the update

**7. VAPI Call Response Webhook**
- Receives tool calls from VAPI
- Triggered when prospect requests booking
- Extracts customer info and preferences

**8. Send an SMS/MMS/phone message (Twilio)**
- Sends phone with Calendly link
- Uses prospect's phone number
- Branded message template

**9. Send a message (Gmail)**
- Sends email with appointment details
- Calendly link included
- Professional template

**10. Respond to Webhook2**
- Returns success message to VAPI
- AI confirms link was sent
- Continues conversation

---

### Disabled Nodes (Calendly Direct Booking)

The workflow includes optional Calendly integration for direct booking:

- **Get User URI** - Fetches Calendly user ID
- **Get UUID Of Event Types** - Gets event type for booking
- **Get Schedule UUID** - Retrieves availability schedule
- **Add Default slot Time** - Calculates meeting duration
- **Check Availability** - Verifies time slot is free
- **If2** - Checks if slot is available
- **HTTP Request** - Books the appointment

**To enable:** Connect these nodes after "VAPI Call Response Webhook"

---

## 🎯 Use Cases

### 1. Real Estate Lead Follow-Up

**Scenario:** Call property inquiries automatically

**Setup:**
- Airtable: Property interests, budget, location
- VAPI Script: Reference property type and area
- Follow-up: Virtual tour booking links

**Variables:**
```
property_type: "3-bedroom house"
budget_range: "$300k-400k"
preferred_area: "Downtown"
move_timeline: "3 months"
```

---

### 2. Financial Services Prospecting

**Scenario:** Retirement planning consultations

**Setup:**
- Airtable: Age, income bracket, current savings
- VAPI Script: Discuss retirement goals
- Follow-up: Financial planning session booking

**Current Example:** Protect Fortunes workflow

---

### 3. Insurance Policy Follow-Up

**Scenario:** Call insurance quote requests

**Setup:**
- Airtable: Coverage type, current provider
- VAPI Script: Compare quotes and benefits
- Follow-up: Policy review appointment

---

### 4. Loan Application Outreach

**Scenario:** Mortgage pre-qualification calls

**Setup:**
- Airtable: Loan amount, property type
- VAPI Script: Discuss rates and terms
- Follow-up: Application completion link

---

### 5. Event Registration Follow-Up

**Scenario:** Confirm webinar/seminar attendance

**Setup:**
- Airtable: Event interest, topics
- VAPI Script: Confirm details and benefits
- Follow-up: Registration link and calendar invite

---

## 🐛 Troubleshooting

### Issue: VAPI Call Not Initiating

**Symptoms:** No phone call made

**Solutions:**
1. Verify VAPI API token is correct
2. Check Assistant ID and Phone Number ID
3. Ensure VAPI account has credits
4. Test phone number is valid (E.164 format)
5. Check n8n execution logs for errors
6. Verify JSON body syntax in HTTP Request

---

### Issue: Airtable Not Updating

**Symptoms:** Call status stays "To Do"

**Solutions:**
1. Verify Airtable credential is connected
2. Check Base ID and Table ID are correct
3. Ensure "Call" field exists in table
4. Test Airtable API permissions
5. Check record ID is being passed correctly

---

### Issue: Webhook Not Receiving Data

**Symptoms:** No follow-up email/SMS sent

**Solutions:**
1. Verify webhook URL is correct
2. Check VAPI assistant has server URL configured
3. Test webhook manually with test data
4. Ensure workflow is active
5. Check webhook path matches: `/Calendly_tool`

---

### Issue: phone/SMS Not Sending

**Symptoms:** Twilio messages fail

**Solutions:**
1. Verify Twilio credentials (SID + Auth Token)
2. Check "From" number is phone-enabled
3. Ensure recipient number has correct format
4. Verify Twilio account has credit
5. Check phone opt-in requirements

---

### Issue: Gmail Not Sending

**Symptoms:** Email fails to send

**Solutions:**
1. Re-authorize Gmail OAuth2
2. Check email template syntax
3. Verify recipient email exists
4. Test with personal email first
5. Check Gmail API quotas

---

### Issue: Schedule Not Running

**Symptoms:** Workflow doesn't execute automatically

**Solutions:**
1. Activate workflow (toggle in top-right)
2. Verify cron expression is correct
3. Check timezone setting
4. Ensure at least one "To Do" lead exists
5. Review n8n execution history

---

### Issue: Phone Number Validation Failing

**Symptoms:** All leads marked "Number not Found"

**Solutions:**
1. Check Python code in format node
2. Verify Number field in Airtable has data
3. Ensure numbers are stored without "+"
4. Test with different phone formats
5. Add logging to Python node:
   ```python
   print(f"Phone: {phone}")
   ```

---

## 💡 Best Practices

### Lead Management

**Airtable Organization:**
- Keep leads clean and updated
- Use consistent naming
- Regular data validation
- Archive old leads
- Tag by campaign source

**Status Tracking:**
```
To Do → Ready to call
Done → Call completed
Number not Found → Invalid contact
Booked → Appointment scheduled (add this)
Not Interested → Declined (add this)
```

---

### Calling Strategy

**Best Times:**
- **B2B:** 10 AM - 12 PM, 2 PM - 4 PM
- **B2C:** 6 PM - 8 PM weekdays, 10 AM - 2 PM weekends
- Avoid: Early morning, late evening, lunch time

**Frequency:**
- First attempt: Immediate
- Second attempt: 2-3 days later
- Third attempt: 1 week later
- Max attempts: 3-5

**Call Duration:**
- Aim for: 2-5 minutes
- Too short: Seems rushed
- Too long: Loses interest

---

### VAPI Script Optimization

**Effective Openings:**
```
✅ "Hi John! This is Sarah. Got 2 minutes?"
✅ "Hi! I'm calling about your inquiry for..."
✅ "Quick question - are you still interested in...?"

❌ "Hello, how are you today?" (too salesy)
❌ "Sorry to bother you..." (negative)
❌ Long-winded introduction
```

**Handling Objections:**
```
"Not interested"
→ "I understand. Can I ask what changed?"

"Too busy"
→ "I'll make this quick - just 90 seconds?"

"Call back later"
→ "Sure! What time works best for you?"

"Already working with someone"
→ "That's great! How's that going for you?"
```

---

### Cost Optimization

**VAPI Costs:**
- Typical: $0.05-0.10 per minute
- Average call: 3 minutes = $0.15-0.30
- 100 calls/day = $15-30/day

**Reduce costs:**
- Keep calls short (under 5 min)
- Pre-qualify leads (better targeting)
- Use text follow-up instead of calls for low-priority
- Schedule during best conversion times

**Twilio Costs:**
- SMS: ~$0.0075 per message
- phone: ~$0.005 per message
- 100 messages = $0.50-0.75

**Total Monthly Cost (100 calls/day):**
```
VAPI: $450-900
Twilio: $15-30
Gmail: Free
Airtable: $20
n8n: $20 (cloud) or $0 (self-hosted)
Total: $505-970/month
```

---

### Data Privacy & Compliance

**GDPR/Privacy:**
- Get consent before calling
- Offer opt-out option
- Store data securely
- Delete upon request

**TCPA Compliance (US):**
- Only call during allowed hours (8 AM - 9 PM)
- Maintain do-not-call list
- Provide opt-out mechanism
- Keep call records

**Best Practices:**
- Record consent in Airtable
- Include privacy policy link in emails
- Honor unsubscribe requests immediately
- Regular data audits

---

## 📊 Performance Tracking

### Key Metrics to Monitor

**In Airtable:**
- Total leads
- Calls made (Done)
- Conversion rate (Booked / Done)
- Invalid numbers (Number not Found)
- Average time to call

**In VAPI Dashboard:**
- Call duration
- Successful connections
- Tool calls triggered (bookings)
- Cost per call

**Create Views:**
1. **To Call Today** - Call = "To Do"
2. **Called This Week** - Call = "Done", filter by date
3. **Appointments Booked** - Call = "Booked"
4. **Need Follow-up** - Called but no booking

---

## 🚀 Advanced Customization

### Add SMS Confirmation

After booking, send confirmation SMS:

1. Add Twilio node after booking
2. Message template:
```
Hi {{name}}! Your appointment with [Company] is confirmed for 
{{date}} at {{time}}. 

Reply CANCEL to reschedule.
```

---

### Integrate with CRM

Replace Airtable with:
- **Salesforce** - Use n8n Salesforce nodes
- **HubSpot** - HubSpot integration
- **Pipedrive** - Pipedrive nodes

Same workflow logic applies!

---

### Add Lead Scoring

Before calling, score leads:

**Python scoring logic:**
```python
score = 0
if job_title in ['CEO', 'Owner', 'President']:
    score += 30
if industry in ['Finance', 'Technology']:
    score += 20
if email_status == 'verified':
    score += 10

# Only call if score > 40
if score >= 40:
    return "proceed_to_call"
```

---

### Multi-Campaign Support

Add campaign field in Airtable:

```
campaign_name: "Q4 Promotion"
campaign_type: "Cold Outreach"
```

Update VAPI variables:
```javascript
"campaign": "{{ $json.campaign_name }}"
```

Use in script: `{{campaign}}`

---

## 📄 License

This project is licensed under the **MIT License**.

✅ Commercial use allowed  
✅ Modification allowed  
✅ Distribution allowed  
✅ Private use allowed  
⚠️ No warranty or liability

---

## 🌟 Acknowledgments

Built with powerful tools:

- [n8n](https://n8n.io) - Workflow automation
- [VAPI](https://vapi.ai) - Voice AI platform
- [Airtable](https://airtable.com) - Lead database
- [Twilio](https://twilio.com) - phone/SMS
- [Gmail](https://gmail.com) - Email delivery
- [Calendly](https://calendly.com) - Appointment scheduling

---

## 📞 Ready to Automate Your Calls!

**Start calling leads automatically:**

1. ✅ Set up Airtable with leads
2. ✅ Configure VAPI assistant
3. ✅ Import workflow to n8n
4. ✅ Add all API keys
5. ✅ Test with your number
6. ✅ Activate and let it run!

**Made with 📞 for sales and business professionals**

⭐ Scale your outreach effortlessly!  
🤖 AI handles the calls!  
📈 Book more appointments!

---
