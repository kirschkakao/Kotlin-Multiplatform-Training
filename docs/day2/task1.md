# Task 1: Navigation Graph and Architecture

### Why Navigation and Architecture?
Navigation manages app flow, while architecture (like MVVM with DI) keeps code organized. This task shows how to structure a KMP app with NavHost, Destinations, and injected dependencies for scalability.

### Task: Implement Navigation and Basic Architecture
1. **Define Destinations**: Create object classes for screens (e.g., WelcomeDestination, SelectPlayerDestination).
2. **Set Up NavHost**: Use NavHostController and composable routes to navigate between screens.
3. **Create Architecture Layers**: Implement DAOs (e.g., PlayerDao), Repositories, and ViewModels. Use AppContainer for DI.
4. **Integrate**: Wire everything together so navigation triggers screen changes.

### What It Should Look Like Now
- App compiles and navigates between placeholder screens.
- DI works (e.g., ViewModels receive dependencies).
- No UI yet, but structure is in place.

!!! tip
    Use the provided codebase as a guide for patterns.

---

[Previous: Day 2 Overview](../index.md) | [Next: Task 2](task2.md)