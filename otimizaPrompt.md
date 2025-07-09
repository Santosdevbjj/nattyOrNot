## 🤖 Otimização de Prompt Engineering


Prompting Multimodal (Texto, Imagem e Áudio)

Defina claramente o papel da IA: Use “You are a professional persona creator…”, definindo tom, estilo e público  .

Ordens passo a passo: Solicite instruções estruturadas (por exemplo, “Generate image, bio, then audio script”)  .

Exemplos positivos/negativos (“few-shot”): Inclua amostras de bios reais e falsas para orientar o GPT-4  .



**2. Prompt para Imagens Realistas**

Detalhamento específico: luz natural suave, resolução alta, textura da pele, lente, composição  .

Evite o termo “photorealistic” — prefira “photo of…” ou “hyper‑realistic portrait…”  .

Incorpore metadados fotográficos: ISO, aperture, shutter speed, estilo de fotógrafo renomado (com atenção aos direitos)  .



**3. Prompt para Áudio (TTS)**

Solicite tom, velocidade, emoção (ex: “friendly tone, moderate pace, slight warmth”)

Considere usar instruções de estilo dentro de “system message” antes de enviar ao TTS.


**4. Iteração e Refinamento**

Aplique abordagem iterativa: refine usando feedback com “rewrite to be more concise/persuasive”  .

Use meta‑prompting: peça ao GPT para melhorar seus próprios prompts  . 




## 🧩 Templates de Prompt

**A) GPT‑4 (bio)**

System: You are a persona creation specialist.  
User: Create a 100‑word biography for a fictional person.  
- Role: marketing strategist  
- Age: 34  
- Background: grew up in São Paulo, works in fintech  
- Tone: professional but warm  
Include a surprising personal hobby.  
Example Real Bio: …  
Example Fake Bio: …




**B) DALL·E‑3 (imagem)**

A photo of a Brazilian adult woman, 34 years old, friendly expression, natural soft lighting, shallow depth of field, high-resolution (8K), taken with 50 mm lens at f/2.0, ISO 200, by a portrait photographer, neutral background. 


**C) ElevenLabs (áudio)**


System: Use a warm, conversational tone.  
User: “Olá, eu sou a Júlia, analista de UX apaixonada por crochê e maratonas…” – duração ~8s. 


## 💻 Aplicação Web


**Backend:** endpoints para gerar e armazenar persona multimodal

**Frontend:** exibir imagem, bio, botão de áudio e quiz “Natty ou Fake”

**Métricas:** taxa de acerto, tempo de resposta, feedback qualitativo 




