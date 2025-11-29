# cal-scheduling-agent
AI scheduling agent for Cal.com. Natural language booking, guest management, and availability checking with LangGraph + GPT-4 on Agentman platform.

## 🎯 Overview

This agent enables users to:
- Check calendar availability across multiple durations (15, 30, 45, 60 minutes)
- Book meetings with automatic calendar invitations
- Add multiple guests and meeting agendas
- Handle timezone conversions automatically

## ✨ Features

- **Natural Language Processing**: Understands scheduling requests in plain English
- **Multi-Duration Support**: Handles 15, 30, 45, and 60-minute meetings
- **Guest Management**: Add multiple attendees to bookings
- **Meeting Notes**: Include agendas and notes with bookings
- **Timezone Aware**: Defaults to Pacific Time, supports custom timezones

## 🛠️ Tech Stack

- **Framework**: LangGraph (state management and agent orchestration)
- **LLM**: GPT-4.1
- **API Integration**: Cal.com API v2
- **Authentication**: Cal.com API Key (Bearer token)

## 📋 Prerequisites

- Cal.com account with API access
- Cal.com API key ([Get one here](https://cal.com/settings/developer/api-keys))
- Event types configured for each duration (15min, 30min, 45min, 60min)
- LangGraph setup (or compatible agent framework)

## 🚀 Quick Start

### 1. Get Your Cal.com Event Type IDs
### 1. Get Your Cal.com Event Type IDs

You can find your event type IDs in the Cal.com dashboard URL when viewing an event type:
```
https://app.cal.com/event-types/[YOUR_EVENT_TYPE_ID]
```

Or via the v2 API:
```bash
curl -X GET "https://api.cal.com/v2/event-types" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "cal-api-version: 2024-08-13"
```

The response will include all your event types with their IDs and slugs:
```json
{
  "status": "success",
  "data": [
    {
      "id": 1405384,
      "slug": "15min",
      "title": "15 Min Meeting",
      "lengthInMinutes": 15
    },
    {
      "id": 1405383,
      "slug": "30min",
      "title": "30 Min Meeting",
      "lengthInMinutes": 30
    }
  ]
}
```

Note down the IDs for each duration.

### 2. Configure Environment Variables
```bash
export CAL_ACCESS_TOKEN="your_api_key"
export EVENT_TYPE_ID_15MIN="your_15min_id"
export EVENT_TYPE_ID_30MIN="your_30min_id"
export EVENT_TYPE_ID_45MIN="your_45min_id"
export EVENT_TYPE_ID_60MIN="your_60min_id"
```

### 3. Install & Run
```bash
# Clone the repository
git clone https://github.com/yourusername/cal-scheduling-agent
cd cal-scheduling-agent

# Install dependencies
pip install -r requirements.txt

# Run the agent
python agent.py
```

## 💬 Example Usage

### Basic Booking
```
User: Can you find me a 30 minute meeting slot for December 1st?
Agent: Here are available 30-minute slots for December 1st:
        • 9:00 AM
        • 10:00 AM
        • 11:00 AM
        Which time works for you?
User: 11am works
Agent: Great! What's your name and email?
User: John Doe, john@example.com
Agent: Your meeting is confirmed for December 1st at 11:00 AM!
```

### With Guests and Agenda
```
User: Book a 60 minute meeting for next Monday at 2pm
Agent: What's your name and email?
User: Jane Smith, jane@example.com. Add john@example.com as guest.
      Agenda: Q4 Planning Review
Agent: Meeting booked! Calendar invites sent to both attendees.
```

## 🏗️ Architecture

### Agent Flow
```
CollectPreferences → CheckAvailability → PresentAvailability 
→ CollectAttendeeInfo → CreateBooking
```

### State Graph (LangGraph)
The agent uses a state machine to track:
- `meeting_duration`: Selected duration (15, 30, 45, or 60 minutes)
- `preferred_date_range`: Requested date(s)
- `attendee_timezone`: User's timezone
- `selected_slot`: Chosen time slot
- `attendee_info`: Name, email, guests, notes

### API Integration

**CheckAvailability**
```
GET /v2/slots
Params: start, end, eventTypeSlug, username
Returns: Available time slots
```

**CreateBooking**
```
POST /v2/bookings
Body: start, attendee, eventTypeId, guests, bookingFieldsResponses
Returns: Booking confirmation with meeting link
```

## 🔧 Configuration

The agent uses a task-based architecture with these components:

1. **CollectPreferences**: Extract date and duration from user input
2. **CheckAvailability**: Query Cal.com API for available slots
3. **PresentAvailability**: Display options to user
4. **CollectAttendeeInfo**: Gather name, email, guests, agenda
5. **CreateBooking**: Create meeting and send invites

[See full configuration](./agent-config.json)

## 🐛 Known Limitations

- **Cancellation**: Requires manual cancellation via Cal.com dashboard. (agent enhancements coming soon)
- **Rescheduling**: Not yet implemented
- **Recurring Meetings**: Not supported
- **Team Events**: Single-user events only

## 🔮 Future Enhancements

- [ ] Cancellation support
- [ ] Rescheduling workflow  
- [ ] Multi-timezone support for team scheduling
- [ ] Meeting type recommendations based on past bookings
- [ ] Slack/Teams integration

## 📚 Documentation

- [Setup Guide](./docs/setup-guide.md)
- [API Reference](./docs/api-reference.md)
- [Architecture Deep Dive](./docs/architecture.md)
- [Development Notes](./docs/development-process.md)

## 🤝 Contributing

Contributions welcome! This is a sample implementation - feel free to fork and adapt for your use case.

## 📝 License

MIT License

## 👤 Author

Built by Gayatri Seetharaman

## 🙏 Acknowledgments

- Cal.com for excellent API documentation
- Agentman platform for agent development tools
- Claude (Anthropic) for development assistance
