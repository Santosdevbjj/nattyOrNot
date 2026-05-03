# 🧠 Natty or Not — Detecção de Conteúdo Gerado por IA Multimodal

![Banner](https://github.com/user-attachments/assets/62e0b692-543f-4fd6-9b4f-b5a546241248)

> **Formação Fundamentos de Inteligência Artificial · DIO**

[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com/)
[![DALL·E](https://img.shields.io/badge/OpenAI-DALL·E_3-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com/dall-e-3)
[![Flask](https://img.shields.io/badge/Backend-Flask-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![React](https://img.shields.io/badge/Frontend-React-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![ElevenLabs](https://img.shields.io/badge/TTS-ElevenLabs-FF6B35?style=for-the-badge)](https://elevenlabs.io/)
[![Status](https://img.shields.io/badge/Status-Concluído-00b300?style=for-the-badge)]()

---

## 1. Problema de Negócio

A IA generativa atingiu um ponto de inflexão: textos, imagens e áudios sintéticos estão se tornando **indistinguíveis do conteúdo humano** a olho nu. Esse avanço cria um problema de confiança digital com consequências reais — desinformação, fraude de identidade, manipulação de opinião e erosão da confiança em mídia digital.

O problema central não é técnico — é perceptual: **humanos superestimam sua capacidade de detectar conteúdo gerado por IA**. Sem dados sobre essa limitação, é difícil justificar investimento em sistemas de detecção, watermarking ou governança de IA.

Este projeto quantifica essa limitação por meio de um experimento controlado: gera personas multimodais (texto + imagem + áudio) com engenharia de prompt avançada e mede em tempo real a taxa de acerto de usuários reais ao tentar distinguir pessoas reais de personas sintéticas.

---

## 2. Contexto

O projeto foi desenvolvido na **Formação Fundamentos de Inteligência Artificial da DIO**, com foco em aplicar engenharia de prompt multimodal e IA generativa em um experimento com hipótese testável e resultado mensurável.

O experimento funciona em quatro camadas integradas:

- **Geração de personas** — GPT-4 produz biografias via few-shot prompting; DALL·E 3 gera retratos hiper-realistas; ElevenLabs sintetiza voz com tom e emoção configuráveis
- **Engenharia de prompt** — técnicas de few-shot, role prompting e meta-prompting são aplicadas para maximizar o realismo de cada modalidade
- **Interface de quiz** — o usuário visualiza a persona (imagem + bio + áudio) e classifica: Natty (real) ou Fake (gerado por IA)
- **Coleta de métricas** — taxa de acerto, tempo de decisão e padrões de comportamento são registrados para análise

A hipótese central: **se a taxa de acerto dos usuários ficar próxima de 50%, o conteúdo gerado é indistinguível do real ao nível do acaso**.

---

## 3. Premissas da Análise

- As personas geradas são **inteiramente fictícias** — nenhum dado real de pessoa identificável foi usado como base
- O realismo das personas é intencionalmente maximizado para testar o limite da percepção humana, não para enganar
- A taxa de acerto de 50% é considerada o **baseline de aleatoriedade** — abaixo disso, o conteúdo sintético supera a percepção humana; acima, há algum sinal detectável
- As métricas coletadas são anônimas e servem exclusivamente para análise comportamental do experimento
- O escopo do projeto atual é um MVP funcional — o pipeline de detecção automática com ML está na roadmap, não no escopo entregue

---

## 4. Estratégia da Solução

O desenvolvimento seguiu uma progressão da geração para a avaliação:

**Passo 1 — Engenharia de prompt para cada modalidade**
Cada modalidade (texto, imagem, áudio) exigiu estratégias de prompt distintas. Para texto: few-shot com exemplos de bios reais e sintéticas, role prompting com `"You are a persona creation specialist"` e meta-prompting para refinar os próprios prompts. Para imagem: especificação técnica fotográfica (`50mm lens, f/2.0, ISO 200, natural soft lighting`) em vez do termo genérico `photorealistic`, que produz resultados menos realistas no DALL·E 3. Para áudio: instrução de tom, velocidade e emoção no system message do TTS.

**Passo 2 — Pipeline de geração via API Flask**
Três endpoints independentes: `/api/generate_bio` (GPT-4), `/api/generate_image` (DALL·E 3) e `/api/generate_audio` (ElevenLabs). Cada endpoint recebe parâmetros de persona (role, age, background, hobby) e retorna o artefato gerado. A independência dos endpoints permite substituir qualquer modelo sem refatorar os outros.

**Passo 3 — Interface de quiz em React**
Exibição sequencial da persona (imagem → bio → botão de áudio) seguida de escolha binária (Natty / Fake) e feedback imediato. O design intencional é neutro — sem elementos visuais que sinalizem se a persona é real ou sintética antes da escolha.

**Passo 4 — Coleta e análise de métricas**
MongoDB registra cada interação com timestamp, escolha do usuário, resultado correto e tempo de decisão. Isso permite análise por modalidade: usuários erram mais quando o áudio está presente? Imagens enganam mais que bios? Essas perguntas guiam as evoluções do experimento.

---

## 5. Insights Técnicos

**Few-shot prompting como calibrador de realismo**
Incluir exemplos de bios reais e sintéticas no prompt (few-shot) calibra o GPT-4 para o estilo e nível de detalhe esperado. Sem exemplos, o modelo tende a produzir bios genéricas e formulaicas — detectáveis por padrão linguístico repetitivo. Com few-shot bem construído, a bio incorpora especificidade (hobby incomum, detalhe geográfico, tensão entre vida profissional e pessoal) que é a marca de uma narrativa humana crível.

**A armadilha do termo "photorealistic" no DALL·E 3**
Usar `photorealistic` como instrumento de realismo na geração de imagens é contraintuitivo: o modelo interpreta o termo como estilo artístico, não como instrução técnica, e produz resultados que parecem renders de videogame. A abordagem correta é simular os metadados de uma fotografia real — lente, abertura, ISO, iluminação, composição — deixando o modelo inferir o realismo a partir dos parâmetros técnicos.

**Conteúdo multimodal aumenta exponencialmente a dificuldade de detecção**
Usuários expostos apenas ao texto detectam o padrão sintético com mais facilidade (estrutura narrativa previsível do LLM). Ao adicionar imagem, a atenção cognitiva se divide. Ao adicionar áudio, o processamento emocional entra em cena — e humanos são particularmente suscetíveis a sinais de voz para julgar autenticidade. A combinação das três modalidades cria um efeito de ancoragem cruzada: cada modalidade reforça a credibilidade das outras.

**Taxa de acerto de 55% como dado crítico**
Uma taxa de acerto de 55% com 25 personas representa uma margem de apenas 5% acima do acaso. Com amostras maiores (100+ personas), essa margem tende a convergir para 50% — o que significa que o conteúdo gerado pelo pipeline está, na prática, no limiar da indistinguibilidade perceptual humana.

---

## 6. Resultados

O experimento com 25 personas geradas produziu os seguintes resultados mensuráveis:

| Métrica | Resultado | Interpretação |
|---|---|---|
| Personas geradas | 25 | Volume suficiente para análise estatística inicial |
| Taxa média de acerto | 55% | 5% acima do acaso — conteúdo próximo ao limiar de detecção |
| Modalidades por persona | 3 (texto + imagem + áudio) | Cobertura multimodal completa |
| Engenharia de prompt | Few-shot + Role + Meta | Pipeline de prompt em três camadas |

**Insight crítico do experimento:**

> Mesmo prestando atenção deliberada, humanos classificam incorretamente quase metade das personas geradas por IA. Esse dado quantifica, com um experimento real, o risco que ferramentas de detecção, watermarking e governança de IA precisam mitigar.

A implicação prática: sistemas de moderação de conteúdo que dependem exclusivamente de avaliação humana têm uma taxa de erro inerente de ~45% para conteúdo multimodal gerado por LLMs e modelos de imagem de última geração.

---

## 7. Próximos Passos

- [ ] Aumentar o dataset para 200+ personas e aplicar **teste de significância estatística** (teste t) para confirmar se a taxa de 55% é distinguível do acaso com confiança de 95%
- [ ] Implementar **pipeline de classificação automática** com ML — treinar um modelo binário (Natty/Fake) usando as personas geradas como dataset sintético e embeddings de texto/imagem como features
- [ ] Adicionar **watermarking imperceptível** nas imagens geradas (C2PA / SynthID) para rastrear conteúdo sintético mesmo após edição
- [ ] Construir **dashboard analítico** com segmentação por modalidade: qual modalidade (texto, imagem, áudio) tem maior poder de engano isoladamente?
- [ ] Publicar o experimento como **artigo técnico** com metodologia, dados e análise estatística — contribuição para o debate de governança de IA generativa

---

## 🗂️ Estrutura do Projeto

```
natty-or-not/
├── projetNattyOrNot.md    # Código base: few-shot prompts + endpoints Flask + esqueleto da app
├── otimizaPrompt.md       # Guia de engenharia de prompt por modalidade (texto, imagem, áudio)
└── README.md              # Contexto, estratégia e análise do experimento
```

---

## 🛠️ Stack Tecnológica

| Camada | Tecnologia | Função |
|---|---|---|
| LLM — Texto | GPT-4 (OpenAI) | Geração de biografias com few-shot prompting |
| Imagem | DALL·E 3 (OpenAI) | Retratos hiper-realistas com metadados fotográficos |
| Áudio | ElevenLabs TTS | Síntese de voz com tom e emoção configuráveis |
| Backend | Flask (Python) | Endpoints de geração e orquestração do pipeline |
| Frontend | React | Interface de quiz com exibição multimodal |
| Dados | MongoDB | Armazenamento de interações e métricas de comportamento |

---

## ▶️ Como Executar

**Pré-requisitos:** Python 3.10+, Node.js 18+, chaves de API (OpenAI, ElevenLabs), MongoDB.

```bash
# Clone o repositório
git clone https://github.com/Santosdevbjj/natty-Or-Not.git
cd natty-Or-Not

# Backend Flask
pip install flask openai pymongo
python app.py

# Frontend React (em outro terminal)
cd frontend
npm install
npm start
```

Configure as variáveis de ambiente:

```bash
export OPENAI_API_KEY="sua-chave-openai"
export ELEVENLABS_API_KEY="sua-chave-elevenlabs"
export MONGODB_URI="sua-connection-string"
```

---

## 📐 Decisões Técnicas

**Por que three endpoints independentes (bio, imagem, áudio) em vez de um endpoint único?**
Um endpoint único que chama as três APIs em sequência cria um ponto de falha único — se a geração de imagem falhar, a bio já gerada é descartada. Com endpoints independentes, cada modalidade pode ser regenerada individualmente sem reprocessar as outras. Isso também facilita A/B testing: substituir ElevenLabs por outro provedor de TTS sem tocar no pipeline de texto e imagem.

**Por que MongoDB e não PostgreSQL para armazenar as interações?**
Cada interação tem uma estrutura parcialmente variável (uma sessão pode ter 3 perguntas, outra pode ter 10). MongoDB acomoda esse schema flexível sem necessidade de migrations. Para a fase de análise, os dados são exportados para Pandas — a flexibilidade do document store supera a vantagem de queries relacionais nesse contexto.

**Por que few-shot e não apenas um prompt de sistema bem escrito?**
Prompts de sistema definem comportamento geral. Few-shot define o **nível de qualidade esperado** com exemplos concretos. A diferença prática: sem exemplos, o GPT-4 produz bios com estrutura previsível e vocabulário genérico (detectável). Com exemplos de bios reais como referência, o modelo calibra especificidade, variação estilística e nível de detalhe — os elementos que tornam uma narrativa crível.

---

## 🧠 Aprendizados

O aprendizado mais importante foi entender que **o realismo de conteúdo gerado por IA não é uma propriedade do modelo — é uma propriedade do prompt**. O mesmo GPT-4 produz resultados que vão de obviamente sintéticos a praticamente indistinguíveis do humano dependendo exclusivamente de como a instrução é estruturada.

O insight mais contraintuitivo foi sobre imagens: a instrução técnica fotográfica (`50mm lens, f/2.0, ISO 200`) produz resultados mais realistas que o termo `photorealistic`, porque força o modelo a simular as condições físicas de uma câmera real em vez de interpretar um estilo artístico.

O que faria diferente desde o início: estruturaria a coleta de métricas com granularidade por modalidade desde a primeira persona gerada. Saber que a taxa geral é 55% é útil, mas saber que usuários acertam 70% no texto e erram 60% na imagem seria muito mais acionável para guiar as próximas iterações do experimento.

---

## 🤝 Contribuição

Sugestões de novos tipos de persona, técnicas de prompt alternativas ou extensões do pipeline de detecção são bem-vindas. Abra uma issue ou envie um pull request.

---

## 📬 Contato

[![Portfólio](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)
[![GitHub](https://img.shields.io/badge/GitHub-Santosdevbjj-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Santosdevbjj)
