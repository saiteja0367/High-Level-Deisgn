# Design WhatsApp — Problem Statement

## Problem Statement

Design a scalable, highly available, real-time messaging application similar to WhatsApp. Users should be able to exchange messages in individual and group conversations, share images, and receive messages even after periods of being offline.

The system should support millions of concurrent connections and billions of messages per day while providing low-latency delivery, reliable message storage, and secure communication.

## Functional Requirements

- Send and receive one-to-one text messages.
- Create group conversations and send messages to group members.
- Send and receive images.
- Deliver messages to users who reconnect after being offline.
- Retrieve conversation history.
- Display message status: sent, delivered, and read.
- Show online/offline presence and last-seen information, subject to privacy settings.
- Support end-to-end encrypted messaging.

## Non-Functional Requirements

- **Low latency:** Deliver messages quickly to online recipients.
- **High availability:** Keep messaging operational despite server failures.
- **Scalability:** Handle millions of concurrent users and high message throughput.
- **Durability and reliability:** Prevent loss of acknowledged messages and support safe retries.
- **Ordering:** Maintain consistent ordering within a conversation.
- **Security and privacy:** Protect message contents and user information.

## Scale Assumptions

- 100 million daily active users.
- 50 messages sent per user per day.
- Approximately 5 billion messages per day.
- Approximately 10 million concurrent connections.
- Text and image messages are in scope; media sizes vary.

## Out of Scope

- Voice and video calling.
- Stories/status posts.
- Payments and business messaging.
- Advanced message search.

## Design Challenge

Propose a high-level architecture, data model, APIs or communication protocols, and approaches for real-time delivery, offline synchronization, group fan-out, message ordering, security, and scaling. Explain the major trade-offs and how the system behaves when components fail.

## Solution

See [SOLUTION.md](./SOLUTION.md) for the proposed design.
