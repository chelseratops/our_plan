# Our Week: setup guide

A wall display for her iPad: the custody schedule as a colored band across a multi-week calendar, your other calendar events, and a Google Tasks chore list she can check off. Everything runs in the browser; there is no server and no subscription.

You need three things: somewhere free to host the files, a Google Cloud OAuth Client ID, and about 20 minutes.

## 1. Host the files (GitHub Pages, free)

1. Create a GitHub account if you don't have one, then create a new **public** repository, for example `our-week`.
2. Upload everything in this folder (`index.html`, `manifest.webmanifest`, `sw.js`, `README.md`, and the icon images) to the repository.
3. In the repository, go to **Settings > Pages**, set Source to **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute your app is live at `https://YOUR-USERNAME.github.io/our-week/`.

The repository only holds app code. Your calendar data and Client ID never go into it.

## 2. Create a Google OAuth Client ID

1. Go to https://console.cloud.google.com and create a new project (name it anything, like "Our Week").
2. **APIs & Services > Library**: enable **Google Calendar API** and **Google Tasks API**.
3. **APIs & Services > OAuth consent screen** (also labeled "Google Auth Platform"):
   - User type: **External**. App name: "Our Week". Add your email where asked.
   - Leave publishing status as **Testing**.
   - Under **Test users** (or Audience), add the Google account the display will sign in with.
4. **APIs & Services > Credentials > Create credentials > OAuth client ID**:
   - Application type: **Web application**.
   - Authorized JavaScript origins: `https://YOUR-USERNAME.github.io`
   - Authorized redirect URIs: `https://YOUR-USERNAME.github.io/our-week/` (the trailing slash matters; the setup screen shows the exact values to copy)
5. Copy the Client ID. It ends in `.apps.googleusercontent.com`.

When you sign in, Google will warn that the app isn't verified. That's expected for a personal app in Testing mode: tap **Continue**.

## 3. Set up her iPad

1. Open your GitHub Pages address in **Safari**.
2. Paste the Client ID, tap **Save and connect**, and sign in.
3. Parent settings open automatically. Pick the calendars to show, the custody calendar, and the chore list.
4. Tap **Share > Add to Home Screen**, then open Our Week from the home screen.
5. Sign in once more inside the home-screen app (it keeps its own storage, separate from Safari).

To make it behave like a wall display:
- Settings > Display & Brightness > Auto-Lock > **Never** (the app also asks the screen to stay on while it's open).
- Settings > Accessibility > **Guided Access** on, then triple-click the side or top button while Our Week is open to lock the iPad to it.

## Using it

- **Parent settings**: press and hold the gear in the top corner for about a second. A quick tap does nothing, so she can't change things by accident.
- **Custody band**: each day shows whoever has her at 8 pm, so exchange days show where she sleeps that night. Works with all-day events ("Dad's house", Fri to Fri) and timed events (Fri 6 pm to Fri 6 pm). Set a different **event color** for each parent's events in Google Calendar and the band uses those colors. Without colors, each title gets its own color automatically.
- **Chores**: shows tasks due today, overdue tasks (labeled with the day they were due), and tasks with no date. Subtasks show indented under their parent. Checked tasks stay crossed off until midnight, then disappear. Add task notes for extra instructions; they show in small text under the chore.
- **Refreshing**: pulls new data every 5 minutes and whenever the screen wakes. If Wi-Fi drops, it keeps showing the last saved schedule.
- Tap any day to see its full event list.

## Things to know

**Which Google account.** Google Tasks lists can't be shared between accounts, so the chore list has to live in whichever account the display signs into. Two good setups:
- *Simplest*: sign the display into your own account. Note this means a signed-in Google session for your account exists inside the app on her iPad (the app itself only asks for read access to calendars and access to Tasks).
- *Cleaner*: make a separate Google account for the display (or use her Family Link account). Share the custody calendar and any others with it from your account, and add that account in the Google Tasks app on your phone so you can manage her chores from there.

**Repeating chores.** Google Tasks repeating tasks are managed by Google's own apps, and Google's Tasks API doesn't fully expose repeat settings. Test one repeating chore: check it off on the display and confirm the next occurrence appears in the Google Tasks app. If it doesn't, the reliable workaround is adding a week's chores at once with due dates (quick to do in the Google Tasks app), so each day's list appears on its own.

**Signing in again.** Google's sign-in tokens last about an hour. The app renews quietly in the background, which may briefly flash a Google page. If Google ever needs a tap, the display shows a "Sign in with Google" button.

**Updating the app.** Edit files in the GitHub repository; the iPad picks up the new version the next time it reloads.
