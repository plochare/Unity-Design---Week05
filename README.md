# Unity Design - Week 05

## Agenda

1. [Multiplayer with Photon Fusion](#Multiplayer-Photon-Fusion)  
2. [Unity Editor AI Tools](#Unity-AI-Tools)  
3. [Midterm Assignment](#Midterm-Assignment)  
4. [Design Process : Wireframes, Flow Diagrams & Storyboards](#Design-Process)  

---
Week 5 - Multiplayer Photon - AI Tools - Midterm Assignment - Desktop or Mobile Prototype of Gamified Learning -  Documents / Storyboards / Wireframe / Flow Diagram

---

# 🚀 Unity Multiplayer with Photon Fusion 

Photon Fusion is a high-performance networking SDK for Unity that makes it easier to build scalable, low-latency multiplayer games. This guide will walk you through the fundamentals of setting up Photon Fusion, connecting players, and synchronizing gameplay.

---

## 📌 What is Photon Fusion?
Photon Fusion is the next generation of Photon’s networking solutions for Unity, designed to replace **PUN** and **Bolt** with:
- 🔥 **High performance**: Supports 6,000+ CCU on a single server.
- 🕹 **Multiple topologies**: Client-Hosted, Shared, and Dedicated Server.
- 🎮 **Tick-based simulation**: Handles state replication and prediction with minimal effort.
- ⚡ **Lag compensation**: Hit detection and inputs are auto-reconciled.

---

## 🛠 Requirements
- Unity **2021 LTS or newer** (Fusion works best on latest LTS versions).
- [Photon Fusion SDK](https://doc.photonengine.com/fusion/current/getting-started/installation).
- Photon [Dashboard Account](https://dashboard.photonengine.com/).

---

## 📥 Installation

1. **Download Photon Fusion SDK**
   - Get it from the [Photon website](https://doc.photonengine.com/fusion/current/getting-started/installation) or via Unity’s **Package Manager**.

2. **Import into Unity**
   - Drag the `.unitypackage` into your project or add via Git URL in Package Manager.

3. **Set Up App ID**
   - Register a new **Fusion App** in the [Photon Dashboard](https://dashboard.photonengine.com/).
   - Copy the **App ID** and paste it into:  
     `Fusion → Setup Wizard → Enter App ID`

---

## 🚀 Quick Start Tutorial

### 1. Create a Fusion Runner
The **Runner** manages networking, session creation, and game logic.

```csharp
using Fusion;
using Fusion.Sockets;
using System;
using UnityEngine;

public class GameLauncher : MonoBehaviour
{
    private NetworkRunner runner;

    async void Start()
    {
        runner = gameObject.AddComponent<NetworkRunner>();
        runner.ProvideInput = true;

        await runner.StartGame(new StartGameArgs()
        {
            GameMode = GameMode.AutoHostOrClient,
            SessionName = "TestRoom",
            Scene = UnityEngine.SceneManagement.SceneManager.GetActiveScene().buildIndex,
            SceneManager = gameObject.AddComponent<NetworkSceneManagerDefault>()
        });
    }
}
```

✅ This script will **auto-host** a session if no one is connected, or **join** an existing session.

---

### 2. Create a Networked Player

1. Create a simple **Player Prefab** (Cube/Sphere).
2. Add a **`NetworkObject`** component to it.
3. Create a **PlayerSpawner** script:

```csharp
using Fusion;
using UnityEngine;

public class PlayerSpawner : NetworkBehaviour
{
    public GameObject playerPrefab;

    public override void Spawned()
    {
        if (Object.HasInputAuthority)
        {
            Runner.Spawn(playerPrefab, Vector3.zero, Quaternion.identity, Object.InputAuthority);
        }
    }
}
```

4. Register the prefab in:
   `Fusion → Network Project Config → Prefabs`

---

### 3. Handle Player Movement

Fusion provides **input prediction** to reduce lag.

```csharp
using Fusion;
using UnityEngine;

public class PlayerMovement : NetworkBehaviour
{
    [Networked] private Vector3 Position { get; set; }

    public override void FixedUpdateNetwork()
    {
        if (GetInput(out NetworkInputData data))
        {
            Vector3 move = new Vector3(data.horizontal, 0, data.vertical) * 5f * Runner.DeltaTime;
            Position += move;
        }

        transform.position = Position;
    }
}

public struct NetworkInputData : INetworkInput
{
    public float horizontal;
    public float vertical;
}
```

And an **Input Handler**:

```csharp
using Fusion;
using UnityEngine;

public class InputHandler : MonoBehaviour, INetworkRunnerCallbacks
{
    public void OnInput(NetworkRunner runner, NetworkInput input)
    {
        NetworkInputData data = new NetworkInputData
        {
            horizontal = Input.GetAxisRaw("Horizontal"),
            vertical = Input.GetAxisRaw("Vertical")
        };
        input.Set(data);
    }

    // Other INetworkRunnerCallbacks methods can be left empty
}
```

---

## 🧪 Playtest

* Open **two instances** of your game.
* One will **host** (server), the other will **join** automatically.
* Move each player independently with `WASD`.

---

## ⚡ Advanced Features

* **Lag Compensation** → `Runner.LagCompensation` for hitscan accuracy.
* **Shared Mode** → Multiple peers simulate together (no central server).
* **Client Prediction & Reconciliation** → Built-in for smooth gameplay.
* **Tick-based Events** → Deterministic simulation.

---

## 📚 Learning Resources

* [Photon Fusion Documentation](https://doc.photonengine.com/fusion/current/getting-started/overview)
* [Photon Fusion Samples on GitHub](https://github.com/Exit-Games/Fusion)
* [Fusion Academy Tutorials](https://doc.photonengine.com/fusion/current/tutorials/overview)

---

## ✅ Best Practices

* Always **register prefabs** in Network Project Config.
* Use **`Networked` properties** instead of manually syncing variables.
* Minimize use of `RPCs` – prefer state synchronization.
* Test in **builds**, not just the Unity Editor (two Editors can cause issues).

---

## 🏆 Summary

You now have:

* A Fusion Runner that auto-hosts/joins sessions.
* Networked players that spawn and move across clients.
* Input prediction for smoother movement.

Photon Fusion is powerful for real-time, large-scale multiplayer. With this foundation, you can expand to:

* Matchmaking
* Dedicated servers
* Physics-based gameplay
* Advanced lag compensation

---

# Unity Editor AI Tools 🤖✨

AI is rapidly transforming the way we create games, and Unity Editor AI tools are here to **accelerate prototyping, automate repetitive tasks, and enhance creativity**.

---

## 📌 What Are Unity Editor AI Tools?
Unity Editor AI tools integrate artificial intelligence into the Unity Editor to help developers:
- 🖌 **Generate assets** (textures, sprites, 3D models).
- 🛠 **Automate code snippets** (C# scripting with AI assistance).
- 🎨 **Assist level design** (AI terrain sculpting, scene layout).
- ⚡ **Accelerate iteration** (dialogue generation, testing bots).
- 🧪 **Prototype gameplay ideas** faster.

Some AI tools are **Unity Official** (like Unity Muse), while others come from **community packages** or **third-party services**.

---

## 🛠 Requirements
- Unity **2021 LTS or newer** (AI tools often target newer LTS versions).
- A Unity account with access to [Unity Muse](https://unity.com/products/muse) or AI-integrated packages.
- Internet connection (many AI tools call external APIs).

---

## 📥 Installation

### Option 1: Unity Muse (Official)
1. Open **Unity Hub → Projects**.
2. Install the **Muse package** from the **Package Manager**.
3. Log in with your Unity account (Muse requires subscription access).
4. Access Muse panels from:  
   `Window → Muse → Chat / Sprite / Texture`

### Option 2: Third-Party AI Packages
- Import via **Unity Package Manager → Add package from Git URL**.  
  Example:  
  ```text
  https://github.com/<org>/<unity-ai-package>.git
````

* Or download `.unitypackage` releases from GitHub and import manually.

---

## 🚀 Quick Start Tutorial

### AI Code Generation with Muse Chat

Muse Chat is like **ChatGPT inside Unity**.

* Open: `Window → Muse → Chat`.
* Ask questions like:

  ```
  Generate a C# script to move a character with WASD input.
  ```
* Muse suggests a script, which you can **Insert** directly into your project.

---

### AI Asset Generation

Muse and other tools can generate **sprites, textures, and materials** directly.

Example workflow:

1. Open: `Window → Muse → Sprite`.
2. Type a prompt:

   ```
   "Pixel art potion bottle, glowing blue"
   ```
3. Click **Generate** → Muse creates sprite variations.
4. Drag into **Scene Hierarchy** as a prefab.

---

## 🧩 Example Workflow: AI-Assisted Game Jam

1. Use **Muse Chat** to scaffold movement + camera scripts.
2. Generate placeholder sprites with **Muse Sprite**.
3. Use AI Terrain for **fast prototyping** of landscapes.
4. Auto-generate **dialogue trees** for NPCs.
5. Polish later with custom art/code.

⏱ Result: Build a playable prototype in hours instead of days.

---

## ⚡ Advanced Use Cases

* **AI Playtesting** → simulate player behavior with agent-based AI testers.
* **Procedural AI Generation** → create infinite variations of levels/assets.
* **Localization** → AI-assisted translation of dialogue/UI.
* **Shader Suggestions** → AI-generated Shader Graph snippets.

---

## 📚 Learning Resources

* [Unity Muse Documentation](https://docs.unity3d.com/muse)
* [Unity Blog: AI Tools](https://blog.unity.com/tag/ai)
* [Unity AI Roadmap](https://unity.com/ai)

---

## ✅ Best Practices

* Treat AI outputs as **starting points**, not finished products.
* Keep **version control** active (AI changes can be large).
* Use AI for **rapid iteration**, but refine manually for performance/quality.
* Test generated code for **performance + security**.

---

# 📘 Midterm Assignment: Design an Interactive Learning Application with Gamified Features

## 🎯 Objective

The goal of this assignment is to apply **user experience design principles** and **game mechanics** to create a concept for an **interactive learning application**. Students will demonstrate their ability to design educational technology that is **engaging, user-friendly, and motivating**.

---

## 📋 Assignment Description

You will design and present a prototype for an **interactive learning application** (desktop, web, or mobile). The application must teach users a specific skill or subject area (e.g., math drills, language learning, history exploration, science simulations, coding tutorials).

To enhance engagement, your app must include **at least three gamified features**, such as:

* Points, badges, or leaderboards
* Levels or progression systems
* Timed challenges or quizzes
* Unlockable achievements or rewards
* Story-driven or narrative-based learning
* Avatar customization or virtual economy

Your design should focus on **how gamification motivates learners** and enhances the educational experience.

---

## 🔑 Deliverables

1. **Project Proposal (1–2 pages)**

   * Chosen subject area (what the app teaches).
   * Target audience (age group, learning style).
   * Educational goals (skills or knowledge to be gained).

2. **Design Documentation**

   * **User Flow Diagram** – show how learners navigate the app.
   * **Wireframes** – layout of key screens.
   * **Storyboard** – demonstrate a typical user’s learning journey.

3. **Gamification Strategy (1–2 pages)**

   * Identify at least **3 gamification elements**.
   * Explain how each element supports **motivation** and **learning outcomes**.

4. **Prototype (Unity Application)**

   * Setup a low-fidelity Unity functional prototype
   
---

## 📚 Suggested Tools & References

* [Figma](https://www.figma.com/) – Wireframing & prototyping
* [Canva Storyboard Maker](https://www.canva.com/storyboards/)
* [Gamification in Learning – Edutopia](https://www.edutopia.org/article/gamification-in-education)
* [Yu-kai Chou’s Octalysis Framework](https://yukaichou.com/gamification-examples/octalysis-complete-gamification-framework/)

---

✅ **By the end of this assignment, students will:**

* Practice **design thinking and UX documentation**
* Understand the **role of gamification in learning applications**
* Gain experience prototyping concepts in Unity

---

# 🖥️ Design Process : Wireframes, Flow Diagrams & Storyboards

## 🎯 Why a Design Process?
A structured design process helps to:
- ✅ Clarify requirements  
- ✅ Reduce costly redesigns later  
- ✅ Improve collaboration between designers, developers & stakeholders  
- ✅ Ensure a smooth, user-friendly desktop app  

---

## 🛠️ Stages of the Design Process

### 1. Requirements Gathering
- Interview stakeholders & users.  
- Identify **core use cases** (e.g., file management, data visualization, task automation).  
- Define technical constraints (OS compatibility, frameworks, APIs).  

📚 Reference: [Nielsen Norman Group – User Research Basics](https://www.nngroup.com/articles/which-ux-research-methods/)

---

### 2. User Flow Diagrams
Flow diagrams visualize **how users move through the app**.  

**Example:**  
- Login → Dashboard → Feature Module → Save → Logout  

📌 Tools:  
- [Lucidchart](https://www.lucidchart.com/)  
- [Draw.io](https://app.diagrams.net/)  
- [Whimsical](https://whimsical.com/)  

---

### 3. Wireframing
Wireframes represent the **UI layout** without detailed styling.  
They show **screen structure, menus, buttons, navigation flow**.  

**Types of Wireframes**:
- Low-fidelity → Simple boxes & placeholders  
- Mid-fidelity → Interactive but minimal visuals  
- High-fidelity → Close to final UI look  

📌 Tools:  
- [Figma](https://www.figma.com/)  
- [Balsamiq](https://balsamiq.com/)  
- [Adobe XD](https://www.adobe.com/products/xd.html)  

📚 Reference: [Wireframing Basics – Interaction Design Foundation](https://www.interaction-design.org/literature/topics/wireframing)

---

### 4. Storyboarding
Storyboards illustrate **how a user interacts step-by-step** with the app in a **narrative format**.  
They help visualize **real-world usage scenarios**.  

**Example Use Case:**
- A student opens the app to track assignments → receives reminders → marks tasks as complete.  

📌 Tools:  
- [Storyboard That](https://www.storyboardthat.com/)  
- [Canva Storyboards](https://www.canva.com/storyboards/)  

📚 Reference: [UX Storyboarding – Nielsen Norman Group](https://www.nngroup.com/articles/storyboards-ux/)

---

### 5. Prototyping
After wireframes and storyboards, build an **interactive prototype**.  
- Validate workflows with real users.  
- Identify usability issues before development.  

📌 Tools:  
- [Figma Prototyping](https://help.figma.com/hc/en-us/articles/360040528314-Create-prototypes-in-Figma)  
- [InVision](https://www.invisionapp.com/)  

---

### 6. Iteration & Feedback
- Conduct **usability testing**.  
- Collect feedback.  
- Refine design before coding begins.  

📚 Reference: [Usability Testing Guide – Usability.gov](https://www.usability.gov/how-to-and-tools/methods/usability-testing.html)

---

## 🧪 Best Practices

* Keep flows **simple & intuitive**.
* Test wireframes with **real users early**.
* Ensure **consistency** across screens.
* Focus on **accessibility** (font sizes, contrast, keyboard navigation).

📚 Reference: [Microsoft UX Guidelines](https://learn.microsoft.com/en-us/windows/apps/design/)

---

## ✅ Summary

By using:

* **Flow diagrams** → Define user journeys
* **Wireframes** → Visualize screen layouts
* **Storyboards** → Capture real-world scenarios

… you create a clear design blueprint for a desktop app, reducing errors and improving usability.

---

