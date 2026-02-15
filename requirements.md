# Requirements Document: RationMitra

## Introduction

RationMitra is a WhatsApp-based AI assistant designed to help Indian citizens, particularly those in rural areas with low digital literacy, discover government benefits they're entitled to but not receiving, and guide them through the claiming process. The system addresses the critical problem that 60% of eligible Indian citizens don't know about government schemes they qualify for, resulting in thousands of rupees in unclaimed benefits annually.

The system targets rural citizens (farmers, elderly, women, daily wage workers, BPL families) as primary users, with urban migrants and first-generation benefit seekers as secondary users. It provides conversational profile building, intelligent eligibility matching, personalized claiming guides, application tracking, document assistance, and multilingual support through WhatsApp.

## Glossary

- **RationMitra**: The WhatsApp-based AI assistant system
- **User**: An Indian citizen interacting with RationMitra to discover and claim government benefits
- **Profile**: Structured user data collected through conversational questions (age, location, income, family composition, etc.)
- **Scheme**: A government benefit program with specific eligibility criteria and claiming procedures
- **Eligibility_Matcher**: The component that determines which schemes a user qualifies for
- **Claiming_Guide**: Step-by-step instructions for applying to a specific scheme
- **Application_Tracker**: The component that manages user-reported progress on benefit applications
- **Conversational_AI**: The LLM-powered component that handles natural language interactions
- **Profile_Builder**: The component that conducts the initial 5-7 question interview
- **Document_Assistant**: The component that helps users understand and manage required documents
- **Reminder_System**: The component that sends time-based follow-up messages
- **Scheme_Database**: The repository of 20+ government schemes with eligibility rules and procedures
- **Speech_Processor**: The component handling voice note transcription and text-to-speech
- **WhatsApp_Interface**: The Twilio-based integration with WhatsApp Business API
- **Gap_Analysis**: The process of identifying benefits a user is eligible for but not receiving
- **Escalation_Path**: Instructions for what to do when an application is delayed beyond expected timeframes

## Requirements

### Requirement 1: Conversational Profile Building

**User Story:** As a rural citizen with low digital literacy, I want to answer simple questions in my own words through WhatsApp, so that the system can understand my situation without requiring me to fill complex forms.

#### Acceptance Criteria

1. WHEN a new user initiates contact, THE Profile_Builder SHALL greet the user and begin a conversational interview
2. THE Profile_Builder SHALL ask between 5 and 7 questions to collect essential profile information
3. WHEN a user responds in Hindi or English, THE Conversational_AI SHALL extract structured data from natural language responses
4. WHEN a user provides incomplete sentences or colloquial language, THE Conversational_AI SHALL interpret the intent and extract relevant information
5. WHEN a user mixes Hindi and English in responses, THE Conversational_AI SHALL process the mixed-language input correctly
6. THE Profile_Builder SHALL complete the interview process within 5 minutes for users providing direct answers
7. WHEN profile data is collected, THE RationMitra SHALL store the profile securely in the database
8. THE Profile_Builder SHALL collect at minimum: age, gender, location (state/district), income level, family composition, current benefits received, and occupation
9. WHEN a user provides vague or ambiguous responses, THE Profile_Builder SHALL ask clarifying follow-up questions
10. THE Profile_Builder SHALL adapt question phrasing based on user's language preference and literacy level

### Requirement 2: Intelligent Eligibility Matching

**User Story:** As a user who has completed my profile, I want to know which government schemes I qualify for, so that I can understand what benefits I'm missing out on.

#### Acceptance Criteria

1. WHEN a user profile is complete, THE Eligibility_Matcher SHALL evaluate the profile against all schemes in the Scheme_Database
2. THE Eligibility_Matcher SHALL match profiles against at least 20 major government schemes
3. WHEN evaluating eligibility, THE Eligibility_Matcher SHALL apply rule-based matching with AI-enhanced reasoning for edge cases
4. THE Eligibility_Matcher SHALL categorize schemes into three groups: currently receiving, eligible but not receiving, and potentially eligible in future
5. WHEN a user is eligible for a scheme, THE Eligibility_Matcher SHALL provide an explanation of why they qualify with reference to official criteria
6. THE Eligibility_Matcher SHALL calculate the estimated financial value of missing benefits
7. THE Eligibility_Matcher SHALL rank eligible schemes by priority based on financial value and ease of claiming
8. WHEN eligibility rules have exceptions or special conditions, THE Eligibility_Matcher SHALL correctly apply these conditions
9. THE Eligibility_Matcher SHALL handle state-specific and district-specific eligibility variations
10. WHEN a user's profile changes, THE Eligibility_Matcher SHALL re-evaluate eligibility and notify of new opportunities

### Requirement 3: Gap Analysis and Benefit Discovery

**User Story:** As a user, I want to see a clear report of benefits I'm missing, so that I understand the financial impact and can prioritize which benefits to claim first.

#### Acceptance Criteria

1. WHEN eligibility matching is complete, THE RationMitra SHALL generate a personalized gap analysis report
2. THE Gap_Analysis SHALL display currently received benefits with confirmation
3. THE Gap_Analysis SHALL highlight eligible but unclaimed benefits with estimated annual value
4. THE Gap_Analysis SHALL show total potential annual benefit amount across all missing schemes
5. THE Gap_Analysis SHALL rank missing benefits by a combination of financial value and claiming difficulty
6. WHEN displaying each missing benefit, THE RationMitra SHALL include scheme name, benefit amount, and brief explanation
7. THE Gap_Analysis SHALL identify future eligibility opportunities with timeline information
8. THE RationMitra SHALL present the gap analysis in simple language appropriate for low-literacy users
9. WHEN a user has no missing benefits, THE RationMitra SHALL congratulate them and offer to check for future opportunities
10. THE Gap_Analysis SHALL be shareable via WhatsApp to family members or helpers

### Requirement 4: Personalized Claiming Guides

**User Story:** As a user who wants to claim a benefit, I want detailed step-by-step instructions in my language, so that I know exactly what to do, where to go, and what to say.

#### Acceptance Criteria

1. WHEN a user selects a scheme to claim, THE RationMitra SHALL provide a personalized claiming guide
2. THE Claiming_Guide SHALL include a difficulty rating (easy, moderate, difficult)
3. THE Claiming_Guide SHALL provide a complete checklist of required documents
4. THE Claiming_Guide SHALL include specific office locations with addresses and map links
5. THE Claiming_Guide SHALL provide conversation scripts in the user's preferred language for speaking with officials
6. THE Claiming_Guide SHALL include relevant helpline numbers and contact information
7. THE Claiming_Guide SHALL provide troubleshooting tips for common issues
8. THE Claiming_Guide SHALL be downloadable or shareable via WhatsApp
9. WHEN claiming procedures vary by state or district, THE Claiming_Guide SHALL provide location-specific instructions
10. THE Claiming_Guide SHALL break down the process into numbered steps with clear actions
11. THE Claiming_Guide SHALL include estimated timeframes for each step and overall processing time
12. WHEN online application is available, THE Claiming_Guide SHALL provide website links and digital application guidance

### Requirement 5: Application Tracking and Progress Management

**User Story:** As a user who has submitted an application, I want to track my progress and receive reminders, so that I don't forget to follow up and can take action if there are delays.

#### Acceptance Criteria

1. WHEN a user reports submitting an application, THE Application_Tracker SHALL record the submission date and scheme name
2. THE Application_Tracker SHALL allow users to store receipt numbers or reference numbers
3. THE Application_Tracker SHALL allow users to manually update application status through conversational input
4. WHEN 15 days have passed since submission, THE Reminder_System SHALL send a follow-up reminder
5. WHEN 30 days have passed since submission, THE Reminder_System SHALL send a status check reminder
6. WHEN 45 days have passed without approval, THE Reminder_System SHALL provide escalation suggestions
7. THE Application_Tracker SHALL provide instructions for checking application status
8. WHEN a user reports successful benefit receipt, THE RationMitra SHALL send a celebration message
9. THE Reminder_System SHALL send renewal reminders for benefits that require periodic renewal
10. THE Application_Tracker SHALL maintain a history of all applications and their outcomes
11. WHEN a user has multiple active applications, THE Application_Tracker SHALL provide a summary view
12. THE Reminder_System SHALL adapt reminder frequency based on typical processing times for each scheme

### Requirement 6: Document Assistance

**User Story:** As a user preparing to apply for a benefit, I want clear guidance on required documents, so that I can gather everything needed and avoid application rejection.

#### Acceptance Criteria

1. WHEN a user views a claiming guide, THE Document_Assistant SHALL provide a complete list of required documents
2. THE Document_Assistant SHALL explain the purpose of each required document in simple language
3. WHEN a user is missing a document, THE Document_Assistant SHALL provide instructions on where and how to obtain it
4. THE Document_Assistant SHALL allow users to upload document photos for secure storage
5. WHEN documents are uploaded, THE RationMitra SHALL encrypt and store them securely
6. THE Document_Assistant SHALL provide validation tips for ensuring documents meet requirements
7. THE Document_Assistant SHALL offer form-filling guidance for application forms
8. THE Document_Assistant SHALL provide a checklist feature for tracking document collection progress
9. WHEN documents have expiry dates, THE Document_Assistant SHALL track expiry and send renewal reminders
10. THE Document_Assistant SHALL explain acceptable alternatives when primary documents are unavailable
11. THE Document_Assistant SHALL provide examples of correctly filled forms when available

### Requirement 7: Multilingual Conversational AI

**User Story:** As a user who speaks Hindi or has low literacy, I want to communicate naturally in my language using text or voice, so that I can use the system without language barriers.

#### Acceptance Criteria

1. THE Conversational_AI SHALL support Hindi and English for all interactions
2. THE Conversational_AI SHALL accept text input as the primary interaction mode
3. THE Conversational_AI SHALL accept voice notes and transcribe them to text
4. WHEN a user sends a voice note, THE Speech_Processor SHALL transcribe it using speech-to-text technology
5. THE Conversational_AI SHALL generate text-to-speech audio responses when requested by the user
6. THE Conversational_AI SHALL understand natural language input including incomplete sentences and colloquialisms
7. THE Conversational_AI SHALL adapt communication style based on user's literacy level
8. WHEN a user demonstrates low literacy, THE Conversational_AI SHALL use simpler vocabulary and shorter sentences
9. THE Conversational_AI SHALL maintain conversation context across multiple messages
10. THE Conversational_AI SHALL demonstrate cultural awareness appropriate for Indian rural contexts
11. WHEN a user switches between Hindi and English, THE Conversational_AI SHALL handle code-switching appropriately
12. THE Conversational_AI SHALL recognize and respond appropriately to common Indian English phrases and expressions

### Requirement 8: Scheme Information Database

**User Story:** As the system, I need comprehensive and accurate information about government schemes, so that I can provide correct eligibility criteria and claiming procedures to users.

#### Acceptance Criteria

1. THE Scheme_Database SHALL contain information for at least 20 high-impact central and state schemes
2. THE Scheme_Database SHALL store both official scheme names and common names used by citizens
3. THE Scheme_Database SHALL include benefit amounts or benefit descriptions for each scheme
4. THE Scheme_Database SHALL store detailed eligibility criteria with all conditions and exceptions
5. THE Scheme_Database SHALL include required documents for each scheme
6. THE Scheme_Database SHALL store application process steps for each scheme
7. THE Scheme_Database SHALL include office locations and contact information
8. THE Scheme_Database SHALL store helpline numbers for each scheme
9. THE Scheme_Database SHALL handle state-specific and district-specific variations in schemes
10. THE Scheme_Database SHALL source information from official government sources including MyScheme.gov.in
11. THE Scheme_Database SHALL be updatable to reflect changes in scheme rules or procedures
12. THE Scheme_Database SHALL include scheme categories (pension, agriculture, housing, insurance, etc.)

### Requirement 9: WhatsApp Integration

**User Story:** As a user, I want to interact with RationMitra through WhatsApp, so that I can use a familiar platform without installing new apps.

#### Acceptance Criteria

1. THE WhatsApp_Interface SHALL integrate with WhatsApp Business API via Twilio
2. WHEN a user sends a message to the RationMitra WhatsApp number, THE WhatsApp_Interface SHALL receive and process it
3. THE WhatsApp_Interface SHALL send text messages to users through WhatsApp
4. THE WhatsApp_Interface SHALL support receiving voice notes from users
5. THE WhatsApp_Interface SHALL support sending images and documents to users
6. THE WhatsApp_Interface SHALL handle message delivery failures gracefully
7. WHEN WhatsApp API rate limits are reached, THE WhatsApp_Interface SHALL queue messages appropriately
8. THE WhatsApp_Interface SHALL maintain conversation threading and context
9. THE WhatsApp_Interface SHALL support WhatsApp message formatting (bold, italic, lists)
10. THE WhatsApp_Interface SHALL handle user-initiated conversations and system-initiated reminders

### Requirement 10: Data Security and Privacy

**User Story:** As a user sharing personal information, I want my data to be secure and private, so that my sensitive information is protected.

#### Acceptance Criteria

1. WHEN user profile data is stored, THE RationMitra SHALL encrypt sensitive personal information
2. THE RationMitra SHALL store data in a secure PostgreSQL database with access controls
3. WHEN documents are uploaded, THE RationMitra SHALL encrypt files at rest
4. THE RationMitra SHALL use secure HTTPS connections for all API communications
5. THE RationMitra SHALL not share user data with third parties without explicit consent
6. THE RationMitra SHALL implement authentication to ensure users can only access their own data
7. WHEN a user requests data deletion, THE RationMitra SHALL remove all personal information within 30 days
8. THE RationMitra SHALL log access to sensitive data for security auditing
9. THE RationMitra SHALL comply with Indian data protection regulations
10. THE RationMitra SHALL use secure API keys and credentials stored as environment variables

### Requirement 11: System Architecture and Scalability

**User Story:** As a system administrator, I want RationMitra to be reliable and scalable, so that it can serve many users without performance degradation.

#### Acceptance Criteria

1. THE RationMitra SHALL use FastAPI as the backend framework
2. THE RationMitra SHALL use PostgreSQL on Supabase for data persistence
3. THE RationMitra SHALL integrate with Google Gemini API or Groq API for LLM capabilities
4. THE RationMitra SHALL use OpenAI Whisper for speech-to-text processing
5. THE RationMitra SHALL use gTTS for text-to-speech generation
6. THE RationMitra SHALL be deployable on free-tier cloud platforms (Render or Railway)
7. WHEN user load increases, THE RationMitra SHALL handle concurrent requests efficiently
8. THE RationMitra SHALL implement appropriate error handling and logging
9. THE RationMitra SHALL have API endpoints for all core functionalities
10. THE RationMitra SHALL separate concerns between WhatsApp interface, business logic, and data layers
11. THE RationMitra SHALL implement health check endpoints for monitoring
12. THE RationMitra SHALL use asynchronous processing for long-running tasks

### Requirement 12: User Engagement and Retention

**User Story:** As a returning user, I want to easily check my application status and discover new benefits, so that I stay engaged with the system over time.

#### Acceptance Criteria

1. WHEN a user returns after initial onboarding, THE RationMitra SHALL provide a personalized greeting with status summary
2. THE RationMitra SHALL allow users to check application status through simple conversational commands
3. WHEN new schemes are added that match a user's profile, THE RationMitra SHALL proactively notify the user
4. THE RationMitra SHALL celebrate milestones (first application, first approval, benefits claimed)
5. WHEN a user's life circumstances change, THE RationMitra SHALL allow profile updates and re-evaluation
6. THE RationMitra SHALL provide a help command that explains available features
7. THE RationMitra SHALL track user engagement metrics (profile completion rate, guide access, reminder responses)
8. WHEN a user has been inactive for 60 days with pending applications, THE RationMitra SHALL send a re-engagement message
9. THE RationMitra SHALL provide quick action buttons or menu options for common tasks
10. THE RationMitra SHALL maintain conversation history for context in future interactions

### Requirement 13: Error Handling and Fallback Mechanisms

**User Story:** As a user encountering technical issues, I want clear error messages and alternative options, so that I can still accomplish my goals despite problems.

#### Acceptance Criteria

1. WHEN the LLM API is unavailable, THE RationMitra SHALL provide a graceful error message and retry mechanism
2. WHEN speech-to-text fails, THE RationMitra SHALL ask the user to send a text message instead
3. WHEN the system cannot understand user input after multiple attempts, THE RationMitra SHALL offer to connect with human support
4. WHEN database connection fails, THE RationMitra SHALL queue operations and retry automatically
5. WHEN WhatsApp message delivery fails, THE RationMitra SHALL retry with exponential backoff
6. THE RationMitra SHALL log all errors with sufficient context for debugging
7. WHEN a scheme's information is incomplete, THE RationMitra SHALL acknowledge the limitation and provide available information
8. THE RationMitra SHALL validate user input and provide helpful error messages for invalid data
9. WHEN external APIs timeout, THE RationMitra SHALL fail gracefully without losing user context
10. THE RationMitra SHALL provide fallback responses when AI-generated content is inappropriate or unclear

### Requirement 14: Initial Scheme Coverage

**User Story:** As a user, I want RationMitra to cover the most important and high-impact government schemes, so that I can discover benefits that make a real difference to my life.

#### Acceptance Criteria

1. THE Scheme_Database SHALL include PM-KISAN (farmer income support)
2. THE Scheme_Database SHALL include Old Age Pension schemes
3. THE Scheme_Database SHALL include Widow Pension schemes
4. THE Scheme_Database SHALL include Disability Pension schemes
5. THE Scheme_Database SHALL include Ayushman Bharat (health insurance)
6. THE Scheme_Database SHALL include Ration Card/PDS (public distribution system)
7. THE Scheme_Database SHALL include LPG Ujjwala (cooking gas subsidy)
8. THE Scheme_Database SHALL include Sukanya Samriddhi Yojana (girl child savings)
9. THE Scheme_Database SHALL include PM Matru Vandana Yojana (maternity benefit)
10. THE Scheme_Database SHALL include PM Awas Yojana (housing)
11. THE Scheme_Database SHALL include Kisan Credit Card
12. THE Scheme_Database SHALL include PM Fasal Bima Yojana (crop insurance)
13. THE Scheme_Database SHALL include Soil Health Card scheme
14. THE Scheme_Database SHALL include Atal Pension Yojana
15. THE Scheme_Database SHALL include PM Jeevan Jyoti Bima Yojana (life insurance)
16. THE Scheme_Database SHALL include PM Suraksha Bima Yojana (accident insurance)
17. THE Scheme_Database SHALL include Stand Up India (entrepreneurship)
18. THE Scheme_Database SHALL include MUDRA Loan (micro-enterprise financing)
19. THE Scheme_Database SHALL include NSAP (National Social Assistance Programme)
20. THE Scheme_Database SHALL include major Scholarship Schemes for education
