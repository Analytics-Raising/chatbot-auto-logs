# Logging Chatbot Conversations with Trubrics

## Prerequisites
- Trubrics API key (available upon request)

## Step-by-Step Implementation

### 1. Install Trubrics SDK
```bash
npm install @trubrics/trubrics
```

### 2. Set Up Environment Variables
Create a `.env` file in your project root:
```
VITE_TRUBRICS_API_KEY=your_trubrics_api_key_here
```
**Security Tip:** Consider serving the API key from the backend in future.

### 3. Create Trubrics Logging Function
Create `src/api/Trubrics.tsx`:

```typescript
import { Trubrics } from "@trubrics/trubrics";

type TrubricsConfig = {
    apiKey: string;
}

const config: TrubricsConfig = {
    apiKey: import.meta.env.VITE_TRUBRICS_API_KEY || "",
}

const trubrics = new Trubrics(config);

const logToTrubrics = async (prompt: string, response: any, email: string) => {
    trubrics.track({
        event: "Generation",
        user_id: email,
        properties: {
            prompt: prompt,
            response: response,
            source: "Chatbot"
        }
    });
}

export default logToTrubrics;
```

### 4. Integrate Logging in Chat Component
In your chat component (e.g., `ChatPage.tsx`):

```typescript
import logToTrubrics from "../api/Trubrics";

const handleSendMessage = async () => {
    try {
        const response = await fetch(url);
        const receivedText = await response.text();
        
        logToTrubrics(newMessage.text, receivedText, user);
    } catch (error) {
        console.error("Error:", error);
    }
};
```

## Key Points
- Use `source` to specify web or mobile implementation
- The `logToTrubrics` function logs conversations with user prompt, bot response, and user email

## Additional Resources
- Trubrics Documentation: [https://docs.trubrics.com/](https://docs.trubrics.com/)
