# Frontend (HTML/JS/CSS)

Template für Frontend-Entwicklung (HTML, JavaScript, CSS).

---

## JavaScript

### Coding Standards

#### Modern JavaScript
- SHOULD: ES6+ Features nutzen (const, let, arrow functions)
- SHOULD: Async/Await statt Callback-Hell
- MUST: `const` für unveränderliche Werte, `let` für veränderliche

```javascript
// ✅ Beispiel
const API_URL = "https://api.example.com";
let userData = null;

async function fetchUser(id) {
    const response = await fetch(`${API_URL}/users/${id}`);
    return await response.json();
}
```

#### Error Handling
- MUST: Try-Catch für async Operations
- SHOULD: User-friendly Error Messages

```javascript
async function loadData() {
    try {
        const data = await fetchData();
        displayData(data);
    } catch (error) {
        console.error("Failed to load data:", error);
        showErrorMessage("Could not load data. Please try again.");
    }
}
```

---

### Security

#### XSS (Cross-Site Scripting) Prevention

**MUST:**
- `.textContent` statt `.innerHTML` für User-Content
- Framework-eigenes Escaping nutzen (React, Vue, Angular)
- DOMPurify für HTML-Sanitization (wenn unvermeidbar)

**MUST NOT:**
- `innerHTML = user_input` ohne Sanitization
- `eval(user_input)`
- `document.write(user_input)`
- `<script>` Tags mit User-Content

**Beispiel:**
```javascript
// ❌ FALSCH
const userComment = getUserInput();
element.innerHTML = userComment;  // XSS-Risiko!

// ✅ RICHTIG
const userComment = getUserInput();
element.textContent = userComment;  // Automatisch escaped

// Wenn HTML nötig (z.B. Markdown-Rendering):
import DOMPurify from 'dompurify';
element.innerHTML = DOMPurify.sanitize(userComment);
```

---

#### Secrets & API-Keys

**MUST NOT:**
- API-Keys im JavaScript-Code
  ```javascript
  // ❌ FALSCH (im Client sichtbar!)
  const apiKey = "sk-abc123def456";
  fetch(`https://api.example.com?key=${apiKey}`);
  ```

- Passwords oder Tokens in localStorage
  ```javascript
  // ❌ FALSCH
  localStorage.setItem("password", userPassword);
  ```

- Sensitive Data in URL-Parameters
  ```javascript
  // ❌ FALSCH
  window.location.href = `/profile?token=${authToken}`;
  ```

**MUST:**
- Backend-Proxy für API-Calls mit Secrets
  ```javascript
  // ✅ RICHTIG (API-Key im Backend)
  fetch("/api/proxy/external-service", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ query: userQuery })
  });
  ```

- HttpOnly Cookies für Session-Tokens (Server-side gesetzt)
  ```javascript
  // Backend setzt Cookie:
  // Set-Cookie: session_token=abc123; HttpOnly; Secure; SameSite=Strict
  
  // Frontend braucht nichts zu tun, Cookie wird automatisch gesendet
  fetch("/api/user/profile");  // Cookie automatisch included
  ```

- HTTPS für alle Requests mit sensitive Data

---

#### Data Storage

**MUST:**
- Session-Data in `sessionStorage` (gelöscht bei Tab-Close)
  ```javascript
  sessionStorage.setItem("tempData", JSON.stringify(data));
  ```

- Sensitive Data NUR in httpOnly Cookies (Server-side)

- Encryption für sensitive Data in localStorage (wenn unvermeidbar)
  ```javascript
  // Nur wenn wirklich nötig, besser vermeiden
  import CryptoJS from 'crypto-js';
  const encrypted = CryptoJS.AES.encrypt(sensitiveData, encryptionKey).toString();
  localStorage.setItem("data", encrypted);
  ```

**MUST NOT:**
- Passwords in localStorage
  ```javascript
  // ❌ NIEMALS
  localStorage.setItem("password", password);
  ```

- Session-Tokens in localStorage
  ```javascript
  // ❌ FALSCH (XSS kann auslesen)
  localStorage.setItem("auth_token", token);
  
  // ✅ RICHTIG (httpOnly Cookie, Server-side gesetzt)
  // Nicht im Frontend-Code!
  ```

- PII (Personally Identifiable Information) unverschlüsselt
  ```javascript
  // ❌ FALSCH
  localStorage.setItem("ssn", userSSN);
  ```

---

#### eval() und Code Execution

**MUST NOT:**
- `eval()` mit User-Input
  ```javascript
  // ❌ FALSCH
  const userCode = getUserInput();
  eval(userCode);  // Code Injection!
  ```

- `Function()` Constructor mit User-Input
  ```javascript
  // ❌ FALSCH
  const func = new Function(userInput);
  ```

- `setTimeout/setInterval` mit String
  ```javascript
  // ❌ FALSCH
  setTimeout(userInput, 1000);  // String wird evaluated
  
  // ✅ RICHTIG
  setTimeout(() => safeFunction(), 1000);
  ```

**MUST:**
- JSON.parse() für Data-Parsing
  ```javascript
  // ✅ RICHTIG
  const data = JSON.parse(userInput);
  ```

---

#### CORS & CSP

**SHOULD:**
- Content Security Policy (CSP) Header
  ```
  Content-Security-Policy: 
    default-src 'self'; 
    script-src 'self' https://trusted-cdn.com; 
    style-src 'self' 'unsafe-inline';
  ```

**MUST NOT:**
- `Access-Control-Allow-Origin: *` in Production
  ```javascript
  // ❌ FALSCH (zu permissive)
  res.setHeader('Access-Control-Allow-Origin', '*');
  
  // ✅ RICHTIG (specific origin)
  res.setHeader('Access-Control-Allow-Origin', 'https://yourdomain.com');
  ```

---

### Tooling

#### Linting
- SHOULD: ESLint verwenden
- SHOULD: Prettier für Formatierung

```bash
# ESLint
npm run lint

# Prettier
npm run format
```

#### Testing

Siehe auch: [standard/testing.md](../standard/testing.md)

**Jest (Unit-Tests):**
- SHOULD: Jest für Unit-Tests verwenden
- SHOULD: React Testing Library für React Components

**Cypress/Playwright (E2E-Tests):**
- SHOULD: Cypress oder Playwright für E2E-Tests
- E2E-Tests: Wenige, fokussiert auf kritische User-Flows

**Test-Struktur:**
```
project/
├── src/
│   ├── calculator.js
│   ├── calculator.test.js
│   ├── components/
│   │   ├── Button.jsx
│   │   └── Button.test.jsx
└── e2e/
    └── app.spec.js
```

**Naming:**
- Test-Files: `*.test.js`, `*.spec.js`

**Jest Beispiel:**
```javascript
// src/calculator.test.js
import { add, divide } from './calculator';

describe('Calculator', () => {
  describe('add', () => {
    it('adds two positive numbers', () => {
      expect(add(2, 3)).toBe(5);
    });

    it('adds negative numbers', () => {
      expect(add(-2, -3)).toBe(-5);
    });

    it('adds zero (edge case)', () => {
      expect(add(0, 5)).toBe(5);
    });
  });

  describe('divide', () => {
    it('divides normally', () => {
      expect(divide(10, 2)).toBe(5);
    });

    it('throws on division by zero (error path)', () => {
      expect(() => divide(10, 0)).toThrow(Error);
    });
  });
});
```

**React Testing Library Beispiel:**
```javascript
// src/components/Button.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Button from './Button';

describe('Button', () => {
  it('renders with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click</Button>);
    
    fireEvent.click(screen.getByText('Click'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Click</Button>);
    expect(screen.getByText('Click')).toBeDisabled();
  });
});
```

**Cypress E2E Beispiel:**
```javascript
// e2e/login.spec.js
describe('Login Flow', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('logs in successfully with valid credentials', () => {
    cy.get('input[name="email"]').type('user@example.com');
    cy.get('input[name="password"]').type('password123');
    cy.get('button[type="submit"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.contains('Welcome').should('be.visible');
  });

  it('shows error with invalid credentials', () => {
    cy.get('input[name="email"]').type('invalid@example.com');
    cy.get('input[name="password"]').type('wrong');
    cy.get('button[type="submit"]').click();
    
    cy.contains('Invalid credentials').should('be.visible');
  });

  it('validates empty fields', () => {
    cy.get('button[type="submit"]').click();
    cy.contains('Email is required').should('be.visible');
  });
});
```

**Playwright E2E Beispiel:**
```javascript
// e2e/login.spec.js
import { test, expect } from '@playwright/test';

test.describe('Login Flow', () => {
  test('logs in successfully', async ({ page }) => {
    await page.goto('/login');
    
    await page.fill('input[name="email"]', 'user@example.com');
    await page.fill('input[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    
    await expect(page).toHaveURL(/.*dashboard/);
    await expect(page.locator('text=Welcome')).toBeVisible();
  });
});
```

**Setup/Teardown (Jest):**
```javascript
describe('Database Tests', () => {
  let db;

  beforeAll(async () => {
    // Run once before all tests
    db = await connectDatabase();
  });

  afterAll(async () => {
    // Run once after all tests
    await db.close();
  });

  beforeEach(async () => {
    // Run before each test
    await db.clearAll();
  });

  it('saves user', async () => {
    const user = await db.save({ name: 'Alice' });
    expect(user.name).toBe('Alice');
  });
});
```

**Mocking (Jest):**
```javascript
// Mocking external API
jest.mock('./api');
import { fetchUser } from './api';

describe('UserProfile', () => {
  it('displays user name', async () => {
    fetchUser.mockResolvedValue({ name: 'Alice' });
    
    render(<UserProfile id={1} />);
    
    expect(await screen.findByText('Alice')).toBeInTheDocument();
  });
});
```

**Run Tests:**
```bash
# Jest (Unit Tests)
npm test                    # All tests
npm test -- --watch         # Watch mode
npm test -- --coverage      # With coverage

# Cypress (E2E)
npm run e2e                 # Headless
npx cypress open           # Interactive

# Playwright (E2E)
npm run test:e2e            # Headless
npx playwright test --ui    # Interactive
```

**Coverage:**
```bash
# Generate coverage report
npm test -- --coverage

# Open report
open coverage/lcov-report/index.html
```

---

### Frameworks (React, Vue, Angular)

#### React

**Security:**
- React escaped automatisch in JSX
- ABER: `dangerouslySetInnerHTML` ist gefährlich

```jsx
// ✅ RICHTIG (automatisch escaped)
<div>{userInput}</div>

// ❌ GEFÄHRLICH (nur mit Sanitization!)
<div dangerouslySetInnerHTML={{__html: userInput}} />

// ✅ MIT SANITIZATION
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(userInput)}} />
```

#### Vue

```vue
<!-- ✅ RICHTIG (automatisch escaped) -->
<div>{{ userInput }}</div>

<!-- ❌ GEFÄHRLICH -->
<div v-html="userInput"></div>

<!-- ✅ MIT SANITIZATION -->
<div v-html="sanitize(userInput)"></div>
```

#### Angular

```typescript
// ✅ RICHTIG (automatisch escaped)
<div>{{ userInput }}</div>

// ✅ FÜR HTML (mit DomSanitizer)
import { DomSanitizer } from '@angular/platform-browser';
trustedHtml = this.sanitizer.sanitize(SecurityContext.HTML, userInput);
```

---

## HTML

### Security

#### Form Security

**MUST:**
- CSRF-Tokens für State-Changing Requests
  ```html
  <form method="POST" action="/api/update">
      <input type="hidden" name="csrf_token" value="{{ csrf_token }}">
      <!-- ... -->
  </form>
  ```

**SHOULD:**
- `autocomplete="off"` für sensitive Fields
  ```html
  <input type="password" name="password" autocomplete="off">
  ```

#### Link Security

**SHOULD:**
- `rel="noopener noreferrer"` für externe Links
  ```html
  <a href="https://external.com" target="_blank" rel="noopener noreferrer">
      External Link
  </a>
  ```

---

## CSS

### Best Practices

**SHOULD:**
- BEM oder ähnliche Naming-Convention
- CSS Modules oder Scoped Styles (Vue, Angular)
- Responsive Design (Mobile-First)

**MUST NOT:**
- Inline Styles mit User-Input
  ```javascript
  // ❌ FALSCH
  element.style.cssText = userInput;  // CSS Injection möglich
  ```

---

## Definition of Done (Beispiel)

```bash
# 1. Linting
npm run lint

# 2. Formatierung
npm run format

# 3. Tests
npm test

# 4. Build
npm run build

# Bei Fehler: Iterieren bis alle grün!
```

---

## Security Checklist (Frontend)

### Vor jeder Code-Generierung

1. **User-Input wird in DOM geschrieben?**
   - `.textContent` nutzen, nicht `.innerHTML`

2. **API-Keys benötigt?**
   - Backend-Proxy implementieren

3. **Sensitive Data speichern?**
   - httpOnly Cookies (Server-side), nicht localStorage

4. **eval() oder innerHTML?**
   - Vermeiden oder mit DOMPurify sanitizen

5. **Externe Links?**
   - `rel="noopener noreferrer"` nutzen

### Bei Unsicherheit

**MUST:** User fragen, nicht raten

---

## Zusammenfassung

### Top Security-Regeln (Frontend)

1. **MUST:** `.textContent` statt `.innerHTML` für User-Content
2. **MUST NOT:** API-Keys im JavaScript-Code
3. **MUST NOT:** Passwords in localStorage
4. **MUST NOT:** `eval()` mit User-Input
5. **MUST:** Backend-Proxy für API-Calls mit Secrets
6. **MUST:** HttpOnly Cookies für Session-Tokens
7. **SHOULD:** ESLint + Prettier
8. **SHOULD:** CSP Header nutzen

### Framework-Specifics

- **React:** Automatisches Escaping (außer `dangerouslySetInnerHTML`)
- **Vue:** Automatisches Escaping (außer `v-html`)
- **Angular:** Automatisches Escaping (mit DomSanitizer für HTML)

### Bei Unsicherheit

**Immer User fragen bei Security-Fragen.**
