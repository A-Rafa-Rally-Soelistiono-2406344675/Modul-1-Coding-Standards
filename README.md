# Eshop Reflection

## Overview
This repository implements a simple product flow using Spring Boot (create, list, edit, delete). This reflection reviews the current code against clean code principles and secure coding practices from the module, and notes improvements.

## Clean Code Principles Applied
- Separation of concerns: controller, service, and repository are split across packages (`controller`, `service`, `repository`).
- Single responsibility: each class has one main responsibility (routing, business logic, data access).
- Meaningful naming: methods like `create`, `findAll`, `findById`, `update`, and `deleteById` are descriptive.
- Small focused methods: controller handlers map directly to one action and return a view or redirect.
- Consistent flow: create/edit forms are implemented in a consistent, predictable way.

## Secure Coding Practices Applied
- No direct string concatenation for SQL (in-memory repository only), avoiding SQL injection concerns for now.
- Minimal data exposure in views: only fields required by the UI are rendered.
- Server-side redirect after state change (Post/Redirect/Get pattern) reduces duplicate submission risk.

## Mistakes / Gaps Found and Improvements
- Deleting via GET is unsafe. It should be a POST/DELETE with CSRF protection to prevent unintended deletions. Improve by using a form with method POST and enabling Spring Security CSRF tokens.
- Input validation is missing. `productName` and `productQuantity` should be validated (e.g., `@NotBlank`, `@Min(0)`) and binding errors should be handled in the controller.
- Error handling is minimal. When a product is not found, the controller redirects without feedback. Improve by adding a user-visible message or an error page.
- Business rules are weak. `update` returns null when not found, and callers ignore the result. Improve by throwing a domain exception or returning a structured result and handling it in the controller.
- Repository uses a mutable in-memory list without thread-safety. In a real app, use a database or synchronize access, and avoid static/global mutable state.
- Product ID is generated in service, but there is no uniqueness check for manually provided IDs. Improve by always generating IDs server-side or validating uniqueness.
- Form fields use `type="text"` for quantity, which allows invalid values. Improve by using `type="number"` and server-side validation.

## Summary
The code is readable and modular, but security and robustness can be improved, especially around delete semantics, validation, and error handling.
