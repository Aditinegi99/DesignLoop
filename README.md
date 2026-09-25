# 📐 DesignLoop — LLD Practice Platform

Practice Low-Level Design problems and actually find out if your design is any good, instead of just... hoping it is.


# 🤔 What is this, actually?

LLD problems (Parking Lot, Elevator System, Vending Machine, etc.) are easy to attempt and really hard to grade yourself on. There's rarely one "correct" answer, so how do you know if your class split is actually good, or if you just talked yourself into it?

DesignLoop gives you a small loop to practice this properly:

pick a problem → write your design → submit → get feedback → see what to fix → try again

The twist is in the feedback: every score is labeled either checked (a mechanical, deterministic check, did you include the right entities, is one class doing everything) or ai judgment (an actual LLM reasoning about your trade-offs and responsibility split). You always know which is which, instead of just trusting a mystery number. 🎯

# ✨ Features
- 📋 3 real LLD problems with proper requirements + constraints, not toy examples
- ✍️ Submit a design rationale + code/pseudocode stubs
- ✅🤖 Feedback split between deterministic checks and AI judgment, clearly tagged
- 💬 A few "worth thinking about" follow-up questions on every submission, not just a score, something to chew on
- 🔁 Resubmit as many times as you want under the same attempt, and watch your scores actually move
- 📊 Full history across every problem you've tried
- 🛟 If the AI call fails or times out, you still get your deterministic feedback, never a blank screen

# 🛠️ Built with

Backend: FastAPI · SQLAlchemy · SQLite · Groq (Llama 3.3 70B) for the judgment-based scoring Frontend: React · Vite · Tailwind CSS Deployed on: Render (API) + Vercel (frontend)
