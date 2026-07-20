from flask import Flask, render_template, request, jsonify
from google import genai
import os

app = Flask(__name__)

# Initialize the GenAI client
# Make sure to set your GEMINI_API_KEY environment variable
client = genai.Client()

SYSTEM_INSTRUCTION = """
You are an expert technical interviewer specializing in Quality Assurance (QA), Automation Testing, and Business Analysis.
Your goal is to conduct a professional, engaging, and comprehensive mock interview.
You should ask questions about:
1. Manual Testing concepts and methodologies.
2. Automation Testing frameworks and tools, specifically Selenium, Playwright, and using multiple programming languages.
3. Business Analysis techniques, requirements gathering, and agile methodologies.

Ask one question at a time. Evaluate the user's previous answer briefly, and then provide the next question.
Keep the conversation flowing naturally.
"""

conversation_history = []

@app.route('/')
def index():
    conversation_history.clear()
    conversation_history.append({'role': 'system', 'content': SYSTEM_INSTRUCTION})
    return render_template('index.html')

@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message')
    conversation_history.append({'role': 'user', 'content': user_message})

    try:
        response = client.models.generate_content(
            model='gemini-2.5-flash',
            contents=conversation_history
        )
        
        ai_response = response.text
        conversation_history.append({'role': 'model', 'content': ai_response})
        return jsonify({'response': ai_response})
        
    except Exception as e:
        return jsonify({'response': 'Error communicating with Gemini API. Please check your API key.'}), 500

if __name__ == '__main__':
    app.run(debug=True)
