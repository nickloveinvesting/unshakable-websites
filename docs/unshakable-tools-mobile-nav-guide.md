# Updating the Unshakable Tools Navigation for Mobile

This guide walks through the exact edits that were made to `unshakable-tools.html` so that the tab buttons feel great on phones while preserving the desktop layout. You can follow the steps below even if you are brand new to HTML/CSS.

## 1. Open the File
1. Launch your preferred code editor (VS Code, Sublime Text, etc.).
2. Open the repository folder.
3. Open `unshakable-tools.html` from the file list.

> **Tip:** Always make a copy of the file before editing (for example, `unshakable-tools-backup.html`) so you can revert if needed.

## 2. Add the Mobile Helper Styles
1. Scroll near the top of the file where the `<style>` block lives.
2. Look for the existing rules for `.nav-button.active`.
3. Immediately after that section, paste the following CSS helper so touch devices can swipe without scrollbars showing:
   ```html
   /* Utility to hide scrollbars for touch-friendly strips */
   .no-scrollbar {
       -ms-overflow-style: none; /* IE and Edge */
       scrollbar-width: none;    /* Firefox */
   }
   .no-scrollbar::-webkit-scrollbar {
       display: none;           /* Chrome, Safari, Opera */
   }
   ```
4. Save the file.

## 3. Replace the Navigation Markup
1. In the `<nav>` section, locate the **desktop buttons**: the `<div class="hidden sm:flex ...">` wrapper.
2. Update each button so it has the `nav-button` class and remove any duplicated styling. Each button should look like this:
   ```html
   <button data-view="rehabEstimatorView" class="nav-button ...">Rehab Estimator</button>
   ```
   Ensure the first button keeps the `active` class so the Rehab Estimator stays selected on load.
3. Directly after the desktop block, insert the **mobile-only buttons**:
   ```html
   <!-- Navigation Links (Mobile) -->
   <div class="sm:hidden w-full">
       <div class="grid grid-cols-3 gap-2 no-scrollbar">
           <button data-view="rehabEstimatorView" class="nav-button active ...">Rehab Estimator</button>
           <button data-view="profitAnalysisView" class="nav-button ...">Profit Analysis</button>
           <button data-view="submitDealView" class="nav-button ...">Submit Deal</button>
       </div>
   </div>
   ```
4. Save the file once more.

## 4. Keep the Buttons in Sync (JavaScript)
1. Near the bottom of the file, inside the `<script>` tag, find the `setupViewSwitcher()` function.
2. Confirm that the first lines inside the function read:
   ```javascript
   const navButtons = document.querySelectorAll('.nav-button');
   const views = document.querySelectorAll('.view-container');
   ```
   This single query grabs both the desktop and mobile buttons.
3. Inside the click handler, verify the `navButtons.forEach` loop that toggles the `active` class:
   ```javascript
   navButtons.forEach(btn => {
       btn.classList.toggle('active', btn.dataset.view === targetViewId);
   });
   ```
   This ensures whichever button you tap (mobile or desktop) highlights in both places.
4. No further JavaScript changes are required.

## 5. Test Locally
1. Open the file in your browser by double-clicking it or running a simple static server (e.g., VS Code Live Server).
2. Resize the browser window to a phone width or use the device toolbar in dev tools to confirm:
   - Buttons display in a tidy three-column grid.
   - Swiping the nav strip does not show scrollbars.
   - Switching tabs updates both the desktop and mobile button states.
3. Return to a desktop-sized window to confirm nothing changed on large screens.

## 6. Commit Your Changes
1. Run `git status` to confirm only `unshakable-tools.html` changed.
2. Stage and commit:
   ```bash
   git add unshakable-tools.html
   git commit -m "Improve mobile navigation for Unshakable tools"
   ```
3. Push your branch as usual.

Following these steps will reproduce the updated mobile navigation exactly as demonstrated. Feel free to customize the button copy or gradients, but keep the class names and structure the same so the JavaScript keeps working.
