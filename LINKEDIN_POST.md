I've been working on Supernizo Autocall: a platform that connects live website activity with conversations.

The workflow brings together a live visitor dashboard, website chat, and browser-based voice and video calls. Agents can see session context and reach out while visitors are browsing. Visitors choose whether to accept a call and grant microphone or camera access.

One engineering decision I enjoyed working through was separating three different needs:

- PostgreSQL for durable conversations and call history.
- Redis and Upstash Realtime for presence and live updates.
- LiveKit for WebRTC audio and video.

Next.js and TypeScript connect those parts through validated APIs and application services, with an embeddable tracker SDK on the website side.

I've put together a project showcase covering the features, architecture, and interaction flow:

https://github.com/Geethanjana99/supernizo-autocall-showcase

#Nextjs #TypeScript #WebRTC #FullStackDevelopment #SoftwareEngineering
