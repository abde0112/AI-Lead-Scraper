# CRITICAL: WEBSITE EXTRACTION

When scraping each business from Google Maps, you MUST extract the website URL. Here's how:

## Website Extraction Methods:

1. **Primary Method - Google Maps Business Profile:**
   - Look for the website field in the Google Maps business listing
   - It's usually displayed with a 🌐 globe icon
   - Extract the full URL (e.g., "https://sqala.ma", "https://www.restaurant.com")

2. **Alternative Method - Business Description/About:**
   - Check the business description or "About" section
   - Sometimes websites are mentioned in the text

3. **Fallback Method - Social Media Search:**
   - If no official website, check if they have a Facebook business page
   - Some businesses use Facebook as their primary web presence
   - Format: Use their Facebook URL as website if no official site exists

## Website Field Rules:

- If website is found: Store the COMPLETE URL including https://
  Example: "https://sqala.ma"
  
- If NO website is found after all methods: Use empty string ""
  NOT "-" or "null" or "N/A"

- Clean the URL:
  * Remove any tracking parameters
  * Ensure it starts with http:// or https://
  * Remove any whitespace

## Example Data Structure:
```json
{
  "CompanyName": "La Sqala",
  "Website": "https://sqala.ma",  // ✅ Full URL
  "Phone": "05222-60960",
  ...
}
```

OR if no website found:
```json
{
  "CompanyName": "Some Restaurant",
  "Website": "",  // ✅ Empty string, NOT "-"
  "Phone": "05222-12345",
  ...
}
```

# GOOGLE MAPS API INTEGRATION

If using Google Places API, here's how to get the website:
```javascript
// When fetching place details, include this field:
const placeDetails = await maps.places.details({
  place_id: placeId,
  fields: [
    'name',
    'formatted_address',
    'formatted_phone_number',
    'website',  // ← This is the key field!
    'rating',
    'user_ratings_total',
    'opening_hours',
    'geometry',
    'business_status',
    'url'  // This gives the Google Maps URL
  ]
});

// Extract website
const website = placeDetails.website || "";  // Use empty string if not found
```

# DISPLAY IN UI

In your results table:
- If website exists: Show blue button "🔗 Visit" that opens website in new tab
- If website is empty: Show gray text "-"

Example:
```javascript
{lead.Website && lead.Website !== "" ? (
  <a href={lead.Website} target="_blank" class="website-link">
    🔗 Visit
  </a>
) : (
  <span class="no-data">-</span>
)}
```

# VALIDATION

Before sending to webhook, validate:
1. Website field must exist in JSON (even if empty)
2. If URL is present, it must start with http:// or https://
3. No placeholder values like "N/A", "null", "-"

## Quality Score Adjustment:

Update your quality score calculation to properly handle websites:
