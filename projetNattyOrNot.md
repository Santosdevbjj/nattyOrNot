# Projeto: Natty or Not – Personas IA 👁️‍🗨️
# Estrutura Inicial com prompts few-shot, endpoints para IA e esqueleto de app

# 1) Few-shot prompts (Exemplos para GPT-4)
few_shot_examples = [
    {
        "role": "user",
        "content": "Create a 100-word biography for a fictional person.\n- Role: marketing strategist\n- Age: 34\n- Background: grew up in São Paulo, works in fintech\n- Tone: professional but warm\nInclude a surprising personal hobby."
    },
    {
        "role": "assistant",
        "content": "Júlia Martins, 34, is a dynamic marketing strategist based in São Paulo, Brazil. Raised in a bustling suburb, she developed an early interest in consumer behavior, which led her to a successful career in fintech. Her days are filled with data analytics and campaign design, helping startups build brands that resonate. Despite her corporate flair, Júlia is passionate about crocheting miniature animals, a hobby she discovered during the pandemic. She often donates her creations to local orphanages. Her unique blend of analytical thinking and artistic creativity makes her a respected voice in Brazil's marketing scene."
    }
]



# 2) Configuração de endpoints Flask para IA generativa
from flask import Flask, request, jsonify
import openai  # Use a SDK compatível com GPT e DALL-E

app = Flask(__name__)



# Endpoint para gerar biografia
@app.route('/api/generate_bio', methods=['POST'])
def generate_bio():
    data = request.json
    role = data.get("role")
    age = data.get("age")
    background = data.get("background")
    hobby = data.get("hobby")

    prompt = f"""
    Create a 100-word biography for a fictional person.
    - Role: {role}
    - Age: {age}
    - Background: {background}
    - Tone: professional but warm
    Include a surprising personal hobby: {hobby}
    """

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are a persona creation specialist."},
            *few_shot_examples,
            {"role":
