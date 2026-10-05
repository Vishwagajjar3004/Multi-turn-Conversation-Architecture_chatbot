Multi-turn Conversation Architecture_chatbot
# Multi-Turn CLI Chatbot

A command-line chatbot that holds an ongoing, multi-turn conversation and
tracks token usage as you go. Two versions are included, sharing the same
features and commands:

| File | Backend |
|---|---|
| `chatbot.py` | Anthropic Claude API |
| `chatbot_gemini.py` | Google Gemini API |

Use whichever matches the API key you have (or both).

## Features

- **Ongoing conversation** — keep chatting turn after turn.
- **Full memory** — the entire conversation is sent with every request, so
  the model remembers everything said so far in the session.
- **`history`** — print every message exchanged so far.
- **`clear`** — reset the conversation and start fresh.
- **`save`** — export the conversation to a timestamped JSON file.
- **Live token count** — after every reply, see exactly how many tokens
  the current conversation uses.
- **Context-limit warnings** — a warning at 80% of the context window, and
  a more urgent one past 95%.

## Setup

### Claude (`chatbot.py`)

- `--model` — override the default model.
- `--context-limit` — override the assumed context window size (in
  tokens), used for the 80%/95% warnings.
- `--system` — set a system prompt/instruction for the conversation.

## Example session

```
$ python3 chatbot.py
============================================================
Claude CLI Chatbot  (model: claude-sonnet-5)
Context limit: 200,000 tokens
Type 'help' to see available commands.
============================================================
You: What's the capital of France?

Claude: The capital of France is Paris.

[tokens] 24 / 200,000 (0.0% of context window used)

You: save

Conversation saved to: /mnt/user-data/outputs/conversation_20260701_060000.json

You: exit
Goodbye!
```

## Saved conversation format

`save` writes a JSON file shaped like this:

```json
{
  "model": "claude-sonnet-5",
  "created_at": "2026-07-01T06:00:00+00:00",
  "exported_at": "2026-07-01T06:05:00+00:00",
  "system": null,
  "messages": [
    {"role": "user", "content": "What's the capital of France?"},
    {"role": "assistant", "content": "The capital of France is Paris."}
  ]
}
```

## Notes

- Token counts come from each provider's official counting endpoint, not
  a local estimate, so the numbers reflect what will actually be billed
  and what will actually be sent on the next turn.
- Context window sizes are hard-coded per known model as of writing —
  double check the current figure for your exact model in the provider's
  docs (Anthropic's or Google AI Studio's) if you're relying on the
  warning thresholds for something important, since providers update
  these over time.
- Neither script persists conversations automatically between runs — use
  `save` before exiting if you want to keep a transcript.



output
  
remotedevs2@RD:~/Projects/chatbot$ python3 chatbot.py
============================================================
Gemini CLI Chatbot  (model: gemini-3.5-flash)
Context limit: 1,000,000 tokens
Type 'help' to see available commands.
============================================================
You: hey

Gemini: Hey there! How's it going? How can I help you today?

[tokens] 19 / 1,000,000 (0.0% of context window used)

You: how are you?

Gemini: I'm doing great, thank you for asking! How are you doing today? 

How can I help you out with anything?

[tokens] 53 / 1,000,000 (0.0% of context window used)

You:   
