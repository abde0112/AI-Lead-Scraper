# AI-Lead-Scraper
# prompt 1 
Create a professional lead generation application with the following specifications:

=== INPUT INTERFACE ===
Create a clean form with these fields:
1. Search Query (text input) - Business type (e.g., "plumbers", "restaurants", "dentists")
2. City (text input) - Target city name
3. Country (text input) - Target country name
4. Number of Leads (dropdown selector) - Options: 10, 20, 30, 40, 50, 60, 70, 80, 90, 100

Add a prominent "Generate Leads" button.

=== FUNCTIONALITY ===

STEP 1 - SEARCH WITH GOOGLE MAPS:
When user clicks "Generate Leads", use Google Maps to search for:
"[Search Query] in [City], [Country]"

Request up to the number of results specified by the user.
Show a progress indicator: "Searching Google Maps..."

STEP 2 - DATA EXTRACTION:
For EACH business found by Google Maps, extract ALL available information:

REQUIRED DATA POINTS:
- Company/Business Name
- Business Category/Type
- Business Description or Summary
- Complete Street Address
- City name (extract from address)
- Country name (extract from address)
- GPS Coordinates (latitude, longitude) - Google Maps ALWAYS provides these
- Phone Number
- Email Address (check business description and contact info)
- Website URL (most businesses have one in Maps)
- LinkedIn profile URL (if available)
- Facebook page URL (if available)
- Instagram profile URL (if available)
- Star Rating (out of 5)
- Total Number of Reviews
- Business Operating Hours
- Price Level (if available)

STEP 3 - GENERATE GOOGLE MAPS LINKS:
For each business, create a direct Google Maps link using coordinates:
Format: https://www.google.com/maps/search/?api=1&query=[LATITUDE],[LONGITUDE]
Example: https://www.google.com/maps/search/?api=1&query=40.7388,-73.9874

STEP 4 - QUALITY SCORING:
Calculate a Quality Score (0-100) for each lead based on:
- Has phone number: +20 points
- Has email address: +20 points
- Has website: +20 points
- Rating 4.0 or higher: +10 points
- Has 10+ reviews: +10 points
- Has at least one social media link: +10 points
- Has complete business hours: +10 points

Provide Quality Reasoning (1-2 sentences explaining the score).

STEP 5 - DISPLAY RESULTS:
Show results in a professional table with these columns:
- Lead # (sequential number)
- Company Name
- Category
- Quality Score (color-coded: red <50, yellow 50-70, green >70)
- Phone (clickable tel: link if available)
- Website (clickable link if available, opens in new tab)
- Map (clickable "📍 View on Maps" link)

Display format for links:
- Phone: <a href="tel:[phone_number]">[formatted_phone]</a>
- Website: <a href="[url]" target="_blank">Visit Website</a>
- Map: <a href="[map_link]" target="_blank">📍 View on Maps</a>

Show summary at top: "Successfully generated [X] leads!"

STEP 6 - JSON EXPORT:
Create a downloadable JSON file with this EXACT structure and field names:

{
  "leads": [
    {
      "GeneratedDate": "YYYY-MM-DD",
      "SearchCity": "[user's city input]",
      "SearchCountry": "[user's country input]",
      "LeadNumber": 1,
      "CompanyName": "",
      "Category": "",
      "Description": "",
      "Address": "",
      "City": "",
      "Country": "",
      "Coordinates": "latitude,longitude",
      "MapLink": "https://www.google.com/maps/search/?api=1&query=lat,lng",
      "Phone": "",
      "Email": "",
      "Website": "",
      "LinkedIn": "",
      "Facebook": "",
      "Instagram": "",
      "Rating": 0.0,
      "ReviewCount": 0,
      "BusinessHours": "",
      "QualityScore": 0,
      "QualityReasoning": "",
      "Status": "New",
      "Contacted": "No",
      "Notes": ""
    }
  ]
}

CRITICAL JSON REQUIREMENTS:
- Use these EXACT field names (case-sensitive)
- LeadNumber starts at 1 and increments sequentially
- GeneratedDate is current date in YYYY-MM-DD format
- SearchCity and SearchCountry are the user's input values
- Coordinates format: "latitude,longitude" with NO SPACES (e.g., "40.7388,-73.9874")
- Use empty string "" for missing data, NEVER null or undefined
- All 26 fields must be present for every lead
- Status is always "New"
- Contacted is always "No"
- Notes is always empty string ""

STEP 7 - WEBHOOK INTEGRATION:
Below the results table, provide:
- Text input field labeled "Webhook URL (optional)"
- "Send to Webhook" button
When clicked, POST the complete JSON data to the provided webhook URL
Show success/error message after sending

Add a "Download JSON" button that downloads the file as:
leads_[search_query]_[city].json

=== DATA EXTRACTION RULES ===

GPS COORDINATES:
- Google Maps ALWAYS provides coordinates
- Extract latitude and longitude
- Format as "lat,lng" with comma, no spaces
- Example: "40.7388,-73.9874"

WEBSITE EXTRACTION:
- Most businesses have websites in their Maps listing
- Look in the business profile data
- Include full URL with https://
- If truly missing, use empty string ""

EMAIL EXTRACTION:
- Check business description text
- Check contact information section
- Look for email patterns in Maps data
- If not found, use empty string ""

SOCIAL MEDIA:
- Facebook, Instagram, LinkedIn may be in Maps data
- Extract full profile URLs
- If not available, use empty string "" for each

CITY & COUNTRY PARSING:
- Extract from the full address
- City: typically before state/province
- Country: last part of address or ISO code
- Example address: "42 E 20th St, New York, NY 10003, USA"
  - City: "New York"
  - Country: "USA"

DESCRIPTION:
- Use business summary from Maps
- Include business highlights or specialties
- Keep under 200 characters
- If no description, use empty string ""

=== ERROR HANDLING ===

If Google Maps returns NO results:
- Display: "No businesses found for '[query]' in [city], [country]"
- Suggest: "Try different search terms or location"

If fewer results than requested:
- Display: "Found [X] leads (requested [Y])"
- Show all available results

If Maps API fails:
- Display: "Unable to connect to Google Maps. Please try again."

=== PROGRESS INDICATORS ===

While searching, show:
"🔍 Searching Google Maps for [query] in [city], [country]..."
"📊 Found [X] businesses, extracting details..."
"✅ Successfully generated [X] leads!"

=== STYLING REQUIREMENTS ===

- Use professional, modern UI design
- Quality Score badges with colors:
  - 0-49: Red badge
  - 50-74: Yellow/Orange badge
  - 75-100: Green badge
- Clickable links should be clearly styled (blue, underlined)
- Table should be responsive and easy to read
- Progress indicators should be visible and animated
- Success/error messages should be prominent

=== IMPORTANT NOTES ===

1. Extract ALL available data from Google Maps - don't leave fields empty unnecessarily
2. GPS coordinates are ALWAYS available from Maps - extract them
3. Most established businesses have websites - find them
4. Number leads sequentially starting from 1
5. Use current date for GeneratedDate
6. Preserve exact JSON field names for compatibility
7. Test webhook functionality before deployment
8. Handle missing data gracefully with empty strings
9. Ensure MapLink works for direct navigation
10. Make phone numbers and websites clickable in display

=== VALIDATION ===

Before displaying results, verify:
✓ All 26 JSON fields present for each lead
✓ Coordinates are in "lat,lng" format
✓ LeadNumber increments correctly (1, 2, 3...)
✓ SearchCity and SearchCountry match user input
✓ GeneratedDate is current date
✓ MapLink is properly formatted
✓ Quality Score is between 0-100
✓ Status = "New" and Contacted = "No"

=== FINAL OUTPUT ===

Provide three ways to access data:
1. Visual table display (with clickable links)
2. Download JSON button
3. Send to Webhook button (with URL input)

Show clear success message with count of leads generated.


# prompt 2
CRITICAL INSTRUCTIONS FOR EACH LEAD:

1. MapLink: You MUST generate a Google Maps search URL for every business using this exact format:
   https://www.google.com/maps/search/?api=1&query=COMPANY_NAME+FULL_ADDRESS
   
   - Replace all spaces with + signs
   - Include the complete address
   - Do NOT leave this field empty
   
   Example for "Gramercy Tavern" at "42 E 20th St, New York, NY 10003, USA":
   MapLink: "https://www.google.com/maps/search/?api=1&query=Gramercy+Tavern+42+E+20th+St+New+York+NY+10003+USA"

2. Website: Extract the website URL from Google Maps business listing if available. If not found, set to empty string "".

3. NEVER return "-" or null values. Use empty string "" if data is not available.









