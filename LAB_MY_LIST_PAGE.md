# Lab Activity: Building the My List Tab (Tab 3)

## Objective

Build a **My List** screen that shows a saved watchlist of movies. All movie data must come from a **dummy JSON file** in your project (no REST API, no TMDB). Each item shows a poster, title, year, duration, and rating. When the user taps the **Play** control on an item, a **full-screen modal** opens and plays a **YouTube trailer** using the video ID stored in your JSON. The video should **autoplay** after the modal opens (the tap counts as a user gesture, which helps on mobile).

You will practice: **Expo Router** (tabs or single screen), **reading local JSON**, **lists and grids**, **Modal**, **WebView**, and **TypeScript** types.

---

## Prerequisites

- Comfortable with React function components, `useState`, and `useEffect` (or loading data once on mount).
- Basic React Native: `View`, `Text`, `Image`, `ScrollView`, `TouchableOpacity`, `Modal`, `ActivityIndicator`, `Dimensions`.
- Expo project running (`npx expo start`).
- Optional: NativeWind/Tailwind (`className`) *or* `StyleSheet`—use whatever your course already uses.

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

## Two Ways to Follow This Lab

You can submit **either** setup. Both are valid.

| | **Track A — Third tab (multi-tab app)** | **Track B — Single-screen app (no tab bar)** |
|---|----------------------------------------|-----------------------------------------------|
| **Use case** | Your project already has Home, Search, etc., and you add My List as another tab. | You only need one screen for grading; no bottom tabs. |
| **Main file** | `app/(tabs)/my-list.tsx` | `app/index.tsx` (or one screen in a `Stack`) |
| **Navigation** | Register a new `Tabs.Screen` in `app/(tabs)/_layout.tsx` | Root layout shows only this screen (see Part 0). |

**Important:** The **UI code** for the My List screen is almost the same in both tracks—you only change **where** the file lives and **whether** you add a tab.

---

## Part 0: Single-Screen App (Track B Only)

Skip this part if you use **Track A** (tabs).

### Step 0.1: Create or simplify the project

1. Create a new Expo app with the **blank** template (recommended for one screen):

   ```bash
   npx create-expo-app@latest my-list-lab --template blank
   cd my-list-lab
   ```

2. If your template uses **Expo Router**, you should have an `app` folder. Ensure you have at least:
   - `app/_layout.tsx`
   - `app/index.tsx`

3. If your blank template **does not** use Expo Router, use the **tabs** template instead and **ignore** the tab bar visually by only implementing `app/(tabs)/index.tsx` as your full My List UI and hiding other tabs in `_layout.tsx`—or switch to a Router-based blank. The simplest path is **Expo Router + single `index.tsx`**.

### Step 0.2: Root layout with one screen

Your `app/_layout.tsx` should render a **Stack** (or default layout) so that `app/index.tsx` is the only screen:

```tsx
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <Stack screenOptions={{ headerShown: false }} />
  );
}
```

(Exact imports may match your Expo Router version; the goal is **one stack, one screen**.)

### Step 0.3: Put the My List UI in `app/index.tsx`

All steps in **Parts 2–6** below refer to “the My List screen file.” For Track B, that file is **`app/index.tsx`**.

**Checkpoint (Track B):** Running the app opens **only** your My List screen with no bottom tab bar.

---

## Part 1: Add the My List Tab (Track A Only)

Skip this part if you use **Track B**.

### Step 1.1: Locate the tab layout

- Open **`app/(tabs)/_layout.tsx`**.

### Step 1.2: Add a new tab screen

1. Create **`app/(tabs)/my-list.tsx`** with a placeholder:

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

2. In **`_layout.tsx`**, add a `<Tabs.Screen />` with:
   - `name="my-list"` (must match the filename `my-list.tsx`)
   - `title="My List"` (or similar)
   - `tabBarIcon` using `@expo/vector-icons` (e.g. Feather `bookmark`)

### Step 1.3: Order of tabs

Place the My List tab **third** if your instructor requires “third tab”; otherwise follow the course order.

**Checkpoint (Track A):** Tapping the new tab shows the placeholder.

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

In your My List screen file:

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
- [ ] Works on **Track A** (tab) or **Track B** (single screen), as chosen.

---

## Deliverables

1. **`data/my-list-dummy.json`** with the `movies` array.
2. **Types** for your JSON (file or inline).
3. **My List screen** file:
   - Track A: `app/(tabs)/my-list.tsx` + updated `app/(tabs)/_layout.tsx`
   - Track B: `app/index.tsx` (and minimal `app/_layout.tsx`)
4. **Short report** (if required): which track you used, how you imported JSON, and one problem you solved (e.g. WebView, layout width, TypeScript).

---

## Evaluation criteria (suggested)

| Area | Criteria |
|------|----------|
| Data | All list content from dummy JSON; trailer id comes from JSON |
| UI | Header, grid, card fields, loading/empty states |
| Interaction | Play opens modal; close works; WebView shows YouTube embed |
| Code quality | Typed data, `key` on list items, readable structure |
| Flexibility | Student can run **either** single-screen or tab version |

---

## Hints (no full solution)

- **Card width:** `(screenWidth - horizontalPadding * 2 - gapBetweenColumns) / 2`.
- **YouTube id:** Always 11 characters in standard URLs; copy from `youtube.com/watch?v=`.
- **Modal + WebView:** If the screen flashes black, check `flex: 1` on the WebView container.
- **JSON typos:** Validate JSON in VS Code or [jsonlint.com](https://jsonlint.com) before running.

---

## Troubleshooting

| Problem | Things to try |
|---------|----------------|
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
- Implement the **same screen** in **`app/(tabs)/my-list.tsx`** (third tab) **or** **`app/index.tsx`** (single-screen app).
- Use **Modal + WebView + YouTube embed URL** for playback with **autoplay** query parameters.

This lab is intentionally **API-free** for the list so you can focus on layout, navigation shape, and embedded video.
