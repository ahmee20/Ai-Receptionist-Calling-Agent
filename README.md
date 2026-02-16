# AI Receptionist Calling Agent for Appointment Scheduling (n8n)

This workflow is designed to automate appointment scheduling for a business using an AI voice calling agent integrated through VAPI. It handles booking creation, updates, cancellations, availability checks, and service information retrieval by connecting Google Calendar, Google Sheets, and Gmail within n8n.

## Workflow Overview

### Webhook Trigger

The workflow is triggered through a webhook endpoint which receives tool calls from the AI assistant during an ongoing phone call. Based on the function name received in the request, the workflow routes the execution to the relevant branch for:

- Booking creation
- Updating an existing appointment
- Cancelling an appointment
- Checking calendar availability
- Retrieving available services and pricing

### Appointment Booking

When a caller requests to book an appointment, the workflow:

1. Extracts customer details:
   - Name
   - Email
   - Phone number
   - Selected service
   - Preferred date and time
   - Cost

2. Creates an event in Google Calendar to reserve the requested time slot
3. Appends booking information to a Google Sheets document to maintain a structured booking database
4. Automatically sends a confirmation email to the customer

### Availability Checks

For availability queries, the workflow:

1. Queries Google Calendar within the specified start and end time range
2. Returns unavailable status if an existing event is found within that slot
3. Marks the slot as available if no event is found, allowing the booking process to proceed

### Appointment Updates

When updating an appointment during a call, the workflow:

1. Retrieves the previously booked appointment from Google Sheets using the customer's phone number and previous booking date
2. Deletes the existing calendar event
3. Creates a new event based on the updated time or service details
4. Updates the corresponding booking record in Google Sheets
5. Sends an updated confirmation email to the customer

### Appointment Cancellation

If a caller requests to cancel an appointment, the workflow:

1. Searches for the booking using the phone number and appointment date
2. Deletes the associated Google Calendar event
3. Removes the corresponding entry in Google Sheets
4. Sends a cancellation confirmation email to the customer

### Service Information Retrieval

The workflow supports retrieval of:

- Available services
- Pricing
- Upsell offers
- Working days
- Working hours

This information is processed using a JavaScript code node and returned as a structured response to the AI calling agent to assist in answering caller queries during conversations.

## Key Integrations

- **VAPI**: AI voice calling agent interface
- **Google Calendar**: Schedule and availability management
- **Google Sheets**: Customer booking database and service information storage
- **Gmail**: Automated confirmation and cancellation email notifications
- **n8n**: Workflow orchestration and logic routing

## Requirements

- n8n Instance (Cloud or Self-hosted)
- VAPI Account and API Key
- Google Calendar API credentials
- Google Sheets API credentials
- Gmail API credentials or authenticated email service
- Phone-accessible webhook endpoint

## How to Use

1. Import the workflow JSON into n8n
2. Configure API credentials:
   - VAPI API Key
   - Google Calendar API
   - Google Sheets API
   - Gmail API
3. Set up Google Sheets with:
   - Booking database sheet
   - Services and pricing information sheet
4. Activate the Webhook node
5. Integrate webhook URL with VAPI assistant tools

### Webhook Request Format

```json
{
  "toolName": "create_appointment|update_appointment|cancel_appointment|check_availability|get_services",
  "data": {
    "customerName": "string",
    "email": "string",
    "phoneNumber": "string",
    "service": "string",
    "preferredDateTime": "string (ISO 8601)",
    "cost": "number"
  }
}
```

## Output

The workflow returns structured responses to the AI calling agent:

- **Booking Creation**: Confirmation with appointment details and reference number
- **Availability Check**: Available/unavailable status with alternative time slots
- **Appointment Update**: Confirmation of changes and updated appointment details
- **Cancellation**: Confirmation of successful cancellation
- **Service Info**: List of services, pricing, and business hours

## Use Cases

- Fully Automated Appointment Scheduling
- Customer Service Automation
- Booking Confirmations and Reminders
- Availability Management
- Multi-Service Business Scheduling
- Lead Qualification and Service Inquiries
