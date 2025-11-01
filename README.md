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

# 🔄 Workflow Breakdown

__1. Chat Trigger Node__
-	__Type__: @n8n/n8n-nodes-langchain.chatTrigger
-	__Purpose__: Receives user input via chat interface
-	__Configuration__: Allows file uploads (though not utilized in current flow)
  
__2. File Name Generation__
-	__Type__: @n8n/n8n-nodes-langchain.openAi
-	__Model__: GPT-4o-mini
-	__Purpose__: Creates contextual file names based on user input
-	__Example__: Input "how to install wordpress" → Output "Presentation_2025-10-31_How to Install Wordpress"
-	__System Prompt__: Acts as a file name generator with specific formatting rules
  
__3. Create Presentation__
-	__Type__: n8n-nodes-base.googleSlides
-	__Purpose__: Creates empty Google Slides presentation
-	__Output__: Returns presentationId for subsequent operations
  
__4. Share File__
-	__Type__: n8n-nodes-base.googleDrive
-	__Purpose__: Sets presentation permissions
-	__Configuration__: 
1.	__Role__: Writer
2.	__Type__: Anyone (public access)
   
__5. Edit Fields (Configuration)__
-	__Type__: n8n-nodes-base.set
-	__Purpose__: Sets workflow parameters
-	__Configuration__: total_slides: 5
  
__6. AI Agent (Content Generation)__
-	__Type__: @n8n/n8n-nodes-langchain.agent
-	__Model__: GPT-4o-mini
-	__Purpose__: Generates structured slide content
-	__Output Format__:
json
[
  {
    "title": "Slide Title",
    "subtitle": "Slide Subtitle",
    "bullets": ["Point 1", "Point 2", "Point 3", "Point 4", "Point 5"]
  }
]
-	__Requirements__: 5 detailed bullet points per slide
  
__7. Parse JSON__
-	__Type__: n8n-nodes-base.code
-	__Purpose__: Extracts and validates JSON from AI output
-	__Features__: 
1.	Strips markdown code fences
2.	Handles malformed output
3.  Extracts JSON arrays
4.	Returns one item per slide
   
__8. Loop Over Items__
-	__Type__: n8n-nodes-base.splitInBatches
-	__Purpose__: Processes slides sequentially
-	__Batch Size__: Dynamic (based on total_slides parameter)
  
__9. Slides Layout__
-	__Type__: n8n-nodes-base.set
-	__Purpose__: Prepares slide element data
-	__Configuration__: 
1.	Logo text: "@nikhil"
2.	Unique IDs for each element per slide
3.	Text formatting for bullets
   
__10. HTTP Request (Batch Update)__
-	__Type__: n8n-nodes-base.httpRequest
-	__API__: Google Slides API v1 batchUpdate
-	__Purpose__: Creates and formats slide elements
-	__Operations Per Slide__: 
1.	Create blank slide
2.	Add logo text box
3.	Add title text box
4.	Add subtitle text box
5.	Add bullet points text box
6.	Apply formatting and styling
   
__11. Wait Node__
-	__Type__: n8n-nodes-base.wait
-	__Purpose__: Prevents API rate limiting between slide creations
  
# 🎨 Slide Styling Specifications
__Logo__
-	__Font__: Open Sans, 15pt
-	__Color__: RGB(0.73, 0.75, 0.78) - Light Gray
-	__Position__: 32pt from left, 24pt from top
-	__Size__: 400pt × 36pt
  
__Title__
-	__Font__: Roboto, 35pt, Bold
-	__Color__: RGB(0.95, 0.35, 0.15) - Orange-Red
-	__Position__: 32pt from left, 96pt from top
-	__Size__: 600pt × 80pt
  
__Subtitle__
-	__Font__: Open Sans, 25pt, Bold
-	__Color__: RGB(0.20, 0.20, 0.20) - Dark Gray
-	__Position__: 32pt from left, 168pt from top
-	__Size__: 600pt × 60pt
  
__Bullet Points__
-	__Font__: Open Sans, 20pt
-	__Color__: RGB(0.20, 0.20, 0.20) - Dark Gray
-	__Position__: 32pt from left, 240pt from top
-	__Size__: 700pt × 280pt
-	__Style__: Disc-Circle-Square bullets
-	__Line Spacing__: 140%
-	__Indent__: 18pt



  
