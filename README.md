# Dark-light-mood

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
@custom-variant dark (&:is(.dark *));

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
