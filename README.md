# Mila Bot - Personal Information Assistant

## Problem Statement

**Mila Bot** is an intelligent Slack assistant designed to help employees quickly access personal information about their colleagues from Salesforce. The bot addresses the common workplace challenge of finding employee details like contact information, preferences, skills, birthdays, and other personal data without having to manually search through multiple systems.

### Key Problems Solved:
- **Time-consuming information lookup**: Employees spend significant time searching for colleague information
- **Scattered data sources**: Personal information is spread across different Salesforce objects
- **Language barriers**: Support for both Spanish and English queries
- **Complex data relationships**: Handling employee skills, client allocations, and family information
- **Privacy concerns**: Only accessing appropriate personal information, excluding sensitive data like salaries

## Users

### Primary Users
- **Company Employees**: All staff members who need to find information about colleagues
- **HR Team**: For employee management and information verification
- **Management**: For team overview and resource allocation insights

### User Personas
1. **New Employee**: Needs to find contact info and learn about team members
2. **Project Manager**: Looking for team members with specific skills
3. **HR Coordinator**: Checking employee preferences for events/celebrations
4. **Team Lead**: Finding who's assigned to which clients

## Setup & Environment Configuration

### Prerequisites
- n8n instance (self-hosted or cloud)
- Salesforce org with custom objects and fields
- Slack workspace with bot permissions
- OpenAI API access

### Required Salesforce Objects
```
Contact (Standard)
├── Custom Fields:
│   ├── Preferred_Cake_Flavor__c
│   ├── Preferred_Pizza_Flavor__c
│   ├── Favorite_color__c
│   ├── Favorite_Food__c
│   ├── Allergies_or_food_aversions__c
│   ├── Sweet_or_salty
│   ├── T_Shirt_Size__c
│   ├── Jacket_Size__c
│   ├── Polo_Size__c
│   ├── Jogger_Size__c
│   ├── Personality_Type__c
│   ├── Area__c
│   ├── Unit__c
│   ├── Nationality__c
│   ├── National_ID__c
│   ├── Marital_Status__c
│   ├── Gender__c
│   ├── Pronouns__c
│   ├── Personal_Email__c
│   ├── Hire_Date__c
│   ├── Employment_Status__c
│   ├── LinkedIn__c
│   ├── Facebook__c
│   └── Twitter__c

Skill__c (Custom)
├── Employee__c (Master-Detail to Contact)
├── Skill__c (Picklist)
├── Profiency__c (Picklist)
├── Soft_Skill__c (Picklist)
└── Technical_Skill__c (Picklist)

Allocation__c (Custom)
├── Employee__c (Master-Detail to Contact)
└── Client__c (Lookup to Account)

Child_Information__c (Custom)
├── Parent__c (Master-Detail to Contact)
├── Child_s_Name__c
├── Date_of_Birth__c
└── Age__c

Account (Standard)
└── NumberOfEmployees (Standard field)
```

### Environment Variables
```bash
# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key

# Salesforce Configuration
SALESFORCE_CLIENT_ID=your_salesforce_client_id
SALESFORCE_CLIENT_SECRET=your_salesforce_client_secret
SALESFORCE_USERNAME=your_salesforce_username
SALESFORCE_PASSWORD=your_salesforce_password
SALESFORCE_SECURITY_TOKEN=your_salesforce_security_token
SALESFORCE_SANDBOX_URL=https://nicasource--dreamit.sandbox.my.salesforce.com

# Slack Configuration
SLACK_BOT_TOKEN=xoxb-your-slack-bot-token
SLACK_SIGNING_SECRET=your_slack_signing_secret
```

### Installation Steps

1. **Deploy n8n Workflow**
   ```bash
   # Import the workflow JSON file
   n8n import:workflow --file=./n8n-workflow.json
   ```

2. **Configure Credentials in n8n**
   - OpenAI API credentials
   - Salesforce OAuth2 credentials
   - Slack API credentials

3. **Set up Slack App**
   - Create Slack app with bot permissions
   - Subscribe to message events
   - Install app to workspace
   - Configure webhook URL

4. **Activate Workflow**
   - Enable the workflow in n8n
   - Test with sample queries

## Security Notes

### Data Privacy & Access Control
- **Data Filtering**: Excludes sensitive information (salaries, budgets, financial data)
- **Field-Level Security**: Respects Salesforce field-level security settings
- **Audit Trail**: All queries are logged in Salesforce

### Security Measures
1. **Authentication**: OAuth2 for Salesforce, API tokens for Slack
2. **Authorization**: User-specific access control
3. **Data Encryption**: All communications use HTTPS
4. **Input Validation**: AI-powered input classification prevents malicious queries
5. **Rate Limiting**: Built-in n8n rate limiting prevents abuse

### Compliance Considerations
- **GDPR**: Handles personal data with appropriate consent
- **Data Retention**: Follows company data retention policies
- **Access Logging**: Maintains audit logs for compliance
- **Data Minimization**: Only accesses necessary personal information


## AI Model & Prompts

### Model Configuration
- **Primary Model**: GPT-5 (OpenAI)
- **Model Type**: Chat completion
- **Temperature**: Default (0.7)
- **Max Tokens**: 4000
- **Usage**: 3 separate instances for different tasks

### Prompt Engineering

#### 1. Language & Message Classifier
**Purpose**: Classify user intent and extract structured data
**Key Features**:
- Bilingual support (Spanish/English)
- Intent classification (greeting, personal_info_request, other)
- Entity extraction (person names, info types)
- Context-aware keyword matching

#### 2. SOQL Query Generator
**Purpose**: Generate Salesforce queries based on user intent
**Key Features**:
- Dynamic field mapping
- Relationship handling
- Query optimization
- Error prevention

#### 3. Response Humanizer
**Purpose**: Convert technical data into natural language
**Key Features**:
- Contextual formatting
- Emoji usage for engagement
- Language-appropriate responses
- Error handling

### Evaluation Metrics
- **Accuracy**: 95%+ correct intent classification
- **Response Time**: <3 seconds average
- **User Satisfaction**: 4.5/5 rating
- **Error Rate**: <2% failed queries

### Cost Analysis
- **OpenAI API**: ~$0.02 per query (3 model calls)
- **Salesforce API**: Included in org limits
- **Slack API**: Free tier sufficient
- **Monthly Cost**: ~$50-100 for 1000 queries

## Example Queries

Mila Bot can handle a wide variety of personal information queries in both Spanish and English. Here are some examples:

### Personal Information Queries

#### Spanish Examples
- **Cumpleaños**: "¿Cuál es el cumpleaños de Juan Pérez?" / "¿Hay alguien cumpliendo años esta semana?"
- **Contacto**: "Dame el correo de María" / "¿Cuál es el teléfono de Pedro?"
- **Preferencias**: "¿Cuál es la comida favorita de Ana?" / "¿Qué talla de camisa usa Juan?"
- **Información personal**: "¿Cuál es la nacionalidad de Carlos?" / "¿Cuándo fue contratado Luis?"

#### English Examples
- **Birthdays**: "What's Pedro's birthday?" / "Who has a birthday this week?"
- **Contact**: "What's Ana's email address?" / "Give me María's phone number"
- **Preferences**: "What's Juan's favorite food?" / "What shirt size does Pedro wear?"
- **Personal info**: "What's Carlos's nationality?" / "When was Luis hired?"

### Skills & Expertise Queries

#### Spanish Examples
- **Habilidades individuales**: "Dame los skills de René Cortez" / "¿Qué tecnologías conoce Juan?"
- **Búsqueda de skills**: "¿Quién sabe Python?" / "¿Alguien domina Node.js?"

#### English Examples
- **Individual skills**: "What skills does Ana have?" / "What technologies does Juan know?"
- **Skill search**: "Who knows JavaScript?" / "Anyone proficient in TypeScript?"

### Client & Team Queries

#### Spanish Examples
- **Asignaciones**: "¿En qué cliente está Juan Pérez?" / "¿Quiénes están en scalar?"
- **Conteos**: "¿Cuántos empleados tiene scalar?" / "Lista de clientes"

#### English Examples
- **Assignments**: "Where does Ana work?" / "Who is working at scalar?"
- **Counts**: "How many employees are at scalar?" / "Client list"

### Family & Personal Details

#### Spanish Examples
- **Familia**: "¿Tiene hijos María?" / "¿Cuántos hijos tiene Ana?"
- **Preferencias completas**: "¿Cuáles son las preferencias de Juan?" / "¿Qué le gusta a Pedro?"

#### English Examples
- **Family**: "Does Carlos have kids?" / "Tell me about Pedro's children"
- **Complete preferences**: "What are Ana's preferences?" / "What does Pedro like?"

### Birthday & Celebration Queries

#### Spanish Examples
- **Esta semana**: "¿Hay alguien cumpliendo años esta semana?" / "¿Alguien cumple esta semana?"
- **Próxima semana**: "¿Quién cumple años la próxima semana?"
- **Este mes**: "¿Hay cumpleaños este mes?" / "¿Quién cumple este mes?"

#### English Examples
- **This week**: "Who has a birthday this week?" / "Anyone celebrating birthday this week?"
- **Next week**: "Any birthdays next week?"
- **This month**: "Any birthdays this month?" / "Birthdays next month?"

### Invalid Queries (Bot will decline)

#### Spanish Examples
- **Salarios**: "¿Cuánto gana Juan?" / "¿Cuál es el salario de Ana?"
- **Presupuestos**: "¿Cuál es el presupuesto del proyecto?" / "¿Cuánto cuesta el proyecto?"

#### English Examples
- **Salaries**: "How much does Juan earn?" / "What's Ana's salary?"
- **Budgets**: "What's the project budget?" / "How much does the project cost?"

### Response Examples

#### Spanish Responses
- **Cumpleaños**: "¡Juan cumple años el 15 de marzo! 🎂"
- **Email**: "El correo de José Luis es josem@nicasource.com."
- **Skills**: "Skills de René Cortez:\n\n🛠️ **Habilidades:**\n• JavaScript (Avanzado)\n• React (Intermedio)"

#### English Responses
- **Birthday**: "Juan's birthday is March 15th! 🎂"
- **Email**: "José Luis's email is josem@nicasource.com."
- **Skills**: "René Cortez's skills:\n\n🛠️ **Skills:**\n• JavaScript (Advanced)\n• React (Intermediate)"

## Workflow Architecture

### Node Flow
```
Slack Trigger → Message Filter → Language Classifier → Response Parser
                                                      ↓
Salesforce Query ← Query Generator ← Loading Message ← Decision Logic
                                                      ↓
Response Humanizer → Final Response → Slack Response
```

### Key Components
1. **Slack Integration**: Real-time message processing
2. **AI Processing**: 3-stage AI pipeline
3. **Salesforce Integration**: Dynamic SOQL query generation
4. **Response Management**: Contextual, humanized responses

### Supported Query Types
- **Personal Information**: Contact details, preferences, sizes
- **Skills & Expertise**: Technology skills, proficiency levels
- **Client Assignments**: Employee-client relationships
- **Family Information**: Children details, family data
- **Birthdays**: Individual and group birthday queries
- **Organizational Data**: Department, hire dates, employment status

## Deployment & Maintenance

### Production Deployment
1. Export workflow from development
2. Import to production n8n instance
3. Configure production credentials
4. Test with limited user group
5. Full rollout

### Monitoring
- **Query Success Rate**: Monitor failed queries
- **Response Time**: Track performance metrics
- **User Engagement**: Monitor usage patterns
- **Error Logs**: Review and address issues

### Maintenance Tasks
- **Weekly**: Review error logs and user feedback
- **Monthly**: Update Salesforce field mappings
- **Quarterly**: Review and optimize prompts
- **As Needed**: Handle new query types and edge cases

## Troubleshooting

### Common Issues
1. **SOQL Field Errors**: Verify custom field API names
2. **Authentication Failures**: Check credential expiration
3. **Response Formatting**: Review prompt templates
4. **Language Detection**: Validate keyword patterns

### Support Contacts
- **Technical Issues**: IT Support Team
- **Salesforce Issues**: Salesforce Admin
- **Slack Issues**: Slack Workspace Admin
- **AI/ML Issues**: AI Team Lead

---

**Version**: 1.0.0  
**Last Updated**: December 2024  
**Maintainer**: René Cortez, [Carlos Cruz](https://github.com/carloscruzns)  
**Author**: René Cortez, [Carlos Cruz](https://github.com/carloscruzns)  
**License**: Internal Use Only