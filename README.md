The reason this exists is because installing NativeWind
has overall been a pain, and I need proper guidelines if I 
am to do this again. 

# Basic
1. Start project and `cd` into it
```aiignore
npx create-expo-app@latest
```
2. Install dependencies
```aiignore
npm install
```
3. Start a fresh, bare-bones project
```aiignore
npm run reset-project
```

# NativeWind
> Create all files at root or project unless otherwise stated.
1. Install NativeWind and its dependencies
```aiignore
npm install nativewind react-native-reanimated@~3.17.4 react-native-safe-area-context@5.4.0
npm install -D tailwindcss
```
2. Tailwind config
```aiignore
npx tailwindcss init
```
3. Replace contents of `tailwind.config.js` with:
```aiignore
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./App.tsx", "./components/**/*.{js,jsx,ts,tsx}", "./app/**/*.{js,jsx,ts,tsx}"],
  presets: [require("nativewind/preset")],
  theme: { extend: {} },
  plugins: [],
};
```
4. Create `babel.config.js` file and write:
```aiignore
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
5. Create `metro.config.js` and write:
```aiignore
{ getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require("nativewind/metro");

const config = getDefaultConfig(__dirname);

module.exports = withNativeWind(config, { input: "./global.css" });
```
6. Create `global.css` and write: 
```aiignore
@tailwind base;
@tailwind components;
@tailwind utilities;
```
7. Import `global.css` to `_layout.tsx`. 
8. Modify `app.json` such that the `bundler` is `metro`:
```aiignore
{
  "expo": {
    "web": {
      "bundler": "metro"
    }
  }
}
```

# TypeScript
1. Create `nativewind-env.d.ts` and write:
```aiignore
/// <reference types="nativewind/types" />
```

# Test it out: 
`_layout.tsx` should look like this: 
```aiignore
import "../global.css"
import { Text, View } from "react-native";

export default function RootLayout() {
  return (
      <View className="flex-1 items-center justify-center bg-white">
        <Text className="text-xl font-bold text-blue-500">
          Welcome to Nativewind!
        </Text>
      </View>
  );
}
```  
Now, run the following command to see "Welcome Nativewind" in blue text.
```aiignore
npx expo start
```

# Sources
- [reactnative docs](https://reactnative.dev/docs/environment-setup)
- [expo docs](https://docs.expo.dev/get-started/start-developing/)
- [nativewind docs](https://www.nativewind.dev/docs/getting-started/installation)
- [another struggling soul's blog](https://dev.to/agent_69/how-to-setup-tailwind-css-to-your-expo-project-2l4i)
