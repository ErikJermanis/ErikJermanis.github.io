# Solution for caret jumping in React inputs

## What are we solving?

Let's say that we have a controled input and, on change, we are updating the input value by replacing `" "` (spaces) with `-` symbols.

The component looks like this:

```js linenums="1" title="CustomInput.jsx"
import { useState } from "react";

const CustomInput = () => {
	const [value, setValue] = useState("");

	const handleChange = (e) => {
		setValue(e.target.value.replace(/ /g, "-"));
	};

	return <input value={value} onChange={handleChange} />;
};

export default CustomInput;
```

Now, if we type in `Hello World`, it will get formatted as `Hello-World`.

Let's say that we want it to say "Hello React World" instead. We position our cursor caret behind letter `o`, and type `<space>React`.

The problem is that as soon as we enter the first character, in this situation `<space>`, the caret jumps to the end of the input value. If we just continued typing, input value would be `Hello--WorldReact` instead of `Hello-React-World`.

## Why does this happen?

To solve this problem, it's necessary to understand that this has nothing to do with _React_. The same problem would occur if you used plain _JavaScript_ + _HTML_.

The reason why this happens is because we are injecting a modified value in a DOM input (the one where we replace `" "` with `-`). When that happens, the input moves caret to the end.

## Preventing jumps

This can be fixed by saving the caret position before changing the input value, and restoring it afterwards.

We can do that in two steps:

1. Saving caret position (`selectionRange`) to state.
2. Updating input's `selectionStart` and `selectionEnd` values.

### 1. Saving caret position

First we need to add state for storing selectionRange and then, store it in `handleChange` function.

```js linenums="1" title="CustomInput.jsx" hl_lines="5 9"
import { useState } from "react";

const CustomInput = () => {
	const [value, setValue] = useState("");
	const [selection, setSelection] = useState(null);

	const handleChange = (e) => {
		setValue(e.target.value.replace(/ /g, "-"));
		setSelection([e.target.selectionStart, e.target.selectionEnd]);
	};

	return <input value={value} onChange={handleChange} />;
};

export default CustomInput;
```

### 2. Updating selectionRange

Now, we need to restore caret position.

Firstly, add a _ref_ to the input, and then use it to access input element and set it's _selectionRange_. To set the _selectionRange_, we will use the `useLayoutEffect` hook from _React_.

```js linenums="1" title="CustomInput.jsx" hl_lines="1 4 9-14 21"
import { useState, useLayoutEffect, useRef } from "react";

const CustomInput = () => {
	const inputRef = useRef(null);

	const [value, setValue] = useState("");
	const [selection, setSelection] = useState(null);

	useLayoutEffect(() => {
		if (selection && inputRef.current) {
			[inputRef.current.selectionStart, inputRef.current.selectionEnd] =
				selection;
		}
	}, [selection]);

	const handleChange = (e) => {
		setValue(e.target.value.replace(/ /g, "-"));
		setSelection([e.target.selectionStart, e.target.selectionEnd]);
	};

	return <input ref={inputRef} value={value} onChange={handleChange} />;
};

export default CustomInput;
```

!!! tip

    `useLayoutEffect` is very similar to `useEffect`. The difference is that `useLayoutEffect` makes sure that any state updates or logic are processed before the browser repaints the screen. By using `useLayoutEffect` instead of `useEffect`, we are preventing any potential screen flickers.

That's it! Now the caret is no longer jumping to the end.
