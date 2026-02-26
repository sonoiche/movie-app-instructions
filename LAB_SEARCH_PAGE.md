# Lab Activity: Building the Search Tab (Tab 2)

## Objective
Add a Search tab to the bottom tab bar and build a Search page that lets users search and browse movies using **dummy (hardcoded) data only**. You will implement the tab registration, state management, filtering logic, and UI—all without using any API. Style the page with React Native **StyleSheet**.

## Prerequisites
- Completed or understood the Home/Index page lab (useState, list from array, StyleSheet)
- Expo project with tabs template
- Basic React (useState, conditional rendering)
- Basic React Native (View, Text, ScrollView, TextInput, TouchableOpacity, Image, StyleSheet)

## Duration
Estimated time: 2–3 hours

---

## Part 1: Add the Search Tab to the Tab Bar

### Step 1.1: Locate the Tab Layout File

- Open the file that defines the bottom tab navigation.
- Path: **`app/(tabs)/_layout.tsx`**

### Step 1.2: Check Existing Tabs

- In that file you will see a `<Tabs>` component and one or more `<Tabs.Screen>` components.
- Identify the **route name** used for each tab (e.g. `index`, `explore`). The route name must match the **filename** of the screen (without extension) inside `app/(tabs)/`.

### Step 1.3: Add a Tab for Search

- Add a **new** `<Tabs.Screen>` for the Search screen.
- Use the **name** `"search"`. This means Expo Router will load the screen from **`app/(tabs)/search.tsx`**.
- Set the **title** to `"Search"` (or similar) so it appears under the icon in the tab bar.
- Set a **tabBarIcon** so the tab has an icon. Use any icon library already in the project (e.g. Feather from `@expo/vector-icons`). A search/magnifying-glass icon is appropriate.

**What you need to do (no full code):**
- Insert one new `Tabs.Screen` with `name="search"`, `title="Search"`, and a search icon in `tabBarIcon`.
- Do not remove existing tabs unless the lab asks you to replace one.

### Step 1.4: Create the Search Screen File

- In the **`app/(tabs)/`** folder, create a new file: **`search.tsx`**.
- Export a default function component (e.g. `SearchScreen`) that returns a simple placeholder (e.g. a `<View>` with a `<Text>` saying "Search").
- Save and run the app. Tap the Search tab and confirm that this placeholder screen is shown.

**Checkpoint:** The Search tab appears in the bar and shows your placeholder screen when selected.

---

## Part 2: Define Dummy Data (No API)

All data for this lab must be **hardcoded**. Do not call any external API.

### Step 2.1: Movie Data Structure

- Define a **TypeScript interface** (or type) for a single movie. It should include at least:
  - `id` (string)
  - `title` (string)
  - `year` (number)
  - `rating` (number)
  - `genre` (string, or array of strings if you prefer)
  - `thumbnail` (string: image URL)

You can add more fields (e.g. `description`) if useful for the UI.

### Step 2.2: Movies Array

- Create a **constant array** of movies (e.g. `MOVIES_DATA` or `DUMMY_MOVIES`) that matches your interface.
- Put at least **8–10 movies** in the array so that search and filtering have enough data.
- Use real-looking titles, years, ratings, and genres. For `thumbnail`, you may use placeholder image URLs (e.g. from Unsplash or similar) or a single placeholder URL for all.

Define this array **outside** the component (at the top of the file or in a separate data file that you import).

### Step 2.3: Genres (for “Browse by genre”)

- Create a **constant array** of genre names (e.g. `["Action", "Drama", "Comedy", "Horror", "Sci-Fi", "Thriller", "Romance", "Crime"]`).
- You will use this so the user can tap a genre and see movies that match that genre from your dummy list.

**Checkpoint:** You have one movies array and one genres array, both hardcoded and used only in memory (no API).

### Step 2.4: Sample Dummy Data (Copy and Use)

Use the interface and data below in your project. You may add more movies or change fields as long as your filtering logic matches.

**Interface and movies array:**

```typescript
interface Movie {
  id: string;
  title: string;
  year: number;
  rating: number;
  genre: string;
  thumbnail: string;
}

const MOVIES_DATA: Movie[] = [
  {
    id: '1',
    title: 'The Dark Knight',
    year: 2008,
    rating: 9.0,
    genre: 'Action',
    thumbnail: 'https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=400',
  },
  {
    id: '2',
    title: 'Inception',
    year: 2010,
    rating: 8.8,
    genre: 'Sci-Fi',
    thumbnail: 'https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=400',
  },
  {
    id: '3',
    title: 'Interstellar',
    year: 2014,
    rating: 8.6,
    genre: 'Sci-Fi',
    thumbnail: 'https://images.unsplash.com/photo-1446776653964-20c1d3a81b06?w=400',
  },
  {
    id: '4',
    title: 'The Matrix',
    year: 1999,
    rating: 8.7,
    genre: 'Action',
    thumbnail: 'https://images.unsplash.com/photo-1518709268805-4e9042af2176?w=400',
  },
  {
    id: '5',
    title: 'Pulp Fiction',
    year: 1994,
    rating: 8.9,
    genre: 'Crime',
    thumbnail: 'https://images.unsplash.com/photo-1535016120720-40c646be5580?w=400',
  },
  {
    id: '6',
    title: 'The Shawshank Redemption',
    year: 1994,
    rating: 9.3,
    genre: 'Drama',
    thumbnail: 'https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=400',
  },
  {
    id: '7',
    title: 'Fight Club',
    year: 1999,
    rating: 8.8,
    genre: 'Drama',
    thumbnail: 'https://images.unsplash.com/photo-1518709268805-4e9042af2176?w=400',
  },
  {
    id: '8',
    title: 'Forrest Gump',
    year: 1994,
    rating: 8.8,
    genre: 'Drama',
    thumbnail: 'https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=400',
  },
  {
    id: '9',
    title: 'The Godfather',
    year: 1972,
    rating: 9.2,
    genre: 'Crime',
    thumbnail: 'https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?w=400',
  },
  {
    id: '10',
    title: 'Goodfellas',
    year: 1990,
    rating: 8.7,
    genre: 'Crime',
    thumbnail: 'https://images.unsplash.com/photo-1446776653964-20c1d3a81b06?w=400',
  },
];
```

**Genres array (for chips and "Browse by genre"):**

```typescript
const GENRES = ['Action', 'Drama', 'Sci-Fi', 'Crime', 'Comedy', 'Horror', 'Romance', 'Thriller'];
```

**Note:** Some genres in `GENRES` (e.g. Comedy, Horror, Romance, Thriller) may have no movies in `MOVIES_DATA`. Your filtering and empty state should handle that. You can add more movies to `MOVIES_DATA` to match those genres if you want.

---

## Part 3: State for the Search Page

Use **useState** for all of the following. Plan the types so TypeScript is satisfied.

### Step 3.1: Search Input

- **State:** current search text (string).
- **Initial value:** empty string `""`.
- **Usage:** Bind this to a `TextInput` so the user can type. When the user types, update this state.

### Step 3.2: Filtered / Displayed Movies

- **State:** list of movies to show (array matching your Movie type).
- **Initial value:** you may use the full dummy movies array, or an empty array—your choice; document it.
- **Usage:** This is the list you will **render**. It should either be:
  - the result of filtering by search text, or  
  - the result of filtering by selected genre, or  
  - the full list when there is no search and no genre filter (or when “All” is selected).

Define clearly in your own notes when you show “all”, “search results”, or “genre results”.

### Step 3.3: Selected Genre (Optional but Recommended)

- **State:** currently selected genre (string or null).
- **Usage:** When the user taps a genre, set this state and then set the “filtered movies” state to only movies that have that genre. When the user clears search or selects “All”, reset this.

### Step 3.4: Optional State

- You may add more state (e.g. loading flag for future use, or “has searched” flag) if it helps your logic. For this lab, no API calls are required, so no real loading from network.

**Checkpoint:** You have a clear plan for which state variables you use and how they drive what the user sees.

---

## Part 4: Filtering Logic (Client-Side Only)

All filtering must be done in JavaScript on the **dummy data array**. No API calls.

### Step 4.1: Filter by Search Text

- Write a function (e.g. `filterBySearch(movies, query)`) that:
  - Takes the full movies array and a search string.
  - Returns a new array of movies where the **title** (or title + genre, etc.) matches the query.
  - Matching should be **case-insensitive**.
  - If the query is empty or only whitespace, return the full list (or the “genre-filtered” list if you have genre selection).

**Hint:** Use `string.toLowerCase()` and `string.includes()` or similar. Do not mutate the original array; return a new array.

### Step 4.2: Filter by Genre

- Write a function (e.g. `filterByGenre(movies, genre)`) that:
  - Takes the full movies array and a genre string.
  - Returns movies whose `genre` (or `genres` array) includes that genre.
  - If genre is null or "All", return the full list.

### Step 4.3: When to Apply Filters

- Decide when you recalculate the list to show:
  - **On every change of search text:** e.g. in a `useEffect` that depends on the search state, or inside an `onChangeText` handler. Recompute filtered list from dummy data and update the “filtered movies” state.
  - **On genre tap:** set selected genre state, then recompute the list (by genre, and optionally also by current search text if you want combined filters).

Implement these functions yourself and call them from the appropriate places in your component.

**Checkpoint:** Typing in the search box updates the visible list; tapping a genre updates the visible list; behavior is consistent with your state design.

---

## Part 5: Layout and UI Sections

Build the following sections **yourself** in `search.tsx`. Use **StyleSheet** for all styles (no NativeWind/className for this lab).

### Step 5.1: Overall Structure

- Use a root container (e.g. `SafeAreaView` or `View`) that fills the screen.
- Use **ScrollView** for the scrollable content so that the page scrolls when the list or content is long.
- Keep a consistent padding/margin so content is not flush against the edges.

### Step 5.2: Header

- At the top, show a **title** (e.g. “Search”).
- Below it, add a **TextInput** that:
  - Is bound to your search text state (value and onChangeText).
  - Has a placeholder like “Search movies…”.
  - Has clear styling (height, padding, border or background) so it looks like a search bar.

You may add an icon inside or next to the input if you like; it is not required.

### Step 5.3: “Popular” or “Quick” Searches (Chips)

- Below the search bar, add a **section title** (e.g. “Popular searches” or “Quick search”).
- Under it, render a row (or wrapped row) of **tappable chips** (e.g. `TouchableOpacity` with `Text`).
- Each chip shows a **genre name** or a fixed search term (e.g. “Action”, “Drama”, “Comedy”).
- **On press:** set the search input state to that term (and optionally run your filter logic so the list updates immediately).

Use your dummy genres array (or a subset) to render these chips. Style them so they look like pills/chips (padding, border or background, maybe rounded corners).

### Step 5.4: “Browse by Genre”

- Add another section title (e.g. “Browse by genre”).
- Render a list or grid of **genre buttons** using your dummy genres array.
- **On press:** set the selected genre state and update the “filtered movies” state to only movies in that genre. If the user had typed something, decide whether to clear the search or combine filters; document your choice.

Style the genre buttons so they are clearly tappable and readable.

### Step 5.5: Results List

- Add a section title that reflects what is shown (e.g. “All movies”, “Search results”, “Action”, or “No results”).
- **Render the current “filtered movies” list** with `.map()`.
- Each item should show at least:
  - Thumbnail image (use the `thumbnail` URL from your dummy data; use React Native’s `Image` with `source={{ uri: ... }}`).
  - Title.
  - Year and rating (and genre if you want).
- Each item should be **tappable** (e.g. `TouchableOpacity`). For this lab, on press you can show an `Alert` with the movie title or do nothing; the goal is to have a working list and tap target.
- Use a **unique key** for each item (e.g. `movie.id`).

### Step 5.6: Empty State

- When the filtered list is **empty** (e.g. no search results or no movies for selected genre), show a message such as “No movies found” or “Try another search” instead of an empty space.
- Use conditional rendering: if `filteredMovies.length === 0`, render the message; otherwise render the list.

**Checkpoint:** The screen has header, search bar, chips, genre section, and a results list (or empty state), all laid out and wired to your state and dummy data.

---

## Part 5B: Design with Code Snippets (Structure Only)

The snippets below show **structure and patterns** from the codebase. Use them as a guide; implement the full JSX and logic yourself.

### Imports

Your file will need at least:

```typescript
import React, { useState, useEffect } from 'react';
import {
  View,
  Text,
  ScrollView,
  TextInput,
  TouchableOpacity,
  Image,
  StyleSheet,
  SafeAreaView,
  Dimensions,
  Alert,
} from 'react-native';
import { Feather } from '@expo/vector-icons';
```

You may add or remove components (e.g. `ActivityIndicator`) as needed.

### Component skeleton

- Default-export a function component (e.g. `SearchScreen`).
- Declare your state at the top (search text, filtered movies, selected genre).
- Optionally use `useEffect` to compute filtered list when search or genre changes.
- Return a root container, then `ScrollView`, then your sections in order.

Structure only (you fill in the JSX):

```typescript
export default function SearchScreen() {
  const [searchQuery, setSearchQuery] = useState('');
  const [filteredMovies, setFilteredMovies] = useState<Movie[]>([]);
  const [selectedGenre, setSelectedGenre] = useState<string | null>(null);

  // Add your filter functions and useEffect/handlers here.

  return (
    <SafeAreaView style={styles.container}>
      <ScrollView style={styles.scrollView}>
        {/* Header + search input */}
        {/* Popular searches chips */}
        {/* Browse by genre */}
        {/* Results list or empty state */}
      </ScrollView>
    </SafeAreaView>
  );
}
```

### Header and search input (pattern)

- Use a `View` for the header block.
- Title: `Text` with a style (e.g. `styles.headerTitle`).
- Search bar: a `View` wrapping `TextInput` (and optionally an icon). Bind `value` and `onChangeText` to your search state.

Example pattern (you complete styling and layout):

```typescript
<View style={styles.header}>
  <Text style={styles.headerTitle}>Search</Text>
  <View style={styles.searchInputContainer}>
    <Feather name="search" size={20} color="#999" />
    <TextInput
      style={styles.searchInput}
      value={searchQuery}
      onChangeText={setSearchQuery}
      placeholder="Search movies..."
      placeholderTextColor="#999"
    />
  </View>
</View>
```

### Popular searches chips (pattern)

- Section title, then a horizontal `ScrollView` or a flex-wrapped `View`.
- Map over `GENRES` (or a subset). Each chip is a `TouchableOpacity` that sets the search text (and optionally triggers filtering).

Example pattern:

```typescript
<Text style={styles.sectionTitle}>Popular searches</Text>
<ScrollView horizontal showsHorizontalScrollIndicator={false} style={styles.chipsScroll}>
  {GENRES.map((genre) => (
    <TouchableOpacity
      key={genre}
      style={styles.chip}
      onPress={() => setSearchQuery(genre)}
    >
      <Text style={styles.chipText}>{genre}</Text>
    </TouchableOpacity>
  ))}
</ScrollView>
```

### Browse by genre (pattern)

- Section title, then a row or grid of genre buttons.
- Map over `GENRES`. On press: set `selectedGenre` and update `filteredMovies` (e.g. by calling your `filterByGenre` and then `setFilteredMovies`).

Example pattern:

```typescript
<Text style={styles.sectionTitle}>Browse by genre</Text>
<View style={styles.genreRow}>
  {GENRES.map((genre) => (
    <TouchableOpacity
      key={genre}
      style={[styles.genreButton, selectedGenre === genre && styles.genreButtonActive]}
      onPress={() => {
        setSelectedGenre(genre);
        setFilteredMovies(filterByGenre(MOVIES_DATA, genre));
      }}
    >
      <Text style={styles.genreButtonText}>{genre}</Text>
    </TouchableOpacity>
  ))}
</View>
```

(You must implement `filterByGenre` and decide how it interacts with the search query.)

### Results list – single item (pattern)

- Section title that changes with context (e.g. "All movies", "Search results", or selected genre).
- If `filteredMovies.length === 0`, render your empty-state message.
- Otherwise, map over `filteredMovies`. Each item: `TouchableOpacity` with `key={movie.id}`, containing an `Image` and a `View` with title, year, rating, genre.

Example pattern for one item (you style and wire `onPress`):

```typescript
{filteredMovies.length === 0 ? (
  <Text style={styles.emptyStateText}>No movies found. Try another search.</Text>
) : (
  <>
    <Text style={styles.sectionTitle}>
      {selectedGenre ? selectedGenre : searchQuery ? 'Search results' : 'All movies'}
    </Text>
    {filteredMovies.map((movie) => (
      <TouchableOpacity
        key={movie.id}
        style={styles.movieCard}
        onPress={() => Alert.alert('Movie', movie.title)}
      >
        <Image source={{ uri: movie.thumbnail }} style={styles.movieImage} resizeMode="cover" />
        <View style={styles.movieInfo}>
          <Text style={styles.movieTitle}>{movie.title}</Text>
          <View style={styles.movieDetails}>
            <Text style={styles.movieYear}>{movie.year}</Text>
            <Text style={styles.movieGenre}>{movie.genre}</Text>
            <Text style={styles.movieRating}>⭐ {movie.rating}</Text>
          </View>
        </View>
      </TouchableOpacity>
    ))}
  </>
)}
```

### StyleSheet examples (pattern)

Define all styles in `StyleSheet.create({ ... })`. Example entries to get started (you add the rest and adjust values):

```typescript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#FFFFFF',
  },
  scrollView: {
    flex: 1,
  },
  header: {
    paddingHorizontal: 16,
    paddingTop: 12,
    paddingBottom: 16,
  },
  headerTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#000000',
    marginBottom: 12,
  },
  searchInputContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: '#F5F5F5',
    borderRadius: 10,
    paddingHorizontal: 12,
    paddingVertical: 10,
  },
  searchInput: {
    flex: 1,
    marginLeft: 8,
    fontSize: 16,
    paddingVertical: 4,
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: '600',
    color: '#000000',
    marginBottom: 12,
    marginTop: 20,
  },
  chip: {
    paddingHorizontal: 16,
    paddingVertical: 8,
    backgroundColor: '#E0E0E0',
    borderRadius: 20,
    marginRight: 8,
  },
  chipText: {
    fontSize: 14,
    color: '#333333',
  },
  movieCard: {
    flexDirection: 'row',
    backgroundColor: '#F9F9F9',
    borderRadius: 12,
    marginBottom: 12,
    overflow: 'hidden',
    padding: 12,
  },
  movieImage: {
    width: 80,
    height: 120,
    borderRadius: 8,
  },
  movieInfo: {
    flex: 1,
    marginLeft: 12,
    justifyContent: 'center',
  },
  movieTitle: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#000000',
    marginBottom: 4,
  },
  movieDetails: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 8,
  },
  emptyStateText: {
    fontSize: 16,
    color: '#666666',
    textAlign: 'center',
    marginTop: 24,
  },
});
```

Add styles for `chipsScroll`, `genreRow`, `genreButton`, `genreButtonActive`, `genreButtonText`, `movieYear`, `movieGenre`, `movieRating`, and any others you need. Use `Dimensions.get('window')` for width-based values if desired.

**Checkpoint:** You have a clear picture of the layout and patterns; the rest of the implementation is yours.

---

## Part 6: Styling with StyleSheet

- Create a **StyleSheet** at the bottom of `search.tsx` (or in a separate styles block) using `StyleSheet.create({ ... })`.
- **Do not use** `className` or NativeWind for this lab; use only the `style` prop and StyleSheet.
- Define named styles for:
  - Container, scroll content, header, title.
  - Search input (container and optional icon).
  - Section titles.
  - Chip container and chip text.
  - Genre grid/row and genre button.
  - Movie list item (card or row): image, text container, title, year, rating, genre.
  - Empty state text.
- Use consistent spacing (e.g. 8, 16, 24) and colors (e.g. background, text, borders) so the page looks clean and readable.
- Use **Dimensions** if you need width-based sizes (e.g. image width) so the layout works on different screens.

**Checkpoint:** The entire Search page is styled with StyleSheet only; no inline style objects unless for a single dynamic value (e.g. width).

---

## Part 7: Wiring and Behavior Summary

Before you finish, verify:

1. **Tab:** Search tab is in the tab bar and shows your Search screen.
2. **Data:** All data comes from your dummy movies and genres arrays; no API.
3. **State:** Search text, filtered list, and (if used) selected genre are in state and drive the UI.
4. **Search:** Typing in the input updates the list based on title (and your matching rule).
5. **Chips:** Tapping a chip sets the search text and updates the list.
6. **Genres:** Tapping a genre shows only movies for that genre (and your empty state when none).
7. **List:** Movie list is rendered with `.map()`, with unique keys and at least image, title, year, rating.
8. **Empty state:** When there are no results, a clear message is shown.
9. **Styles:** All styles use StyleSheet.

---

## Part 8: Testing Checklist

- [ ] Search tab appears and is the second tab (or as required).
- [ ] Tapping Search shows the Search screen without errors.
- [ ] Typing in the search box filters the list (or shows “no results”).
- [ ] Clearing the search box restores the list (or genre-filtered list).
- [ ] Tapping a chip updates the search and the list.
- [ ] Tapping a genre shows only movies of that genre; empty state when none.
- [ ] List scrolls when there are many items.
- [ ] Each list item shows image, title, and at least year/rating.
- [ ] No API calls; all data is from your dummy arrays.

---

## Deliverables

1. **Code**
   - `app/(tabs)/_layout.tsx` with the Search tab added (if it wasn’t already there).
   - `app/(tabs)/search.tsx` with your implementation (dummy data, state, filtering, UI, StyleSheet). Do **not** submit full code in the report; only the actual project files.

2. **Short write-up**
   - How you defined dummy data (movie shape and where you defined the array).
   - Which state variables you used and how they interact (search text, filtered list, genre).
   - How you combined search and genre (e.g. genre first, then search within that; or the other way around).
   - One or two issues you ran into and how you fixed them.

---

## Evaluation Criteria

- **Tab integration:** Search tab correctly added and visible; `search.tsx` exists and is the default export.
- **Dummy data:** Movies and genres are hardcoded; no API usage.
- **State:** Appropriate useState usage; filtered list and input stay in sync.
- **Filtering:** Client-side filter by text and by genre; no mutation of original array.
- **UI completeness:** Header, search bar, chips, genre section, results list, empty state.
- **List rendering:** `.map()` with unique keys; at least image, title, year, rating.
- **Styling:** StyleSheet only; readable and consistent layout.
- **Behavior:** Matches the testing checklist above.

---

## Hints (No Full Code)

- **TextInput:** Use `value={searchText}` and `onChangeText={setSearchText}` so the input is controlled.
- **Filtering on type:** You can run your filter in `onChangeText` and then `setFilteredMovies(...)`, or with `useEffect` that depends on the search string.
- **Keys in list:** Use `key={movie.id}` (or a unique field) in the outermost component inside `.map()`.
- **Image:** Use `<Image source={{ uri: movie.thumbnail }} style={...} />` and a defined size so images don’t collapse.
- **ScrollView:** Wrap the whole page content in `<ScrollView style={...}>` so long lists scroll; you can use `ScrollView` for a horizontal chip row if needed.
- **StyleSheet:** Keep style names descriptive (e.g. `searchInput`, `movieCard`, `emptyStateText`).

---

## Optional Extensions

- Add a “Clear” button next to the search input that clears the text and resets the list.
- Highlight the selected genre button (different background or border).
- Add a “See all” that clears genre and search and shows the full list.
- Show result count (e.g. “5 movies”) near the list title.
- Support multiple genres (e.g. “Action” and “Drama”) and show movies that match any of them.

---

**Do not implement API calls in this lab; use only dummy data. Implement all UI and logic yourself; this document does not provide the complete code for the Search page.**
