# AI LinkedIn Post Generator with Image

This workflow automatically generates engaging LinkedIn posts with custom AI-generated images using n8n's AI Agent Tools.  
Perfect for content creators, marketers, and professionals who want to maintain consistent LinkedIn presence with high-quality, thoughtful posts.

The pipeline consists of:
1. **AI Agent** - Generates the LinkedIn post content and image prompt
2. **Image Generator** - Creates a visual based on the post theme
3. **LinkedIn Integration** - Publishes the complete post automatically

---

## How It Works

### 1. Trigger
- Workflow activates when a chat message is received
- User provides a topic or title for the LinkedIn post

### 2. AI Agent (Post Generator)
- Takes the topic/title as input
- Generates a complete LinkedIn post with:
  - Strong opening hook
  - 4 concise paragraphs (2-3 sentences each)
  - Thought-provoking closing question
  - 4-6 relevant hashtags
  - Strategic emoji usage
- Also creates an image prompt for visual content

### 3. Image Generation
- Uses the AI-generated image prompt
- Creates a custom image that visually represents the post's main theme
- Image is optimized for LinkedIn's format

### 4. LinkedIn Publishing
- Combines the generated post text with the image
- Publishes directly to LinkedIn
- Post goes live automatically

---

## System Prompt
```
You are a professional LinkedIn post writer who creates engaging, insightful content.

You will be given a topic or title. Your task is to create:

1. A LinkedIn post that:
   - Starts with a strong, attention-grabbing hook
   - Contains exactly 4 short paragraphs, each 2-3 sentences long
   - Breaks down key insights or challenges common assumptions
   - Ends with an engaging question to spark discussion
   - Includes 4-6 relevant hashtags
   - Uses emojis sparingly for emphasis

2. A concise image_prompt (under 30 words) that visually represents the main idea of the post

Formatting rules:
- Insert newline characters (\n\n) between paragraphs
- Properly escape all text for JSON
- Avoid quotes and special characters that could break JSON
- Keep total post under 3000 characters

Return as a single clean JSON object only, exactly in this format (no markdown, no backticks):
{"post": "your linkedin post content here", "image_prompt": "your short image prompt here"}
```

Please paste this into the output parser:
```
{
	"post": "linkedin post",
	"prompt": "image prompt"
}
```

---

## Example Usage

**Input:**
```
"The hidden cost of technical debt in startups"
```

**Generated Output:**
```json
{
  "post": "Technical debt doesn't show up on your balance sheet, but it's bleeding your startup dry.\n\nMost founders celebrate shipping fast. They should be asking what breaks when they scale. Every shortcut today becomes a roadblock tomorrow, and those roadblocks compound faster than you think.\n\nYour engineering team spends 40% of their time on maintenance instead of innovation. That's not a tech problem, that's a business problem. While competitors build features, you're fixing foundations.\n\nThe companies that win aren't the ones who move fastest early. They're the ones who can still move fast in year three. Clean code isn't perfectionism, it's competitive advantage.\n\nHow much is technical debt actually costing your company? 💭\n\n#TechnicalDebt #StartupStrategy #EngineeringLeadership #ProductDevelopment #ScalingStartups #TechManagement",
  
  "image_prompt": "Crumbling foundation with shiny new building on top, cracks spreading, money falling through gaps"
}
```

---

## Configuration Requirements

### Required Credentials
1. **OpenAI API Key** (or compatible AI model)
   - For post generation
   - Model: GPT-4 or Claude recommended

2. **Image Generation API**
   - Gemini or similar image generation service
   - Configured in the "Generate an image" node

3. **LinkedIn API Access**
   - OAuth credentials for posting
   - Configured in the "Create a post" node

### Node Configuration

#### AI Agent Node
- **Model**: OpenAI Chat Model (or equivalent)
- **Output Parser**: Structured Output Parser (JSON)
- **System Prompt**: Use the prompt provided above

#### Image Generator Node
- **Service**: Gemini or preferred image generation API
- **Input**: Uses `image_prompt` from AI agent output

#### LinkedIn Node
- **Action**: Create a post
- **Content**: Uses `post` from AI agent output
- **Image**: Attached from image generator output

---

## Workflow Diagram
```
Chat Input → AI Agent → Image Generator → LinkedIn Post
                ↓              ↓
           (Generates      (Creates
            post text +     visual
            image prompt)   content)
```

---

## Best Use Cases

- Regular LinkedIn content scheduling
- Thought leadership posts
- Industry insights and commentary
- Professional storytelling
- Company announcements with visuals
- Educational content sharing

---

## Customization Options

### Adjust Post Style
Modify the system prompt to change:
- Tone (professional, casual, technical, inspirational)
- Length (shorter/longer paragraphs)
- Hashtag count
- Emoji usage
- Question style

### Change Image Style
Modify image generation parameters:
- Art style (realistic, abstract, minimalist)
- Color scheme
- Composition
- Aspect ratio for LinkedIn

### Add Scheduling
Integrate with:
- Calendar triggers for automated posting
- Buffer/Hootsuite for queue management
- Time zone optimization

---

## Performance Characteristics

- **AI Generation**: ~5-10 seconds
- **Image Creation**: ~10-20 seconds
- **LinkedIn Publishing**: ~2-5 seconds
- **Total Time**: ~20-35 seconds per post
- **Success Rate**: High (with proper error handling)

---

## Error Handling

The workflow includes graceful degradation:
- If image generation fails, post is published with text only
- If LinkedIn publishing fails, content is saved for manual posting
- All errors are logged for troubleshooting

---

## Limitations

- Requires active API keys and valid credentials
- LinkedIn API rate limits apply (check LinkedIn's developer docs)
- Image generation quality depends on prompt clarity
- Post character limit: 3000 characters (LinkedIn's limit)

---

## Tips for Best Results

1. **Clear Topics**: Provide specific, focused topics rather than broad themes
2. **Review Before Publishing**: Add a manual approval step if desired
3. **A/B Testing**: Generate multiple versions and compare performance
4. **Engagement Tracking**: Monitor which post styles perform best
5. **Consistent Scheduling**: Use regularly for maximum impact

---

## Future Enhancements

Potential additions:
- Multi-platform posting (Twitter, Medium, etc.)
- Content calendar integration
- Performance analytics dashboard
- A/B testing automation
- Audience targeting customization

---

## Version

v1.0 — Built for n8n AI Agent Tools with LinkedIn integration

---

## Support

For issues or questions:
- Check n8n documentation for node-specific problems
- Verify API credentials and permissions
- Review LinkedIn's API guidelines
- Test with simple topics first before complex ones