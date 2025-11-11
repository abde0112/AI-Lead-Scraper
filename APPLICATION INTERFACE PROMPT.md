Google AI Studio Prompt
You are an AI Lead Generation Assistant. Create a web-based application with the following features:

# APPLICATION INTERFACE

## Input Form with Smart Features:

1. **Business Type Input** (with autocomplete)
   - Text input field with autocomplete dropdown
   - As user types, show matching suggestions
   - Common business types: Restaurant, Plumber, Electrician, Dentist, Lawyer, Marketing Agency, Coffee Shop, Gym, Hair Salon, Bakery, Car Repair, Real Estate Agent, Accountant, Photographer, Web Designer, Consultant, etc.
   - Match suggestions starting with typed text (e.g., "rest" shows "Restaurant", "Restobar")

2. **City Input** (with autocomplete)
   - Text input field with autocomplete dropdown
   - Show cities starting with typed text
   - Common cities: Casablanca, Casa, New York, Los Angeles, London, Paris, Tokyo, Dubai, Sydney, Toronto, Berlin, Madrid, Rome, Amsterdam, Barcelona, etc.
   - Match "casa" to show "Casablanca", "Casa Grande", "Casale", etc.

3. **Country Input** (with autocomplete)
   - Text input field with autocomplete dropdown
   - Show countries starting with typed text
   - Countries: USA, Morocco, UK, France, Germany, Spain, Italy, Canada, Australia, Japan, UAE, Netherlands, etc.

4. **Number of Leads** (flexible number input)
   - Number input field that accepts ANY number from 1 to 100
   - User can type custom numbers like: 11, 15, 23, 47, 99, etc.
   - Include +/- buttons for easy increment/decrement
   - Default value: 10
   - Min: 1, Max: 100

## Search Button:
- Large, prominent "Generate Leads" button
- Shows loading state when searching

# DATA COLLECTION & SCRAPING

When user clicks "Generate Leads":
1. Use Google Maps API or search to find businesses matching the criteria
2. Collect EXACTLY the number of leads requested by user (if user enters 11, return 11 leads)
3. For each business, extract:

## Required Data Fields:
- GeneratedDate: Current date in YYYY-MM-DD format
- SearchCity: The city user searched for
- SearchCountry: The country user searched for
- LeadNumber: Sequential number (1, 2, 3... up to requested count)
- CompanyName: Business name
- Category: Business category/type
- Description: Business description from Google Maps
- Address: Full street address
- City: Business city
- Country: Business country
- Coordinates: Latitude,Longitude (format: "40.7128,-74.0060")
- MapLink: Google Maps URL in this format:
  https://www.google.com/maps/search/?api=1&query=[CompanyName]+[FullAddress]
  Example: https://www.google.com/maps/search/?api=1&query=Gramercy+Tavern+42+E+20th+St+New+York+NY+10003+USA
  (Replace all spaces with + signs)
- Phone: Phone number with country code
- Email: Email address (extract if available, otherwise empty string "")
- Website: Website URL (extract if available, otherwise empty string "")
- LinkedIn: LinkedIn URL (if available, otherwise empty string "")
- Facebook: Facebook URL (if available, otherwise empty string "")
- Instagram: Instagram URL (if available, otherwise empty string "")
- Rating: Google rating (e.g., 4.5)
- ReviewCount: Number of reviews (e.g., 1234)
- BusinessHours: Full business hours string
- QualityScore: Calculate score 0-100 based on:
  * Has phone: +20 points
  * Has email: +10 points
  * Has website: +15 points
  * Has social media: +10 points
  * Rating > 4.0: +15 points
  * ReviewCount > 100: +15 points
  * Complete business hours: +15 points
- QualityReasoning: Short explanation of quality score
- Status: Always "New"
- Contacted: Always "No"
- Notes: Empty string ""

# OUTPUT FORMAT

Return results as JSON:
```json
{
  "SearchCity": "user's city input",
  "SearchCountry": "user's country input",
  "leads": [
    {
      "GeneratedDate": "2025-11-11",
      "SearchCity": "Casablanca",
      "SearchCountry": "Morocco",
      "LeadNumber": 1,
      "CompanyName": "Business Name",
      "Category": "Restaurant",
      "Description": "Description text",
      "Address": "Full address",
      "City": "Casablanca",
      "Country": "Morocco",
      "Coordinates": "33.5731,-7.5898",
      "MapLink": "https://www.google.com/maps/search/?api=1&query=...",
      "Phone": "+212522123456",
      "Email": "contact@business.com",
      "Website": "https://website.com",
      "LinkedIn": "",
      "Facebook": "https://facebook.com/business",
      "Instagram": "",
      "Rating": 4.5,
      "ReviewCount": 234,
      "BusinessHours": "Monday: 9:00 AM - 10:00 PM, Tuesday: ...",
      "QualityScore": 75,
      "QualityReasoning": "High quality lead with complete contact info and good reviews",
      "Status": "New",
      "Contacted": "No",
      "Notes": ""
    }
  ]
}
```

# DISPLAY RESULTS

Show results in a modern table with these columns:
- LEAD # (yellow badge)
- COMPANY NAME (bold)
- CATEGORY (gray text)
- RATING (gold badge with star ⭐)
- REVIEWS (gray text, show count)
- PHONE (blue clickable link)
- WEBSITE (blue button "🔗 Visit" or "-" if empty)
- MAP (green button "📍 View on Maps")

# WEBHOOK INTEGRATION

Provide these buttons after results:
1. "Download JSON" - Downloads the results as JSON file
2. Webhook URL display field (shows: http://localhost:5678/webhook/lead-scraper)
3. "Send to Webhook" - POST the JSON to the webhook URL

# UI STYLING

Use dark theme:
- Background: #0f172a
- Cards: #1e293b
- Text: #f8fafc
- Accent: #6366f1 (purple/blue)
- Success: #10b981 (green)
- Warning: #fbbf24 (yellow/gold)
- Modern, rounded corners, smooth animations

# IMPORTANT RULES

1. NEVER use placeholder data - always perform real searches
2. MapLink must ALWAYS be generated - never empty or "-"
3. Return EXACTLY the number of leads user requested (11 leads = 11 results)
4. Autocomplete must be case-insensitive
5. Autocomplete shows results as user types (minimum 2 characters)
6. Number input accepts any value from 1-100, not just increments of 10
7. Phone numbers must include country code
8. All URLs must be complete and valid
9. Quality Score must be calculated accurately
10. Do NOT include Quality Score in the display table (only in JSON for webhook)
