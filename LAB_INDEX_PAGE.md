# Lab Activity: Building the Home Page with useState and Lists

## Objective
Create a mobile streaming application home page that displays movie data using React Native's `useState` hook and renders lists from arrays. This lab focuses on understanding state management, data arrays, and React Native StyleSheet for styling.

## Prerequisites
- Basic knowledge of React and JavaScript
- Node.js and npm installed
- Expo CLI installed (`npm install -g expo-cli`)
- Android Studio installed with Android Emulator configured
- A code editor (VS Code recommended)
- Basic understanding of React Native components

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

**Example:**
```typescript
const [count, setCount] = useState(0);
const [movies, setMovies] = useState([]);
const [isLoading, setIsLoading] = useState(true);
```

### Step 2.2: Why Use useState?

- **Dynamic Content**: Update UI when data changes
- **User Interactions**: Handle button clicks, form inputs
- **Data Management**: Store and update lists of data
- **Conditional Rendering**: Show/hide elements based on state

---

## Part 3: Creating the Data Array

### Step 3.1: Create Movie Data Array

We'll create a hardcoded array of movies. Open `app/(tabs)/index.tsx` and add this data structure at the top of the file (outside the component):

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

// Hardcoded movie data array
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
];
```

**Note:** You can add more movies to this array or modify the existing ones.

---

## Part 4: Setting Up useState Hooks

### Step 4.1: Import Required Components

At the top of your `index.tsx` file, add these imports:

```typescript
import React, { useState } from 'react';
import {
  View,
  Text,
  ScrollView,
  Image,
  TouchableOpacity,
  StyleSheet,
  SafeAreaView,
  Dimensions,
} from 'react-native';
import { Feather } from '@expo/vector-icons';
```

### Step 4.2: Initialize useState Hooks

Inside your component function, add useState hooks to manage different pieces of state:

```typescript
export default function HomeScreen() {
  // State for storing the list of movies
  const [movies, setMovies] = useState<Movie[]>([]);
  
  // State for loading indicator
  const [isLoading, setIsLoading] = useState(true);
  
  // State for featured movie (optional)
  const [featuredMovie, setFeaturedMovie] = useState<Movie | null>(null);

  // Rest of your component code...
}
```

**Explanation:**
- `movies`: Array to store all movies
- `isLoading`: Boolean to show/hide loading indicator
- `featuredMovie`: Single movie object for featured section

### Step 4.3: Load Data into State

Create a function to load movies into state. Add this inside your component:

```typescript
// Function to load movies
const loadMovies = () => {
  setIsLoading(true);
  
  // Simulate loading delay (optional, for demonstration)
  setTimeout(() => {
    setMovies(MOVIES_DATA);
    setFeaturedMovie(MOVIES_DATA[0]); // Set first movie as featured
    setIsLoading(false);
  }, 1000);
};

// Call loadMovies when component mounts
React.useEffect(() => {
  loadMovies();
}, []);
```

**What this does:**
- Sets loading to `true`
- After 1 second, loads the movies array into state
- Sets the first movie as featured
- Sets loading to `false`

---

## Part 5: Creating the UI Layout

### Step 5.1: Basic Component Structure

Replace your existing component with this structure:

```typescript
export default function HomeScreen() {
  const [movies, setMovies] = useState<Movie[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [featuredMovie, setFeaturedMovie] = useState<Movie | null>(null);

  const loadMovies = () => {
    setIsLoading(true);
    setTimeout(() => {
      setMovies(MOVIES_DATA);
      setFeaturedMovie(MOVIES_DATA[0]);
      setIsLoading(false);
    }, 1000);
  };

  React.useEffect(() => {
    loadMovies();
  }, []);

  // Loading state
  if (isLoading) {
    return (
      <SafeAreaView style={styles.container}>
        <View style={styles.loadingContainer}>
          <Text style={styles.loadingText}>Loading movies...</Text>
        </View>
      </SafeAreaView>
    );
  }

  return (
    <SafeAreaView style={styles.container}>
      <ScrollView style={styles.scrollView}>
        {/* Header */}
        <View style={styles.header}>
          <Text style={styles.headerTitle}>StreamFlix</Text>
          <View style={styles.headerIcons}>
            <Feather name="bell" size={24} color="#000" />
            <Feather name="cast" size={24} color="#000" style={styles.iconSpacing} />
          </View>
        </View>

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

        {/* Movies List Section */}
        <View style={styles.moviesSection}>
          <Text style={styles.sectionTitle}>All Movies</Text>
          
          {/* Render list of movies */}
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
      </ScrollView>
    </SafeAreaView>
  );
}
```

---

## Part 6: Creating Styles with StyleSheet

### Step 6.1: Create StyleSheet Object

Add this StyleSheet at the bottom of your file (outside the component):

```typescript
const { width } = Dimensions.get('window');

const styles = StyleSheet.create({
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
});
```

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

```typescript
// Basic syntax
array.map((item) => {
  return <Component key={item.id} data={item} />;
});

// With arrow function
array.map((item) => <Component key={item.id} data={item} />);
```

### Step 7.2: Rendering Movie List

In your component, render the movies array:

```typescript
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
- Always include `key` prop when rendering lists
- Use unique identifier (like `movie.id`) for the key
- Each item in the array becomes a component

---

## Part 8: Adding Interactivity

### Step 8.1: Add Button to Refresh Movies

Add a refresh button that reloads the movies:

```typescript
// Add this state
const [refreshCount, setRefreshCount] = useState(0);

// Modify loadMovies function
const loadMovies = () => {
  setIsLoading(true);
  setTimeout(() => {
    setMovies(MOVIES_DATA);
    setFeaturedMovie(MOVIES_DATA[0]);
    setIsLoading(false);
    setRefreshCount(refreshCount + 1);
  }, 1000);
};

// Add refresh button in header
<TouchableOpacity onPress={loadMovies} style={styles.refreshButton}>
  <Feather name="refresh-cw" size={20} color="#000" />
</TouchableOpacity>
```

### Step 8.2: Add Movie Selection Handler

Add a function to handle movie selection:

```typescript
const handleMoviePress = (movie: Movie) => {
  console.log('Selected movie:', movie.title);
  // You can add navigation or other logic here
  alert(`You selected: ${movie.title}`);
};

// Update TouchableOpacity
<TouchableOpacity 
  key={movie.id} 
  style={styles.movieCard}
  onPress={() => handleMoviePress(movie)}
>
  {/* Movie content */}
</TouchableOpacity>
```

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
```typescript
// Temporarily set empty array
setMovies([]);

// Add empty state UI
{movies.length === 0 && (
  <View style={styles.emptyContainer}>
    <Text style={styles.emptyText}>No movies available</Text>
  </View>
)}
```

**Add to styles:**
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

---

## Part 11: Complete Code Example

Here's the complete `index.tsx` file structure:

```typescript
import React, { useState, useEffect } from 'react';
import {
  View,
  Text,
  ScrollView,
  Image,
  TouchableOpacity,
  StyleSheet,
  SafeAreaView,
  Dimensions,
  Alert,
} from 'react-native';
import { Feather } from '@expo/vector-icons';

// Movie interface
interface Movie {
  id: string;
  title: string;
  year: number;
  rating: number;
  genre: string;
  thumbnail: string;
}

// Movie data array
const MOVIES_DATA: Movie[] = [
  // ... your movie data here
];

const { width } = Dimensions.get('window');

export default function HomeScreen() {
  // useState hooks
  const [movies, setMovies] = useState<Movie[]>([]);
  const [isLoading, setIsLoading] = useState(true);
  const [featuredMovie, setFeaturedMovie] = useState<Movie | null>(null);

  // Load movies function
  const loadMovies = () => {
    setIsLoading(true);
    setTimeout(() => {
      setMovies(MOVIES_DATA);
      setFeaturedMovie(MOVIES_DATA[0]);
      setIsLoading(false);
    }, 1000);
  };

  // useEffect to load movies on mount
  useEffect(() => {
    loadMovies();
  }, []);

  // Handle movie press
  const handleMoviePress = (movie: Movie) => {
    Alert.alert('Movie Selected', `You selected: ${movie.title}`);
  };

  // Loading state
  if (isLoading) {
    return (
      <SafeAreaView style={styles.container}>
        <View style={styles.loadingContainer}>
          <Text style={styles.loadingText}>Loading movies...</Text>
        </View>
      </SafeAreaView>
    );
  }

  return (
    <SafeAreaView style={styles.container}>
      <ScrollView style={styles.scrollView}>
        {/* Header */}
        <View style={styles.header}>
          <Text style={styles.headerTitle}>StreamFlix</Text>
          <View style={styles.headerIcons}>
            <Feather name="bell" size={24} color="#000" />
            <Feather name="cast" size={24} color="#000" style={styles.iconSpacing} />
          </View>
        </View>

        {/* Featured Movie */}
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

        {/* Movies List */}
        <View style={styles.moviesSection}>
          <Text style={styles.sectionTitle}>All Movies</Text>
          
          {movies.length === 0 ? (
            <View style={styles.emptyContainer}>
              <Text style={styles.emptyText}>No movies available</Text>
            </View>
          ) : (
            movies.map((movie) => (
              <TouchableOpacity
                key={movie.id}
                style={styles.movieCard}
                onPress={() => handleMoviePress(movie)}
              >
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
            ))
          )}
        </View>
      </ScrollView>
    </SafeAreaView>
  );
}

// Styles
const styles = StyleSheet.create({
  // ... all your styles here
});
```

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
