# RationMitra 🇮🇳

**AI-powered WhatsApp assistant helping Indian citizens discover and claim government benefits they're entitled to**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com/)

## 🎯 Problem Statement

- **60% of eligible Indian citizens** don't know about government schemes they qualify for
- Complex eligibility rules across **100+ schemes**
- Language barriers and low digital literacy prevent access
- Citizens lose **thousands of rupees annually** in unclaimed benefits
- No personalized guidance on HOW to actually claim benefits

## 💡 Solution

RationMitra is a conversational AI assistant accessible via WhatsApp that:

1. **Discovers** - Identifies government benefits users are eligible for but not receiving
2. **Guides** - Provides step-by-step claiming instructions in user's language
3. **Tracks** - Sends intelligent reminders and helps with application follow-ups
4. **Empowers** - Makes government schemes accessible to rural, elderly, and low-literacy populations

## 🎥 Demo

> *Coming soon - Video walkthrough of user journey*

## ✨ Key Features

### 1. Conversational Profile Builder
- WhatsApp-based interaction (text + voice notes)
- 5-7 simple questions to build user profile
- Hindi & English support (expandable to 10+ languages)
- Completes in under 5 minutes

### 2. Intelligent Eligibility Matching
- Matches against 20+ major government schemes
- Calculates total financial impact: "You're missing ₹X per year"
- Explains WHY user qualifies with official criteria
- Priority ranking by value and ease of claiming

### 3. Step-by-Step Claiming Guides
- Difficulty ratings (Easy ⭐⭐ / Medium ⭐⭐⭐ / Hard ⭐⭐⭐⭐)
- Complete document checklists
- Office locations with Google Maps links
- Conversation scripts in local language
- Troubleshooting for common problems

### 4. Application Tracking & Reminders
- User-reported progress tracking
- Intelligent reminder system (Day 15, 30, 45)
- Escalation suggestions (helpline → complaint → RTI)
- Status checking instructions
- Celebration messages on success 🎉

### 5. Document Assistance
- Clear explanations of required documents
- Guidance on obtaining missing documents
- Photo upload for secure storage
- Document expiry tracking

### 6. Multilingual Support
- Natural language understanding
- Voice note support (speech-to-text)
- Adaptive communication based on literacy level
- Cultural context awareness

## 🏗️ Architecture

```
┌─────────────────┐
│  WhatsApp User  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Twilio API     │ (WhatsApp Business)
└────────┬────────┘
         │
         ▼
┌─────────────────────────────────────┐
│         FastAPI Backend             │
│  ┌──────────────────────────────┐  │
│  │  Webhook Handler             │  │
│  │  - Message routing           │  │
│  │  - Session management        │  │
│  └──────────────────────────────┘  │
│  ┌──────────────────────────────┐  │
│  │  AI Engine (Gemini/Groq)    │  │
│  │  - NLU & Intent extraction   │  │
│  │  - Eligibility reasoning     │  │
│  │  - Response generation       │  │
│  └──────────────────────────────┘  │
│  ┌──────────────────────────────┐  │
│  │  Business Logic              │  │
│  │  - Profile builder           │  │
│  │  - Scheme matcher            │  │
│  │  - Guide generator           │  │
│  └──────────────────────────────┘  │
│  ┌──────────────────────────────┐  │
│  │  Reminder System (Celery)   │  │
│  │  - Scheduled reminders       │  │
│  │  - Escalation logic          │  │
│  └──────────────────────────────┘  │
└────────┬────────────────────────────┘
         │
         ▼
┌─────────────────┐
│  PostgreSQL     │ (Supabase)
│  - Users        │
│  - Profiles     │
│  - Schemes      │
│  - Applications │
│  - Reminders    │
└─────────────────┘
```

## 🛠️ Tech Stack

### Backend
- **Framework**: FastAPI (Python 3.9+)
- **Database**: PostgreSQL (Supabase free tier)
- **Task Queue**: Celery + Redis
- **AI/ML**: Google Gemini API / Groq API
- **Speech**: OpenAI Whisper (local), gTTS

### Integrations
- **Messaging**: Twilio WhatsApp Business API
- **Maps**: Google Maps API (optional)
- **Hosting**: Render / Railway (free tier)

### DevOps
- **Containerization**: Docker + Docker Compose
- **CI/CD**: GitHub Actions
- **Monitoring**: Sentry (free tier)
- **Version Control**: Git + GitHub

## 🚀 Quick Start

### Prerequisites
- Python 3.9 or higher
- Docker & Docker Compose (optional but recommended)
- Twilio account (free sandbox for testing)
- Supabase account (free tier)
- Gemini API key or Groq API key (free)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/rationmitra.git
cd rationmitra
```

2. **Set up environment variables**
```bash
cp .env.example .env
# Edit .env with your API keys
```

Required environment variables:
```env
# Database
DATABASE_URL=postgresql://user:password@host:5432/rationmitra

# AI
GEMINI_API_KEY=your_gemini_api_key
# OR
GROQ_API_KEY=your_groq_api_key

# Twilio WhatsApp
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_WHATSAPP_NUMBER=whatsapp:+14155238886

# Redis
REDIS_URL=redis://localhost:6379/0

# App
SECRET_KEY=your_secret_key
ENVIRONMENT=development
```

3. **Run with Docker (Recommended)**
```bash
docker-compose up -d
```

4. **Or run locally**
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run database migrations
alembic upgrade head

# Start Redis (in separate terminal)
redis-server

# Start Celery worker (in separate terminal)
celery -A app.tasks worker --loglevel=info

# Start FastAPI server
uvicorn app.main:app --reload
```

5. **Access the application**
- API: http://localhost:8000
- API Docs: http://localhost:8000/docs
- Health Check: http://localhost:8000/health

### Testing with Twilio Sandbox

1. Join Twilio WhatsApp Sandbox:
   - Send "join <your-sandbox-code>" to +1 415 523 8886 on WhatsApp

2. Configure webhook in Twilio Console:
   - Set webhook URL to: `https://your-domain.com/webhook/whatsapp`
   - Use ngrok for local testing: `ngrok http 8000`

3. Start chatting:
   - Send "Hi" to the Twilio number to begin

## 📊 Database Schema

```sql
-- Users table
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    phone_number VARCHAR(20) ENCRYPTED,
    preferred_language VARCHAR(10),
    created_at TIMESTAMP,
    last_active TIMESTAMP
);

-- Profiles table
CREATE TABLE profiles (
    profile_id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(user_id),
    age INTEGER,
    occupation VARCHAR(50),
    state VARCHAR(50),
    district VARCHAR(50),
    land_ownership DECIMAL,
    bpl_card_status BOOLEAN,
    current_benefits JSONB,
    profile_completed_at TIMESTAMP
);

-- Schemes table
CREATE TABLE schemes (
    scheme_id UUID PRIMARY KEY,
    scheme_name VARCHAR(200),
    common_name VARCHAR(200),
    benefit_amount DECIMAL,
    benefit_frequency VARCHAR(20),
    eligibility_rules JSONB,
    required_documents JSONB,
    application_steps JSONB,
    state_specific_data JSONB,
    office_locations JSONB,
    helpline_numbers JSONB,
    portal_urls JSONB
);

-- Applications table
CREATE TABLE applications (
    application_id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(user_id),
    scheme_id UUID REFERENCES schemes(scheme_id),
    submission_date DATE,
    receipt_number VARCHAR(100),
    current_status VARCHAR(20),
    last_checked_date DATE,
    status_history JSONB,
    next_reminder_date DATE
);
```

## 🎯 Target Users

### Primary
- Rural citizens (farmers, elderly, women, daily wage workers)
- BPL families
- Low digital literacy populations

### Secondary
- Urban migrants
- First-generation benefit seekers

### Characteristics
- Prefer voice over text
- Speak regional languages
- Limited smartphone experience
- Trust WhatsApp as communication platform

## 📈 Success Metrics

- **Profile completion rate**: >90% complete in <5 minutes
- **Benefit discovery**: 2+ missing benefits for 60%+ users
- **Claiming engagement**: 70%+ save/share guides
- **Reminder response**: 60%+ respond to reminders
- **User retention**: 50%+ return for progress tracking

## 🗺️ Roadmap

### Phase 1: MVP (Current)
- [x] Conversational profile builder
- [x] 20 major schemes database
- [x] Eligibility matching engine
- [x] Step-by-step guides
- [x] Hindi & English support
- [ ] WhatsApp integration
- [ ] Reminder system

### Phase 2: Enhancement
- [ ] Voice note support
- [ ] Document photo upload
- [ ] 10+ Indian languages
- [ ] 50+ schemes coverage
- [ ] SMS fallback for feature phones

### Phase 3: Scale
- [ ] All central + state schemes (500+)
- [ ] 22 scheduled languages
- [ ] NGO partnership program
- [ ] Government integration
- [ ] Mobile app (optional)

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

### Development Setup
```bash
# Install dev dependencies
pip install -r requirements-dev.txt

# Run tests
pytest

# Run linting
flake8 app/
black app/

# Run type checking
mypy app/
```

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built for **AI for Bharat Hackathon** - Track 3: AI for Communities, Access & Public Impact
- Scheme data sourced from [MyScheme.gov.in](https://www.myscheme.gov.in/)
- Inspired by the need to bridge the information gap in government welfare delivery

## 📞 Contact

- **Project Lead**: [Your Name]
- **Email**: [your.email@example.com]
- **Twitter**: [@rationmitra](https://twitter.com/rationmitra)
- **Website**: [https://rationmitra.org](https://rationmitra.org)

## 🌟 Support

If you find this project useful, please consider:
- ⭐ Starring the repository
- 🐛 Reporting bugs
- 💡 Suggesting new features
- 🤝 Contributing code
- 📢 Spreading the word

---

**Made with ❤️ for Bharat** | Empowering citizens through technology
