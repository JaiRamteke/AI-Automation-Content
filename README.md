# n8n YouTube AI Automation Workflow

## Overview
This n8n workflow automates YouTube content planning:
- Fetches video titles from YouTube.
- Generates placeholder blog content, SEO keywords, and video prompts.
- Populates a Google Sheet with topic, content prompts, timestamp, and status.
- Processes exactly 3 videos per workflow execution.

## Workflow Steps
1. YouTube Node:
   - Fetches video metadata including title and description.

2. Function Node (Prepare Content Items):
   - Extracts first 3 video titles.
   - Generates placeholders:
     - blog_prompt: "Write a blog about {video title}"
     - seo_keywords: "AI, n8n, Automation"
     - video_prompt: "Create a video about {video title}"
   - Adds timestamp and status "Pending Review".
   - Outputs JSON items for Google Sheets node.

   Example Function Node Script:
   --------------------------------
   const items = [];
   const allVideos = $json.items || [];

   for (const video of allVideos.slice(0, 3)) {
     const title = video?.snippet?.title || "Unknown Topic";

     items.push({
       json: {
         topic: title,
         blog_prompt: `Write a blog about ${title}`,
         seo_keywords: "AI, n8n, Automation",
         video_prompt: `Create a video about ${title}`,
         timestamp: new Date().toISOString(),
         status: "Pending Review"
       }
     });
   }

   return items;
   --------------------------------

3. Basic LLM Chain Node (Optional):
   - Generates actual blog content, SEO keywords, and video prompts using AI.
   - Can replace placeholders from the Function node.

4. Google Sheets Node:
   - Appends the JSON items into the sheet with columns:
     Topic, Blog Prompt, SEO Keywords, Video Prompt / Job, Timestamp, Status

## Google Sheet Columns
- Topic: YouTube video title
- Blog Prompt: Placeholder or AI-generated content
- SEO Keywords: Placeholder or AI-generated keywords
- Video Prompt / Job: Placeholder or AI-generated video prompt
- Timestamp: ISO timestamp
- Status: "Pending Review"

## Setup Instructions
1. Connect YouTube node with desired channel or playlist.
2. Add Function node using the provided script.
3. (Optional) Add AI node to generate content.
4. Configure Google Sheets node to map JSON fields to columns.
5. Run workflow and verify first 3 videos are appended.

## Notes
- Adjust `.slice(0, 3)` to change number of videos processed.
- Replace placeholders with AI-generated content for full automation.
- Ensure Google Sheet columns match the JSON fields exactly.
'
