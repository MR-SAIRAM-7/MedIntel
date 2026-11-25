# MedIntel AI Health Assistant - Installation Guide

## Overview
MedIntel is a comprehensive AI-powered health assistant that analyzes medical reports, images, and answers health questions in multiple languages using GPT-4o-mini model.

## Features
- 🏥 **AI Medical Assistant**: Professional medical analysis with proper disclaimers
- 📁 **Multi-format File Support**: Images, PDFs, text files
- 🌍 **Multilingual Support**: 10+ languages via AI prompts
- 💬 **Real-time Chat Interface**: Modern, professional UI
- 📊 **Session Management**: Save and manage consultation history
- 🔒 **Medical Ethics**: Proper disclaimers and professional boundaries

## Tech Stack
- **Frontend**: React 19.0.0, Tailwind CSS, Shadcn UI Components
- **Backend**: FastAPI 0.110.1, MongoDB, emergentintegrations
- **AI**: GPT-4o-mini via emergentintegrations library
- **Database**: MongoDB for session and message storage

## Quick Setup

### Prerequisites
- Python 3.11+
- Node.js 20+
- MongoDB (local or cloud)
- Yarn package manager

### Installation Steps

1. **Extract the ZIP file**
   ```bash
   unzip MedIntel_AI_Health_Assistant.zip
   cd MedIntel_AI_Health_Assistant
   ```

2. **Backend Setup**
   ```bash
   cd backend
   
   # Install Python dependencies
   pip install -r requirements.txt
   
   # Configure environment variables in .env
   # Update MONGO_URL if needed (default: mongodb://localhost:27017)
   # EMERGENT_LLM_KEY is pre-configured for testing
   ```

3. **Frontend Setup**
   ```bash
   cd ../frontend
   
   # Install Node.js dependencies
   yarn install
   
   # Update .env if needed
   # REACT_APP_BACKEND_URL is pre-configured
   ```

### Running the Application

1. **Start Backend** (from backend directory)
   ```bash
   python -m uvicorn server:app --host 0.0.0.0 --port 8001 --reload
   ```

2. **Start Frontend** (from frontend directory)
   ```bash
   yarn start
   ```

3. **Access the Application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8001
   - API Docs: http://localhost:8001/docs

## API Configuration

### Using Your Own OpenAI API Key

1. **Get OpenAI API Key**
   - Visit: https://platform.openai.com/api-keys
   - Create a new API key

2. **Update Backend Configuration**
   ```bash
   # In backend/.env, replace:
   EMERGENT_LLM_KEY=your-key-here
   
   # With your OpenAI key:
   OPENAI_API_KEY=your-openai-api-key-here
   ```

3. **Modify server.py** (if using direct OpenAI integration)
   ```python
   # Replace emergentintegrations with direct OpenAI client
   # Update the get_ai_response function accordingly
   ```

### Alternative: Keep Using Emergent Universal Key
The app comes pre-configured with `EMERGENT_LLM_KEY` which works out of the box for testing and development.

## Database Configuration

### Local MongoDB
```bash
# Default configuration (no changes needed)
MONGO_URL="mongodb://localhost:27017"
DB_NAME="test_database"
```

### MongoDB Atlas (Cloud)
```bash
# Update backend/.env with your Atlas connection string:
MONGO_URL="mongodb+srv://username:password@cluster.mongodb.net/"
DB_NAME="meditel_production"
```

## File Structure
```
MedIntel_AI_Health_Assistant/
├── backend/
│   ├── server.py           # FastAPI application
│   ├── requirements.txt    # Python dependencies
│   └── .env               # Environment variables
├── frontend/
│   ├── src/
│   │   ├── App.js         # Main React component
│   │   ├── App.css        # Styling
│   │   └── components/ui/ # Shadcn UI components
│   ├── package.json       # Node.js dependencies
│   └── .env              # Frontend environment
└── README.md             # This file
```

## Key API Endpoints

- `POST /api/chat/session` - Create new chat session
- `POST /api/chat/message` - Send text message
- `POST /api/chat/upload` - Upload and analyze files
- `GET /api/chat/session/{id}/messages` - Get chat history
- `DELETE /api/chat/session/{id}` - Delete session

## Deployment

### Production Deployment
1. Set up production MongoDB
2. Configure environment variables for production
3. Build frontend: `yarn build`
4. Use production WSGI server for backend (gunicorn)
5. Set up reverse proxy (nginx)

### Docker Deployment (Optional)
Create Dockerfile for containerized deployment:
```dockerfile
# Frontend build
FROM node:20 AS frontend-build
WORKDIR /app/frontend
COPY frontend/package.json frontend/yarn.lock ./
RUN yarn install
COPY frontend/ ./
RUN yarn build

# Backend
FROM python:3.11
WORKDIR /app
COPY backend/requirements.txt ./
RUN pip install -r requirements.txt
COPY backend/ ./
COPY --from=frontend-build /app/frontend/build ./static
CMD ["uvicorn", "server:app", "--host", "0.0.0.0", "--port", "8001"]
```

## Troubleshooting

### Common Issues

1. **MongoDB Connection Error**
   ```bash
   # Ensure MongoDB is running
   sudo systemctl start mongod  # Linux
   brew services start mongodb  # macOS
   ```

2. **Port Already in Use**
   ```bash
   # Kill processes on ports
   lsof -ti:8001 | xargs kill -9  # Backend
   lsof -ti:3000 | xargs kill -9  # Frontend
   ```

3. **AI Integration Issues**
   - Verify EMERGENT_LLM_KEY or OPENAI_API_KEY
   - Check network connectivity
   - Review backend logs for errors

4. **Frontend Build Issues**
   ```bash
   # Clear node_modules and reinstall
   rm -rf node_modules yarn.lock
   yarn install
   ```

## Support & Documentation

- **API Documentation**: Visit `/docs` endpoint when backend is running
- **Component Library**: Shadcn UI components in `src/components/ui/`
- **AI Integration**: emergentintegrations library documentation

## Security Notes

- Never commit API keys to version control
- Use environment variables for sensitive configuration
- Implement proper authentication for production use
- Regular security updates for dependencies

## License
This project is licensed under the MIT License.

---

**MedIntel AI Health Assistant** - Professional AI medical consultation platform built with modern technologies.

For questions or support, please check the troubleshooting section or review the API documentation.