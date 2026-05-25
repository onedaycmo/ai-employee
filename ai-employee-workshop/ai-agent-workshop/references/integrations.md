# Integrations Reference

Read this file whenever a tool connection is needed and Claude does not have a native integration
for it. Give the attendee exact steps. Do not tell them to figure it out or look it up.

---

## Native Claude Integrations (No Setup Required Beyond Authorization)

These tools connect directly inside Claude Settings > Integrations. Walk the attendee through
authorization if they have not done it yet.

- Google Drive
- Gmail
- Slack
- GitHub
- Jira (via Atlassian)

For each, the steps are:
1. Go to claude.ai and click the profile icon in the top right.
2. Click Settings, then Integrations.
3. Find the tool and click Connect.
4. Sign in and authorize access.
5. Confirm back here when it is done.

---

## Tools Without Native Integration: Use Zapier or Make

For any tool that does not appear in Claude's native integrations, the connection runs through
Zapier or Make as the bridge. These are no-code tools. Treat the attendee as non-technical and
give exact steps.

### When to use Zapier vs Make

Use Zapier if the attendee has never used either. It is simpler and has more pre-built templates.
Use Make if they already have a Make account or if their workflow is complex with multiple
branches.

---

## Setting Up a Zapier Connection to Claude

Use this when their tool is not natively supported.

### What this does

When something happens in their tool (a new form submission, a CRM update, a new row in a
spreadsheet), Zapier sends that information to Claude, Claude processes it and responds, and
Zapier routes the response back to wherever it needs to go.

### Steps

1. "Go to zapier.com and create a free account if you do not have one."

2. "Click Create Zap."

3. "For the Trigger, search for [their tool name]. Select the event that should start the
   workflow. For example: New Form Submission, New Row in Spreadsheet, New Lead in CRM."

4. "Connect your [tool name] account when prompted and select the specific form, sheet, or
   pipeline you want to monitor."

5. "Click Continue and test the trigger to confirm Zapier can read data from your tool."

6. "For the Action, search for Webhooks by Zapier. Select POST."

7. "In the URL field, enter: https://api.anthropic.com/v1/messages"

8. "Set Payload Type to JSON."

9. "In the Data section, enter this exactly, replacing the prompt text with what you want
   Claude to do with the incoming information:"

```json
{
  "model": "claude-opus-4-5",
  "max_tokens": 1024,
  "messages": [
    {
      "role": "user",
      "content": "Here is the incoming information: {{your trigger data field}}. [Describe what Claude should do with it and what format the output should be in.]"
    }
  ]
}
```

10. "In the Headers section, add:"
    - Key: x-api-key | Value: [their Anthropic API key]
    - Key: anthropic-version | Value: 2023-06-01
    - Key: content-type | Value: application/json

11. "To get an Anthropic API key: go to console.anthropic.com, sign in or create an account,
    click API Keys, and create a new key. Copy it and paste it into the Zapier header."

12. "Test the action. You should see a response from Claude in the test output."

13. "Add a final action to send Claude's response to wherever it needs to go: a Slack message,
    a Gmail draft, a CRM note, a new spreadsheet row, or wherever makes sense."

14. "Name the Zap clearly, for example: New Client Inquiry to Claude to Gmail Draft. Turn it on."

---

## Setting Up a Make Connection to Claude

Use this for attendees who already use Make or need more complex branching logic.

### Steps

1. "Go to make.com and sign in or create an account."

2. "Click Create a new scenario."

3. "Add a module for [their tool]. Select the trigger event."

4. "Add an HTTP module. Select Make a request."

5. "Configure the HTTP module:"
   - URL: https://api.anthropic.com/v1/messages
   - Method: POST
   - Headers: Add x-api-key with their Anthropic API key, anthropic-version as 2023-06-01,
     content-type as application/json
   - Body type: Raw
   - Content type: JSON

6. "In the body, enter:"

```json
{
  "model": "claude-opus-4-5",
  "max_tokens": 1024,
  "messages": [
    {
      "role": "user",
      "content": "{{your trigger data}}. [What Claude should do and what format to respond in.]"
    }
  ]
}
```

7. "Map the response to the next module in their workflow."

8. "Run the scenario once manually to test. Confirm Claude's response appears correctly."

9. "Turn the scenario on and set the schedule for how often it checks for new triggers."

---

## Common Tool Connections

### CRM (HubSpot, Salesforce, Pipedrive, GoHighLevel)

Use Zapier or Make with the trigger set to New Contact, New Lead, or Deal Stage Changed.
Send the contact or deal data to Claude with instructions to draft a follow-up, classify the
lead, or generate next steps. Route the output back to the CRM as a note or task.

### Typeform or Google Forms

Use Zapier trigger: New Submission. Send the form responses to Claude. Claude processes the
intake, drafts a response or summary, and routes it to Gmail, Slack, or the CRM.

### Notion

Notion has limited Zapier support for triggers. Use Make for Notion workflows. Trigger on
New Database Item. Send the item to Claude and route the response back as a new property
or comment on the item.

### Airtable

Use Zapier trigger: New Record or Record Updated. Send the record fields to Claude. Route
Claude's output back as a new field in the same record or as a message to Slack or Gmail.

### Calendly or Acuity

Trigger on Invitee Created (new booking). Send the booking details to Claude. Claude drafts
a personalized confirmation or prep email. Route to Gmail as a draft or send directly
depending on their approval preference.

---

## When Setup Fails

If the attendee gets stuck on a step, do not tell them to refer to documentation. Ask them
to describe exactly what they see on screen and walk them through it from that point. If an
error appears, ask them to read the error message out loud and diagnose from there.

If a tool has no Zapier or Make connector at all, ask: "Is there another tool in your workflow
that does connect to Zapier that feeds into or out of this one?" Find the nearest connected
point and bridge from there.
