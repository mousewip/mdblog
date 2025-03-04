---
title: "Implement Binary Role-Based Access Control (RBAC) In .NET"
date: "2025-03-04"
excerpt: "In .NET, you can implement **Binary Role-Based Access Control (RBAC)** using bitwise operations to efficiently manage user permissions. This approach allows you to store multiple roles or permissions as flags within an integer or enum, making access checks fast and memory-efficient."
category: "Chat GPT"
tags: ["Bitwise Flags", "Coding", "Authorization", "RBAC"]
coverImage: "https://raw.githubusercontent.com/Dicklesworthstone/yto_blog_posts/refs/heads/main/blog_01_banner.webp"
author: "Phong Ho"
authorImage: "https://pbs.twimg.com/profile_images/1225476100547063809/53jSWs7z_400x400.jpg"
authorBio: "Software Engineer"
---

In .NET, you can implement **Binary Role-Based Access Control (RBAC)** using bitwise operations to efficiently manage user permissions. This approach allows you to store multiple roles or permissions as flags within an integer or enum, making access checks fast and memory-efficient.

---

### **1. Define Permissions as Flags**
Use a `[Flags]` Enum to represent different roles or permissions.

```csharp
[Flags]
public enum UserRoles
{
    None        = 0,
    Read        = 1 << 0, // 1
    Write       = 1 << 1, // 2
    Delete      = 1 << 2, // 4
    Admin       = Read | Write | Delete  // 7
}
```

---

### **2. Assign Roles to Users**
Each user is assigned a role using a bitwise OR operation (`|`).

```csharp
public class User
{
    public string Name { get; set; }
    public UserRoles Roles { get; set; }
}
```

Example user creation:

```csharp
var user1 = new User { Name = "Alice", Roles = UserRoles.Read | UserRoles.Write }; // 3 (Read + Write)
var user2 = new User { Name = "Bob", Roles = UserRoles.Admin }; // 7 (Admin)
```

---

### **3. Check Permissions Using Bitwise AND**
To check if a user has a specific permission, use the `HasFlag` method or bitwise AND (`&`).

```csharp
public static bool HasPermission(User user, UserRoles permission)
{
    return (user.Roles & permission) == permission;
}

// Example Usage:
bool canRead = HasPermission(user1, UserRoles.Read);  // true
bool canDelete = HasPermission(user1, UserRoles.Delete);  // false
bool isAdmin = HasPermission(user2, UserRoles.Admin);  // true
```

---

### **4. Grant or Remove Permissions Dynamically**
You can modify roles dynamically using bitwise operations.

#### **Grant a Role**
```csharp
user1.Roles |= UserRoles.Delete;  // Now user1 has Read, Write, and Delete (7)
```

#### **Revoke a Role**
```csharp
user1.Roles &= ~UserRoles.Write;  // Removes Write permission (now 5: Read + Delete)
```

---

### **5. Storing in a Database**
Since the roles are stored as integers, they can be saved in a database.

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Roles { get; set; } // Store enum as an integer
}
```

To retrieve and convert back:

```csharp
UserRoles userRoles = (UserRoles)userFromDb.Roles;
```

---

### **Advantages of Binary RBAC**
✅ **Fast Permission Checks** (Bitwise operations are very efficient)  
✅ **Compact Storage** (Uses a single integer instead of multiple records)  
✅ **Flexible** (Easily add or remove permissions dynamically)

Would you like to see a full implementation in ASP.NET Core? 🚀