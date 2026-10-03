# Auto Ticket Classification using Flow Designer

## Project Overview

The **Auto Ticket Classification using Flow Designer** project automates the classification of IT support tickets for a school IT helpdesk using **ServiceNow Flow Designer**.

The system analyzes keywords in the Incident short description and automatically assigns the appropriate **Category** and **Subcategory**. It also sends an email notification to the caller after ticket creation.

The solution is designed as a no-code, maintainable, and scalable automation. fileciteturn1file0L2-L13

## Problem Statement

The school IT helpdesk receives multiple incident requests from students and teachers, including Wi-Fi issues, projector failures, password problems, and slow computers. IT staff currently review each request manually and assign a category, which is time-consuming and inefficient.

## Objectives

- Automatically classify incidents at the time of creation.
- Reduce manual effort for IT agents.
- Improve ticket routing efficiency.
- Implement a no-code and easily maintainable solution.
- Automatically assign Category and Subcategory.
- Send an automated email notification to the caller.
- Store ticket information in a structured format.
- Support future maintenance and scalability.

## Technologies Used

- ServiceNow
- Flow Designer
- Custom Tables
- Choice Fields
- Reference Fields
- Update Sets
- Email Notifications
- No-code Automation

---

# Phase 1: Requirement Analysis & Planning

### Business Requirements

The system must:

1. Automatically classify IT tickets based on the issue description.
2. Assign both Category and Subcategory without manual intervention.
3. Support dependent choice logic between Category and Subcategory.
4. Send an automated email notification to the caller upon ticket creation.
5. Store ticket information in a structured and standardized format.
6. Ensure easy maintenance and future scalability.

### Create a New Update Set

Navigate to:

**All → System Update Sets → Local Update Sets**

Create a new Update Set:

- **Name:** `Project Update Set`
- **State:** `In progress`
- **Application:** `Global`

Save the form and select **Make This My Current Set**.

---

# Phase 2: Backend Development & Configuration

## Activity 1: Create Custom Table

Navigate to:

**All → System Definition → Tables**

Create a new table.

- **Label:** `Incident WorkFlow`
- **Create module:** Unchecked
- **Auto Number:** Enabled

Save the table and create the required fields using Form Designer.

## Activity 2: Field Creation

| Field Label | Type | Reference / Choices |
|---|---|---|
| Number | Auto Number | — |
| Caller | Reference | `sys_user` |
| Category | Choice | Network, Hardware, Access, Performance |
| Subcategory | Choice | Wi-Fi, Projector, Forgot Password, Slow Computer |
| Short Description | String | — |
| Description | String | — |
| State | Choice | New, In progress, On hold, Resolved, Closed |
| Assigned Group | Reference | `sys_user_group` |
| Assigned To | Reference | `sys_user` |

Add choices to **Category**, **Subcategory**, and **State**, then save the form.

## Activity 3: Category and Subcategory Dependency

Configure the **Subcategory** field to depend on **Category**.

Navigate to:

**Subcategory → Configure Dictionary → Advanced View**

Set:

- **Use dependent field:** `true`
- **Dependent on:** `Category`

### Dependency Mapping

| Subcategory | Category |
|---|---|
| Wi-Fi | Network |
| Projector | Hardware |
| Forgot Password | Access |
| Slow Computer | Performance |

This makes Subcategory options dynamically depend on the selected Category. fileciteturn1file0L91-L114

---

# Phase 3: Automation Using Flow Designer & Email Notification

When a student creates an IT support ticket, Flow Designer evaluates the Short Description for predefined keywords and automatically assigns the appropriate Category and Subcategory. fileciteturn1file0L116-L135

### Classification Rules

| Keyword | Category | Subcategory |
|---|---|---|
| WiFi / Network | Network | Wi-Fi |
| Projector | Hardware | Projector |
| Password / Login | Access | Forgot Password |
| Slow / Hanging | Performance | Slow Computer |

## Activity 1: Create Flow

Navigate to:

**All → Process Automation → Flow Designer**

Create a new Flow:

- **Name:** `Auto Classify School IT Tickets`
- **Application:** `Global`

Click **Build Flow**.

## Activity 2: Trigger Configuration

Configure:

- **Trigger:** `Record Created`
- **Table:** `Incident Workflow`
- **Condition:** `Category is Empty`

Then add Flow Logic using conditional branches.

## Activity 3: Wi-Fi Classification

Create an **If** condition:

- Short Description contains `Wi-Fi`
- Short Description contains `Network`

Then use **Update Record**:

- **Category:** `Network`
- **Subcategory:** `Wi-Fi`

## Activity 4: Projector Classification

Create an **Else If** condition:

- Short Description contains `Projector`

Update the record:

- **Category:** `Hardware`
- **Subcategory:** `Projector`

## Activity 5: Password Classification

Create an **Else If** condition:

- Short Description contains `Forgot password`

Update the record:

- **Category:** `Access`
- **Subcategory:** `Forgot Password`

## Activity 6: Slow Computer Classification

Create an **Else If** condition:

- Short Description contains `Slow Computer`

Update the record:

- **Category:** `Performance`
- **Subcategory:** `Slow Computer`

## Activity 7: Email Notification

Configure a **Send Email** action:

- **To:** Caller → Email
- **Subject:** `Your Request for the issue has been submitted.`
- **Body:** Ticket confirmation message

Finally, click **Activate**. fileciteturn1file0L196-L204

---

# Phase 4: Testing & Validation

## Data Validation Checks

Verify:

- Mandatory fields are captured correctly.
- Auto-number is generated without duplication.
- Category and Subcategory values are stored accurately.
- Reference fields resolve correctly to user and group tables.

## Test Scenario 1: Wi-Fi Issue

1. Enter a Caller.
2. Set Short Description to `WiFi not working in library`.
3. Save the form.
4. Reload the form.

### Expected Result

- **Category:** Network
- **Subcategory:** Wi-Fi
- Email is sent to the caller.

## Check Email Notification

Navigate to:

**All → System Logs → Emails**

Search using the configured subject, open the email record, and select **Preview Mail**.

## Test Scenario 2: Projector Issue

1. Enter a Caller.
2. Set Short Description to `Projector not turning on`.
3. Save the form.
4. Reload the form.

### Expected Result

- **Category:** Hardware
- **Subcategory:** Projector
- Email is sent to the caller. fileciteturn1file0L217-L242

---

# Phase 5: Deployment & Final Documentation

## Complete the Update Set

Navigate to:

**All → System Update Sets → Local Update Sets**

Open `Project Update Set`.

Change:

`In progress` → `Complete`

Save the form.

Under Related Links, select **Export to XML**. The Update Set XML file can then be downloaded and shared. fileciteturn1file0L243-L258

---

# End-to-End Workflow

```text
Student/Teacher Creates Ticket
            ↓
       Record Created
            ↓
      Flow Designer
            ↓
   Read Short Description
            ↓
   ┌────────┼──────────────┐
   ↓        ↓              ↓
 WiFi    Projector   Password / Slow
   ↓        ↓              ↓
Network  Hardware   Access / Performance
   ↓        ↓              ↓
 Wi-Fi   Projector  Appropriate Subcategory
            ↓
       Update Record
            ↓
       Send Email
            ↓
      Ticket Classified
```

# Expected Benefits

- Reduces manual ticket categorization.
- Improves consistency in ticket classification.
- Automatically assigns Category and Subcategory.
- Improves ticket routing efficiency.
- Sends automatic confirmation to callers.
- Uses no-code Flow Designer automation.
- Makes the solution easier to maintain and extend.

# Conclusion

The **Auto Ticket Classification using Flow Designer** project provides an end-to-end automation solution for a school IT helpdesk. It automatically classifies IT tickets based on keywords in the Short Description, uses dependent Category and Subcategory fields to maintain data accuracy, and sends automated email notifications to callers.

The project follows a phased approach covering requirements, backend configuration, Flow Designer automation, testing, validation, deployment, and documentation. It demonstrates how ServiceNow can build scalable and maintainable automation without complex scripting or machine learning. fileciteturn1file0L260-L278

