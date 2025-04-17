# Mobile App Starter

A comprehensive starter for building Ionic Angular mobile applications wrapped with Capacitor and powered by SQLite, all within a single GitHub Codespace.

## Prerequisites

- A GitHub account
- Basic familiarity with Git and GitHub
- No local installs required—everything runs in Codespaces

## 1. Create your GitHub repository

1. Sign in to GitHub and click **New repository**.  
2. Name it (e.g. `mobile-app-starter`), choose **Public** or **Private**, then click **Create repository**.

## 2. Add the Codespaces configuration

Create a folder named `.devcontainer` at the root of your repo, with a single file:

```jsonc
// .devcontainer/devcontainer.json
{
  "name": "Ionic‑Angular Codespace",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:20",
  "postCreateCommand": "npm install -g @angular/cli @ionic/cli @capacitor/cli sqlite3",
  "forwardPorts": [8100, 4200],
  "remoteUser": "node"
}
```

- **Node 20** satisfies Capacitor v5+ requirements.  
- Installs Angular CLI, the new Ionic CLI (`@ionic/cli`), Capacitor CLI, and the `sqlite3` binary.  
- Forwards ports **8100** (Ionic serve) and **4200** (pure Angular).

Commit and push:

```bash
git add .devcontainer/devcontainer.json
git commit -m "chore: add devcontainer (Node20 + Ionic/Angular/Capacitor/SQLite)"
git push origin main
```

## 3. Rebuild the Codespace

After pushing the `.devcontainer` folder, rebuild so Codespaces picks up the new settings:

- **In VS Code:** `Ctrl+Shift+P` → **Codespaces: Rebuild Container** → Confirm.  
- **In GitHub UI:** Repo → **Codespaces** tab → **…** on your codespace → **Rebuild Container**.

## 4. Verify the Ionic CLI version

In the terminal:

```bash
ionic --version
# should show 7.x.x or later
```

If it’s older, run:

```bash
npm uninstall -g ionic
npm i -g @ionic/cli
```

## 5. Scaffold the Ionic Angular app

```bash
# Navigate to your workspace root
cd /workspaces/$(basename $(git rev-parse --show-toplevel))

# Generate a blank Ionic Angular project with Capacitor
ionic start mobile-app blank \
  --type=angular \
  --capacitor \
  --no-interactive

# Enter the project folder
cd mobile-app
```

*The Ionic CLI will run `npx cap init` for you without the obsolete `--npm-client` flag.*

## 6. Install the SQLite plugin

```bash
npm install @capacitor-community/sqlite
npx cap sync
```

## 7. Add native platforms

```bash
npm install @capacitor/android
npx cap add android

npm install @capacitor/ios
npx cap add ios
```

You should now see `android/` and `ios/` folders in your project root.

## 8. Build & sync your web assets

By default Capacitor expects a `www/` folder. To stick with this convention, run:

```bash
ionic build        # generates the ./www folder
npx cap copy       # copies www/ into native projects
npx cap sync       # wires in plugins
```

> **TL;DR:** If you want to stick with `www/`, just run `ionic build` **before** `npx cap sync`.

**Alternative:** To use Angular’s `dist/mobile-app` output instead, update `webDir` in `capacitor.config.ts`:

```ts
const config: CapacitorConfig = {
  appId: 'io.ionic.starter',
  appName: 'mobile-app',
  webDir: 'dist/mobile-app',
  bundledWebRuntime: false
};
```

Then:

```bash
npm run build
npx cap copy
npx cap sync
```

## 9. Preview & test

- **Browser (PWA):**
  ```bash
  ionic serve --host 0.0.0.0
  ```  
  Forward port **8100** in the Codespaces “Ports” panel.

- **Android APK:**
  ```bash
  cd android
  ./gradlew assembleDebug
  ```  
  Your debug APK is at `android/app/build/outputs/apk/debug/app-debug.apk`.

- **iOS (Xcode on macOS):**
  ```bash
  npx cap open ios
  ```  
  Opens the Xcode workspace—build/run on simulator or device.

## 10. Commit & push your starter

```bash
git add .
git commit -m "feat: initialize Ionic Angular + Capacitor + SQLite starter"
git push origin main
```

---

🎉 You now have a fully configured **Ionic Angular + Capacitor + SQLite** stack running in a single GitHub Codespace—ready for development, live previews, and native builds. Happy coding!

