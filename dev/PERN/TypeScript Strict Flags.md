## TypeScript Strict Flags (For Notes)

### High Priority (Catch Real Bugs)

**`noImplicitReturns: true`**

- Catches: Functions that don't return on all code paths
- Example: `if (x) return "yes"` without else/return
- Keep: ✅ YES

**`noUnusedLocals: true`**

- Catches: Variables you declare but never use
- Example: `const user = getUser()` but never use `user`
- Keep: ✅ YES

**`noUnusedParameters: true`**

- Catches: Function parameters you don't use
- Example: `function greet(name, age) { return name; }` (age unused)
- Keep: ✅ YES

**`noFallthroughCasesInSwitch: true`**

- Catches: Switch cases that fall through without break/return
- Example: `case "a": console.log("a"); case "b":` (missing break)
- Keep: ✅ YES

**`noImplicitOverride: true`**

- Catches: Overriding parent class methods without `override` keyword
- Example: Parent removes method, your override still there (undetected)
- Keep: ✅ YES

---

### Low Priority (Style Preference)

**`noPropertyAccessFromIndexSignature: true`**

- Enforces: Use bracket notation for dynamic properties
- Example: `process.env.PORT` ❌ vs `process.env["PORT"]` ✅
- Catch bugs: ❌ NO (just syntax style)
- Keep: ⚠️ OPTIONAL (disable if annoying)

---

### Other Useful Flags

**`strict: true`**

- Enables all type checking. Shorthand for many strict flags.

**`noUnusedLocals` + `noUnusedParameters`**

- Prefix unused variables with `_` to tell TypeScript "intentionally unused"
- Example: `function greet(name, _age) { }`

---

### Quick Reference

|Flag|Catches Bugs|Keep?|
|---|---|---|
|noImplicitReturns|✅|✅|
|noUnusedLocals|✅|✅|
|noUnusedParameters|✅|✅|
|noFallthroughCasesInSwitch|✅|✅|
|noImplicitOverride|✅|✅|
|noPropertyAccessFromIndexSignature|❌|⚠️|