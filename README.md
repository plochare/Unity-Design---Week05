# Unity Design - Week 05

## Agenda

1. [Features-Review](#Unity-Features)  
2. [Lighting System](#Unity-Lighting)  
3. [Tweening with DOTween](#DO-Tween)  
4. [Animator Controllers / Mixamo](#Animator-Controller) 
5. [Unity Mobile App Build](#Unity-Mobile-App-Build-Process)  
6. [Unity Remote](#Unity-Remote)  
7. [Cross-Platform UI](#Cross-Platform-UI)
8. [Midterm Assignment](#Midterm-Assignment)  
9. [Design Process : Wireframes, Flow Diagrams & Storyboards](#Design-Process)  

---

# 🌟 Unity Lighting

Lighting is one of the most powerful tools in Unity for creating mood, realism, and atmosphere in your games. It not only affects how objects look but also impacts performance and player immersion.

---

## 🔦 What is Lighting in Unity?

In Unity, **lighting** determines how objects in a scene are illuminated and perceived by the camera. Lighting can simulate sunlight, lamps, glowing effects, or even abstract moods.

Unity’s Lighting system consists of three main parts:

1. **Light Components** (Directional, Point, Spot, Area)
2. **Global Illumination (GI)** (real-time or baked light bounces)
3. **Environment Lighting** (skybox, ambient light, reflection probes)

---

## 💡 Types of Light Components

Unity provides several built-in light types, each useful for different scenarios:

| Light Type                  | Use Case                                   | Icon | Notes                                              |
| --------------------------- | ------------------------------------------ | ---- | -------------------------------------------------- |
| **Directional Light**       | Simulates sunlight or moonlight            | ☀️   | Parallel rays; great for outdoor scenes.           |
| **Point Light**             | Emits light in all directions from a point | 💡   | Ideal for lamps, torches, glowing objects.         |
| **Spot Light**              | Cone-shaped light, like a flashlight       | 🔦   | Perfect for focused effects like stage lights.     |
| **Area Light** (Baked only) | Rectangular light source                   | 📐   | Great for soft indoor lighting, windows, or lamps. |

---

## ⚙️ Key Light Component Properties

When you select a Light in Unity, you’ll see these common settings in the **Inspector**:

* **Type** → Directional, Point, Spot, Area
* **Color** → Tint of the light (warm, cool, dramatic tones)
* **Intensity** → Brightness of the light
* **Range** (Point/Spot only) → Distance the light reaches
* **Spot Angle** (Spot only) → Width of the cone
* **Shadows** → Enable and adjust **Hard** or **Soft Shadows**
* **Mode** →

* **Realtime** – Updates every frame, dynamic but costly
* **Baked** – Precomputed, static, performance-friendly
* **Mixed** – Combines real-time and baked

---

## 🌍 Global Illumination (GI)

Lighting doesn’t stop at direct rays — **GI** simulates how light bounces around the scene.

* **Realtime GI** → Updates dynamically (good for day/night cycles).
* **Baked GI** → Precomputed for static environments (faster runtime performance).

Unity’s **Lighting Window** (`Window > Rendering > Lighting`) lets you configure GI, skyboxes, ambient light, and reflection probes.

---

## 🖼️ Environment Lighting

* **Skybox** → Wraps the scene with a 360° texture (day, night, fantasy worlds).
* **Ambient Light** → Adds soft light to areas not hit by direct lights.
* **Reflection Probes** → Capture reflections for shiny surfaces.

---

## 📌 Creative Core: Lighting Tutorial
https://learn.unity.com/tutorial/get-started-with-lighting?version=6.0

---

## 📌 Live Session Video: Lighting
** This session explores different types of light and their specific use cases.**
You will learn about:
- color temperature, intensity and exposure
- the relationship between light and shadow and how these direct the viewer’s eye
- use volumetric light to add polish by creating “god rays,”
- how to output final shots via the recorder

- https://learn.unity.com/tutorial/lighting-july-28-2021

---

# 🎬 Unity DOTween

Tweening (short for *in-betweening*) lets you smoothly animate values over time, such as movement, rotation, scale, UI transitions, or color changes.

**[DOTween](http://dotween.demigiant.com/)** is a fast, efficient, full-featured tweening engine for Unity. It’s widely used in professional projects because of its flexibility and performance.

---

## 🚀 What is DOTween?

DOTween provides:

* ✨ **Powerful animations** for any value, not just transforms.
* 🔗 **Sequences** – chain multiple tweens in order.
* 🎛️ **Ease of use** – simple one-liners to animate properties.
* ⚡ **High performance** – optimized for mobile and consoles.
* 🛠️ **Extensibility** – works with custom variables, UI, shaders, and more.

---

## 📦 Installation

### Option 1: Unity Asset Store

1. Open **Unity Asset Store** inside the Unity Editor.
2. Search for **DOTween** and install the free version (or Pro for extra features).

### Option 2: Package from Website

1. Download DOTween from [dotween.demigiant.com](http://dotween.demigiant.com/).
2. Import the `.unitypackage` into your Unity project.
3. Run **DOTween Utility Panel** via `Tools > Demigiant > DOTween Utility Panel` to set up.

---

## 💡 Basic Usage

Here’s how to move a GameObject smoothly across the screen:

```csharp
using UnityEngine;
using DG.Tweening; // DOTween namespace

public class DoTweenExample : MonoBehaviour
{
    void Start()
    {
        // Move the object to position (5, 2, 0) over 2 seconds
        transform.DOMove(new Vector3(5f, 2f, 0f), 2f);
    }
}
```

---

## 🛠️ DOTween API Overview

### 1. Move Object

```csharp
transform.DOMove(Vector3 targetPosition, float duration);
```

Moves a GameObject to the target position.

---

### 2. Rotate Object

```csharp
transform.DORotate(new Vector3(0, 180, 0), 2f);
```

Rotates a GameObject smoothly.

---

### 3. Scale Object

```csharp
transform.DOScale(Vector3.one * 2f, 1f);
```

Doubles the size of the object in 1 second.

---

### 4. Color Tween

```csharp
spriteRenderer.DOColor(Color.red, 1.5f);
```

Tweens a sprite’s color over 1.5 seconds.

---

### 5. UI Tweening

```csharp
myText.DOFade(0f, 2f);  // fade out text
myImage.DOFillAmount(1f, 3f); // fill radial image
```

---

### 6. Sequences

Chain multiple animations together:

```csharp
Sequence mySequence = DOTween.Sequence();
mySequence.Append(transform.DOMoveX(3, 1f))
          .Append(transform.DOScale(Vector3.one * 2, 0.5f))
          .Append(transform.DORotate(new Vector3(0, 180, 0), 1f))
          .OnComplete(() => Debug.Log("Sequence Finished!"));
```

---

### 7. Looping

```csharp
transform.DOMoveY(3f, 1f).SetLoops(-1, LoopType.Yoyo);
```

Makes the object bounce up and down forever.

---

### 8. Callbacks

```csharp
transform.DOMoveX(5, 2f).OnComplete(() => Debug.Log("Done!"));
```

---

## 🎮 Tutorial: Bouncy UI Button

Let’s animate a UI button to “bounce” when clicked:

```csharp
using UnityEngine;
using UnityEngine.UI;
using DG.Tweening;

public class DoTweenButton : MonoBehaviour
{
    public Button myButton;

    void Start()
    {
        myButton.onClick.AddListener(() =>
        {
            myButton.transform.DOScale(Vector3.one * 1.2f, 0.2f)
                .SetEase(Ease.OutBack)
                .OnComplete(() =>
                {
                    myButton.transform.DOScale(Vector3.one, 0.2f).SetEase(Ease.InBack);
                });
        });
    }
}
```

✅ Result: The button pops when clicked with a smooth easing curve.

---

## 📸 Example Scenarios

* Smooth **camera transitions**
* UI elements sliding in/out
* Looping **background animations**
* Complex cutscene animations using **Sequences**
* Sprite flashing or color transitions

---

## 📚 Resources

* [DOTween Official Documentation](http://dotween.demigiant.com/documentation.php)
* [DOTween Pro](http://dotween.demigiant.com/pro.php) – extended features for UI and visual tools


# 📱 Unity Mobile App Build Process

This guide covers **how to build and publish Unity applications** for **Android** and **iOS** platforms.  

---

## 📌 Prerequisites

### Common Requirements
- Unity installed with **Android Build Support** and/or **iOS Build Support**.
- **Scripting Backend**: IL2CPP recommended.
- **Player Settings** configured (bundle ID, version, orientation, icons).
- Device for testing (Android phone/tablet or iPhone/iPad).

### Tools
- **Android**:
  - Android Studio (for SDK & NDK)
  - Java JDK (usually comes with Android Studio)
- **iOS**:
  - Xcode installed on macOS

---

## ⚙️ Configuring Player Settings

1. Open **File → Build Settings → Player Settings**.
2. Configure the following:

| Setting | Android | iOS |
|---------|---------|-----|
| **Company & Product Name** | ✅ | ✅ |
| **Package/Bundle Identifier** | `com.yourcompany.app` | `com.yourcompany.app` |
| **Version & Build Number** | ✅ | ✅ |
| **Orientation** | Portrait / Landscape | Portrait / Landscape |
| **Icon & Splash Screen** | ✅ | ✅ |
| **Minimum OS Version** | API level 19+ (Android) | iOS 11+ |

---

## 📦 Android Build Process

1. Open **File → Build Settings → Android** → Switch Platform.
2. Connect **Android device** (optional) or build APK/ABB.
3. Configure **Build Settings**:
   - Target: **Google Android**
   - Texture Compression: **ASTC recommended**
   - Build System: **Gradle**
4. Click **Build** or **Build and Run**.
5. Locate APK/AAB file in chosen directory.
6. Test the build on a real device.

**Publishing to Google Play:**
1. Sign APK/AAB using a keystore.
2. Upload the signed APK/AAB to [Google Play Console](https://play.google.com/console).
3. Fill store listing, screenshots, and submit for review.

Official Unity docs: [Android Build Process](https://docs.unity3d.com/6000.2/Documentation/Manual/android-BuildProcess.html)

---

## 📦 Google Play Setup
- https://developer.android.com/games/pgs/unity/unity-start

---

## 🍎 iOS Build Process

1. Open **File → Build Settings → iOS** → Switch Platform.
2. Click **Build** → choose a folder → Unity generates an **Xcode project**.
3. Open the generated project in **Xcode**.
4. Configure in Xcode:
   - **Signing & Capabilities** → select team and provisioning profile.
   - **Bundle Identifier** matches Player Settings in Unity.
   - Target device and deployment target.
5. Build and run on device or simulator.

**Publishing to App Store:**
1. Archive the app in Xcode (`Product → Archive`).
2. Upload to App Store Connect.
3. Fill app metadata, screenshots, and submit for review.

Official Unity docs: [iOS Build Process](https://docs.unity3d.com/6000.2/Documentation/Manual/iphone-BuildProcess.html)

---

## 🍎 IOS Build Tutorial
- https://learn.unity.com/tutorial/publishing-for-ios

---

## 🛠️ Build Tips

- Use **IL2CPP** for better performance and app store compliance.
- Enable **Development Build** for testing with logs.
- Test frequently on real devices to check performance and resolution.
- Optimize textures, audio, and assets for mobile performance.
- Keep Android min SDK and iOS deployment target consistent with your target devices.

---

## 📚 Further Learning

- [Unity Manual: Android Build](https://docs.unity3d.com/6000.2/Documentation/Manual/android-BuildProcess.html)  
- [Unity Manual: iOS Build](https://docs.unity3d.com/6000.2/Documentation/Manual/iphone-BuildProcess.html)  

---

# 📱 Unity Remote 

**Unity Remote** is a tool that allows you to **test and play your Unity project on a mobile device in real-time** without building the full application.  
This guide explains Unity Remote and provides a step-by-step tutorial for setup and use.

Unity Remote lets you:

- Stream your **Game View** from the Unity Editor to a mobile device.
- Test **touch input, accelerometer, and device sensors** without building the full APK or iOS app.
- Rapidly iterate on mobile UI and controls during development.

> Note: Unity Remote is for **testing only**; final performance may differ on actual builds.

---

## ⚙️ Prerequisites

### Common Requirements

- Unity installed.
- Mobile device (Android or iOS).
- USB cable for device connection.
- Unity Remote app installed on the device:
  - **Android**: [Unity Remote 5 on Google Play](https://play.google.com/store/apps/details?id=com.unity3d.unityremote5)
  - **iOS**: [Unity Remote 5 on App Store](https://apps.apple.com/us/app/unity-remote-5/id1089798891)

---

## 🔧 Setting Up Unity Remote

### Step 1: Connect Your Device

1. Plug your mobile device into your computer via USB.
2. Enable **Developer Mode / USB Debugging**:
   - Android: Enable **Developer Options → USB Debugging**.
   - iOS: Enable **Developer Mode** in Settings.

---

### Step 2: Configure Unity Editor

1. Open **Edit → Project Settings → Editor**.
2. Under **Unity Remote**, set:
   - **Device**: `Any Android Device` or `Any iOS Device`
   - **Compression Method**: `JPEG` or `PNG`
   - **Resolution**: `Original` or `Scaled`
3. Open **File → Build Settings → Platform**:
   - Ensure your platform matches your device (Android or iOS).

---

### Step 3: Launch Unity Remote

1. Open the **Unity Remote app** on your device.
2. Click **Play** in the Unity Editor.
3. The Game View will stream directly to your mobile device.
4. Interact with the game using **touch, tilt, or other mobile inputs**.

---

## 🛠️ Testing Controls and Input

- **Touch Input**: Unity Remote sends touch positions from device to Editor.
- **Accelerometer**: Device tilt affects `Input.acceleration` in Unity.
- **Gyroscope**: Device rotation affects `Input.gyro` data.

**Example Script for Touch Input:**

```csharp
using UnityEngine;

public class TouchExample : MonoBehaviour
{
    void Update()
    {
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);
            Vector3 worldPos = Camera.main.ScreenToWorldPoint(touch.position);
            Debug.Log("Touch position: " + worldPos);
        }
    }
}
````

Attach this to a GameObject and see touch positions logged while using Unity Remote.

---

## 🚀 Tips

* Use **Unity Remote** early in development to quickly iterate on mobile UI and gameplay.
* Remember **performance will differ** from actual builds; use Unity Remote for input and layout testing only.
* Disable **Unity Remote** for final testing on actual device builds to check performance, resolution, and frame rate.

---

## 📚 Further Learning

* [Unity Manual: Unity Remote](https://docs.unity3d.com/Manual/UnityRemote5.html)
* [Unity Learn: Mobile Development](https://learn.unity.com/tutorial/mobile-game-development)

---

## Mobile Development Tutorial

This endless runner game example (Trash Dash) is optimised for mobile, it shows the use of object pooling, origin reset, asset bundles, additive scene loading and a curved world shader.
- [Endless Runner Sample Game](https://learn.unity.com/tutorial/mobile-development-techniques)

---

# Interactive Guide: Cross-Platform UI
The Interactive Guide to Cross-Platform UI is a set of guided, in-editor tutorials for experienced Unity creators seeking to learn the workflows used for creating cross-platform user interfaces.
- [Cross-Platform UI Project](https://assetstore.unity.com/packages/templates/tutorials/interactive-guide-cross-platform-ui-208063)

---

# Touch Input System
- https://discussions.unity.com/t/get-up-and-running-with-the-input-system/1620267

---

# 📘 Midterm Assignment: Design an Game Based Learning Application

## 🎯 Objective

The goal of this assignment is to apply **user experience design principles** and **game mechanics** to create a concept for an **interactive learning application**. Students will demonstrate their ability to design educational technology that is **engaging, user-friendly, and motivating**.

---

## 📋 Assignment Description

You will design and present a prototype for an **interactive learning application** (desktop, web, or mobile). The application must teach users a specific skill or subject area (e.g., math drills, language learning, history exploration, science simulations, coding tutorials).

Your design should focus on **how Game Based Learning (GBL) motivates learners** and enhances the educational experience.

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

3. **Prototype (Unity Application)**

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

---

### 4. Storyboarding
Storyboards illustrate **how a user interacts step-by-step** with the app in a **narrative format**.  
They help visualize **real-world usage scenarios**.  

**Example Use Case:**
- A student opens the app to track assignments → receives reminders → marks tasks as complete.  

📌 Tools:  
- [Storyboard That](https://www.storyboardthat.com/)  
- [Canva Storyboards](https://www.canva.com/storyboards/)  

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

---
