from flask import Flask, request, jsonify
from docx import Document
import requests
import tempfile
import os

app = Flask(__name__)

@app.route('/api/extract', methods=['POST'])
def extract_text():
    try:
        data = request.json
        file_url = data.get('file_url')
        
        # Descargar .docx
        with tempfile.NamedTemporaryFile(delete=False, suffix='.docx') as tmp:
            tmp_path = tmp.name
            with requests.get(file_url, stream=True) as r:
                r.raise_for_status()
                for chunk in r.iter_content(chunk_size=8192):
                    tmp.write(chunk)
        
        # Extraer texto
        doc = Document(tmp_path)
        text = "\n".join([p.text for p in doc.paragraphs if p.text.strip()])
        
        os.unlink(tmp_path)
        return jsonify({"text": text})
    
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True)
