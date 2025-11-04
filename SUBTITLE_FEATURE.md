# iOS 26+ Subtitle Feature

This fork adds support for iOS 26+ navigation header subtitle feature to react-native-screens.

## Features

- Native iOS 26+ subtitle support
- React Navigation integration with `subtitle` and `headerSubtitle` props
- Support for both regular and large subtitle variants
- Customizable subtitle styles (font family, size, weight, color)

## Installation

### 1. Install react-native-screens from this fork

```bash
npm install github:justin-sqds/react-native-screens#main
# or
yarn add github:justin-sqds/react-native-screens#main
```

### 2. Apply React Navigation patch

This feature requires changes to `@react-navigation/native-stack`. You have two options:

#### Option A: Using patch-package (Recommended)

1. Install patch-package:
```bash
npm install --save-dev patch-package
# or
yarn add -D patch-package
```

2. Add postinstall script to your `package.json`:
```json
{
  "scripts": {
    "postinstall": "patch-package"
  }
}
```

3. Create the patch directory and apply the patch:
```bash
mkdir -p patches
curl -o patches/@react-navigation+native-stack+7.6.7.patch https://raw.githubusercontent.com/justin-sqds/react-native-screens/main/react-navigation-subtitle.patch
```

4. Run install again:
```bash
npm install
# or
yarn install
```

#### Option B: Manual patch

1. Download the patch file:
```bash
curl -o react-navigation-subtitle.patch https://raw.githubusercontent.com/justin-sqds/react-native-screens/main/react-navigation-subtitle.patch
```

2. Apply it to your node_modules:
```bash
cd node_modules/@react-navigation/native-stack
git apply ../../../react-navigation-subtitle.patch
```

Note: You'll need to reapply this patch after every `npm install` / `yarn install`.

## Usage

### Basic Example

```tsx
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

function MyStack() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{
          title: 'My App',
          subtitle: 'Welcome back!', // Simple subtitle
        }}
      />
    </Stack.Navigator>
  );
}
```

### Advanced Example with Styling

```tsx
<Stack.Screen
  name="Article"
  component={ArticleScreen}
  options={{
    title: 'Future of Mobile Dev',
    headerSubtitle: '09/05/2025 • 5 min read',
    headerSubtitleStyle: {
      fontSize: 13,
      fontWeight: '400',
      color: '#666',
    },
  }}
/>
```

### Large Title with Subtitle

```tsx
<Stack.Screen
  name="Profile"
  component={ProfileScreen}
  options={{
    headerLargeTitle: true,
    title: 'Profile',
    subtitle: 'John Doe',
    headerLargeSubtitle: 'Software Engineer',
    headerLargeSubtitleStyle: {
      fontSize: 17,
      fontWeight: '500',
      color: '#007AFF',
    },
  }}
/>
```

## API

### Props

Both `subtitle` and `headerSubtitle` are supported (similar to `title` and `headerTitle`):

- **`subtitle`**: Simple string subtitle (fallback for `headerSubtitle`)
- **`headerSubtitle`**: String to display below the title in regular header
- **`headerSubtitleStyle`**: Style object for subtitle
  - `fontFamily`: string
  - `fontSize`: number
  - `fontWeight`: string
  - `color`: string

- **`headerLargeSubtitle`**: String to display below large title
- **`headerLargeSubtitleStyle`**: Style object for large subtitle (same properties as above)

### Platform Support

- **iOS 26.0+**: Full support
- **iOS < 26.0**: Props are ignored (no error)
- **Android**: Props are ignored (no error)

## TypeScript

The types are included but you may see TypeScript errors if using an older version of `@react-navigation/native-stack`. You can use `@ts-expect-error` comment:

```tsx
options={{
  title: 'My Screen',
  // @ts-expect-error - subtitle support not in published types yet
  subtitle: 'My Subtitle',
}}
```

## Testing

Run the FabricExample app on iOS 26+ simulator to see the feature in action:

```bash
cd FabricExample
yarn install
cd ios && pod install && cd ..
yarn ios --simulator="iPhone 17 Pro"
```

## Requirements

- iOS 26.0 or later
- react-native-screens 4.0.0+
- @react-navigation/native-stack 7.0.0+

## Credits

Original subtitle implementation by [Fabrizio Beccaceci](https://github.com/fbeccaceci/react-native-screens)
