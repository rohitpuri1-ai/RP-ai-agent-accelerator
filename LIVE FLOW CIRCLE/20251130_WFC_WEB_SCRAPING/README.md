# Web-Scraping 101

# 📘 Basic Scraper Automations (DSM Edition)

This README explains the three simple web-scraping workflows used in n8n.  
Each automation follows the same basic flow:

1. User provides an input  
2. n8n sends the input to an Apify scraper  
3. The scraper returns results  
4. The automation stores each item as a new row in Google Sheets  

No AI agents are used in these automations.

---

# 🔑 How to Get Your Apify API Token

Follow these steps to use any Apify scraper in n8n:

1. Go to **https://console.apify.com**
2. Sign up or log in
3. Go to settings
4. click on API & Integrations
5. Under **API Tokens**, click **Create new token**
6. Give it a name (example: *DSM Training Token*)
7. Copy the token and save it safely  
8. Paste the token inside the **Apify Actor** node in n8n under:
   - **Authentication → API Token**

You now have full access to run Apify scrapers inside n8n.

---

# 🗺️ 1) Google Maps Scraper

### ✔ What it does  
Takes a **search keyword + location** and returns business listings from Google Maps.

### ✔ Example inputs  
Try any of these:

- “software companies in Bengaluru”
- “cafes in London”
- “dentists in New York”
- “gyms in Dubai”
- “marketing agencies in Toronto”

### ✔ Output fields (basic)
- Title  
- Address  
- City  
- Website  
- Email (when available)  
- Phone (when available)

### ✔ Flow  
1. User submits a form  
2. Scraper runs with the keyword + location  
3. Results are appended to Google Sheets  

---

# 🧳 2) TripAdvisor Scraper

### ✔ What it does  
Takes a **city name** and returns hotels, restaurants, and attractions.

### ✔ Example inputs  
- “Shimla”
- “Singapore”
- “Bangkok”
- “New York restaurants”
- “Tokyo attractions”

### ✔ Output fields (basic)
- Type (Hotel / Restaurant / Attraction)
- Name
- Address
- Rating
- Phone
- Email (when available)
- Website
- Number of reviews
- Price range (when available)

### ✔ Flow  
1. User types the city name in chat  
2. Scraper runs  
3. All fetched listings are saved into the sheet  

---

# ▶️ 3) YouTube Channel Scraper

### ✔ What it does  
Takes a **YouTube channel name** and returns the latest videos.

### ✔ Example inputs  
- “Autocar India”
- “MrBeast”
- “Ali Abdaal”
- “TEDx Talks”
- “NPR Music”

### ✔ Output fields (basic)
- Video title  
- Link  
- Views  
- Likes  
- Comments  
- Channel name  
- Channel URL  
- Location (when available)

### ✔ Flow  
1. User sends a channel name in chat  
2. Scraper runs  
3. Videos are appended to the sheet  

---

# ✔ Tips for Students

- Use simple, clear inputs for best results  
- Some scrapers return different fields depending on available public data  
- Always prepare sheet headers before running the workflow  
- If data is incomplete, try a more specific keyword  
- Avoid spelling mistakes  

---

# 📦 Files Provided

- `[COMPLETED] Google Maps Scraper.json`
- `[COMPLETED] Trip Advisor Scraper.json`
- `[COMPLETED] YouTube Scraper.json`
- `README.md` (this file)

