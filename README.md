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
- **Meeting Agenda**: Include agendas with bookings
- **Timezone Aware**: Defaults to Pacific Time, supports custom timezones
- **Changing User Preference Aware**: Adapts to user changing the scheduled day/time

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

🚀 Quick Start
📖 **[Detailed setup instructions →](setup_guide.md)**

### 1. Configure Cal.com Event Types
Create four event types in your Cal.com dashboard.

### 2. Get Your Event Type IDs
Find them in your Cal.com dashboard or via API.

### 3. Configure Environment
Update `agent-config.json` with your IDs.

### 4. Deploy to Agentman
Upload `agent-config.json` to your Agentman dashboard and activate the agent.

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

### Feature Limitations
- **Cancellation**: Requires manual cancellation via Cal.com dashboard. (agent enhancements coming soon)
- **Rescheduling**: Not yet implemented
- **Recurring Meetings**: Not supported
- **Team Events**: Single-user events only

### Configuration Issues (Won't Fix)
These are cosmetic/documentation issues that don't affect functionality:

- **Tool Documentation Mismatch**: Plan documentation references `CheckCalendarAvailability` and `SendCalendarInvite`, but actual tools are `CheckCalAvailability` and `CreateBooking`. Task flows use correct names.
- **Task Name/ID Inconsistency**: Task names (e.g., "CollectPreferences") don't match their IDs (e.g., "Tf2A6cGFVWZg1I1yqd_-m"). Only affects code readability.
- **Outdated Plan Metadata**: `plan_prompt` mentions 25-min meetings and 8am-4pm restrictions that don't exist in actual implementation. Legacy documentation only.
- **Invalid Task Flow References**: Fallback tasks (`Confirm_Meeting_Details`, `Collect_Meeting_Preferences`) don't exist. Primary flow works correctly.
- **JSON Formatting**: Task steps contain excessive quote escaping and `\r` characters. Doesn't affect parsing or execution.

## 🔮 Future Enhancements

- [ ] Cancellation support
- [ ] Rescheduling workflow  
- [ ] Multi-timezone support for team scheduling
- [ ] Meeting type recommendations based on past bookings

## 📚 Documentation

- [Setup Guide](./docs/setup-guide.md)
- [Architecture Deep Dive](./docs/architecture.md) (coming soon)
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
