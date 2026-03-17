# 🚀 NestJS Real-Time Game Engine Backend

![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![Socket.io](https://img.shields.io/badge/Socket.io-010101?style=for-the-badge&logo=socket.io&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Jest](https://img.shields.io/badge/Jest-C21325?style=for-the-badge&logo=jest&logoColor=white)

## 📖 Context & Architecture
This repository contains the **Core Game Engine Service** for a larger real-time multiplayer browser game (part of the 42 Network "Transcendence" project). 

It is designed as a standalone, decoupled backend microservice. It does not contain frontend code; instead, it handles pure authoritative game logic, custom 2D physics, state management, and real-time networking via WebSockets, communicating with other microservices (like Matchmaking) via Redis events.

## ✨ Key Features & Technical Highlights

### ⚙️ Custom Game Loop & Fixed Timestep
Implemented a custom server-side game loop using a **Time Accumulator** to ensure deterministic physics updates at a fixed rate (60Hz), preventing tunneling and inconsistencies regardless of server load variations.

### 🧠 State Machine Pattern
Extensive use of the State Design Pattern to ensure scalable and clean code:
- **Game States:** Seamless transitions between `LobbyState`, `PlayState`, and `EndState`.
- **Action States:** Player combat mechanics are handled via isolated states (`MeleeAttackState`, `SpellAttackState`, `DefenceAttack`).
- **AI States:** Bot behaviors are modularized (`WanderState`, `ChaseState`, etc.) using a dedicated `AiService`.

### 🚀 Performance Optimizations (Object Pooling)
To prevent Garbage Collection spikes that cause frame drops in Node.js, **Object Pooling** was implemented for projectiles (`MAX_BULLETS = 100`). Deactivated bullets are recycled instead of continuously instantiating and destroying memory objects.

### 📐 Custom 2D Physics & Spatial Partitioning
Built a custom physics system from scratch (`PhysicsSystem`) to handle:
- Circle-to-Circle collisions (Players & Pillars).
- Circle-to-AABB collisions (Walls).
- **Flattened Grid Spatial Partitioning:** The map is divided into a 1D array grid for highly efficient wall-collision queries, filtering out distant entities before calculating exact distances.

### 🛡️ Networking & Security
- **JWT Authentication:** Secure WebSocket handshakes validating HTTP-only cookies.
- **Rate Limiting:** Implemented a custom `@UseGuards(WsThrottlerGuard)` to prevent WebSocket flooding/thrashing.
- **Input Validation:** Strict validation of incoming movement vectors using NestJS pipes and DTOs.

## 🧪 Testing
Since this is an authoritative headless backend, gameplay logic is validated through an extensive suite of **Unit Tests** written in Jest. The tests cover the core engine, physics calculations, combat rules, vector math, and object pooling limits.

```bash
# Run the test suite
$ npm run test
