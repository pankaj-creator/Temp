Here’s how you can build a React application demonstrating React Hook Form and write test cases using Jest.


---

Folder Structure

react-hook-form-demo/
├── src/
│   ├── components/
│   │   ├── Form.js
│   ├── tests/
│   │   ├── Form.test.js
│   ├── App.js
│   ├── index.js
├── package.json
├── README.md


---

1. File: src/components/Form.js

This component demonstrates React Hook Form usage.

import React from "react";
import { useForm } from "react-hook-form";

const Form = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    alert(JSON.stringify(data, null, 2));
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label htmlFor="name">Name:</label>
        <input
          id="name"
          {...register("name", { required: "Name is required" })}
        />
        {errors.name && <p>{errors.name.message}</p>}
      </div>

      <div>
        <label htmlFor="email">Email:</label>
        <input
          id="email"
          type="email"
          {...register("email", {
            required: "Email is required",
            pattern: {
              value: /^\S+@\S+$/i,
              message: "Invalid email address",
            },
          })}
        />
        {errors.email && <p>{errors.email.message}</p>}
      </div>

      <button type="submit">Submit</button>
    </form>
  );
};

export default Form;


---

2. File: src/App.js

The main application file to render the form.

import React from "react";
import Form from "./components/Form";

const App = () => {
  return (
    <div>
      <h1>React Hook Form Demo</h1>
      <Form />
    </div>
  );
};

export default App;


---

3. File: src/index.js

The entry point of the application.

import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);


---

4. File: src/tests/Form.test.js

Test cases using Jest and React Testing Library.

import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import Form from "../components/Form";

describe("Form Component", () => {
  test("renders the form correctly", () => {
    render(<Form />);
    expect(screen.getByLabelText(/name/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
    expect(screen.getByText(/submit/i)).toBeInTheDocument();
  });

  test("shows error messages for empty fields", async () => {
    render(<Form />);
    fireEvent.click(screen.getByText(/submit/i));
    expect(await screen.findByText(/name is required/i)).toBeInTheDocument();
    expect(await screen.findByText(/email is required/i)).toBeInTheDocument();
  });

  test("shows error for invalid email", async () => {
    render(<Form />);
    fireEvent.input(screen.getByLabelText(/email/i), {
      target: { value: "invalid-email" },
    });
    fireEvent.click(screen.getByText(/submit/i));
    expect(await screen.findByText(/invalid email address/i)).toBeInTheDocument();
  });

  test("submits the form with valid data", async () => {
    render(<Form />);
    fireEvent.input(screen.getByLabelText(/name/i), {
      target: { value: "John Doe" },
    });
    fireEvent.input(screen.getByLabelText(/email/i), {
      target: { value: "john.doe@example.com" },
    });
    fireEvent.click(screen.getByText(/submit/i));
    expect(screen.queryByText(/name is required/i)).not.toBeInTheDocument();
    expect(screen.queryByText(/email is required/i)).not.toBeInTheDocument();
  });
});


---

5. File: package.json

Add necessary dependencies.

{
  "name": "react-hook-form-demo",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "react-hook-form": "^7.0.0",
    "react-scripts": "5.0.1"
  },
  "devDependencies": {
    "@testing-library/react": "^14.0.0",
    "@testing-library/jest-dom": "^6.0.0",
    "jest": "^29.0.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}


---

6. File: README.md

Add instructions to run the app.

# React Hook Form Demo

## Steps to Run
1. Clone the repository.
2. Install dependencies: `npm install`.
3. Start the app: `npm start`.
4. Run tests: `npm test`.

Now you have a complete React app showcasing React Hook Form with test cases written using Jest. Let me know if you need additional help!

