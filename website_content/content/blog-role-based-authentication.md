# Role-Based Authentication in React Native

## Introduction

In today’s app-driven world, **security and access control** are essential. While simple authentication (verifying that a user is who they say they are) is a must, **authorization** (controlling what resources a user can access) is equally important. One of the most effective ways to handle authorization is through **Role-Based Authentication** (also called Role-Based Access Control — RBAC).

In this chapter, we'll dive deep into what **Role-Based Authentication** means, why it's critical for scalable apps, and how you can implement it effectively in a **React Native** application.

---

## Explanation

### What Is Role-Based Authentication?

**Role-Based Authentication (RBAC)** is a system where permissions to access certain parts of the application are assigned based on a user's role. Rather than giving permissions to each individual user, we assign permissions to roles, and then users are assigned roles.

* **Authentication** = verifying who you are (e.g., via username and password).
* **Authorization** = verifying what you can access (e.g., Admin can add users; normal User cannot).

With RBAC:

* Admins can perform admin tasks.
* Editors can modify content.
* Regular users can only view content.

> **Why is it important?**
>
> * **Scalability:** Easy to manage many users by assigning them roles instead of individual permissions.
> * **Security:** Ensures least-privilege access — users get only what they need.
> * **Maintainability:** As your app grows, adding a new role is easier than assigning permissions individually.

---

### How It Works Internally

1. **Sign-up/Login:**

   * When a user logs in, the backend authenticates the user and sends an **access token** (JWT) containing the user's `role` as part of the payload (or claims).

2. **Token Validation:**

   * When making API requests, the token is attached to the request header.
   * The backend checks the token and verifies the user's role to allow or deny access to specific endpoints.

3. **Role-Based Frontend Logic:**

   * On the frontend (React Native), the app decodes the token or uses stored user data to render different screens/components based on the user's role.

---

### Common Terminologies

* **Roles:** Labels like `admin`, `user`, `editor`, etc.
* **Permissions:** Actions users with a certain role are allowed to perform.
* **Access Token:** JWT that contains the role in its payload.
* **Protected Route/Component:** A screen or feature that only certain roles can access.

---

## Real-World Use Cases

### 1. **E-commerce App**

* **Admin:** Can add/remove products, manage users.
* **Seller:** Can add/manage their own products.
* **Customer:** Can browse and buy products.

### 2. **Social Media App**

* **Admin:** Moderate content, ban users.
* **Moderator:** Review reports.
* **User:** Create posts, comment, like.

---

## Code Examples

Let’s build a simplified version of **Role-Based Authentication**.

### 1. **User Signup (Backend API Call)**

Example of creating a user with a role:

```bash
curl -X 'POST' \
  'https://backend.ecom.subraatakumar.com/api/v1/auth/signup' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "username": "newuser",
  "password": "Password@123",
  "role": "user"
}'
```

It returns:

```json
{
  "accessToken": "string",
  "refreshToken": "string"
}
```

The `accessToken` will have the `role` information encoded.

---

### 2. **Setting Up Role-Based Authentication in React Native**

Install necessary dependencies:

```bash
npm install @react-navigation/native @react-navigation/stack jwt-decode
```

### 3. **Decoding the JWT to Extract Role**

```javascript
import jwtDecode from 'jwt-decode';

// Example function to decode token
const getUserRoleFromToken = (token) => {
  try {
    const decoded = jwtDecode(token);
    return decoded.role; // Assuming role is stored as "role"
  } catch (error) {
    console.error('Failed to decode token', error);
    return null;
  }
};
```

---

### 4. **Role-Based Navigation**

```javascript
import React, { useEffect, useState } from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import jwtDecode from 'jwt-decode';
import AsyncStorage from '@react-native-async-storage/async-storage';

const Stack = createStackNavigator();

// Screens
import AdminDashboard from './screens/AdminDashboard';
import UserDashboard from './screens/UserDashboard';
import LoginScreen from './screens/LoginScreen';

const AppNavigator = () => {
  const [role, setRole] = useState(null);

  useEffect(() => {
    const fetchRole = async () => {
      const token = await AsyncStorage.getItem('accessToken');
      if (token) {
        const decoded = jwtDecode(token);
        setRole(decoded.role);
      }
    };

    fetchRole();
  }, []);

  if (!role) {
    return (
      <Stack.Navigator>
        <Stack.Screen name="Login" component={LoginScreen} />
      </Stack.Navigator>
    );
  }

  return (
    <Stack.Navigator>
      {role === 'admin' ? (
        <Stack.Screen name="AdminDashboard" component={AdminDashboard} />
      ) : (
        <Stack.Screen name="UserDashboard" component={UserDashboard} />
      )}
    </Stack.Navigator>
  );
};

export default function App() {
  return (
    <NavigationContainer>
      <AppNavigator />
    </NavigationContainer>
  );
}
```

---

### 5. **Role-Based Component Access**

You can hide/show components based on the role:

```javascript
const Dashboard = ({ role }) => {
  return (
    <View>
      <Text>Welcome {role}</Text>
      {role === 'admin' && (
        <Button title="Manage Users" onPress={() => { /* navigate to manage users */ }} />
      )}
      <Button title="View Profile" onPress={() => { /* navigate to profile */ }} />
    </View>
  );
};
```

---

## Best Practices

1. **Always Validate Role on Backend**

   * Never trust the frontend for authorization. Frontend is only for **UI control**, backend is the **true gatekeeper**.

2. **Store Token Securely**

   * Use libraries like `@react-native-async-storage/async-storage`.
   * For higher security, prefer encrypted storage.

3. **Refresh Tokens**

   * Implement token refreshing for better UX.

4. **Minimal Token Payload**

   * Include only essential information like `id`, `username`, and `role` in the JWT payload.

5. **Role Hierarchies**

   * Some apps need hierarchical roles (e.g., `admin > moderator > user`). Plan accordingly.

---

## Common Pitfalls

* **Not verifying role on backend:** Huge security risk.
* **Hardcoding role checks:** Use enums/constants to avoid typos.
* **Storing sensitive data in JWT:** JWT should not contain passwords or sensitive data.
* **No graceful fallback:** Always handle cases where role is invalid or missing.

---

## Key Takeaways

* **Authentication** checks identity; **Authorization** checks permissions.
* **RBAC** simplifies permission management.
* **JWT** commonly stores user roles.
* Frontend can **show/hide** components based on role.
* Backend must **enforce role-based access** for security.

---

## Advanced Insights

### Role Hierarchies

In complex apps, you might have:

```javascript
const Roles = {
  USER: 'user',
  MODERATOR: 'moderator',
  ADMIN: 'admin'
};

const roleHierarchy = {
  user: 1,
  moderator: 2,
  admin: 3
};

function canAccess(requiredRole, userRole) {
  return roleHierarchy[userRole] >= roleHierarchy[requiredRole];
}
```

### Refresh Token Flow

Implement a refresh mechanism to renew the access token without logging the user out:

```javascript
// Pseudo Code
async function refreshAccessToken() {
  const refreshToken = await AsyncStorage.getItem('refreshToken');
  const response = await api.post('/auth/refresh', { refreshToken });
  if (response.ok) {
    await AsyncStorage.setItem('accessToken', response.data.accessToken);
  }
}
```

---

# Conclusion

Role-Based Authentication is an indispensable tool in the React Native developer's toolkit. Whether you are working on a small app or a large enterprise project, knowing how to implement and manage RBAC ensures you build **secure, scalable, and maintainable** applications.

By properly integrating roles into both the backend and frontend, you can control access efficiently and deliver a smooth user experience without compromising security.

---
