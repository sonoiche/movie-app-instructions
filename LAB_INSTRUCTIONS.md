# Lab Activity: Building a Mobile Streaming Application

## Objective
Create a mobile streaming application using React Native and Expo Router with a bottom tab navigation bar and multiple pages. This lab focuses on understanding navigation structure, component creation, and basic UI implementation.

**Important:** This lab starts with the Expo **Tabs Template**, which already includes a working tab bar. You will modify the existing template to create a streaming app.

## Prerequisites
- Basic knowledge of React and TypeScript
- Node.js and npm installed
- Expo CLI installed (`npm install -g expo-cli`)
- A code editor (VS Code recommended)
- Expo Go app installed on your mobile device (optional, for testing)

## Duration
Estimated time: 2-3 hours

## Quick Overview
1. Create project with `--template tabs` (tab bar already included!)
2. Modify existing tab layout to add 5 tabs for streaming app
3. Replace default icons with Feather icons
4. Update existing pages and create new ones
5. Design your own UI for each page

---

## Part 1: Project Setup

### Step 1.1: Create a New Expo Project with Tabs Template

1. Open your terminal/command prompt
2. Navigate to your desired directory
3. Create a new Expo project with the **Tabs** template:

```bash
npx create-expo-app@latest streaming-app --template tabs
```

**Note:** The `--template tabs` flag creates a project with a pre-configured bottom tab navigation.

4. Navigate into the project directory:

```bash
cd streaming-app
```

5. Install dependencies (if not done automatically):

```bash
npm install
```

6. Install NativeWind for TailwindCSS styling:

```bash
npm install nativewind tailwindcss
npx tailwindcss init
```

**Note:** NativeWind allows you to use TailwindCSS utility classes in React Native, making styling much easier!

### Step 1.2: Configure NativeWind

After installing NativeWind, you need to configure it:

1. **Update `tailwind.config.js`** (created by `npx tailwindcss init`):

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./app/**/*.{js,jsx,ts,tsx}", "./components/**/*.{js,jsx,ts,tsx}"],
  presets: [require("nativewind/preset")],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

2. **Update `babel.config.js`**:

```javascript
module.exports = function (api) {
  api.cache(true);
  return {
    presets: [
      ["babel-preset-expo", { jsxImportSource: "nativewind" }],
      "nativewind/babel",
    ],
  };
};
```

3. **Update `metro.config.js`**:

```javascript
const { getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require('nativewind/metro');

const config = getDefaultConfig(__dirname);

module.exports = withNativeWind(config, { input: './global.css' });
```

4. **Create `global.css`** in the root directory:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

5. **Create `nativewind-env.d.ts`** for TypeScript support:

```typescript
/// <reference types="nativewind/types" />
```

### Step 1.3: Explore the Default Template Structure

The tabs template comes with several pre-configured files. Let's explore what's already set up:

**Project Structure:**
```
streaming-app/
├── app/
│   ├── _layout.tsx          # Root layout (don't modify unless needed)
│   └── (tabs)/
│       ├── _layout.tsx      # Tab bar layout (MODIFY THIS)
│       ├── index.tsx         # Home tab (MODIFY THIS)
│       └── explore.tsx       # Explore tab (DELETE or REPURPOSE)
├── components/
│   ├── haptic-tab.tsx        # Tab button component (keep as is)
│   └── ...
├── constants/
│   └── theme.ts              # Theme colors (can customize)
├── hooks/
│   └── use-color-scheme.ts   # Dark/light mode hook (keep as is)
├── global.css                # TailwindCSS styles (NEW)
├── tailwind.config.js        # TailwindCSS config (NEW)
└── nativewind-env.d.ts       # TypeScript types (NEW)
```

**What's Already Working:**
- ✅ Bottom tab navigation bar
- ✅ Two default tabs (index and explore)
- ✅ Dark/light mode theme support
- ✅ Haptic feedback on tab press
- ✅ Basic routing structure
- ✅ TypeScript configuration

**What You Need to Do:**
- 🔧 Modify the tab bar to have 5 tabs for streaming app
- 🔧 Replace default icons with Feather icons
- 🔧 Update existing pages (index.tsx)
- 🔧 Create new pages (search, my-list, downloads, profile)
- 🎨 Design your own UI for each page

### Step 1.4: Examine the Existing Tab Layout

Open `app/(tabs)/_layout.tsx` and examine what's already there. You should see:

- A `Tabs` component with default configuration
- Two existing tabs: `index` and `explore`
- Icons using `IconSymbol` component (we'll replace with Feather)
- Theme support with dark/light mode (we'll keep this)
- Haptic feedback on tab press (we'll keep this)

**Key things to notice:**
- The tab bar is already functional - you just need to modify it
- It uses `useColorScheme()` hook for theme support - keep this
- Icons are configured using the `IconSymbol` component - we'll change to Feather
- The `HapticTab` component provides tactile feedback - keep this

---

## Part 2: Understanding the Existing Code

### Step 2.1: Understanding Expo Router File-Based Routing

Expo Router uses **file-based routing**:
- Files in the `app/` directory become routes
- `_layout.tsx` files define layout components
- Folders wrapped in parentheses `(tabs)` create route groups
- `index.tsx` is the default route for a folder

### Step 2.2: Understanding the Current Tab Setup

The default template includes:
- **`index.tsx`** - Your home screen (default tab)
- **`explore.tsx`** - An explore/discover screen

**Your task:** We'll modify these and add more tabs for a streaming app.

---

## Part 3: Modifying the Tab Bar for Streaming App

### Step 3.1: Install Feather Icons (Optional)

The default template uses `IconSymbol`, but for this lab, we'll use Feather icons. First, verify that `@expo/vector-icons` is installed (it should be in the template):

```bash
npm install @expo/vector-icons
```

### Step 3.2: Update the Tab Layout File

Open `app/(tabs)/_layout.tsx`. You'll see it already has a working tab bar. Now we'll modify it for our streaming app.

**Current structure:**
```typescript
import { Tabs } from 'expo-router';
import { HapticTab } from '@/components/haptic-tab';
import { IconSymbol } from '@/components/ui/icon-symbol';
import { Colors } from '@/constants/theme';
import { useColorScheme } from '@/hooks/use-color-scheme';

export default function TabLayout() {
  const colorScheme = useColorScheme();
  
  return (
    <Tabs
      screenOptions={{
        tabBarActiveTintColor: Colors[colorScheme ?? 'light'].tint,
        headerShown: false,
        tabBarButton: HapticTab,
      }}>
      <Tabs.Screen name="index" ... />
      <Tabs.Screen name="explore" ... />
    </Tabs>
  );
}
```

### Step 3.3: Replace Icons with Feather Icons

1. **Add Feather import** at the top:
```typescript
import { Feather } from '@expo/vector-icons';
```

2. **Update the existing tabs** to use Feather icons. Replace the `tabBarIcon` in each `Tabs.Screen`:

```typescript
<Tabs.Screen
  name="index"
  options={{
    title: 'Home',
    tabBarIcon: ({ color }) => <Feather name="home" size={24} color={color} />,
  }}
/>
```

### Step 3.4: Add New Tabs for Streaming App

For a streaming app, we need 5 tabs:
1. **Home** (index) - Main landing page (already exists, just update icon)
2. **Search** - Search functionality (new)
3. **My List** - User's saved content (new)
4. **Downloads** - Offline content (new)
5. **Profile** - User settings (new)

**Task:** 
1. Update the existing `index` tab to use a `home` Feather icon
2. Remove or rename the `explore` tab
3. Add three new tabs: `search`, `my-list`, `downloads`, and `profile`

**Example for adding a new tab:**
```typescript
<Tabs.Screen
  name="search"
  options={{
    title: 'Search',
    tabBarIcon: ({ color }) => <Feather name="search" size={24} color={color} />,
  }}
/>
```

**Recommended Feather icons:**
- Home: `home`
- Search: `search`
- My List: `bookmark`
- Downloads: `download`
- Profile: `user`

### Step 3.5: Customize Tab Bar Styling (Optional)

You can customize the tab bar appearance by adding `tabBarStyle` to `screenOptions`:

```typescript
<Tabs
  screenOptions={{
    tabBarActiveTintColor: Colors[colorScheme ?? 'light'].tint,
    headerShown: false,
    tabBarButton: HapticTab,
    tabBarStyle: {
      backgroundColor: colorScheme === 'dark' ? '#0a0a0a' : '#ffffff',
      borderTopColor: colorScheme === 'dark' ? '#1a1a1a' : '#e5e5e5',
      borderTopWidth: 1,
      height: 60,
      paddingBottom: 8,
      paddingTop: 8,
    },
  }}>
```

**Task:** Customize the tab bar colors and styling to match your design preferences.

---

## Part 4: Creating and Modifying Page Components

### Step 4.1: Modify the Existing Home Page

1. Open `app/(tabs)/index.tsx`
2. You'll see it already has some content (likely a welcome screen or example content)
3. **Replace or modify** the content to create your Home page

**Basic structure to start with (using NativeWind):**

```typescript
import React from 'react';
import { View, Text, SafeAreaView, ScrollView } from 'react-native';

export default function HomeScreen() {
  return (
    <SafeAreaView className="flex-1 bg-white">
      {/* Header */}
      <View className="p-4 border-b border-gray-200">
        <Text className="text-2xl font-bold">StreamFlix</Text>
        {/* Add icons/buttons here */}
      </View>
      
      {/* Main Content */}
      <ScrollView className="flex-1 p-4">
        <Text className="text-xl font-semibold mb-4">Welcome to Home</Text>
        {/* Your content sections will go here */}
      </ScrollView>
    </SafeAreaView>
  );
}
```

**Key differences with NativeWind:**
- Use `className` prop instead of `style` prop
- Use TailwindCSS utility classes (e.g., `flex-1`, `bg-white`, `p-4`)
- No need for `StyleSheet.create()`
- More concise and readable code

**Task:** Design your Home page with:
- A header section with app name/logo
- Placeholder sections for featured content, categories, trending items
- Use your own design and layout

### Step 4.2: Remove or Modify the Explore Page

The template includes `app/(tabs)/explore.tsx`. You have two options:

**Option 1:** Delete it (if you removed it from tabs)
```bash
# Delete the file if you're not using it
rm app/(tabs)/explore.tsx
```

**Option 2:** Rename/repurpose it as one of your new pages

### Step 4.3: Create New Pages

Create the following new files in `app/(tabs)/`:

1. **`search.tsx`** - Search screen
2. **`my-list.tsx`** - My List screen  
3. **`downloads.tsx`** - Downloads screen
4. **`profile.tsx`** - Profile screen

**Basic template for each page (using NativeWind):**

```typescript
import React from 'react';
import { View, Text, SafeAreaView } from 'react-native';

export default function SearchScreen() {
  return (
    <SafeAreaView className="flex-1 bg-white">
      <View className="p-4 border-b border-gray-200">
        <Text className="text-2xl font-bold">Search</Text>
      </View>
      <View className="flex-1 p-4">
        <Text>This is the Search page</Text>
        {/* Add your content here */}
      </View>
    </SafeAreaView>
  );
}
```

**NativeWind Class Reference:**
- `flex-1` = `{ flex: 1 }`
- `bg-white` = `{ backgroundColor: '#FFFFFF' }`
- `p-4` = `{ padding: 16 }` (padding: 1rem = 16px)
- `text-2xl` = `{ fontSize: 24 }`
- `font-bold` = `{ fontWeight: 'bold' }`
- `border-b` = `{ borderBottomWidth: 1 }`
- `border-gray-200` = `{ borderBottomColor: '#E5E5E5' }`

**Task:** Create all 4 new pages with:
- A title matching the page name
- Basic layout structure
- Placeholder content appropriate for each page
- Your own design and styling

---

## Part 5: Styling Your Application with NativeWind

### Step 5.1: Understanding NativeWind (TailwindCSS for React Native)

NativeWind allows you to use TailwindCSS utility classes in React Native:
- **Use `className` prop** instead of `style` prop
- **Utility classes** - Pre-built classes for common styles (e.g., `flex-1`, `bg-blue-500`)
- **Responsive design** - Built-in responsive utilities
- **Dark mode** - Easy dark mode support with `dark:` prefix
- **No StyleSheet needed** - Write styles directly in JSX

**Common NativeWind Classes:**
- Layout: `flex-1`, `flex-row`, `items-center`, `justify-center`
- Spacing: `p-4` (padding), `m-2` (margin), `gap-4` (gap)
- Colors: `bg-white`, `text-black`, `bg-blue-500`
- Typography: `text-xl`, `font-bold`, `text-center`
- Borders: `border`, `rounded-lg`, `border-gray-200`

### Step 5.2: Style Your Tab Bar

The tab bar styling is already configured in `app/(tabs)/_layout.tsx`. You can customize it further by modifying the `screenOptions`:

```typescript
<Tabs
  screenOptions={{
    tabBarActiveTintColor: Colors[colorScheme ?? 'light'].tint,
    headerShown: false,
    tabBarButton: HapticTab,
    tabBarStyle: {
      backgroundColor: colorScheme === 'dark' ? '#0a0a0a' : '#ffffff',
      borderTopColor: colorScheme === 'dark' ? '#1a1a1a' : '#e5e5e5',
      borderTopWidth: 1,
      height: 60,
      paddingBottom: 8,
      paddingTop: 8,
    },
  }}>
```

**Task:** Customize the tab bar colors and styling to match your design preferences. You can:
- Change active/inactive tab colors
- Modify background colors
- Adjust height and padding
- Change border styles

### Step 5.3: Style Your Pages with NativeWind

**Task:** Design each page with:
- A header section (title, navigation icons)
- Main content area
- Consistent spacing and colors using TailwindCSS classes
- Use of SafeAreaView for proper spacing on devices with notches

**Example structure for a page (using NativeWind):**

```typescript
import { SafeAreaView, View, Text, ScrollView } from 'react-native';
import { Feather } from '@expo/vector-icons';

export default function HomeScreen() {
  return (
    <SafeAreaView className="flex-1 bg-white">
      {/* Header */}
      <View className="flex-row items-center justify-between p-4 border-b border-gray-200">
        <Text className="text-2xl font-bold">StreamFlix</Text>
        <View className="flex-row items-center gap-4">
          <Feather name="bell" size={24} color="#000" />
          <Feather name="cast" size={24} color="#000" />
        </View>
      </View>
      
      {/* Main Content */}
      <ScrollView className="flex-1 p-4">
        {/* Your content sections */}
        <View className="mb-6">
          <Text className="text-xl font-bold mb-4">Featured</Text>
          {/* Content here */}
        </View>
      </ScrollView>
    </SafeAreaView>
  );
}
```

**NativeWind Tips:**
- Use conditional classes: `className={isDark ? 'bg-black' : 'bg-white'}`
- Combine classes: `className="flex-1 p-4 bg-white rounded-lg"`
- Use Tailwind spacing scale: `p-1` (4px), `p-2` (8px), `p-4` (16px), `p-8` (32px)
- Colors: `bg-blue-500`, `text-gray-600`, `border-red-300`
- Responsive: `w-full md:w-1/2` (not always needed in mobile)

---

## Part 6: Adding Functionality (Optional)

### Step 6.1: Add Basic State Management

Use React hooks to manage state:

```typescript
import { useState } from 'react';
import { View, TextInput } from 'react-native';

export default function SearchScreen() {
  const [searchQuery, setSearchQuery] = useState('');
  
  return (
    <View className="p-4">
      <TextInput
        className="border border-gray-300 rounded-lg p-3 bg-white"
        value={searchQuery}
        onChangeText={setSearchQuery}
        placeholder="Search movies..."
        placeholderTextColor="#9CA3AF"
      />
    </View>
  );
}
```

### Step 6.2: Add Navigation Between Screens

For navigation within tabs, you can use:

```typescript
import { useRouter } from 'expo-router';

const router = useRouter();
// Navigate programmatically
router.push('/path');
```

---

## Part 7: Testing Your Application

### Step 7.1: Start the Development Server

```bash
npm start
```

### Step 7.2: Test on Different Platforms

- **iOS Simulator**: Press `i` in the terminal
- **Android Emulator**: Press `a` in the terminal
- **Physical Device**: Scan QR code with Expo Go app
- **Web**: Press `w` in the terminal

### Step 7.3: Test Navigation

1. Verify all tabs are visible
2. Tap each tab to navigate
3. Verify icons change color when active
4. Check that pages render correctly

---

## Part 8: Design Your Application

### Step 8.1: Create a Design Plan

Before coding, plan:
- Color scheme (primary, secondary, background colors)
- Typography (font sizes, weights)
- Layout structure for each page
- Icon choices
- Spacing and padding

### Step 8.2: Implement Your Design

**Task:** Design and implement:
- A cohesive color scheme across all pages
- Consistent typography
- Proper spacing and margins
- Visual hierarchy (what's most important?)
- Dark mode support (optional but recommended)

### Step 8.3: Add Content Sections

For each page, add relevant sections:

**Home Page:**
- Featured content banner
- Categories/genres
- Trending content
- Recently added

**Search Page:**
- Search bar
- Popular searches
- Browse by genre
- Search results

**My List Page:**
- List of saved items
- Empty state (when list is empty)
- Filter/sort options

**Downloads Page:**
- List of downloaded content
- Download settings
- Storage information

**Profile Page:**
- User information
- Settings options
- Account management

---

## Part 9: Best Practices

### Step 9.1: Code Organization

- Keep components modular
- Use consistent naming conventions
- Add comments for complex logic
- Separate styles from component logic

### Step 9.2: Performance

- Use `ScrollView` for long lists
- Optimize images
- Avoid unnecessary re-renders
- Use `React.memo` for expensive components

### Step 9.3: Accessibility

- Add proper labels to icons
- Ensure sufficient color contrast
- Test with screen readers
- Make touch targets at least 44x44 points

---

## Deliverables

Submit the following:

1. **Source Code**
   - All TypeScript/TSX files
   - Properly organized project structure

2. **Screenshots**
   - Screenshot of each tab/page
   - Screenshot of the tab bar

3. **Documentation**
   - Brief explanation of your design choices
   - List of features implemented
   - Any challenges encountered and how you solved them

---

## Evaluation Criteria

Your work will be evaluated on:

1. **Functionality (40%)**
   - All tabs work correctly
   - Navigation functions properly
   - Pages render without errors

2. **Design (30%)**
   - Consistent design language
   - Good use of colors and typography
   - Proper spacing and layout

3. **Code Quality (20%)**
   - Clean, readable code
   - Proper use of React patterns
   - Good component structure

4. **Completeness (10%)**
   - All required pages created
   - Basic content on each page
   - Proper project setup

---

## Troubleshooting

### Common Issues:

1. **Tabs not showing**
   - Check that `_layout.tsx` is in the `(tabs)` folder (should already exist)
   - Verify all tab screens are properly defined in `_layout.tsx`
   - Ensure file names match the `name` prop in `Tabs.Screen`
   - Restart the development server: `npm start`

2. **Icons not displaying**
   - Ensure `@expo/vector-icons` is installed: `npm install @expo/vector-icons`
   - Check icon name spelling (Feather icon names are lowercase, e.g., `home`, `search`)
   - Verify the icon exists in Feather icons: https://feathericons.com/
   - Check that you're importing Feather correctly: `import { Feather } from '@expo/vector-icons';`

3. **Page not found errors**
   - Ensure the file exists in `app/(tabs)/` folder
   - File name must match the `name` prop in `Tabs.Screen` (e.g., `name="search"` → `search.tsx`)
   - For the home tab, use `index.tsx` and `name="index"`
   - Check for typos in file names (use kebab-case: `my-list.tsx` not `myList.tsx`)

4. **Styling not applying (NativeWind)**
   - Ensure NativeWind is properly configured (check `babel.config.js`, `metro.config.js`, `tailwind.config.js`)
   - Verify `global.css` exists and is imported in root `_layout.tsx` (if needed)
   - Check that you're using `className` prop, not `style` prop
   - Restart Metro bundler after configuration changes: `npm start -- --reset-cache`
   - Verify Tailwind classes are spelled correctly (e.g., `bg-white` not `bgwhite`)
   - Check `tailwind.config.js` content paths include your files
   - For dynamic classes, use template literals: `className={`bg-${color}`}` won't work, use conditional: `className={color === 'blue' ? 'bg-blue-500' : 'bg-red-500'}`

5. **Template files causing confusion**
   - The `explore.tsx` file from the template can be deleted if not used
   - Make sure you've updated `_layout.tsx` to remove references to unused tabs
   - Check that all referenced tabs have corresponding files

---

## Additional Resources

- [Expo Router Documentation](https://docs.expo.dev/router/introduction/)
- [React Native Documentation](https://reactnative.dev/docs/getting-started)
- [NativeWind Documentation](https://www.nativewind.dev/) - TailwindCSS for React Native
- [TailwindCSS Documentation](https://tailwindcss.com/docs) - Utility-first CSS framework
- [Feather Icons](https://feathericons.com/)
- [Expo Vector Icons](https://docs.expo.dev/guides/icons/)
- [NativeWind Setup Guide](https://www.nativewind.dev/quick-starts/expo) - Detailed setup instructions

---

## Extension Activities (Optional)

If you finish early, try:

1. Add dark mode support
2. Implement search functionality with real data
3. Add animations between screens
4. Create a detail page for movies/shows
5. Add user authentication flow
6. Implement local storage for "My List"
7. Add pull-to-refresh functionality

---

## Questions for Reflection

After completing the lab, consider:

1. What design patterns did you use?
2. How did you ensure consistency across pages?
3. What challenges did you face?
4. How would you improve the app further?
5. What would you do differently next time?

---

**Good luck with your lab! 🚀**
