# LLM-training-project-n8n
My Training project on slides generation and sharing using n8n

# AI-Powered Google Slides Presentation Generator
An intelligent n8n workflow that automatically creates professional Google Slides presentations from natural language prompts using OpenAI GPT-4o-mini.

# 🎯 Overview
This workflow transforms user text input into fully-formatted Google Slides presentations with custom styling, bullet points, and automated content generation. Simply describe what presentation you need, and the AI will generate a complete slide deck with titles, subtitles, and detailed content.

# ✨ Features

- __Natural Language Input__: Describe your presentation topic in plain English
- __AI-Powered Content Generation__: GPT-4o-mini creates structured slide content
- __Automated Slide Creation__: Programmatically builds slides with custom layouts
- __Professional Styling__: Consistent fonts, colors, and formatting
- __Smart File Naming__: AI generates contextual presentation titles
- __Public Sharing__: Automatically sets presentation to "writer" access for anyone
- __Batch Processing__: Efficiently creates multiple slides in sequence

  # 🏗️ Architecture
# High-Level Architecture
┌────────────────────────┐
│ 💬 User Input          │
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 💾 File Name Generation│
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 🖼️ Create Presentation │
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 🌐 Share Presentation  │
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 🧠 Generate Content    │
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 🧩 Parse JSON Output   │
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 🔄 Loop & Create Slides│
└────────┬───────────────┘
         │
         ▼
┌────────────────────────┐
│ 🎨 Format Slides       │
└────────────────────────┘



  
