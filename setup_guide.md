## 📅 Cal.com Event Type Setup

Before using the agent, you need to create event types for each meeting duration in your Cal.com account.

### 1. Create Event Types

Go to https://app.cal.com/event-types and create **four event types**:

#### 15-Minute Meeting
1. Click **+ New Event Type**
2. Set **Title**: "15 Min Meeting"
3. Set **URL/Slug**: `15min`
4. Set **Duration**: 15 minutes
5. Configure your availability and location (e.g., Google Meet, Zoom)
6. Click **Save**
7. Note the Event Type ID from the URL: `https://app.cal.com/event-types/[ID]`

#### 30-Minute Meeting
1. Click **+ New Event Type**
2. Set **Title**: "30 Min Meeting"
3. Set **URL/Slug**: `30min`
4. Set **Duration**: 30 minutes
5. Configure availability and location
6. Click **Save**
7. Note the Event Type ID

#### 45-Minute Meeting
1. Click **+ New Event Type**
2. Set **Title**: "45 Min Meeting"
3. Set **URL/Slug**: `45-min-meeting`
4. Set **Duration**: 45 minutes
5. Configure availability and location
6. Click **Save**
7. Note the Event Type ID

#### 60-Minute Meeting
1. Click **+ New Event Type**
2. Set **Title**: "60 Min Meeting"
3. Set **URL/Slug**: `60-min-meeting`
4. Set **Duration**: 60 minutes
5. Configure availability and location
6. Click **Save**
7. Note the Event Type ID

### 2. Important: Event Type Slugs

The agent expects these **exact slugs**:
- ✅ `15min`
- ✅ `30min`
- ✅ `45-min-meeting`
- ✅ `60-min-meeting`

If you use different slugs, update the `CheckAvailability` task in the agent configuration.

### 3. Recommended Settings

For each event type:
- **Location**: Google Meet or Zoom (automatic video links)
- **Booking window**: At least 2 hours notice
- **Buffer time**: 0-5 minutes between meetings
- **Requires confirmation**: Disabled (for instant booking)

### 4. Get Your Event Type IDs

After creating all event types, collect their IDs:

**Option A: From Dashboard**
1. Click on each event type
2. Copy the ID from the URL: `https://app.cal.com/event-types/[ID]`

**Option B: Via API**
```bash
curl "https://api.cal.com/v2/event-types" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "cal-api-version: 2024-08-13"
```

Save these IDs - you'll need them for the environment variables.

### 5. Configure Environment Variables

Update your configuration with the IDs:
```json
"environment": {
  "cal_access_token": "YOUR_CAL_API_KEY",
  "cal_username": "your-username",
  "event_type_id_15min": "event_type_id",
  "event_type_id_30min": "event_type_id",
  "event_type_id_45min": "event_type_id",
  "event_type_id_60min": "event_type_id"
}
```

### Troubleshooting

**Agent can't find availability?**
- Verify event type slugs match exactly (`15min`, `30min`, etc.)
- Check that event types are active and published
- Ensure you have availability set in your Cal.com schedule

**Wrong meeting duration booked?**
- Double-check Event Type IDs in environment variables
- Verify IDs match the correct durations
