# 🚀 Setup Guide

## Prerequisites

Before you begin, ensure you have:

- [ ] Gmail/Google Workspace account
- [ ] Airtable account (free tier)
- [ ] Zapier account (free tier)
- [ ] Claude API key (Anthropic)
- [ ] Access to your ATS system

## Step-by-Step Setup

### Part 1: Airtable Database Setup (30 minutes)

#### 1.1 Create Base
1. Log into [Airtable](https://airtable.com)
2. Click "Add a base" → "Start from scratch"
3. Name it "Staffing Analytics Hub"

#### 1.2 Create Tables

**Table 1: Candidates**

Click "Add table" → Name: "Candidates"

| Field Name | Field Type | Description |
|------------|------------|-------------|
| Candidate ID | Single line text | Unique identifier |
| Full Name | Single line text | Primary field |
| Email | Email | Contact email |
| Phone | Phone | Contact number |
| LinkedIn URL | URL | LinkedIn profile |
| Status | Single select | Active, Placed, Rejected, etc. |
| Current Stage | Single select | Application, Screen, Interview, etc. |
| Source | Single select | LinkedIn, Referral, Job Board, etc. |
| Assigned Recruiter | Link to Recruiters table | Who's managing |
| Job Applied | Link to Jobs table | Position interested in |
| Application Date | Date | When they applied |
| Last Contact | Date | Last interaction |
| Match Score | Number | AI-generated score 0-100 |
| Skills | Long text | Comma-separated |
| Experience Years | Number | Years of experience |
| Current Salary | Currency | Current compensation |
| Expected Salary | Currency | Salary expectations |
| Resume | Attachment | Resume file |
| Notes | Long text | Recruiter notes |

**Add Views:**
- All Candidates (default)
- Active Pipeline (Status = Active)
- By Stage (Grouped by Current Stage)
- High Match (Match Score > 80)
- This Week (Application Date within 7 days)

**Table 2: Job Requisitions**

Similar structure for jobs...

[Continue with detailed instructions for each table]

### Part 2: Zapier Automation Setup (45 minutes)

#### 2.1 Connect Your ATS

1. Go to [Zapier](https://zapier.com)
2. Click "Create Zap"
3. **Trigger:** Search for your ATS
   - Greenhouse: "New Candidate"
   - Lever: "Candidate Stage Changed"
   - BambooHR: "New Applicant"
4. **Connect** your ATS account
5. **Test** trigger

#### 2.2 Create Airtable Action

1. **Action:** Search "Airtable"
2. **Choose:** "Create Record"
3. **Connect** Airtable account
4. **Select** Base: "Staffing Analytics Hub"
5. **Select** Table: "Candidates"
6. **Map fields:**
   - Full Name → Candidate Name (from ATS)
   - Email → Email (from ATS)
   - Status → Set to "Active"
   - Application Date → Today's Date
   [etc...]

7. **Test** the action
8. **Turn on** Zap

[Continue with each workflow...]

### Part 3: Claude API Integration (1 hour)

#### 3.1 Get API Key

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Create account
3. Navigate to "API Keys"
4. Click "Create Key"
5. **Copy** and save securely
6. **Note:** Free tier includes $5 credit

#### 3.2 Set Up Make.com Scenario

1. Log into [Make.com](https://make.com)
2. Create new scenario
3. **Add Modules:**
   
   **Module 1: Airtable Watch**
   - Trigger: Watch Records
   - Table: Candidates
   - Filter: Resume is not empty

   **Module 2: Download Resume**
   - Action: Download attachment
   - File: Resume field

   **Module 3: Extract Text**
   - Tool: CloudConvert or PDF.co
   - Convert PDF to Text

   **Module 4: HTTP Request (Claude API)**
```json
   URL: https://api.anthropic.com/v1/messages
   Method: POST
   Headers:
     x-api-key: YOUR_CLAUDE_API_KEY
     anthropic-version: 2023-06-01
     content-type: application/json
   
   Body:
   {
     "model": "claude-sonnet-4-5-20250929",
     "max_tokens": 1024,
     "messages": [{
       "role": "user",
       "content": "Analyze this resume and extract: Name, Email, Phone, Skills (comma-separated), Total Experience (years), Education, Previous Companies. Resume text: {{text}}"
     }]
   }
```

   **Module 5: Parse JSON**
   - Parse Claude's response

   **Module 6: Update Airtable**
   - Update the candidate record with parsed data

4. **Test** scenario
5. **Schedule** to run every hour

[Continue with detailed setup...]

### Part 4: Dashboard Creation (1 hour)

[Detailed Looker Studio setup...]

### Part 5: Testing & Validation (30 minutes)

[Testing checklist...]

## Troubleshooting

### Common Issues

**Issue 1: Zapier not triggering**
- Check if ATS webhook is enabled
- Verify API permissions
- Test with manual trigger

**Issue 2: Claude API errors**
- Check API key is correct
- Verify you have remaining credits
- Check request format

[Continue with solutions...]

## Next Steps

After setup:
1. Import historical data
2. Train your team
3. Monitor for 1 week
4. Gather feedback
5. Optimize workflows

## Support

Questions? Open an issue on [GitHub](link to your repo)
