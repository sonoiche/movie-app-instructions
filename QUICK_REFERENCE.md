# Quick Reference Guide

## Essential Code Snippets

### Tab Layout Template

```typescript
// app/(tabs)/_layout.tsx
import { Tabs } from 'expo-router';
import { Feather } from '@expo/vector-icons';
import React from 'react';

export default function TabLayout() {
  return (
    <Tabs
      screenOptions={{
        headerShown: false,
        tabBarActiveTintColor: '#007AFF',
        tabBarInactiveTintColor: '#8E8E93',
      }}>
      <Tabs.Screen
        name="index"
        options={{
          title: 'Home',
          tabBarIcon: ({ color, size }) => (
            <Feather name="home" size={size} color={color} />
          ),
        }}
      />
      {/* Add more tabs here */}
    </Tabs>
  );
}
```

### Basic Page Template (NativeWind)

```typescript
// app/(tabs)/index.tsx
import React from 'react';
import { View, Text, SafeAreaView } from 'react-native';

export default function HomeScreen() {
  return (
    <SafeAreaView className="flex-1 bg-white">
      <View className="p-4 border-b border-gray-200">
        <Text className="text-2xl font-bold">Home</Text>
      </View>
      <View className="flex-1 p-4">
        {/* Your content here */}
      </View>
    </SafeAreaView>
  );
}
```

### Common React Native Components (NativeWind)

```typescript
// ScrollView for scrollable content
<ScrollView className="flex-1">
  {/* Content */}
</ScrollView>

// TextInput for user input
<TextInput
  className="border border-gray-300 rounded-lg p-3 bg-white"
  value={value}
  onChangeText={setValue}
  placeholder="Enter text..."
  placeholderTextColor="#9CA3AF"
/>

// TouchableOpacity for buttons
<TouchableOpacity 
  className="bg-blue-500 px-6 py-3 rounded-lg"
  onPress={handlePress}
>
  <Text className="text-white font-semibold text-center">Button</Text>
</TouchableOpacity>

// Image for displaying images
<Image
  source={{ uri: 'https://example.com/image.jpg' }}
  className="w-24 h-24 rounded-lg"
/>
```

### Common Feather Icons

```typescript
// Navigation
<Feather name="home" />
<Feather name="search" />
<Feather name="user" />

// Actions
<Feather name="play" />
<Feather name="bookmark" />
<Feather name="download" />
<Feather name="heart" />
<Feather name="share" />

// UI Elements
<Feather name="menu" />
<Feather name="settings" />
<Feather name="bell" />
<Feather name="chevron-right" />
```

### NativeWind Common Classes

```typescript
// Layout Classes
className="flex-1"                    // flex: 1
className="flex-row"                  // flexDirection: 'row'
className="items-center"               // alignItems: 'center'
className="justify-center"            // justifyContent: 'center'
className="justify-between"            // justifyContent: 'space-between'

// Spacing Classes
className="p-4"                       // padding: 16px
className="px-4"                      // paddingHorizontal: 16px
className="py-2"                      // paddingVertical: 8px
className="m-2"                       // margin: 8px
className="gap-4"                     // gap: 16px

// Colors
className="bg-white"                  // backgroundColor: '#FFFFFF'
className="bg-blue-500"               // backgroundColor: '#3B82F6'
className="text-black"                // color: '#000000'
className="text-gray-600"             // color: '#4B5563'

// Typography
className="text-xl"                    // fontSize: 20px
className="text-2xl"                   // fontSize: 24px
className="font-bold"                  // fontWeight: 'bold'
className="font-semibold"              // fontWeight: '600'
className="text-center"                // textAlign: 'center'

// Borders & Rounded
className="border"                     // borderWidth: 1
className="border-gray-200"           // borderColor: '#E5E7EB'
className="rounded-lg"                 // borderRadius: 8px
className="rounded-full"               // borderRadius: 9999px
```

### Flexbox Layout Guide (NativeWind)

```typescript
// Container with flex
<View className="flex-1 flex-row items-center justify-center gap-4">
  {/* Content */}
</View>

// Common flex patterns
className="flex-1"              // Take all available space
className="flex-row"            // Horizontal layout
className="flex-col"            // Vertical layout (default)
className="items-center"        // Center items cross-axis
className="justify-between"     // Space between items
className="gap-4"               // Space between items
```

### Color Palette Suggestions (NativeWind)

```typescript
// Light Theme Classes
className="bg-white"           // Background
className="text-black"         // Text
className="bg-blue-500"         // Primary color
className="border-gray-200"    // Borders

// Dark Mode (use conditional)
className={isDark ? 'bg-black' : 'bg-white'}
className={isDark ? 'text-white' : 'text-black'}

// Common Tailwind Colors
className="bg-blue-500"        // Blue
className="bg-red-500"          // Red
className="bg-green-500"        // Green
className="bg-yellow-500"       // Yellow
className="bg-purple-500"       // Purple
className="bg-gray-500"         // Gray
className="bg-indigo-500"       // Indigo
className="bg-pink-500"         // Pink
```

### Common Patterns

#### State Management
```typescript
const [value, setValue] = useState('');
const [loading, setLoading] = useState(false);
const [items, setItems] = useState([]);
```

#### Conditional Rendering (with NativeWind)
```typescript
{loading ? (
  <ActivityIndicator className="flex-1" />
) : (
  <View className="flex-1 p-4">{/* Content */}</View>
)}

{items.length > 0 && (
  <View className="p-4">{/* Show items */}</View>
)}

// Conditional classes
<View className={`p-4 ${isActive ? 'bg-blue-500' : 'bg-gray-200'}`}>
  <Text className={isDark ? 'text-white' : 'text-black'}>Content</Text>
</View>
```

#### List Rendering (with NativeWind)
```typescript
{items.map((item) => (
  <View key={item.id} className="p-4 border-b border-gray-200">
    <Text className="text-lg font-semibold">{item.name}</Text>
  </View>
))}
```
