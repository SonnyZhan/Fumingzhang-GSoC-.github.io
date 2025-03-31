# MIT App Inventor GSoC Application Answers - Fuming Zhang

**Email:** [fumingzhang@umass.edu](mailto:fumingzhang@umass.edu)

This document contains my application answers for the MIT App Inventor Google Summer of Code program.

---

## Short-Answer Questions

### Interest in App Inventor
As a former TA for a CS class outside university, I believe App Inventor is an excellent tool for inspiring beginners. It democratizes app development and makes technology accessible. Joining MIT App Inventor will give me the hands‑on experience I need and inspire my journey, just as many other beginners.

### Interest in Introductory Programming 
I have taught programming fundamentals to some of friends through peer tutoring and informal workshops. I excel at translating complex ideas into engaging, accessible lessons that build confidence, foster creativity, and ignite a lifelong passion for coding.

### Proposed summer project
I would love to work on Trainable ChatBot interface and AI component with potential mentor Natalie Lao

### Experience with Development Tools
In my UMass Meal Planner project, I leveraged my expertise in front‑end and back‑end technologies to build a dynamic and fully integrated meal planning system for UMass students. I used **React.js, HTML, and CSS** to create a responsive, user-friendly interface, and a secure **Django** back-end to manage user authentication, data processing, and meal scheduling. I also implemented web scraping techniques to fetch real‑time dining menu data via APIs and applied data science methods to analyze nutritional values. This project not only deepened my practical skills in full‑stack development but also taught me how to maintain a scalable and maintainable codebase. You can review my code on my [GitHub repository](https://github.com/SonnyZhan/Meal-planner-app---CS320) 

### Experience with Teams, Online Developer Communities, and Large Code Bases
Throughout my academic and professional journey, I have had extensive experience collaborating with diverse teams and managing large code bases. During my internship at Accoladi Auditions, I worked as part of a cross-functional team where we developed and maintained an Angular-based messaging interface that served over 40,000 users. This environment honed my skills in agile development, code reviews, and continuous integration practices. Additionally, projects like the UMass Meal Planner have involved coordinated efforts with classmates to build a robust full‑stack application, reinforcing my ability to navigate complex repositories and maintain clear documentation. I also actively participate in online developer communities on platforms such as GitHub and Stack Overflow, where I contribute code, provide assistance, and engage in discussions, further strengthening my collaborative and problem‑solving abilities.

### Challenge 1: Creating a non-trivial app

### AIA format: https://drive.google.com/file/d/1QjcC2jNvbskeQG8qidP_iYoJyfGyLJbo/view?usp=sharing

### APK format: https://drive.google.com/file/d/1Av6n57iRvI0kCVpeXE4VWTh8241wlZz7/view?usp=sharing


### Challenge 2: Design Challenge – Enhancing the Camera Operation

#### Analysis of the 'useFront' Property Issue
After reviewing Android documentation and digging through the code, I discovered that the 'useFront' property stopped working because the legacy Camera API (android.hardware.Camera) was deprecated in Android 5.0 (API level 21) and replaced by newer APIs like CameraX. The current App Inventor implementation still relies on outdated methods, which newer Android versions no longer support.

#### Part 2: Auto-Capture Feature Design
To modernize the Camera component and add enhanced functionality, I propose an auto-capture feature that allows the camera to take pictures automatically without requiring user intervention. Here’s the design:

**New Properties:**
- **AutoCaptureEnabled (boolean):** Enables/disables auto-capture.
- **CaptureInterval (number):** Time between captures in milliseconds.
- **MaxCaptures (number):** Maximum number of pictures to take (0 for unlimited).

**New Methods:**
- **StartAutoCapture():** Initiates auto-capture.
- **StopAutoCapture():** Terminates auto-capture.
- **PauseAutoCapture()/ResumeAutoCapture():** (Optional) Control auto-capture temporarily.

**New Events:**
- **AutoCaptureStarted:** Fired when auto-capture begins.
- **AutoCaptureStopped:** Fired when auto-capture ends.
- **CaptureLimitReached:** Fired when the maximum number of captures is reached.

**Implementation Approach:**
I would extend the existing Camera component to maintain compatibility with current apps. The main modifications would occur in:
- **Camera.java (component definition):** Add new properties, methods, and events.
- **CameraComponent.java (Android implementation):** Implement the auto-capture functionality.
- **Camera.java (block editor):** Expose the new features to users.

Below is a simplified code snippet demonstrating how the `StartAutoCapture()` method could be implemented:

```java
private Handler autoCaptureHandler;
private Runnable autoCaptureRunnable;
private int captureCount = 0;

public void StartAutoCapture() {
    if (!autoCaptureEnabled) return;
    
    autoCaptureHandler = new Handler(Looper.getMainLooper());
    autoCaptureRunnable = new Runnable() {
        @Override
        public void run() {
            if (autoCaptureEnabled && (maxCaptures == 0 || captureCount < maxCaptures)) {
                takePicture();
                captureCount++;
                autoCaptureHandler.postDelayed(this, captureInterval);
            }
        }
    };
    autoCaptureHandler.post(autoCaptureRunnable);
}
