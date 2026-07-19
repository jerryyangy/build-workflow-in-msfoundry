# Build a Workflow in Microsoft Foundry

This project demonstrates how to create a customer support triage workflow in Microsoft Foundry and invoke it from a Python client application using the Azure AI Projects SDK.

## Overview

In this exercise, we will build a sequential workflow for a fictional company called ContosoPay. The workflow processes support tickets one by one, classifies each ticket with an AI agent, handles low-confidence results, and routes billing issues for escalation. Non-billing tickets are passed to a resolution agent that drafts a response. [page:1]

## What We Will Build

The workflow includes:

- A predefined array of customer support tickets.
- A `for-each` loop to process tickets individually.
- A **Triage Agent** that classifies each ticket as Billing, Technical, or General.
- Conditional logic to handle low-confidence classifications.
- Conditional routing for billing vs. non-billing tickets.
- A **Resolution Agent** to draft responses for technical and general issues. [page:1]

## Prerequisites

Before starting, make sure you have:

- Visual Studio Code installed.
- An active Azure subscription.
- Python 3.13 or later installed.
- Git installed locally. [page:1]

## Lab Steps

### 1. Create a Foundry Project

1. Open the Foundry portal at `https://ai.azure.com`.
2. Turn on the **New Foundry** toggle.
3. Create a new project if prompted.
4. Name the project and wait for it to finish provisioning. [page:1]

### 2. Create the Workflow

1. In Foundry, go to **Build** > **Agents** > **Workflows**.
2. Create a **Blank workflow**.
3. Save the workflow with a name such as `ContosoPay-Customer-Support-Triage`. [page:1]

### 3. Add Sample Tickets

1. Add a **Set variable** node.
2. Create a variable named `SupportTickets`.
3. Populate it with sample ticket text strings. [page:1]

### 4. Loop Through Tickets

1. Add a **For each** node.
2. Set the loop source to `Local.SupportTickets`.
3. Create a loop variable named `CurrentTicket`. [page:1]

### 5. Classify with a Triage Agent

1. Add an **Invoke Agent** node.
2. Create a new agent named `Triage-Agent`.
3. Configure JSON schema output with these fields:
   - `customer_issue`
   - `category`
   - `confidence`
4. Set instructions so the agent classifies tickets into Billing, Technical, or General. [page:1]

### 6. Handle Low Confidence

1. Add an **If/Else** node.
2. Check whether `Local.TriageOutputJson.confidence > 0.6`.
3. If confidence is low, send a message asking for more details. [page:1]

### 7. Route Billing Issues

1. Add another **If/Else** node.
2. Check whether `Local.TriageOutputJson.category = "Billing"`.
3. If true, send a message to escalate the issue to human support. [page:1]

### 8. Draft Responses for Other Tickets

1. For non-billing tickets, create another agent named `Resolution-Agent`.
2. Configure it to generate a concise support response.
3. Pass `Local.TriageOutputText` as the input.
4. Save the output into `ResolutionOutputText`. [page:1]

### 9. Preview the Workflow

1. Save the workflow.
2. Start **Preview**.
3. Enter a trigger message such as `Start processing support tickets.`
4. Confirm that billing tickets are escalated and other tickets receive drafted responses. [page:1]

### 10. Invoke from Code

1. Clone the starter repository:
   '(https://github.com/jerryyangy/build-workflow-in-msfoundry.git)`
2. Open the lab folder for exercise 06.
3. Copy the project endpoint from the Foundry code view.
4. Install Python dependencies.
5. Update `.env` with your endpoint.
6. Modify `workflow.py` to connect to Azure AI Projects and invoke the workflow.
7. Run `az login` and then `python workflow.py`. [page:1]

## Example Output

A typical output shows:

- The current ticket being processed.
- The JSON classification result from the triage agent.
- A drafted response for technical or general issues.
- Escalation text for billing-related issues. [page:1]


- The workflow builder in Microsoft Foundry is currently in preview.
- You may see warnings or unexpected behavior during the lab.
- If the lab gets stuck, recreating the project or workflow may help. [page:1]
