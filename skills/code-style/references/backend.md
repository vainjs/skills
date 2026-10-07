# Backend Conventions

## Directory Naming

Use kebab-case for business-module directories.

## API Responses

Backend business JSON APIs use the uniform response structure `{ code: string, message: string, data: {} }`.

- `code` is a string business status code.
- `message` is a string message.
- `data` is a business data object whose fields are defined by the API.

This illustrates the response structure; it is not a language-specific type definition to copy verbatim. Non-JSON APIs, such as file downloads and streaming responses, follow their respective protocols.
