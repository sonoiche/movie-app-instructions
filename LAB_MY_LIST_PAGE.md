# Lab Activity: Building the My List Tab (Tab 3)

## Objective

Build a **My List** screen that shows a saved watchlist of movies. All movie data must come from a **dummy JSON file** in your project (no REST API, no TMDB). Each item shows a poster, title, year, duration, and rating. When the user taps the **Play** control on an item, a **full-screen modal** opens and plays a **YouTube trailer** using the video ID stored in your JSON. The video should **autoplay** after the modal opens (the tap counts as a user gesture, which helps on mobile).

You will practice: **Expo Router** (bottom tabs), **reading local JSON**, **lists and grids**, **Modal**, **WebView**, and **TypeScript** types.

---

## Prerequisites

- Comfortable with React function components, `useState`, and `useEffect` (or loading data once on mount).
- Basic React Native: `View`, `Text`, `Image`, `ScrollView`, `TouchableOpacity`, `Modal`, `ActivityIndicator`, `Dimensions`.
- Expo project running (`npx expo start`).
- Optional: NativeWind/Tailwind (`className`) *or* `StyleSheet`—use whatever your course already uses.

---

## Project setup

Create the app with the **default** Expo template. From your terminal, run **`npx create-expo-app`** with a project name and **do not** pass `--template` (the default template is used automatically):

```bash
npx create-expo-app my-movie-app
cd my-movie-app
```

You may use `npx create-expo-app@latest my-movie-app` instead if you want to align with the newest `create-expo-app` version; both produce the same kind of default project.

This scaffolds a project that **includes an `app/` folder** at the repo root (Expo Router file-based routing). The default template usually includes a **tabs** layout:

- **`app/_layout.tsx`** — root layout (often a `Stack` that contains the tab navigator).
- **`app/(tabs)/`** — folder for tab screens. Inside it, **`_layout.tsx`** defines the **bottom tab bar** (`<Tabs>` and each `<Tabs.Screen>`).
- Each **`Tabs.Screen`** **`name`** must match a **route file** in the same folder: for example `name="index"` → **`index.tsx`**, `name="explore"` → **`explore.tsx`**.

You will **add a new file** (e.g. `my-list.tsx`) and **register a new `Tabs.Screen`** so another tab appears in the bar.

**Requirement:** Your project **must** have an **`app/`** directory with a **`app/(tabs)/`** group and **`app/(tabs)/_layout.tsx`**. If your project does not use tabs, create a new app with **`npx create-expo-app`** only (default template). Do not use `--template blank` unless your instructor says otherwise.

Then install dependencies if prompted and start the dev server:

```bash
npx expo start
```

---

## Duration

Estimated time: **3–4 hours** (first time with WebView + Modal).

---

## What You Will Build

1. A **header** with the title “My List” and a subtitle showing how many items are in the list (e.g. “6 items”).
2. A **scrollable grid** of movie cards (two columns is a common choice).
3. Each card: **thumbnail**, **bookmark** affordance (visual), **rating** badge, **title**, **year • duration**, and a **Play** button.
4. Tapping **Play** opens a **modal** with the movie title and a **close** button; inside, a **WebView** loads YouTube’s embed URL for the `youtubeTrailerId` from JSON, with **autoplay** query parameters.
5. **Loading** state while JSON is “loaded” (optional: simulate delay with `setTimeout` for practice).
6. **Empty list** state if the JSON array is empty (message + icon).

**Out of scope for this lab (unless your instructor extends it):** persisting the list with AsyncStorage, login, or removing items from the list.

---

## Part 1: Add the My List tab to the default tab template

Follow **Project setup** above so you have the default **`app/(tabs)/`** layout. You will add **one new screen file** and **one new `Tabs.Screen`** entry so “My List” appears in the bottom tab bar.

### How the default tabs template works

- **`app/(tabs)/_layout.tsx`** exports a layout component that wraps children in **`<Tabs>`** (from `expo-router`).
- Each **`<Tabs.Screen>`** describes **one tab**: its `name` is the **route segment** and must match a **filename** in `app/(tabs)/` (without `.tsx`). Example: `name="index"` → `index.tsx`, `name="my-list"` → **`my-list.tsx`** (use kebab-case in the file name to match the route).
- **`options`** on each screen set the **label** under the icon (`title`) and the **`tabBarIcon`** function (return a Feather / Ionicons component).

### Step 1.1: Open the tab layout file

Open **`app/(tabs)/_layout.tsx`**. Read the existing `<Tabs.Screen>` entries so you see the pattern (`name`, `options.title`, `options.tabBarIcon`).

### Step 1.2: Create the My List screen file

Create a new file **`app/(tabs)/my-list.tsx`** (same folder as `index.tsx`). Start with a placeholder so you can verify routing:

```tsx
import { View, Text } from 'react-native';

export default function MyListScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>My List</Text>
    </View>
  );
}
```

Use **`export default`** — Expo Router requires a default export for the screen component.

### Step 1.3: Register the new tab in `_layout.tsx`

In **`app/(tabs)/_layout.tsx`**, add another **`<Tabs.Screen />`** **after** or **among** the existing screens:

- **`name="my-list"`** — must match the file **`my-list.tsx`** (Expo Router maps this name to that file).
- **`options.title`:** `"My List"` (or the label your course uses).
- **`options.tabBarIcon`:** return an icon, e.g. from **`@expo/vector-icons`** (Feather **`bookmark`** fits “My List”).

Save and reload the app. You should see **one more tab** in the bottom bar; tapping it opens your placeholder.

### Step 1.4: Tab order (third tab)

If your course requires My List as the **third** tab, reorder the `<Tabs.Screen>` components in **`_layout.tsx`** so “My List” is the third screen in the list (order in JSX usually matches left-to-right order in the tab bar).

**Checkpoint:** The new tab appears, uses the correct icon/title, and shows the placeholder until you build the full UI in **`app/(tabs)/my-list.tsx`**.

All later parts assume your My List UI lives in **`app/(tabs)/my-list.tsx`**.

---

## Part 2: Dummy data as a JSON file

All movie rows must come from a **single JSON file** committed to the repo, for example:

**Path:** `data/my-list-dummy.json`  
(Create the `data` folder at the **project root**, next to `app/`, `package.json`, etc.)

### Step 2.1: JSON shape (schema)

Each movie object should include at least:

| Field | Type | Purpose |
|-------|------|---------|
| `id` | string | Stable unique id for React `key` |
| `title` | string | Movie title |
| `year` | number | Release year |
| `rating` | number | e.g. 8.5 |
| `duration` | string | e.g. `"148m"` or `"2h 28m"` |
| `thumbnail` | string | HTTPS URL to a poster image |
| `youtubeTrailerId` | string | YouTube **video id** (11 characters), used in embed URL |

**No API keys.** The trailer plays via the public embed URL:

`https://www.youtube.com/embed/<youtubeTrailerId>?autoplay=1&playsinline=1&rel=0&modestbranding=1`

### Step 2.2: Sample `data/my-list-dummy.json`

You may copy this file verbatim and then edit titles/images if you want. Replace `youtubeTrailerId` values with **valid** YouTube video IDs (search any movie trailer on YouTube; the ID is in the watch URL `v=XXXXXXXXXXX`).

```json
{
  "movies": [
    {
      "id": "1",
      "title": "The Dark Knight",
      "year": 2008,
      "rating": 9.0,
      "duration": "152m",
      "thumbnail": "https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=400",
      "youtubeTrailerId": "EXeTwQWrcwY"
    },
    {
      "id": "2",
      "title": "Inception",
      "year": 2010,
      "rating": 8.8,
      "duration": "148m",
      "thumbnail": "https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=400",
      "youtubeTrailerId": "YoHD9XEInc0"
    },
    {
      "id": "3",
      "title": "Interstellar",
      "year": 2014,
      "rating": 8.6,
      "duration": "169m",
      "thumbnail": "https://images.unsplash.com/photo-1446776653964-20c1d3a81b06?w=400",
      "youtubeTrailerId": "zSWdZVtXT7E"
    },
    {
      "id": "4",
      "title": "The Matrix",
      "year": 1999,
      "rating": 8.7,
      "duration": "136m",
      "thumbnail": "https://images.unsplash.com/photo-1518709268805-4e9042af2176?w=400",
      "youtubeTrailerId": "vKQi3ybBAHs"
    },
    {
      "id": "5",
      "title": "Dune: Part One",
      "year": 2021,
      "rating": 8.0,
      "duration": "155m",
      "thumbnail": "https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=400",
      "youtubeTrailerId": "n9xhJrPXop4"
    },
    {
      "id": "6",
      "title": "Blade Runner 2049",
      "year": 2017,
      "rating": 8.0,
      "duration": "164m",
      "thumbnail": "https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=400",
      "youtubeTrailerId": "gCcx85zbxz4"
    }
  ]
}
```

**Note:** Trailer IDs above are examples; if a video is region-blocked or removed, replace that id with another valid trailer.

In this course repository, the same sample content is also available as the file **`data/my-list-dummy.json`** so you can copy it or import it directly while following the lab.

### Step 2.3: TypeScript type

Create a type that matches one row, e.g. in `data/my-list-types.ts` or at the top of your screen file:

```typescript
export interface MyListMovie {
  id: string;
  title: string;
  year: number;
  rating: number;
  duration: string;
  thumbnail: string;
  youtubeTrailerId: string;
}

export interface MyListJson {
  movies: MyListMovie[];
}
```

**Checkpoint:** JSON file exists, validates as JSON (no trailing commas), and matches your interface.

---

## Part 3: Import JSON in React Native / Expo

### Step 3.1: Import the file

In **`app/(tabs)/my-list.tsx`** (your My List screen):

```typescript
import myListData from '@/data/my-list-dummy.json';
import type { MyListMovie, MyListJson } from '@/data/my-list-types';
```

Cast if needed so TypeScript trusts the shape:

```typescript
const { movies } = myListData as MyListJson;
```

### Step 3.2: `tsconfig.json`

Ensure JSON imports work. Expo’s base config often includes `resolveJsonModule`. If you see an error like “Cannot import JSON,” add under `compilerOptions`:

```json
"resolveJsonModule": true
```

### Step 3.3: Path alias `@/*`

If `@/data/...` fails, use a relative import from your screen file, e.g. `../../data/my-list-dummy.json`, or configure `paths` in `tsconfig.json` to match your project.

### Step 3.4: State for the list

```typescript
const [movies, setMovies] = useState<MyListMovie[]>([]);
const [loading, setLoading] = useState(true);
```

On mount (`useEffect`), assign from imported JSON:

```typescript
useEffect(() => {
  const { movies: list } = myListData as MyListJson;
  setMovies(list);
  setLoading(false);
}, []);
```

Optional: wrap in `setTimeout(..., 300)` to practice a loading spinner.

**Checkpoint:** The app shows the correct number of items from JSON without any `fetch()` to the internet for the list (images still load from URLs).

---

## Part 4: Layout and grid

### Step 4.1: Screen structure

- Outer `View` with `flex: 1` and background (respect light/dark mode if required).
- **Header:** title “My List” + item count.
- **`ScrollView`** wrapping the grid so small screens can scroll.

### Step 4.2: Two-column grid

1. Use `Dimensions.get('window').width` to compute card width (account for horizontal padding and gap between columns).
2. Use `flexDirection: 'row'`, `flexWrap: 'wrap'`, and `justifyContent: 'space-between'` **or** a fixed width per card—match patterns from your course.

### Step 4.3: Movie card

For each `movie` in `movies.map()`:

- **`key={movie.id}`** (required).
- **`Image`** with `source={{ uri: movie.thumbnail }}`, fixed width/height, `resizeMode="cover"`, rounded corners.
- **Rating** badge (e.g. star icon + `movie.rating`).
- **Title** (`numberOfLines={1}`).
- **Subtitle:** `year • duration`.
- **Play** `TouchableOpacity` (see Part 5).
- Optional: bookmark icon (can be decorative only for this lab).

**Checkpoint:** List matches JSON; layout does not overflow horizontally.

---

## Part 5: Trailer modal + WebView + autoplay

### Step 5.1: Install WebView

```bash
npx expo install react-native-webview
```

Restart the dev server after installing native modules.

### Step 5.2: State for the modal

- `modalVisible` (boolean)
- `selectedMovie` (`MyListMovie | null`)
- Build embed URL only when `selectedMovie` is set:

```typescript
const embedUri = selectedMovie
  ? `https://www.youtube.com/embed/${selectedMovie.youtubeTrailerId}?autoplay=1&playsinline=1&rel=0&modestbranding=1`
  : undefined;
```

### Step 5.3: Open / close

- **Play onPress:** set `selectedMovie` to that movie, set `modalVisible` to `true`.
- **Close:** set `modalVisible` to `false`, then `selectedMovie` to `null` (clearing stops the WebView when you unmount or clear `source`).

Use React Native’s **`Modal`** with `visible={modalVisible}`, `animationType="slide"`, `presentationStyle="fullScreen"` (iOS), and `onRequestClose` for Android back button.

### Step 5.4: WebView inside the modal

- Header row: movie title + close (`X`) button.
- **`WebView`** with `source={{ uri: embedUri! }}` and `style={{ flex: 1, backgroundColor: '#000' }}`.
- Recommended props for playback behavior:
  - `allowsInlineMediaPlayback` (iOS)
  - `mediaPlaybackRequiresUserAction={false}`
  - `allowsFullscreenVideo`
  - `javaScriptEnabled`
  - `domStorageEnabled`

Use `key={selectedMovie.id}` on `WebView` so switching movies remounts the player.

### Step 5.5: Safe area

If you use `react-native-safe-area-context`, wrap modal content in `SafeAreaView` with `edges={['top', 'left', 'right']}`. If your app root does not use `SafeAreaProvider`, insets may be zero; still acceptable for the lab unless your instructor requires notched devices.

**Checkpoint:** Tap Play → modal opens → trailer starts (user may need to tap again on some emulators; test on a real device if possible).

---

## Part 6: Empty and loading states

### Loading

While `loading` is true, show `ActivityIndicator` and short text (“Loading your list…”).

### Empty list

If `movies.length === 0`, show an icon (e.g. Feather `bookmark`) and short message (“Your list is empty”). You can test by temporarily setting `movies` to `[]` in code.

**Checkpoint:** Both states are visible when you force them.

---

## Part 7: Testing checklist

- [ ] Data loads from **`data/my-list-dummy.json`** only (no movie list API).
- [ ] Header shows correct **item count**.
- [ ] Grid scrolls; images load; no duplicate keys.
- [ ] **Play** opens modal with correct **title**.
- [ ] **Close** dismisses modal; video stops (no audio in background).
- [ ] **Autoplay** works at least after the Play tap (embed URL includes `autoplay=1`).
- [ ] **My List** tab is registered and opens the completed screen from the tab bar.

---

## Deliverables

1. **`data/my-list-dummy.json`** with the `movies` array.
2. **Types** for your JSON (file or inline).
3. **`app/(tabs)/my-list.tsx`** with the full My List UI, plus **`app/(tabs)/_layout.tsx`** updated with the new `Tabs.Screen` for My List.
4. **Short report** (if required): how you added the tab, how you imported JSON, and one problem you solved (e.g. WebView, layout width, TypeScript).

---

## Evaluation criteria (suggested)

| Area | Criteria |
|------|----------|
| Data | All list content from dummy JSON; trailer id comes from JSON |
| UI | Header, grid, card fields, loading/empty states |
| Interaction | Play opens modal; close works; WebView shows YouTube embed |
| Code quality | Typed data, `key` on list items, readable structure |
| Tab integration | My List appears as a tab from the default template; `name` matches `my-list.tsx` |

---

## Hints (no full solution)

- **Tabs:** The string in **`name="..."`** must match the route file: **`my-list.tsx`** → **`name="my-list"`** (kebab-case).
- **Card width:** `(screenWidth - horizontalPadding * 2 - gapBetweenColumns) / 2`.
- **YouTube id:** Always 11 characters in standard URLs; copy from `youtube.com/watch?v=`.
- **Modal + WebView:** If the screen flashes black, check `flex: 1` on the WebView container.
- **JSON typos:** Validate JSON in VS Code or [jsonlint.com](https://jsonlint.com) before running.

---

## Troubleshooting

| Problem | Things to try |
|---------|----------------|
| New tab does not appear or shows “Unmatched route” | Ensure **`name`** on `Tabs.Screen` matches the filename (e.g. `name="my-list"` ↔ **`my-list.tsx`**) and the file is inside **`app/(tabs)/`**. |
| Cannot import JSON | Add `"resolveJsonModule": true`; restart TS server |
| `@/` import fails | Fix `paths` in `tsconfig` or use relative path |
| WebView blank | Check network; try another `youtubeTrailerId`; ensure `https:` URL |
| Autoplay never starts | Confirm `autoplay=1` and `mediaPlaybackRequiresUserAction={false}`; test on device |
| Red box error about WebView | Run `npx expo install react-native-webview` and rebuild |

---

## Optional extensions

- Remove an item from the list with **local state** (still start from JSON on reload).
- **AsyncStorage** to persist edits (advanced).
- **Dark/light** styling using `useColorScheme`.
- **Error boundary** if `youtubeTrailerId` is missing: show message instead of broken WebView.

---

## Summary

- Use a **dummy JSON file** under `data/` as the only source for the watchlist and trailer IDs.
- Add **`app/(tabs)/my-list.tsx`** and register it in **`app/(tabs)/_layout.tsx`** so **My List** appears in the default bottom tab bar.
- Use **Modal + WebView + YouTube embed URL** for playback with **autoplay** query parameters.

This lab is intentionally **API-free** for the list so you can focus on layout, navigation shape, and embedded video.
