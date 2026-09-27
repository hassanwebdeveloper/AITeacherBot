# PROJECT.md — AI Teacher Bot — Release 1

## 1. Product

**AI Teacher Bot** is an AI-powered virtual classroom where an animated AI teacher automatically conducts lessons from uploaded course material.

The goal of Release 1 is to deliver a complete classroom loop:

**Course Material → Course Understanding → Teaching Plan → AI Teacher → Avatar Teaching → Student Q&A → Teacher Questions → Answer Evaluation → Progress Tracking → Adaptive Explanation → Continue Teaching**

The AI teacher should behave like an interactive teacher rather than a simple chatbot. It must teach proactively, listen to students, ask questions, evaluate understanding, and adjust explanations when necessary.

---

## 2. Users

### Student
- Enters an AI-powered classroom.
- Watches/listens to the avatar teacher.
- Views lesson/presentation content.
- Asks questions using voice.
- Answers questions asked by the AI teacher.
- Receives explanations and feedback.
- Has learning progress tracked during the course.

### Course Admin
- Uploads course material.
- Provides course documents/presentations used by the AI teacher.
- Starts/manages courses and classroom sessions.

### Admin
- Create users for Course admin or student.

Release 1 does **not** focus on advanced admin, examination, assignment, or institutional workflows.

---

## 3. Core Capabilities

### A. Course Understanding & Preparation
- Upload course documents/presentations.
- Parse and extract course structure.
- Identify modules, topics, subtopics, concepts and learning objectives.
- Organize content into teachable lessons.
- Generate lesson sequence and teaching plans.
- Prepare explanations, examples and teacher questions.

### B. Automated AI Teaching
- Start and conduct lessons automatically.
- Explain concepts step-by-step.
- Use course material as the primary knowledge source.
- Provide examples and alternative explanations.
- Maintain lesson and classroom context.
- Present/synchronize relevant lesson content.
- Summarize lessons/topics.

### C. Student Interaction
- Student can ask questions during teaching.
- AI answers using course knowledge and current lesson context.
- Support follow-up questions.
- Maintain conversational context.
- Support voice-based interaction.

### D. Teacher Questions & Evaluation
- AI proactively asks students questions.
- Evaluate student responses.
- Identify:
  - Correct understanding
  - Partial understanding
  - Incorrect understanding
  - Knowledge gaps
  - Possible misconceptions
- Provide appropriate feedback/re-explanation.

### E. Progress Tracking & Basic Adaptation
Track:
- Topics covered
- Questions asked/answered
- Student responses
- Understanding level
- Difficult concepts
- Concepts requiring re-explanation

Basic learning loop:

**Teach → Ask → Student Answers → Evaluate → Identify Gap → Explain/Clarify → Continue**

### F. Avatar Classroom
- NVIDIA **Audio2Face (A2F)** for facial/lip animation.
- **Three.js** for browser-based 3D avatar/classroom rendering.
- TTS for AI teacher speech.
- STT for student speech.
- Speaking/listening/thinking states.
- Basic interruption and turn management.
- Synchronize avatar speech with generated audio.

---

## 4. Tech Stack

### Frontend
- React
- TypeScript
- Three.js
- WebSocket/WebRTC where appropriate
- Browser microphone/audio APIs

### Backend
- Python
- FastAPI
- WebSocket-based real-time communication
- Agent/workflow orchestration
- REST APIs for course, classroom and student state

### Database & Storage
- MongoDB — application/course/student/classroom data
- Qdrant — vector search/RAG
- Redis — caching, temporary state and real-time coordination
- Object storage for uploaded course material and generated assets

### AI / Models
- Production LLM such as **Qwen3-class or equivalent**, selected based on accuracy, latency and deployment requirements.
- Faster-Whisper / Whisper-class model — STT.
- Chatterbox TTS or equivalent — TTS.
- NVIDIA Audio2Face — avatar facial/lip animation.
- Embedding model + Qdrant — course knowledge retrieval.
- OCR/document parsing where required.

### Infrastructure
- Docker / Docker Compose
- NVIDIA GPU server for AI workloads
- Linux-based deployment
- FastAPI serving backend APIs
- Three.js classroom served through the frontend
- Separate AI/model services where required

---

## 5. Architecture Constraints

- **Course material is the primary source of truth** for teaching and student Q&A.
- AI responses should use RAG/course context rather than relying only on general LLM knowledge.
- Keep AI teaching/orchestration independent from the avatar/rendering layer.
- A2F is responsible for facial/lip animation; Three.js is responsible for browser rendering.
- Real-time classroom communication should use WebSocket/WebRTC where appropriate.
- Services should be containerized and independently deployable.
- Design the architecture so additional classrooms/users can scale horizontally.
- Avoid tightly coupling model implementations to business logic; models must be replaceable.
- Release 1 should prioritize a reliable end-to-end classroom experience over advanced platform features.

---

## 6. Important Business Rules / Constraints

- AI teacher must **teach proactively**; it must not behave only as a question-answer chatbot.
- Student questions must be answered in the context of the current course/lesson whenever relevant.
- AI teacher should periodically ask questions to verify understanding.
- Student answers must be evaluated before deciding whether to continue or re-explain.
- When a knowledge gap or misconception is detected, the teacher should provide clarification/re-explanation before continuing where appropriate.
- Student progress must persist across the classroom session.
- Formal quizzes, assignments, midterms, finals, advanced grading, labs, projects and advanced analytics are **Release 2**, not Release 1.
- Infrastructure/API/model usage costs are separate from development scope.
- Final GPU sizing must be validated through real latency and concurrent-classroom benchmarking.

---

## 7. Current Status

Release 1 scope and architecture have been defined. Development is planned across **9 weeks**:

- Weeks 1–2: Course & Teaching Preparation
- Weeks 3–4: Automated AI Teacher
- Weeks 5–6: Student Interaction & Evaluation
- Weeks 7–8: Avatar Classroom
- Week 9: End-to-End Integration & Pilot

**Release 1 Definition of Done:** A student can enter a classroom, receive an automated avatar-led lesson, ask questions, answer teacher questions, have responses evaluated, have progress updated, and receive adaptive explanations before the lesson continues.
