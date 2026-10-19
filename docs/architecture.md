# Architecture

Supernizo Autocall combines a Next.js application, an embeddable browser tracker, and shared TypeScript contracts in a pnpm workspace.

## Application boundaries

| Layer          | Responsibility                                                                                  |
| -------------- | ----------------------------------------------------------------------------------------------- |
| Delivery       | App Router pages, Route Handlers, browser components, and widgets.                              |
| Application    | Tracking, conversations, calls, presence, notifications, access, and site-management use cases. |
| Domain         | Call transitions, authorization rules, and other business policies.                             |
| Ports          | Interfaces for repositories, transactions, presence, realtime delivery, and media providers.    |
| Infrastructure | Prisma repositories, Redis operations, Upstash delivery, and LiveKit integration.               |
| Composition    | Wiring application services to production implementations.                                      |

Route Handlers validate incoming data, authorize the request, call an application service, and map the response. React components handle presentation and browser interactions.

## Durable data and live state

PostgreSQL stores site configuration, user records, visitors, sessions, page activity, conversations, messages, call metadata, and notifications. Prisma provides the database access layer and migration history.

Redis holds short-lived presence. Expiration allows a disconnected visitor to leave the online list without treating that status as a permanent event.

Upstash Realtime delivers dashboard, visitor, chat, and call-state updates over Server-Sent Events. Channel authorization controls which updates a client can receive.

## Call flow

1. An authorized agent requests a call for a visitor, or a visitor requests an audio call.
2. The application creates a call record and notifies the receiving participant.
3. The recipient can accept or decline. The browser requests microphone or camera permission when needed.
4. The application authorizes access to the LiveKit room.
5. LiveKit transports media between participants while application services manage call state and metadata.
6. Ending, cancellation, rejection, timeout, and failure are represented as explicit call states.

Audio and video bytes do not pass through Next.js Route Handlers. The application stores call metadata rather than media recordings.

## Validation and access

- Zod validates untrusted request payloads and shared contracts.
- Public tracker requests are checked against the site's public key and allowed origins.
- Rate limiting applies at public entry points.
- Staff access is checked through the Supernizo identity integration.
- Provider credentials and signing secrets remain on the server.
- Visitor microphone and camera access is controlled through browser permissions.

## Verification approach

The application repository contains service tests with injected dependencies, contract and authorization tests, architecture checks, browser scenarios, and deployment configuration checks.

The architecture checks enforce separation between browser code, business logic, concrete infrastructure, and HTTP delivery. TypeScript checks and production builds provide additional validation before a release.
