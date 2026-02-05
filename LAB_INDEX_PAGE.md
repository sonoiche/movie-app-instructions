# Lab Activity: Building the Home Page with useState and Lists

> **⚠️ IMPORTANT: Type all code yourself!**  
> This lab is designed for learning. Copy-pasting code will prevent you from understanding the concepts. Type each line of code manually to build muscle memory and comprehension.

## Objective
Create a mobile streaming application home page that displays movie data using React Native's `useState` hook and renders lists from arrays. This lab focuses on understanding state management, data arrays, and React Native StyleSheet for styling.

## Prerequisites
- Basic knowledge of React and JavaScript
- Node.js and npm installed
- Expo CLI installed (`npm install -g expo-cli`)
- Android Studio installed with Android Emulator configured
- A code editor (VS Code recommended)
- Basic understanding of React Native components

> **💡 Learning Tip:** Throughout this lab, you'll see code examples. **Type every line yourself** - don't copy-paste! Typing code helps build muscle memory and deepens your understanding of the syntax and concepts.

## Duration
Estimated time: 2-3 hours

---

## Part 1: Project Setup

### Step 1.1: Create or Use Existing Expo Project

If you don't have a project yet, create a new Expo project:

```bash
npx create-expo-app@latest streaming-app --template tabs
cd streaming-app
```

If you already have a project, navigate to it:

```bash
cd streaming-app
```

### Step 1.2: Verify Project Structure

Your project should have this structure:

```
streaming-app/
├── app/
│   ├── _layout.tsx
│   └── (tabs)/
│       ├── _layout.tsx
│       └── index.tsx  (This is the file we'll modify)
├── package.json
└── ...
```

### Step 1.3: Install Required Dependencies

Ensure you have the necessary packages installed:

```bash
npm install @expo/vector-icons
```

---

## Part 2: Understanding useState Hook

### Step 2.1: What is useState?

`useState` is a React Hook that allows you to add state to functional components. It returns an array with two elements:
- **State value**: The current value of the state
- **State setter function**: Function to update the state

**Basic Syntax:**
```typescript
const [stateName, setStateName] = useState(initialValue);
```

**Examples (type these yourself to practice):**
```typescript
const [count, setCount] = useState(0);
const [movies, setMovies] = useState([]);
const [isLoading, setIsLoading] = useState(true);
```

**Practice:** Type each example above to get familiar with the useState syntax pattern.

### Step 2.2: Why Use useState?

- **Dynamic Content**: Update UI when data changes
- **User Interactions**: Handle button clicks, form inputs
- **Data Management**: Store and update lists of data
- **Conditional Rendering**: Show/hide elements based on state

---

## Part 3: Creating the Data Array

> **📝 From this point forward, you'll be typing code.**  
> All code examples should be typed manually. Take your time, read each line, and understand what you're typing. This is how you'll learn!

### Step 3.1: Create Movie Data Array

We'll create a hardcoded array of movies. Open `app/(tabs)/index.tsx` and **type** this data structure at the top of the file (outside the component):

**First, define the Movie interface:**
```typescript
// Movie data interface
interface Movie {
  id: string;
  title: string;
  year: number;
  rating: number;
  genre: string;
  thumbnail: string;
}
```

**Then, create the movies array. Type each movie object yourself:**
```typescript
// Hardcoded movie data array
const MOVIES_DATA: Movie[] = [
  // Type the first movie object here
  {
    id: '1',
    title: 'The Dark Knight',
    year: 2008,
    rating: 9.0,
    genre: 'Action',
    thumbnail: 'https://images.unsplash.com/photo-1536440136628-849c177e76a1?w=400',
  },
  // Now add 5 more movie objects with these details:
  // - Inception (2010, 8.8, Sci-Fi)
  // - Interstellar (2014, 8.6, Sci-Fi)
  // - The Matrix (1999, 8.7, Action)
  // - Pulp Fiction (1994, 8.9, Crime)
  // - The Shawshank Redemption (1994, 9.3, Drama)
  // Use similar Unsplash image URLs for thumbnails
];
```

**Note:** Type each movie object manually. You can add more movies or modify the existing ones. The structure should match the Movie interface you defined above.

---

## Part 4: Setting Up useState Hooks

### Step 4.1: Import Required Components

At the top of your `index.tsx` file, **type** these imports yourself:

```typescript
// Import React and useState hook
import React, { useState } from 'react';

// Import React Native components - type each one:
import {
  View,
  Text,
  ScrollView,
  Image,
  TouchableOpacity,
  StyleSheet,
  Dimensions,
} from 'react-native';

import { SafeAreaView } from 'react-native-safe-area-context';

// Import Feather icons
import { Feather } from '@expo/vector-icons';
```

**Remember:** Type each import statement manually. This helps you understand what components you're using.

### Step 4.2: Initialize useState Hooks

Inside your component function, **type** these useState hooks to manage different pieces of state:

```typescript
export default function HomeScreen() {
  // Type this useState hook for storing the list of movies
  const [movies, setMovies] = useState<Movie[]>([]);
  
  // Type this useState hook for loading indicator
  const [isLoading, setIsLoading] = useState(true);
  
  // Type this useState hook for featured movie (optional)
  const [featuredMovie, setFeaturedMovie] = useState<Movie | null>(null);

  // Rest of your component code...
}
```

**Explanation:**
- `movies`: Array to store all movies (initialized as empty array)
- `isLoading`: Boolean to show/hide loading indicator (initialized as `true`)
- `featuredMovie`: Single movie object for featured section (initialized as `null`)

**Practice:** Type each `useState` hook yourself. Notice the pattern: `const [stateName, setStateName] = useState(initialValue);`

### Step 4.3: Load Data into State

**Type** a function to load movies into state. Add this inside your component:

```typescript
// Type this function to load movies
const loadMovies = () => {
  // First, set loading to true
  setIsLoading(true);
  
  // Use setTimeout to simulate loading delay
  setTimeout(() => {
    // Load the movies array into state
    setMovies(MOVIES_DATA);
    // Set the first movie as featured
    setFeaturedMovie(MOVIES_DATA[0]);
    // Set loading to false
    setIsLoading(false);
  }, 1000);
};

// Type this useEffect to call loadMovies when component mounts
React.useEffect(() => {
  loadMovies();
}, []);
```

**What this does:**
- Sets loading to `true` when function starts
- After 1 second delay, loads the movies array into state
- Sets the first movie as featured
- Sets loading to `false` to hide loading indicator

**Practice:** Type the entire function yourself. Pay attention to how state setters (`setMovies`, `setIsLoading`) are used.

---

## Part 5: Creating the UI Layout

### Step 5.1: Basic Component Structure

**Build your component step by step.** Type each section yourself:

**Step 1: Start with the component function and state hooks** (you already have these from Step 4):
```typescript
export default function HomeScreen() {
  const [movies, setMovies] = useState<Movie[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [featuredMovie, setFeaturedMovie] = useState<Movie | null>(null);

  // ... your loadMovies function and useEffect here ...
```

**Step 2: Add the loading state check. Type this conditional return:**
```typescript
  // Loading state - type this if statement
  if (isLoading) {
    return (
      <SafeAreaView style={styles.container}>
        <View style={styles.loadingContainer}>
          <Text style={styles.loadingText}>Loading movies...</Text>
        </View>
      </SafeAreaView>
    );
  }
```

**Step 3: Build the main return statement. Start with the outer structure:**
```typescript
  return (
    <SafeAreaView style={styles.container}>
      <ScrollView style={styles.scrollView}>
        {/* You'll add content here */}
      </ScrollView>
    </SafeAreaView>
  );
}
```

**Step 4: Add the Header section. Type this inside ScrollView:**
```typescript
        {/* Header */}
        <View style={styles.header}>
          <Text style={styles.headerTitle}>StreamFlix</Text>
          <View style={styles.headerIcons}>
            <Feather name="bell" size={24} color="#000" />
            <Feather name="cast" size={24} color="#000" style={styles.iconSpacing} />
          </View>
        </View>
```

**Step 5: Add the Featured Movie section. Type this conditional rendering:**
```typescript
        {/* Featured Movie Section */}
        {featuredMovie && (
          <View style={styles.featuredSection}>
            <Image
              source={{ uri: featuredMovie.thumbnail }}
              style={styles.featuredImage}
              resizeMode="cover"
            />
            <View style={styles.featuredOverlay}>
              <Text style={styles.featuredTitle}>{featuredMovie.title}</Text>
              <View style={styles.featuredInfo}>
                <Text style={styles.featuredYear}>{featuredMovie.year}</Text>
                <Text style={styles.featuredRating}>⭐ {featuredMovie.rating}</Text>
              </View>
            </View>
          </View>
        )}
```

**Step 6: Add the Movies List section. Type this section with the map function:**
```typescript
        {/* Movies List Section */}
        <View style={styles.moviesSection}>
          <Text style={styles.sectionTitle}>All Movies</Text>
          
          {/* Type the map function to render list of movies */}
          {movies.map((movie) => (
            <TouchableOpacity key={movie.id} style={styles.movieCard}>
              <Image
                source={{ uri: movie.thumbnail }}
                style={styles.movieImage}
                resizeMode="cover"
              />
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
        </View>
```

**Important:** Type each section one at a time. Don't copy-paste! Build it piece by piece to understand the structure.

---

## Part 6: Creating Styles with StyleSheet

### Step 6.1: Create StyleSheet Object

**Type the StyleSheet at the bottom of your file** (outside the component). Build it section by section:

**Step 1: Get window dimensions:**
```typescript
const { width } = Dimensions.get('window');
```

**Step 2: Start the StyleSheet.create() call:**
```typescript
const styles = StyleSheet.create({
  // You'll add style objects here
});
```

**Step 3: Type the basic container styles:**
```typescript
  container: {
    flex: 1,
    backgroundColor: '#FFFFFF',
  },
  loadingContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  loadingText: {
    fontSize: 18,
    color: '#666666',
  },
  scrollView: {
    flex: 1,
  },
```

**Step 4: Type the header styles:**
```typescript
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    paddingHorizontal: 16,
    paddingTop: 12,
    paddingBottom: 16,
    backgroundColor: '#FFFFFF',
  },
  headerTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#000000',
  },
  headerIcons: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  iconSpacing: {
    marginLeft: 16,
  },
```

**Step 5: Type the featured section styles:**
```typescript
  featuredSection: {
    width: '100%',
    height: 300,
    marginBottom: 24,
    position: 'relative',
  },
  featuredImage: {
    width: '100%',
    height: '100%',
  },
  featuredOverlay: {
    position: 'absolute',
    bottom: 0,
    left: 0,
    right: 0,
    backgroundColor: 'rgba(0, 0, 0, 0.7)',
    padding: 16,
  },
  featuredTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#FFFFFF',
    marginBottom: 8,
  },
  featuredInfo: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 16,
  },
  featuredYear: {
    fontSize: 14,
    color: '#CCCCCC',
  },
  featuredRating: {
    fontSize: 14,
    color: '#FFD700',
    fontWeight: '600',
  },
```

**Step 6: Type the movies section styles:**
```typescript
  moviesSection: {
    paddingHorizontal: 16,
    paddingBottom: 24,
  },
  sectionTitle: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#000000',
    marginBottom: 16,
  },
  movieCard: {
    flexDirection: 'row',
    backgroundColor: '#F5F5F5',
    borderRadius: 12,
    marginBottom: 16,
    overflow: 'hidden',
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  movieImage: {
    width: 120,
    height: 180,
  },
  movieInfo: {
    flex: 1,
    padding: 16,
    justifyContent: 'center',
  },
  movieTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#000000',
    marginBottom: 8,
  },
  movieDetails: {
    flexDirection: 'row',
    alignItems: 'center',
    gap: 12,
  },
  movieYear: {
    fontSize: 14,
    color: '#666666',
  },
  movieGenre: {
    fontSize: 14,
    color: '#666666',
    backgroundColor: '#E0E0E0',
    paddingHorizontal: 8,
    paddingVertical: 4,
    borderRadius: 4,
  },
  movieRating: {
    fontSize: 14,
    color: '#FFD700',
    fontWeight: '600',
  },
```

**Remember:** Type each style object yourself. Pay attention to the property names (camelCase) and values. This helps you understand React Native styling.

### Step 6.2: Understanding StyleSheet Properties

**Common StyleSheet Properties:**

- **Layout:**
  - `flex: 1` - Takes available space
  - `flexDirection: 'row'` - Horizontal layout
  - `justifyContent: 'center'` - Main axis alignment
  - `alignItems: 'center'` - Cross axis alignment

- **Spacing:**
  - `padding: 16` - Internal spacing
  - `margin: 16` - External spacing
  - `paddingHorizontal: 16` - Left and right padding
  - `paddingVertical: 16` - Top and bottom padding

- **Colors:**
  - `backgroundColor: '#FFFFFF'` - Background color
  - `color: '#000000'` - Text color

- **Typography:**
  - `fontSize: 18` - Text size
  - `fontWeight: 'bold'` - Text weight
  - `textAlign: 'center'` - Text alignment

---

## Part 7: Rendering Lists with .map()

### Step 7.1: Understanding Array.map()

The `.map()` method creates a new array by calling a function on every element:

**Basic syntax patterns (type these examples yourself):**
```typescript
// Pattern 1: With return statement
array.map((item) => {
  return <Component key={item.id} data={item} />;
});

// Pattern 2: With arrow function (shorter syntax)
array.map((item) => <Component key={item.id} data={item} />);
```

**Practice:** Type both patterns above to understand the difference. Notice how Pattern 2 is shorter but does the same thing.

### Step 7.2: Rendering Movie List

In your component, **type** the code to render the movies array using `.map()`:

```typescript
{/* Type the map function - start with movies.map() */}
{movies.map((movie) => (
  <TouchableOpacity key={movie.id} style={styles.movieCard}>
    <Image
      source={{ uri: movie.thumbnail }}
      style={styles.movieImage}
    />
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
```

**Important Notes:**
- Always include `key` prop when rendering lists (type `key={movie.id}`)
- Use unique identifier (like `movie.id`) for the key
- Each item in the array becomes a component
- Type the arrow function `(movie) => (...)` yourself to understand the syntax

---

## Part 8: Adding Interactivity

### Step 8.1: Add Button to Refresh Movies

**Type** a refresh button that reloads the movies:

**Step 1: Add a state for refresh count (optional):**
```typescript
// Type this useState hook
const [refreshCount, setRefreshCount] = useState(0);
```

**Step 2: Modify your loadMovies function to update refresh count:**
```typescript
// Update your existing loadMovies function - add this line inside setTimeout
const loadMovies = () => {
  setIsLoading(true);
  setTimeout(() => {
    setMovies(MOVIES_DATA);
    setFeaturedMovie(MOVIES_DATA[0]);
    setIsLoading(false);
    // Type this line to increment refresh count
    setRefreshCount(refreshCount + 1);
  }, 1000);
};
```

**Step 3: Add refresh button in header. Type this TouchableOpacity:**
```typescript
// Add this inside your header View, after the headerIcons
<TouchableOpacity onPress={loadMovies} style={styles.refreshButton}>
  <Feather name="refresh-cw" size={20} color="#000" />
</TouchableOpacity>
```

### Step 8.2: Add Movie Selection Handler

**Type** a function to handle movie selection:

**Step 1: Create the handler function:**
```typescript
// Type this function inside your component
const handleMoviePress = (movie: Movie) => {
  console.log('Selected movie:', movie.title);
  // You can add navigation or other logic here
  alert(`You selected: ${movie.title}`);
};
```

**Step 2: Update your TouchableOpacity in the map function:**
```typescript
// Update the TouchableOpacity in your movies.map() - add onPress prop
<TouchableOpacity 
  key={movie.id} 
  style={styles.movieCard}
  onPress={() => handleMoviePress(movie)}
>
  {/* Your existing movie content */}
</TouchableOpacity>
```

**Practice:** Type the arrow function `() => handleMoviePress(movie)` yourself to understand how event handlers work.

---

## Part 9: Running on Android Studio Emulator

### Step 9.1: Prerequisites

Before running on Android Studio emulator, ensure:

1. **Android Studio is installed**
   - Download from: https://developer.android.com/studio
   - Install Android SDK and tools

2. **Android Virtual Device (AVD) is created**
   - Open Android Studio
   - Go to Tools → Device Manager
   - Click "Create Device"
   - Select a device (e.g., Pixel 5)
   - Select a system image (API 33 or higher recommended)
   - Finish setup

3. **Environment Variables are set** (usually automatic)
   - `ANDROID_HOME` should point to Android SDK location
   - `PATH` should include Android SDK tools

### Step 9.2: Start Android Emulator

1. **Open Android Studio**
2. **Start the Emulator:**
   - Go to Tools → Device Manager
   - Click the Play button (▶) next to your AVD
   - Wait for emulator to boot (may take 1-2 minutes)

**Alternative:** Start emulator from command line:
```bash
emulator -avd YourAVDName
```

### Step 9.3: Verify Emulator is Running

Check that the emulator is running:
```bash
adb devices
```

You should see your emulator listed, e.g.:
```
List of devices attached
emulator-5554    device
```

### Step 9.4: Start Expo Development Server

1. **Navigate to your project directory:**
```bash
cd streaming-app
```

2. **Start Expo:**
```bash
npm start
```

or

```bash
npx expo start
```

3. **When Expo starts, you'll see options:**
   - Press `a` to open on Android emulator
   - Or scan QR code with Expo Go app

### Step 9.5: Open on Android Emulator

**Method 1: Using Expo CLI**
- Press `a` in the terminal where Expo is running
- Expo will automatically detect and open on the emulator

**Method 2: Using ADB**
- Ensure emulator is running (`adb devices`)
- Expo should automatically detect it
- Press `a` in Expo terminal

**Method 3: Manual Connection**
```bash
adb reverse tcp:8081 tcp:8081
adb reverse tcp:19000 tcp:19000
adb reverse tcp:19001 tcp:19001
```

Then press `a` in Expo terminal.

### Step 9.6: Troubleshooting Android Emulator

**Issue: Emulator not detected**
```bash
# Check if emulator is running
adb devices

# Restart ADB server
adb kill-server
adb start-server

# Check Android SDK path
echo $ANDROID_HOME
```

**Issue: Expo can't connect**
- Ensure emulator is fully booted (wait for home screen)
- Restart Expo: `npm start -- --reset-cache`
- Check firewall settings

**Issue: App crashes on emulator**
- Check Metro bundler logs
- Verify all dependencies are installed: `npm install`
- Clear cache: `npm start -- --clear`

**Issue: Slow performance**
- Use x86/x86_64 system images (faster than ARM)
- Allocate more RAM to emulator (2GB+ recommended)
- Enable hardware acceleration in AVD settings

---

## Part 10: Testing Your Application

### Step 10.1: Test State Updates

1. **Test Loading State:**
   - App should show "Loading movies..." initially
   - After 1 second, movies should appear

2. **Test Movie List:**
   - Verify all movies from array are displayed
   - Check that each movie shows correct information

3. **Test Interactions:**
   - Tap on a movie card (should show alert)
   - Test refresh button (if added)

### Step 10.2: Test Different Scenarios

**Empty State Test:**

**Step 1: Temporarily test with empty array** (for testing only - remove after):
```typescript
// Temporarily modify your loadMovies function to test empty state
// Change: setMovies(MOVIES_DATA);
// To: setMovies([]);
```

**Step 2: Add empty state UI. Type this conditional rendering:**
```typescript
// Add this in your movies section, before the map function
{movies.length === 0 && (
  <View style={styles.emptyContainer}>
    <Text style={styles.emptyText}>No movies available</Text>
  </View>
)}
```

**Step 3: Add empty state styles. Type these in your StyleSheet:**
```typescript
emptyContainer: {
  padding: 32,
  alignItems: 'center',
},
emptyText: {
  fontSize: 16,
  color: '#666666',
},
```

**Remember:** Type the conditional rendering and styles yourself. This helps you understand how to handle empty states.

---

## Part 11: Complete Code Structure Reference

> **⚠️ REMINDER: This is a reference structure, not code to copy-paste!**  
> Use this as a guide to verify your work. You should have typed all the code yourself following the steps above.

Here's what your complete `index.tsx` file structure should look like:

**File Structure Overview:**
1. **Imports** - React, React Native components, icons
2. **Movie Interface** - TypeScript interface definition
3. **MOVIES_DATA Array** - Your hardcoded movie data
4. **Dimensions** - Get window width
5. **Component Function** - HomeScreen component with:
   - useState hooks
   - loadMovies function
   - useEffect hook
   - handleMoviePress function
   - Loading state check
   - Main return JSX
6. **Styles** - StyleSheet.create() with all style objects

**Key Sections to Verify:**

✅ **Imports section** - Should include useState, useEffect, and all React Native components you're using

✅ **Movie interface** - Should match the structure of your movie objects

✅ **MOVIES_DATA array** - Should contain all 6 movie objects you typed

✅ **useState hooks** - Three hooks: movies, isLoading, featuredMovie

✅ **loadMovies function** - Sets loading, uses setTimeout, updates state

✅ **useEffect** - Calls loadMovies on component mount

✅ **Loading check** - Returns loading UI if isLoading is true

✅ **Main JSX** - Contains SafeAreaView, ScrollView, Header, Featured Section, Movies List

✅ **map() function** - Renders each movie from the array

✅ **Styles** - All style objects defined in StyleSheet.create()

**Checklist:**
- [ ] All code typed manually (not copy-pasted)
- [ ] All imports are correct
- [ ] Movie interface matches your data
- [ ] All 6 movies in MOVIES_DATA array
- [ ] useState hooks properly initialized
- [ ] loadMovies function works correctly
- [ ] useEffect calls loadMovies
- [ ] Loading state displays correctly
- [ ] Header renders with icons
- [ ] Featured movie displays
- [ ] Movie list renders using .map()
- [ ] All styles are defined
- [ ] App runs without errors

**If you're stuck:** Review the individual steps above. Each step breaks down what you need to type. Don't copy-paste - type it yourself!

---

## Deliverables

Submit the following:

1. **Source Code**
   - Complete `index.tsx` file
   - All styles properly defined
   - useState hooks implemented correctly

2. **Screenshots**
   - Screenshot of the app running on Android emulator
   - Screenshot showing the movie list
   - Screenshot showing loading state (if possible)

3. **Documentation**
   - Brief explanation of how useState is used
   - Explanation of how the array.map() method works
   - Any challenges encountered and solutions

---

## Evaluation Criteria

Your work will be evaluated on:

1. **Functionality (40%)**
   - useState hooks properly implemented
   - Movies array correctly loaded into state
   - List renders correctly using .map()
   - Loading state works

2. **Code Quality (30%)**
   - Clean, readable code
   - Proper use of TypeScript interfaces
   - Correct use of React hooks
   - Proper key prop in list rendering

3. **Design (20%)**
   - Consistent styling
   - Good use of StyleSheet
   - Proper spacing and layout
   - Visual appeal

4. **Completeness (10%)**
   - All required features implemented
   - App runs without errors
   - Proper project structure

---

## Troubleshooting

### Common Issues:

1. **Movies not displaying**
   - Check that `setMovies(MOVIES_DATA)` is called
   - Verify `MOVIES_DATA` array is not empty
   - Check console for errors

2. **useState not updating**
   - Ensure you're using the setter function (e.g., `setMovies`)
   - Don't mutate state directly
   - Check that state updates are inside functions

3. **List rendering errors**
   - Ensure each item has a unique `key` prop
   - Check that you're using `.map()` not `.forEach()`
   - Verify array is not undefined

4. **Styling not applying**
   - Check StyleSheet syntax
   - Verify style prop names (camelCase)
   - Ensure styles object is properly defined

5. **Android emulator issues**
   - Ensure emulator is fully booted
   - Check `adb devices` shows emulator
   - Restart Expo with `--reset-cache` flag

---

## Extension Activities (Optional)

If you finish early, try:

1. Add a search functionality that filters movies
2. Add a "Favorite" button that toggles movie favorite state
3. Add sorting options (by year, rating, title)
4. Add pull-to-refresh functionality
5. Create a movie detail screen (navigate on press)
6. Add dark mode support
7. Add animations when movies load

---

## Questions for Reflection

After completing the lab, consider:

1. How does useState help manage component state?
2. Why is the `key` prop important when rendering lists?
3. What happens when you call `setMovies()`?
4. How does `.map()` differ from `.forEach()`?
5. What are the advantages of using StyleSheet over inline styles?

---

**Good luck with your lab! 🚀**
