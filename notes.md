# Project Notes

## General Setup

```bash
npm install
npm run dev
```

---

## Capture Data

### Single Handler for All Changes

**Big picture (one sentence):**

One function listens to all inputs and decides which piece of state to update by reading the input’s `name`.

That’s it.

---

## App.jsx Structure

```jsx
<label>
  Full Name
  <input
    name="fullName"
    type="text"
    placeholder="Full Name"
    onChange={handleChange}
  />
</label>

<label>
  Profile Image
  <input
    name="image"
    type="url"
    placeholder="Profile Image"
    onChange={handleChange}
  />
</label>
```

---

## **SINGLE** Handler for **ALL** Changes

```js
const handleChange = (e) => {
  const { name, value, type, checked } = e.target;

  if (type === "checkbox") {
    setGraduated(checked);
    return;
  }

  if (name === "fullName") {
    setFullName(value);
  } else if (name === "image") {
    setImage(value);
  } else if (name === "phone") {
    setPhone(value);
  } else if (name === "email") {
    setEmail(value);
  } else if (name === "program") {
    setProgram(value);
  } else if (name === "graduationYear") {
    setGraduationYear(Number(value));
  }
};
```

---

## Line-by-Line Explanation (Plain English)

### 1️⃣ This function runs every time **any input changes**

When the user:
- types into **Full Name**
- clicks the **Graduated** checkbox
- selects a **Program**

➡️ React calls `handleChange`.

---

### 2️⃣ We pull useful info from the input that changed

```js
const { name, value, type, checked } = e.target;
```

Think of `e.target` as:

> **“The input that was just touched.”**

Example input:

```jsx
<input
  name="fullName"
  type="text"
  placeholder="Full Name"
  onChange={handleChange}
/>
```

Internally:

```js
e = {
  target: <input ... />
}
```

So:

- `name` → which input is this?
- `value` → what did the user type or select?
- `type` → what kind of input is it?
- `checked` → true / false (checkbox only)

This line:

```js
const { name, value } = e.target;
```

is the same as:

```js
const name = e.target.name;
const value = e.target.value;
```

---

### 3️⃣ Handle the special case first: checkbox

```js
if (type === "checkbox") {
  setGraduated(checked);
  return;
}
```

Why?
- Checkboxes do **not** use `value`
- They use `checked` (`true` or `false`)

So:
- If the changed input is a checkbox
- Update `graduated`
- Stop the function

---

### 4️⃣ For everything else, look at the `name`

```js
if (name === "fullName") {
  setFullName(value);
}
```

Translation:

> “If the input that changed is the full name field, update the `fullName` state with what the user typed.”

---

### 5️⃣ Same logic, different fields

```js
else if (name === "image") {
  setImage(value);
}
```

> “If the input is the image field, update the image state.”

Same pattern:
- input `name`
- matching `setState`

---

### 6️⃣ Numbers need one extra step

```js
else if (name === "graduationYear") {
  setGraduationYear(Number(value));
}
```

Why?
- User types `2025`
- Browser gives `"2025"` (string)
- We convert it to `2025` (number)

---

## Final Takeaway (Write This Down)

Inputs identify themselves using **`name`**.  
`handleChange` reads that name and updates the matching state.

---

## Submit Data

### Big picture (one sentence)

When the form is submitted, we stop the page from reloading, bundle up the form data, and add it to the list of students.

---

## Line-by-Line Explanation

### 1️⃣ This function runs when the form is submitted

```js
const handleSubmit = (e) => {
  e.preventDefault();

  const newStudent = {
    fullName,
    image,
    phone,
    email,
    program,
    graduationYear,
    graduated,
  };
```

- User clicks **Add Student**
- Browser submits the form
- React calls `handleSubmit`

---

### 2️⃣ Stop the page from refreshing

```js
e.preventDefault();
```

Normally, submitting a form:
- reloads the page
- wipes your data

This line says:

> “Don’t do that. Stay on this page.”

---

### 3️⃣ Create a new student record

```js
const newStudent = {
  fullName,
  image,
  phone,
  email,
  program,
  graduationYear,
  graduated,
};
```

Meaning:

> “Take everything the user typed (already stored in state) and group it into one object.”

Example result:

```js
{
  fullName: "Ana Perez",
  image: "https://...",
  phone: "123456",
  email: "ana@email.com",
  program: "Web Dev",
  graduationYear: 2025,
  graduated: false
}
```

No guessing.  
No reading from the DOM.  
Just using state.

---

### 4️⃣ Add the new student to the list

```js
setStudents([...students, newStudent]);
```

This means:

> “Copy the existing students array and append the new student.”

After this:
- the old array is discarded
- the new array becomes `students`

---

### 5️⃣ React then

- re-renders the UI
- shows the new student in the table

---

## Why This Is the Correct React Way

- Inputs update state as the user types
- Submit reads from state
- No direct DOM access
- Predictable, clean, testable

---

## One-Line Summary

`handleSubmit` prevents refresh, creates a student object from state, and adds it to the students list.