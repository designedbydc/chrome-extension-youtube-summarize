# YouTube Comments Summarizer API

Backend service for the YouTube Comments Summarizer Chrome extension. This API uses OpenAI to analyze and summarize YouTube comments.

## Technical Overview

This service is built with:
- Node.js
- OpenAI API
- Vercel serverless functions

## Current Status

**⚠️ IMPORTANT: The API is currently experiencing deployment issues.**

The API is experiencing CORS-related issues with Vercel deployment. When requests are made from the extension's content script on `youtube.com`, they are being blocked by CORS policies. 

### Current Workarounds

1. **Background Script**: The Chrome extension has been updated to use a background script for API calls, which bypasses many CORS restrictions.
2. **Local Fallback**: A local summarization algorithm is available in the extension if the API is unreachable.

## API Endpoints

### POST /api/summarize

Generates a summary of YouTube comments.

**Request body**:
```json
[
  {
    "text": "Comment text here",
    "likes": 42
  },
  ...
]
```

**Success Response**:
```json
{
  "summary": "Summary text generated by AI"
}
```

**Error Response**:
```json
{
  "error": "Error description"
}
```

## Local Development

1. Clone this repository
2. Install dependencies:
   ```
   npm install
   ```
3. Create a `.env` file with your OpenAI API key:
   ```
   OPENAI_API_KEY=your-api-key-here
   ```
4. Run the development server:
   ```
   npx vercel dev
   ```

## Deployment

The API is deployed on Vercel:

```
vercel --prod
```

## CORS Configuration

The API is configured to handle CORS in both the API route handler and Vercel configuration:

1. **API Route**: The `summarize.js` file sets CORS headers directly
2. **Vercel Configuration**: The `vercel.json` file contains CORS settings

### Troubleshooting CORS Issues

If CORS issues persist, try:

1. Verifying the `vercel.json` configuration:
   ```json
   {
     "routes": [
       {
         "src": "/(.*)",
         "headers": {
           "Access-Control-Allow-Origin": "*",
           "Access-Control-Allow-Methods": "GET, HEAD, PUT, PATCH, POST, DELETE, OPTIONS",
           "Access-Control-Allow-Headers": "X-Requested-With, Content-Type, Authorization"
         },
         "continue": true
       }
     ]
   }
   ```

2. Ensuring the API route has proper CORS headers:
   ```javascript
   res.setHeader('Access-Control-Allow-Origin', '*');
   res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS, PUT, PATCH, DELETE');
   res.setHeader('Access-Control-Allow-Headers', 'X-Requested-With,content-type,authorization');
   ```

3. Testing with alternative hosting providers that have less restrictive CORS policies.

## License

MIT License 