## Overview

This React hook, `useChatCompletion` aims to enable easy streaming of OpenAi responses into your React components.

### Usage

- `pnpm i react-openai-hooks`

```
  const prompt = [
    {
      content: userPrompt,
      role: 'user',
    },
  ];

  const { messages, submitPrompt } = useChatCompletion({
    model: 'your-model-choice',
    apiKey: OPENAI_API_KEY,
    temperature: 0.9,
  });
<!-- you will receive all messages including your own prompt and the system prompt as well, make sure to filter the arrays for the role and response you want -->
  useEffect(() => {
    messages.length < 1
      ? setContent('No messages yet')
      : setContent(messages.map((msg) => msg.content).join('\n'));
  }, [messages]);


<!-- Here is how I paginated and filtered responses in my own project -->
  useEffect(() => {
    if (messages.length < 1) {
      setContent('No messages yet');
    } else {
      const filteredMessages = messages.filter(
        (msg) => msg.role === 'assistant'
      );
      setAssistantMessages(filteredMessages);
      setContent(filteredMessages[currentPage]?.content || 'No messages yet');
    }
  }, [messages, currentPage]);

    return (
        <div className="editor-container p-4">
        <textarea
          ref={textAreaRef}
          className="textarea textarea-bordered w-full h-96"
          value={content}
          onChange={(e) => setContent(e.target.value)}
        ></textarea>
        <div className="pagination-controls my-4">
          <button
            className="btn btn-primary mr-2"
            onClick={goToPreviousPage}
            disabled={currentPage === 0}
          >
            Previous
          </button>
          <button
            className="btn btn-primary"
            onClick={goToNextPage}
            disabled={currentPage === assistantMessages.length - 1}
          >
            Next
          </button>
        </div>
      )
```

### Special Thanks
