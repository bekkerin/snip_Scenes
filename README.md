# snip_Scenes
new version of the scenes switching project

Note: this project was created with the CoPilot prompt: "Snippets. YOu are a professor teaching students game programming with Unity. You want to provide some simple examples ("snippets") of common functions. the first iss how to transition between scenes. Write step by step instructions for constructing a simple project with three scenes ("Intro", "Game", "Exit"). A button on Intro should take the player to Game, a button on Game should take the player to Exit, and two buttons on Exit should either exit the game or go back to Intro. Also add a countdown timer to Game that automatically shifts to Exit after 15 seconds. Use Textmesh Pro, not the legacy UI objects."

# 🎮 Unity Scene Transition Tutorial: Intro → Game → Exit

## 🧱 Step 1: Set Up the Project and Scenes
1. Create a new **Unity 2D project**.
2. In the `Assets` folder, create three new scenes:
   - `Intro.unity`
   - `Game.unity`
   - `Exit.unity`
3. Save each scene and add them to **Build Settings**:
   - Go to `File > Build Settings`
   - Click `Add Open Scenes` for each one
   - Ensure they're listed in this order:
     ```
     0 - Intro
     1 - Game
     2 - Exit
     ```

## 🎨 Step 2: Design Each Scene

### Intro Scene
- Add a `Canvas` and a `Button` labeled **Start Game**
- Rename the button to `StartButton`

### Game Scene
- Add a `Canvas` and a `Button` labeled **Go to Exit**
- Rename the button to `ExitButton`
- Add a `TextMeshPro - Text (UI)` element to show the countdown timer
- Rename the text to `TimerText`

### Exit Scene
- Add a `Canvas` and two `Buttons`:
  - **Restart** → rename to `RestartButton`
  - **Quit** → rename to `QuitButton`

## 🧠 Step 3: Create the Scene Manager Script

Create a new C# script called `SceneController.cs` and attach it to an empty GameObject in each scene.

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneController : MonoBehaviour
{
    public void LoadScene(string sceneName)
    {
        SceneManager.LoadScene(sceneName);
    }

    public void QuitGame()
    {
        Application.Quit();
        Debug.Log("Game Quit"); // For editor testing
    }
}

## 🧩 Step 4: Hook Up Buttons to SceneController

1. In each scene (`Intro`, `Game`, `Exit`), add an empty GameObject and name it `SceneManager`.
2. Attach the `SceneController.cs` script to the `SceneManager` GameObject.

3. For each button in the scene, configure the **OnClick()** event in the Inspector:

### Intro Scene
- Select `StartButton`
- In the **OnClick()** section:
  - Click the `+` button to add a new event
  - Drag the `SceneManager` GameObject into the object field
  - Select `SceneController → LoadScene(string)`
  - Type `"Game"` as the argument

### Game Scene
- Select `ExitButton`
- In the **OnClick()** section:
  - Click the `+` button to add a new event
  - Drag the `SceneManager` GameObject into the object field
  - Select `SceneController → LoadScene(string)`
  - Type `"Exit"` as the argument

### Exit Scene
- Select `RestartButton`
  - Add OnClick event → `SceneController → LoadScene("Intro")`
- Select `QuitButton`
  - Add OnClick event → `SceneController → QuitGame()`

> 💡 Tip: Make sure the scene names match exactly with those listed in Build Settings.

## ⏱️ Step 5: Add Countdown Timer to Game Scene

1. In the **Game** scene, create an empty GameObject and name it `TimerManager`.
2. Create a new C# script called `CountdownTimer.cs` and attach it to `TimerManager`.

### CountdownTimer.cs

```csharp
using UnityEngine;
using UnityEngine.SceneManagement;
using TMPro; // Import TextMeshPro namespace

public class CountdownTimer : MonoBehaviour
{
    public float timeRemaining = 15f;
    public TextMeshProUGUI timerText;

    void Update()
    {
        if (timeRemaining > 0)
        {
            timeRemaining -= Time.deltaTime;
            // Rounds the timeRemaining float up to the nearest whole number.
            // For example, 14.2 becomes 15. This avoids showing fractions of a second.
            timerText.text = "Time: " + Mathf.Ceil(timeRemaining).ToString(); 
        }
        else
        {
            SceneManager.LoadScene("Exit");
        }
    }
}

