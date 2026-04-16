# Lab Activity: Building the Profile Tab (Tab 5)

## Objective

Build a **Profile** screen for a mobile streaming app. The Profile page should display a user avatar, name, email, plan/membership info, and a list of settings actions (e.g. Edit Profile, Notifications, Privacy, Help, Logout). All data must be **local dummy data** (no authentication, no backend, no API).

You will practice: **Expo Router tabs**, **TypeScript types**, **React Native layout**, **ScrollView**, **touch interactions**, and **Modal** (for editing profile details).

---

## Prerequisites

- Comfortable with React function components and `useState` (and optionally `useEffect`).
- Basic React Native components: `SafeAreaView`, `View`, `Text`, `Image`, `ScrollView`, `TouchableOpacity`, `Switch`, `Modal`, `TextInput`, `Alert`.
- Expo project running (`npx expo start`).
- Styling approach: use **NativeWind (`className`)** *or* **StyleSheet** (use whichever your repository already uses).

---

## Duration

Estimated time: **2–3 hours**.

---

## What You Will Build

1. A **profile header** with:
   - Avatar (local image or remote URL)
   - Display name
   - Email address
   - Optional “Edit” button
2. A **membership card** section (dummy data):
   - Plan name (e.g. “Premium”)
   - Renewal date or status (e.g. “Renews: May 15, 2026”)
3. A **stats row** (dummy data):
   - Items in My List
   - Downloads
   - Watch time (or “Watched” count)
4. A **settings list** (tappable rows) such as:
   - Edit Profile (opens modal)
   - Notifications (toggle)
   - Autoplay (toggle)
   - Download over Wi‑Fi only (toggle)
   - Privacy / Terms / Help (shows `Alert` for this lab)
   - Logout (shows confirmation `Alert`)
5. **Empty/loading states are optional** in this lab, since the profile is local.

**Out of scope for this lab (unless your instructor extends it):** real login, persistence to AsyncStorage, backend profile updates, image picker upload.

---

## Part 1: Add the Profile tab to the bottom tab bar

You will add a new screen file and register it in the tabs layout so it appears in the bottom navigation.

### How the tabs routing works (quick recap)

- The folder **`app/(tabs)/`** contains one file per tab route (example: `search.tsx`, `my-list.tsx`).
- The file **`app/(tabs)/_layout.tsx`** defines `<Tabs>` and multiple `<Tabs.Screen>` entries.
- The `name="..."` on each `<Tabs.Screen>` must match the filename in `app/(tabs)/` (without `.tsx`).

### Step 1.1: Create the Profile screen file

Create **`app/(tabs)/profile.tsx`** with a placeholder screen first:

```tsx
import { View, Text } from 'react-native';

export default function ProfileScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Profile</Text>
    </View>
  );
}
```

Use **`export default`** — Expo Router screens must default-export a component.

### Step 1.2: Register the Profile tab in `app/(tabs)/_layout.tsx`

Open **`app/(tabs)/_layout.tsx`** and add a new `<Tabs.Screen>`:

- **`name="profile"`** — must match `profile.tsx`
- **`options.title`:** “Profile”
- **`tabBarIcon`:** choose a user/account icon (Feather `user` is common)

Save and reload.

**Checkpoint:** A new **Profile** tab appears in the bottom bar and opens your placeholder screen.

---

## Part 2: Define dummy profile data (no login, no API)

The Profile page must be driven by **local dummy data** so every student can build it without backend requirements.

### Step 2.1: Decide where the data lives

Choose one of these approaches:

- **Option A (recommended for this lab):** define dummy data directly in `profile.tsx`
- **Option B:** create a separate file like `data/profile.ts` and import it

### Step 2.2: Define TypeScript types

Create a type for the user:

```ts
type MembershipPlan = 'Free' | 'Standard' | 'Premium';

export interface ProfileUser {
  id: string;
  name: string;
  email: string;
  avatarUri: string; // can be local require(...) or https URL; pick one approach
  plan: MembershipPlan;
  renewsOn: string; // e.g. "May 15, 2026"
}

export interface ProfileStats {
  myListCount: number;
  downloadsCount: number;
  watchHours: number;
}
```

### Step 2.3: Create dummy objects

Create one `USER` object and one `STATS` object:

```ts
const USER: ProfileUser = {
  id: 'u1',
  name: 'Student Name',
  email: 'student@example.com',
  avatarUri: 'https://images.unsplash.com/photo-1527980965255-d3b416303d12?w=200',
  plan: 'Premium',
  renewsOn: 'May 15, 2026',
};

const STATS: ProfileStats = {
  myListCount: 6,
  downloadsCount: 2,
  watchHours: 14,
};
```

**Checkpoint:** Your profile screen can render the name/email from dummy data without any API calls.

---

## Part 3: Build the Profile layout (sections)

### Step 3.1: Screen skeleton

Use a `SafeAreaView` root and a `ScrollView` so the settings list scrolls.

Suggested section order:

1. Header (avatar + name + email + edit button)
2. Membership card (plan + renew date)
3. Stats row (3 values)
4. Settings list (buttons/toggles)

**Checkpoint:** You have the basic structure and it scrolls on smaller screens.

### Step 3.2: Profile header section

Include:

- `Image` avatar (round)
- Name (bold)
- Email (muted)
- Optional “Edit” button (opens modal in Part 4)

Hints:

- Set avatar size explicitly (`width`/`height`) so it doesn’t collapse.
- Use `borderRadius` equal to half the width to make it circular.

### Step 3.3: Membership card

Make a card-like container with:

- “Plan” label and the plan value (`USER.plan`)
- “Renews on” or “Status” line (`USER.renewsOn`)
- Optional badge for Premium/Free

### Step 3.4: Stats row

Render 3 small stat blocks:

- My List: `STATS.myListCount`
- Downloads: `STATS.downloadsCount`
- Watch hours: `STATS.watchHours`

Layout hints:

- Use `flexDirection: 'row'` and `justifyContent: 'space-between'`.
- Keep each stat centered with a number + label.

**Checkpoint:** Stats render neatly and don’t overflow horizontally.

---

## Part 4: Interactions (toggles + edit modal)

This part makes the Profile page feel “real” even with dummy data.

### Step 4.1: Add toggle states

Add local settings state (booleans):

- Notifications enabled
- Autoplay enabled
- Wi‑Fi only downloads enabled

Example:

```ts
const [notificationsEnabled, setNotificationsEnabled] = useState(true);
const [autoplayEnabled, setAutoplayEnabled] = useState(true);
const [wifiOnlyDownloads, setWifiOnlyDownloads] = useState(false);
```

Render each toggle row with a `Switch`.

### Step 4.2: Settings rows (pressable items)

Create a consistent row component pattern:

- Left: icon + label
- Right: chevron icon (or nothing for toggles)
- On press: for this lab, use `Alert.alert(...)` or open a modal

Recommended actions:

- **Edit Profile** → opens modal (Step 4.3)
- **Privacy Policy** → `Alert.alert('Privacy', '...')`
- **Help & Support** → `Alert.alert('Help', '...')`
- **Logout** → show confirmation dialog (Step 4.4)

### Step 4.3: Edit Profile modal

Implement a `Modal` for editing **name** and **email** (in local state).

Recommended approach:

- Store the editable user values in state (`name`, `email`)
- When modal opens, initialize the inputs with current values
- Include **Save** and **Cancel**
  - Save updates the displayed name/email
  - Cancel closes without changes

Inputs:

- `TextInput` for name
- `TextInput` for email (use `keyboardType="email-address"` if desired)

**Checkpoint:** Tap “Edit Profile” → modal opens → editing updates the visible header after Save.

### Step 4.4: Logout confirmation

On Logout press, show a confirmation `Alert`:

- Cancel (do nothing)
- Logout (for this lab: reset local state, or show “Logged out” alert)

**Checkpoint:** Logout row is pressable and shows a confirmation dialog.

---

## Part 5: Accessibility and UX checks (small but important)

- Touch targets should be comfortable (at least ~44px height).
- Toggle rows should be easy to understand (label next to switch).
- Use consistent spacing between sections.
- Use `ScrollView` so content is reachable on small screens.

---

## Part 6: Testing checklist

- [ ] Profile tab appears in the bottom tab bar and opens `app/(tabs)/profile.tsx`.
- [ ] Profile header shows avatar, name, and email from dummy data/state.
- [ ] Membership card shows plan and renewal/status.
- [ ] Stats row shows 3 values and stays aligned on different screen sizes.
- [ ] Toggle switches work and update UI state.
- [ ] Pressable rows (Privacy/Help) show an `Alert`.
- [ ] Edit Profile opens a modal; Save updates name/email; Cancel doesn’t.
- [ ] Logout shows a confirmation dialog.
- [ ] No API calls or authentication required.

---

## Deliverables

1. **`app/(tabs)/profile.tsx`** containing the full Profile UI and interactions.
2. **`app/(tabs)/_layout.tsx`** updated to include the Profile tab (`name="profile"`).
3. (Optional) A small `data/profile.ts` file if you moved dummy data out of the screen file.

---

## Evaluation criteria (suggested)

| Area | Criteria |
|------|---------|
| Tab integration | Profile tab appears and routes correctly (`name="profile"` ↔ `profile.tsx`) |
| UI | Header, membership card, stats row, settings list |
| Interaction | Modal edit works; toggles update state; alerts/confirmations work |
| Code quality | Clear component structure; typed data; readable styles |
| Constraints | Uses dummy/local data only (no login/API) |

---

## Hints (no full solution)

- **Expo Router**: filename and tab name must match: `profile.tsx` → `name="profile"`.
- **Avatar**: set both `width` and `height`. Use `borderRadius: size / 2` for a circle.
- **Modal on Android**: provide `onRequestClose` so the back button can dismiss it.
- **Switch**: use `value={state}` and `onValueChange={setState}`.
- **Keep it simple**: don’t implement real auth; focus on UI + interactions.

---

## Troubleshooting

| Problem | Things to try |
|---------|----------------|
| Profile tab does not appear | Ensure you added `<Tabs.Screen name="profile" ... />` and the file exists at `app/(tabs)/profile.tsx`. |
| “Unmatched route” | The `<Tabs.Screen name="...">` must match the filename (no extension). `profile.tsx` must be in `app/(tabs)/`. |
| Avatar not showing | If using a remote URL, confirm it’s `https:` and accessible. If local, use `source={require('...')}`. |
| Modal won’t close on Android back | Add `onRequestClose={() => setModalVisible(false)}`. |
| Switch looks disabled | Confirm you passed `value` and `onValueChange`. Also check container isn’t blocking touches. |
| Layout overflows on small screens | Wrap content in `ScrollView` and reduce fixed widths; use `flex` layouts. |

---

## Optional extensions

- Persist toggles with **AsyncStorage** (advanced).
- Add a “Change avatar” button (UI only) that cycles through a few preset avatar URLs.
- Add a “Language” setting with a simple picker modal.
- Add “App version” display using `expo-constants` (read-only).

---

## Summary

- Add **`app/(tabs)/profile.tsx`** and register it in **`app/(tabs)/_layout.tsx`** so the Profile tab appears.
- Use **dummy local data** (no login) to display user info, plan, and stats.
- Add a **settings list** with toggles and pressable rows.
- Implement an **Edit Profile modal** to practice state + UI updates.

