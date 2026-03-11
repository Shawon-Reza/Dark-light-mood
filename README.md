# Dark-light-mood

# Dark / Light Mode Implementation

This project implements a **manual dark/light mode toggle** with persistence using:

- React Context
- localStorage
- Tailwind CSS v4 dark mode variant

## Features

- Toggle between light & dark mode with a button
- Preference saved in localStorage (persists across sessions & page reloads)
- Smooth class-based dark mode using Tailwind's `dark:` prefix
- Works without depending on system/OS color scheme preference

## Key Files

| File                        | Purpose                                                                 |
|-----------------------------|-------------------------------------------------------------------------|
| `src/Contexts/ThemeContext.jsx` | Theme state management + localStorage sync + `dark` class on `<html>`  |
| `src/components/DarkModeButton.jsx` | Simple toggle button component                                       |
| `src/index.css`             | Required custom variant setup for Tailwind v4 dark mode               |
| `src/main.jsx`              | Wraps app with `<ThemeProvider>`                                       |

## How to Use Dark Mode in Components

Just add `dark:` prefix to any Tailwind class:

```tsx
<div className="bg-white dark:bg-gray-900 text-black dark:text-gray-100 p-6 rounded-lg">
  This card automatically changes in dark mode!
</div>

<button className="bg-blue-600 dark:bg-blue-500 hover:bg-blue-700 dark:hover:bg-blue-600">
  Works everywhere
</button>
```




# DarkModeButton.jsx

```jsx
import { useDarkMode } from "../../Contexts/ThemeContext";


function DarkModeButton() {
    const { darkMode, setDarkMode } = useDarkMode();

    return (
        <button
            onClick={() => setDarkMode(!darkMode)}
            className="px-4 py-2 rounded bg-gray-200 dark:bg-gray-700 text-gray-900 dark:text-white transition-colors"
        >
            {darkMode ? "Switch to Light" : "Switch to Dark"}
        </button>
    );
}

export default DarkModeButton;

```
# index.css
```jsx

/* This line is required in Tailwind v4 — without it, dark mode toggle won’t work from the website (it only works using device theme). */

@custom-variant dark (&:where(.dark, .dark *));  /* ← this is the magic */
/* @custom-variant dark (&:is(.dark *)); */

/* Used one from this two */

```

# ThemeContext.jsx   under Contexts file 

```jsx
import { createContext, useContext, useEffect, useState } from "react";

const ThemeContext = createContext("light");

export const ThemeProvider = ({ children }) => {
    const [darkMode, setDarkMode] = useState(() => {
        const storedTheme = localStorage.getItem("theme");
        return storedTheme === "dark" ? true : false;
    });

    useEffect(() => {
        if (darkMode) {
            document.documentElement.classList.add("dark");
            localStorage.setItem("theme", "dark");
        } else {
            document.documentElement.classList.remove("dark");
            localStorage.setItem("theme", "light");
        }
    }, [darkMode]);

    return (
        <ThemeContext.Provider value={{ darkMode, setDarkMode }}>
            {children}
        </ThemeContext.Provider>
    );
};

export const useDarkMode = () => useContext(ThemeContext); 

```

# main.jsx

```jsx
 <StrictMode>
    <ThemeProvider>
      <ToastContainer />
      <RouterProvider router={router} />
    </ThemeProvider>
  </StrictMode>,

```
# use like this on classename ="dark:text-gray-300"
```jsx
dark:text-gray-300 

```

# tailwind.config.cjs            / Maybe no needed

```jsx
module.exports = {
  content: [
    './index.html',
    './src/**/*.{js,jsx,ts,tsx,html}'
  ],
  darkMode: 'class',
   theme: {
    extend: {
      colors: {
        customDarkBg: '#1f2937',
        customDarkText: '#f3f4f6',
      },
    },
  },
  plugins: [],
}


```
