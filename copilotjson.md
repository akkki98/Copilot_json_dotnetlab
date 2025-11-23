# Lab: Using `copilot.json` in a .NET Project

This lab guides you through creating and using **`copilot.json`** to configure GitHub Copilot behavior in a .NET project. You will learn how to enforce architecture rules, add custom validations, and control generation quality.

---

## 🎯 **Lab Objectives**

By the end of this lab, you will:

* Understand what `copilot.json` does.
* Create a `copilot.json` configuration file.
* Apply architecture rules for .NET (Controller → Service → Repository).
* Test Copilot behavior using prompts.
* Validate how Copilot follows the configuration.

---

## 🧩 **1. What is `copilot.json`?**

`copilot.json` is a **project-level configuration** file that defines:

* Architecture rules
* Coding conventions
* Allowed dependencies
* Naming conventions
* Test generation rules
* Custom instructions for GitHub Copilot

It ensures consistent code generation across large .NET projects.

---

## 📁 **2. Create the `copilot.json` File**

### 📍 **Location**

Place the file in the **root folder** of your .NET project:

```
MyDotnetApp/
│  Program.cs
│  Startup.cs
│  copilot.json
│  appsettings.json
└── Controllers/
└── Services/
└── Repositories/
```

### ➕ **Steps**

1. Right‑click the project in *Solution Explorer*.
2. Select **Add → New Item → Text File**.
3. Name it: `copilot.json`.
4. Press **Ctrl+S**.

---

## ⚙️ **3. Add a Basic `copilot.json` Configuration**

Paste this content:

```json
{
  "version": 1,
  "architecture": {
    "layers": ["controllers", "services", "repositories"],
    "rules": {
      "controllers": ["must call only services"],
      "services": ["must call repositories"],
      "repositories": ["must not call services or controllers"]
    }
  },
  "naming": {
    "controllers": "*Controller",
    "services": "*Service",
    "repositories": "*Repository"
  },
  "coding_style": {
    "async_methods_require_suffix": true,
    "require_xml_comments_on_public_methods": true
  }
}
```

---

## 🧪 **4. Lab Task 1: Enforce Layered Architecture**

### ✔ Task

Create the following files:

* `CustomerController.cs`
* `CustomerService.cs`
* `CustomerRepository.cs`

Then ask Copilot:

> "Generate the implementation for CustomerController following the project architecture rules."

### 📝 Expected Behavior

Copilot should:

* Only call the service layer from the controller.
* Not call repository directly.
* Use correct naming conventions.

---

## 🧪 **5. Lab Task 2: Naming Enforcement**

Create a new file named:

```
OrderLogic.cs
```

Ask Copilot:

> "Generate logic for OrderLogic class."

### Expected Output

Copilot should:

* Suggest renaming `OrderLogic` → `OrderService`.
* Explain naming convention from `copilot.json`.

---

## 🧪 **6. Lab Task 3: Repository Rule Violation Test**

Ask Copilot:

> "Write code where Repository calls the Service layer."

### Expected Result

Copilot should *refuse* and warn:

* "Repositories must not call the service or controller layers."

---

## 🧪 **7. Lab Task 4: Async Naming Enforcement**

Ask Copilot:

> "Generate an async method for fetching customer data."

### Expected Result

Generated code should end with:

```
FetchCustomerAsync
```

---

## 🧪 **8. Lab Task 5: Test XML Comments Rule**

Ask Copilot:

> "Generate a GetOrder method in OrderService."

### Expected

Copilot adds:

```csharp
/// <summary>
/// Gets order by ID.
/// </summary>
```

---

## 🧪 **9. Bonus: Add Your Own Constraints**

Try adding rules:

* Preventing usage of certain namespaces
* Enforcing dependency injection
* Blocking static classes

Example:

```json
"forbidden_dependencies": ["System.IO", "Newtonsoft.Json"]
```

---

## 🎉 **Lab Completed**

You have successfully configured `copilot.json` and validated all rules within a .NET project.

If you'd like, I can also create:

* A **Java version** of this lab
* A **hands‑on Spring Boot architecture rules lab**
* A **microservices‑specific copilot.json**
