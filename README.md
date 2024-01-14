import { useState } from "react";
import "./styles.css";

export default function App() {
  const [data, setData] = useState({
    name: "",
    email: "",
    message: "",
    password: "",
  });

  const [errors, setErrors] = useState(null);
  const [showPassword, setShowPassword] = useState(false);

  const handleBlur = (e) => {
    const { name, value } = e.target;

    setData({
      ...data,
      [name]: value,
    });
  };

  const handleCheckboxChange = () => {
    setShowPassword(!showPassword);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    const emailRegex = /^[\w.-]+@[a-zA-Z_-]+?.[a-zA-Z]{2,3}$/;
    const nameRegex = /^[a-zA-Z ]+$/;
    const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;

    const newErrors = {};

    if (data.name === "") {
      newErrors.name = "Name is required";
    } else if (!nameRegex.test(data.name)) {
      newErrors.name = "Name should be a valid string";
    }

    if (data.email === "") {
      newErrors.email = "Email is required";
    } else if (!emailRegex.test(data.email)) {
      newErrors.email = "Email should be a valid email";
    }

    if (data.password === "") {
      newErrors.password = "Password is required";
    } else if (!passwordRegex.test(data.password)) {
      newErrors.password = "Password should meet the specified criteria";
    }

    if (data.message === "") {
      newErrors.message = "Message is required";
    } else if (data.message.length < 120) {
      newErrors.message = "Minimum 120 words required";
    } else if (data.message.length > 250) {
      newErrors.message = "Message should be maximum 250 words";
    }

    setErrors(newErrors);

    if (Object.keys(newErrors).length === 0) {
      console.log("Form submitted successfully!");
    }
  };

  return (
    <div>
      <h1>Contact us!</h1>
      <input
        type="text"
        className="rounded-lg border-transparent flex-1 ..."
        placeholder="Name"
        onBlur={handleBlur}
        name="name"
      />
      {errors && errors.name && <div>{errors.name}</div>}

      <input
        type="text"
        className="rounded-lg border-transparent flex-1 ..."
        placeholder="Email"
        onBlur={handleBlur}
        name="email"
      />
      {errors && errors.email && <div>{errors.email}</div>}

      <input
        type={showPassword ? "text" : "password"}
        className="rounded-lg border-transparent flex-1 ..."
        placeholder="Password"
        onBlur={handleBlur}
        name="password"
      />
      <label>
        <input
          type="checkbox"
          checked={showPassword}
          onChange={handleCheckboxChange}
        />
        Show Password
      </label>
      {errors && errors.password && <div>{errors.password}</div>}

      <textarea
        className="flex-1 w-full px-4 py-2 ..."
        placeholder="Enter your comment"
        name="message"
        rows="5"
        cols="40"
        onBlur={handleBlur}
      />
      {errors && errors.message && <div>{errors.message}</div>}

      <button onClick={handleSubmit}>Send</button>
    </div>
  );
}
