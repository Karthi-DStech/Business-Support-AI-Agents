# Business-Support-AI-Agents

This repo is a collection of production-ready n8n workflows that pair LLM agents with common business systems (Gmail, Google Drive/Sheets, Notion, Supabase, Telegram/WhatsApp). AI agents designed to automate business workflows and customer support processes, enabling real-time query handling, task automation, and decision-making to improve efficiency, reduce manual effort, and enhance customer experience.

Each workflow shows a repeatable pattern follows:

**trigger → capture → understand → LLM reasoning → SaaS action → automate → log → persistent record**

They’re modular, cost-aware and built to keep humans in control where it matters (draft-first, guardrails, structured outputs). Also, each workflow is built to industry standards with a modular, component-based architecture where every element is reusable and easy to adapt amd meets new business needs.


### 📂 Project Descriptions

✨ Each project is described in detail below. Click a project name to expand and view its full explanation.
<br><br>


<details>
<summary><b> Automated Customer Sentiment Analysis - Logging & Reply Email </b></summary>

**A fully automated customer-feedback pipeline built with n8n.**

- **Collect & Classify**: A public form gathers name, email, and comments, then an OpenRouter-powered LLM performs instant sentiment analysis (Positive / Negative).

- **Positive Flow**: Positive feedback is logged to a dedicated Google Sheet, a friendly HTML thank-you email is drafted by the AI, and a personalized reply is sent automatically.

- **Negative Flow**: Negative feedback is saved to a separate sheet along with an AI-generated improvement suggestion, and the customer receives a courteous apology email offering a complimentary AI-agent integration.

This workflow provides a seamless loop—from feedback capture to sentiment insight and tailored response—helping teams measure satisfaction, close the feedback loop, and delight customers without manual effort.

</details>



<details>
<summary><b> Customer Support & Booking AI Agent for Any Business </b></summary>
An intelligent restaurant-assistant workflow built with **n8n**, **OpenAI**, and **Postgres PGVector**.

- **Conversational Support**: Incoming chat messages trigger “Rachel,” a friendly AI agent trained to answer questions about Bella Vista’s menu, hours, and policies using a PGVector knowledge base and persistent Postgres chat memory for context.  

- **Smart Bookings**: When a guest wants a reservation, Rachel gathers essential details (name, email, party size, date, time, special requests) and automatically emails the booking to the restaurant via Gmail.  

- **Human Handoff**: If a customer asks for a real staff member, the agent collects their contact information and uses a Gmail tool to alert human support, ensuring a seamless transition.

This setup delivers a **full-service, always-on booking and support agent**, blending real-time LLM conversation with structured database retrieval and email automation.


</details>




<details>
<summary><b> Finance Tracking AI System and Logging </b></summary>

An AI-powered personal finance assistant that records and organizes every expense or income you mention in chat.

- **Smart Expense Parsing** – Users can simply type natural phrases like *“Paid $50 for groceries today”* or *“Salary 3000 on March 1”*.  
  The agent interprets amount, description, date, and whether it’s an **Expense** or **Income**, defaulting to year **2025** for easy future tracking.

- **Automated Ledger Updates** – After extracting key details, the workflow automatically appends a clean, structured row to a Google Sheet named **Expense Tracker**, creating a live financial ledger without manual entry.

- **Context-Aware Conversations** – Short-term memory lets the agent handle follow-ups such as *“add $20 taxi on the same date”* or *“make that $25”*, updating the sheet accurately.

- **Flexible AI Backbone** – Uses both **OpenAI GPT-4.1-mini** and **OpenRouter** models for natural language understanding and reliable fallback, ensuring robust parsing and continuous service.

This workflow acts as a **hands-free personal bookkeeping system**, turning everyday chat messages into a well-organized financial record ready for budgeting and analysis.

</details>








<details>
<summary><b> Full Automated Onboading System for Clients </b></summary>
An AI-driven intake pipeline that transforms a simple web form into a fully automated onboarding process.

- **Intelligent Data Capture** – Clients submit name, contact info, industry, and project goals through a styled onboarding form. Two LLM agents (GPT-4.1 / GPT-4o with OpenRouter fallback) analyze the submission in real time.  
- **Dynamic Email Generation** – The first agent crafts a personalized HTML welcome email that reflects the client’s industry and stated challenges, ensuring every message feels human and context-aware.  
- **Profile Summarization & Storage** – A second agent distills the details into a concise client summary and writes a structured record (Name, Email, Phone, Summary) directly to a Google Sheet, ready for CRM or sales follow-up.  
- **LLM Coordination & Reliability** – Uses multiple language models for parallel tasks—email composition, information extraction, and JSON parsing—providing accuracy, graceful fallback, and consistent formatting.

This workflow shows how **orchestrated LLMs can replace manual onboarding**, delivering instant, tailored communication while keeping customer data neatly organized for downstream business operations.

</details>








<details>
<summary><b> Gmail Multi Agent for Automatic Response </b></summary>

A multi-agent email desk that classifies inbound Gmail and drafts polished replies for each scenario—ready for human review.

- **Auto-Triage with LLM Classifier** – Every new email is labeled into **Support**, **Meeting Request**, **Client Inquiry**, or **Follow-Up** using an AI classifier; messages get the right Gmail label and route to the right agent.  
- **Specialist Agents per Thread Type** – Dedicated agents generate responses with strict **JSON** fields (subject, body) via structured output parsers, ensuring consistent formatting and safer automation.  
- **Calendar-Aware Scheduling** – The Meeting Agent can read Google Calendar availability (Get Events) and propose slots; responses adapt whether a scheduling link exists or not.  
- **Attachment-Friendly & Draft-First** – Polls Gmail hourly, downloads attachments when present, and creates **drafts** (not auto-send) addressed to the original recipient for quick human approval.  
- **Scalable & Maintainable** – Each path (Inquiry/Support/Meeting/Follow-Up) is independent, so teams can tweak tone, templates, or escalation rules without impacting the others.

This workflow delivers a reliable **LLM-assisted inbox**: fast triage, accurate categorization, and high-quality drafts that keep humans in control.

</details>








<details>
<summary><b> Invoice Processing and DB Management Multi Agent </b></summary>

Always-on pipeline that pulls **PDF invoices** from Gmail, extracts structured data with an LLM, and logs everything in **Notion**.

- **Auto-capture & Storage** – Polls Gmail every minute for emails with PDF attachments, splits multi-file threads, and uploads each file to a Google Drive folder (keeps subject + filename for traceability).  

- **LLM Gate & Parsing** – An “Invoice Controller” LLM first decides **invoice vs. not**; non-invoices are deleted from Drive. For invoices, a structured LLM chain extracts: **invoice_name, company_name, total_invoice_amount, line_items[{description, amount}]**, with schema-constrained output.  

- **Notion Sync** – Creates a page in **Invoice Tracking** (subject, sender email, Drive link, total) and iterates **line_items** into an **Invoice Line Items** database—ready for reporting and reconciliation.  

- **Data Integrity** – Prompts enforce itemization and total checks (tolerant to minor rounding), while merge keys and per-file batching ensure reliable, idempotent processing across threads.

This workflow turns your inbox into a **hands-free AP pipeline**: reliable capture, structured extraction, and clean finance records in Notion.

</details>








<details>
<summary><b> Monthly Invoice Summarising & Mail Notif & DB Manager Agent </b></summary>

Hands-free month-end rollup. Just drop a folder of PDFs into **Google Drive** and get a **Notion** summary + email recap.

- **Drive Watch & Prep** – When a new folder appears in **Monthly Invoices**, the workflow waits 2 minutes, lists all PDFs, downloads and OCRs them for clean text.  

- **LLM Guardrail & Extraction** – An “Invoice Controller” LLM filters non-invoices; valid files are parsed into **invoice_name, company_name, total_invoice_amount, month, category** (from a fixed set like *Software & Tools, Web & Hosting,* etc.) with a structured output parser.  

- **Notion Logging** – Each invoice is written to the **All Invoices** database (Month, From, Amount, Category), creating a normalized ledger for analytics.  

- **Monthly Rollup** – The workflow aggregates amounts, then an AI agent returns the **total spend** and **invoice count**; results are saved to **Monthly Summary in PDF Format** and a recap email is sent (totals, count, vendors).  

This delivers a dependable **month-close assistant**: accurate categorization, clean records in Notion, and a ready-to-share summary email.```

</details>




<details>
<summary><b> Nutritionist & Gmail Alert Multi Agent </b></summary>
A multimodal nutrition coach that reads **texts, voice notes, and meal photos** from Telegram, estimates macros with vision LLMs, logs everything to Sheets, and gives **goal-aware guidance**.

- **Multimodal intake** — Detects message type (text/voice/image). Voice is **transcribed**; images are uploaded (Drive/Cloudinary) and passed to a **vision LLM** for itemization, portion sizing, and macro/calorie ranges with clear assumptions.  

- **Structured nutrition facts** — The LLM returns a strict JSON (items, macros, kcal ranges + totals). Values are normalized (midpoints) and **appended to Google Sheets** with timestamp, picture link, and user name.  

- **Personal targets & memory** — Pulls the user’s **goals and daily calorie target** from a Goals sheet, keeps short session memory for continuity, and personalizes outputs to their plan.  

- **Two AI coaches**  
  - **Nutrient AI Agent:** scans last 7 days of logged meals, computes remaining weekly allowance vs. goal, and replies with **one clear sentence** on whether today’s request fits the deficit.  

  - **Motivation Coach:** formats a compact **nutrition card** (kcal/macros + meal name) and delivers **≤2 short coaching lines**, adapting tone (on-track, restaurant caveats, or off-track).  

- **Automation plumbing** — Image URL generation, safe JSON parsing, and data aggregation are handled in-flow so users get **instant feedback** plus a durable history.

Best for busy professionals who want fast nutrition feedback, minimal typing, and auto-logged history and get automated messages through telegream.

</details>





<details>
<summary><b> Real Estate Multi Agent </b></summary>
Automates real-estate lead analysis: from a user form to a daily email of the best investment opportunities.

**How It Works**

1. **Form Intake** – Collects search filters (location, price, beds/baths, etc.).  

2. **Listing Fetch** – Queries Zillow API and normalizes property data.  

3. **Deal Analysis** – A Code node applies standard underwriting (20 % down, 5 % APR, taxes/insurance/maintenance) to compute *Monthly cash flow, Cap rate, Cash-on-cash ROI, Price & rent per sqft*.  

4. **Data Storage** – Results are upserted to a Google Sheet for ongoing tracking.  

5. **Daily KPI Report** – An LLM aggregates all rows and emails an HTML report with:
   - Market sentiment  
   - **Top 3 by ROI & Cap Rate** with cash-flow figures  
   - Portfolio averages and negative-cash-flow alerts.

**Key Capabilities**
- End-to-end automation: form → Zillow → finance math → Sheets → Gmail.  
- Flexible assumptions in the Code node for interest, taxes, or maintenance.  
- Clear, investor-ready metrics delivered to your inbox each morning.

</details>





<details>
<summary><b> WhatsApp Agent & Supabase Datastore </b></summary>
A WhatsApp chatbot with **persistent memory** powered by Supabase and GPT-4o-mini.

**Business Logic**

- Retrieves past user facts from Supabase and merges them with each new message.  
- LLM auto-detects and saves key details (e.g., preferences, events) without user prompts.  
- Replies feel personal and continuous across long gaps.

**LLM Capabilities**

- **Extended Recall**: Uses Supabase to remember conversations beyond the chat window.  
- **Fact Extraction**: Summarizes important info into structured records.  
- **Contextual Reasoning**: Generates answers enriched by historical context.

Ideal for personalized assistants, coaching bots, or customer support with long-term memory.

</details>



---

### Installation & Setup

- Prerequisites (Node.js / n8n / Docker, etc.)

- Steps to clone the repo, install dependencies, and run n8n locally or deploy to cloud.

- Environment variables needed for APIs (OpenAI, Supabase, Gmail, etc.).

- Load the JSON and make the connections. 
