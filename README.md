from flask import Flask, render_template, request, jsonify, send_from_directory
from googletrans import Translator
import os

app = Flask(__name__)
translator = Translator()

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/translate', methods=['POST'])
def translate():
    data = request.json
    text = data.get('text', '')
    target_lang = data.get('target_lang', 'en')

    if not text:
        return jsonify({'error': 'No text provided'}), 400

    translated_text = translator.translate(text, dest=target_lang).text
    return jsonify({'translated_text': translated_text})

# Serve Static Files (CSS & JS)
@app.route('/static/<path:filename>')
def static_files(filename):
    return send_from_directory(os.path.join(app.root_path, 'static'), filename)

if __name__ == '__main__':
    app.run(debug=True)
