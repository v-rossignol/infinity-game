# Infinity - StellarGate (Client Login) - Setup Projet

**Date** : 7 juin 2026  
**Modèle** : Vibe (Mistral Medium 3.5)  
**Auteur** : Roro LeSage  
**Type** : Documentation Technique - Setup Projet

---

## **1. Introduction**

**StellarGate** est le client dédié à l'**authentification** (login, création de compte, récupération de mot de passe) pour le jeu **Infinity**. Ce client est une **application web légère** qui communique avec le serveur pour gérer les comptes joueurs.

**Objectifs** :

- Permettre aux joueurs de **se connecter** à leur compte.
- Permettre la **création de compte** (inscription).
- Gérer la **récupération de mot de passe** (optionnel).
- Rediriger vers le **Client Galaxie** après une authentification réussie.

---

## **2. Stack Technologique**

### **A. Core Technologies**


| **Catégorie**        | **Technologie**                                                                                       | **Version Recommandée** | **Rôle**                                                                |
| -------------------- | ----------------------------------------------------------------------------------------------------- | ----------------------- | ----------------------------------------------------------------------- |
| **Framework UI**     | [React](https://react.dev/)                                                                           | v18.x                   | Construction de l'interface utilisateur (formulaires, boutons, etc.).   |
| **State Management** | [Zustand](https://github.com/pmndrs/zustand)                                                          | v4.x                    | Gestion d'état légère pour les formulaires et l'état de connexion.      |
| **Routing**          | [React Router](https://reactrouter.com/)                                                              | v6.x                    | Navigation entre les pages (login, inscription, etc.).                  |
| **Formulaires**      | [React Hook Form](https://react-hook-form.com/)                                                       | v7.x                    | Gestion des formulaires avec validation.                                |
| **Validation**       | [Zod](https://zod.dev/)                                                                               | v3.x                    | Validation des données des formulaires (alternative à Yup).             |
| **Communication**    | [Axios](https://axios-http.com/)                                                                      | v1.x                    | Requêtes HTTP vers le serveur pour l'authentification.                  |
| **Socket.IO**        | [Socket.IO Client](https://socket.io/docs/v4/client-api/)                                             | v4.x                    | Optionnel : pour une synchronisation en temps réel (ex: notifications). |
| **UI Components**    | [MUI](https://mui.com/) ou [Chakra UI](https://chakra-ui.com/)                                        | v5.x / v2.x             | Bibliothèques de composants prêts à l'emploi pour un design moderne.    |
| **Animations**       | [Framer Motion](https://www.framer.com/motion/)                                                       | v10.x                   | Animations fluides pour les transitions et effets visuels.              |
| **Bundler**          | [Vite](https://vitejs.dev/)                                                                           | v5.x                    | Build rapide et optimisé pour le développement web moderne.             |
| **Langage**          | TypeScript                                                                                            | v5.x                    | Typage statique pour une meilleure maintenabilité.                      |
| **Styles**           | [Tailwind CSS](https://tailwindcss.com/) ou [CSS Modules](https://github.com/css-modules/css-modules) | v3.x                    | Styling des composants.                                                 |


---

## **3. Structure du Projet**

```
stellargate/
├── public/                  # Assets statiques
│   ├── images/              # Logos, icônes, etc.
│   │   ├── logo.svg
│   │   └── background.jpg
│   └── index.html           # Point d'entrée HTML
│
├── src/
│   ├── assets/              # Assets dynamiques
│   │   └── styles/          # Fichiers CSS/SCSS
│   │       ├── global.css
│   │       └── theme.css
│   │
│   ├── components/          # Composants React réutilisables
│   │   ├── ui/              # Composants UI génériques
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   ├── Modal.tsx
│   │   │   └── ...
│   │   ├── auth/            # Composants spécifiques à l'authentification
│   │   │   ├── LoginForm.tsx
│   │   │   ├── RegisterForm.tsx
│   │   │   ├── ForgotPasswordForm.tsx
│   │   │   └── AuthLayout.tsx  # Layout pour les pages d'authentification
│   │   └── common/           # Composants partagés
│   │       ├── Header.tsx
│   │       ├── Footer.tsx
│   │       └── LoadingSpinner.tsx
│   │
│   ├── pages/               # Pages principales
│   │   ├── LoginPage.tsx
│   │   ├── RegisterPage.tsx
│   │   ├── ForgotPasswordPage.tsx
│   │   └── HomePage.tsx       # Page d'accueil (optionnelle)
│   │
│   ├── hooks/               # Hooks React personnalisés
│   │   ├── useAuth.ts         # Gestion de l'état d'authentification
│   │   ├── useForm.ts         # Gestion des formulaires
│   │   └── useSocket.ts       # Gestion de Socket.IO (optionnel)
│   │
│   ├── stores/              # Stores Zustand
│   │   ├── authStore.ts       # État global de l'authentification
│   │   └── uiStore.ts         # État de l'UI (ex: modales ouvertes)
│   │
│   ├── services/            # Services pour les appels API
│   │   ├── api.ts             # Configuration d'Axios
│   │   ├── authService.ts     # Appels API pour l'authentification
│   │   └── userService.ts     # Appels API pour la gestion des utilisateurs
│   │
│   ├── types/               # Types TypeScript
│   │   ├── auth.ts            # Types pour l'authentification (User, LoginPayload, etc.)
│   │   └── api.ts             # Types pour les réponses API
│   │
│   ├── utils/               # Utilitaires
│   │   ├── validation.ts      # Fonctions de validation
│   │   ├── storage.ts         # Gestion du stockage local (localStorage)
│   │   └── helpers.ts         # Fonctions utilitaires
│   │
│   ├── App.tsx              # Point d'entrée de l'application React
│   ├── main.tsx             # Initialisation de React
│   └── vite-env.d.ts         # Déclarations de types pour Vite
│
├── .env                     # Variables d'environnement
├── .gitignore
├── package.json
├── tsconfig.json            # Configuration TypeScript
├── vite.config.ts           # Configuration Vite
└── README.md
```

---

## **4. Dépendances du Projet**

### **A. `package.json**`

```json
{
  "name": "stellargate",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .ts,.tsx"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.21.0",
    "zustand": "^4.4.7",
    "@hookform/resolvers": "^3.3.4",
    "react-hook-form": "^7.49.2",
    "zod": "^3.22.4",
    "axios": "^1.6.2",
    "socket.io-client": "^4.7.2",
    "@mui/material": "^5.15.0",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "framer-motion": "^10.16.16"
  },
  "devDependencies": {
    "@types/react": "^18.2.43",
    "@types/react-dom": "^18.2.17",
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.3.3",
    "vite": "^5.0.8",
    "eslint": "^8.55.0",
    "@typescript-eslint/eslint-plugin": "^6.13.2",
    "@typescript-eslint/parser": "^6.13.2",
    "prettier": "^3.1.1",
    "tailwindcss": "^3.3.6",
    "postcss": "^8.4.32",
    "autoprefixer": "^10.4.16"
  }
}
```

---

## **5. Configuration de Base**

### **A. `vite.config.ts**`

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': '/src',
      '@components': '/src/components',
      '@pages': '/src/pages',
      '@hooks': '/src/hooks',
      '@stores': '/src/stores',
      '@services': '/src/services',
      '@types': '/src/types',
      '@utils': '/src/utils',
    },
  },
  server: {
    port: 3001, // Port différent du Client Planète (3000)
    proxy: {
      '/api': {
        target: 'http://localhost:4000', // Adresse du serveur Infinity
        changeOrigin: true,
      },
      '/socket.io': {
        target: 'http://localhost:4000',
        ws: true,
      },
    },
  },
});
```

---

### **B. `tsconfig.json**`

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@pages/*": ["src/pages/*"],
      "@hooks/*": ["src/hooks/*"],
      "@stores/*": ["src/stores/*"],
      "@services/*": ["src/services/*"],
      "@types/*": ["src/types/*"],
      "@utils/*": ["src/utils/*"]
    }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

---

## **6. Points d'Entrée et Initialisation**

### **A. `main.tsx**`

```typescript
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';
import './assets/styles/global.css';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

---

### **B. `App.tsx**`

```typescript
import { Routes, Route } from 'react-router-dom';
import { AuthLayout } from '@components/auth/AuthLayout';
import { LoginPage } from '@pages/LoginPage';
import { RegisterPage } from '@pages/RegisterPage';
import { ForgotPasswordPage } from '@pages/ForgotPasswordPage';

function App() {
  return (
    <Routes>
      <Route path="/" element={<AuthLayout />}>
        <Route index element={<LoginPage />} />
        <Route path="login" element={<LoginPage />} />
        <Route path="register" element={<RegisterPage />} />
        <Route path="forgot-password" element={<ForgotPasswordPage />} />
      </Route>
    </Routes>
  );
}

export default App;
```

---

## **7. Exemples de Composants**

### **A. `AuthLayout.tsx**`

```typescript
import { Outlet } from 'react-router-dom';
import { motion } from 'framer-motion';
import { Box, Container } from '@mui/material';

export const AuthLayout = () => {
  return (
    <Box
      sx={{
        minHeight: '100vh',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        backgroundImage: 'url(/images/background.jpg)',
        backgroundSize: 'cover',
        backgroundPosition: 'center',
      }}
    >
      <Container maxWidth="sm">
        <motion.div
          initial={{ opacity: 0, y: -20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.5 }}
        >
          <Box
            sx={{
              backgroundColor: 'rgba(0, 0, 0, 0.7)',
              padding: 4,
              borderRadius: 2,
              boxShadow: 3,
            }}
          >
            <Outlet />
          </Box>
        </motion.div>
      </Container>
    </Box>
  );
};
```

---

### **B. `LoginForm.tsx**`

```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useAuthStore } from '@stores/authStore';
import { useNavigate } from 'react-router-dom';
import { Button, TextField, Box, Typography, Link } from '@mui/material';

// Schéma de validation avec Zod
const loginSchema = z.object({
  username: z.string().min(3, 'Username must be at least 3 characters'),
  password: z.string().min(6, 'Password must be at least 6 characters'),
});

type LoginFormData = z.infer<typeof loginSchema>;

export const LoginForm = () => {
  const { login } = useAuthStore();
  const navigate = useNavigate();
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<LoginFormData>({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = async (data: LoginFormData) => {
    try {
      await login(data.username, data.password);
      navigate('/galaxy'); // Redirige vers le Client Galaxie après login
    } catch (error) {
      console.error('Login failed:', error);
    }
  };

  return (
    <Box component="form" onSubmit={handleSubmit(onSubmit)} sx={{ mt: 1 }}>
      <Typography variant="h4" component="h1" gutterBottom>
        StellarGate
      </Typography>
      <TextField
        label="Username"
        fullWidth
        margin="normal"
        {...register('username')}
        error={!!errors.username}
        helperText={errors.username?.message}
      />
      <TextField
        label="Password"
        type="password"
        fullWidth
        margin="normal"
        {...register('password')}
        error={!!errors.password}
        helperText={errors.password?.message}
      />
      <Button
        type="submit"
        fullWidth
        variant="contained"
        sx={{ mt: 3, mb: 2 }}
        disabled={isSubmitting}
      >
        {isSubmitting ? 'Logging in...' : 'Login'}
      </Button>
      <Box sx={{ textAlign: 'center' }}>
        <Link href="/register" color="inherit">
          Don&apos;t have an account? Register
        </Link>
      </Box>
    </Box>
  );
};
```

---

### **C. `RegisterForm.tsx**`

```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { useAuthStore } from '@stores/authStore';
import { useNavigate } from 'react-router-dom';
import { Button, TextField, Box, Typography, Link } from '@mui/material';

// Schéma de validation avec Zod
const registerSchema = z
  .object({
    username: z.string().min(3, 'Username must be at least 3 characters'),
    password: z.string().min(6, 'Password must be at least 6 characters'),
    confirmPassword: z.string(),
    email: z.string().email('Invalid email address'),
  })
  .refine((data) => data.password === data.confirmPassword, {
    message: "Passwords don't match",
    path: ['confirmPassword'],
  });

type RegisterFormData = z.infer<typeof registerSchema>;

export const RegisterForm = () => {
  const { register: registerUser } = useAuthStore();
  const navigate = useNavigate();
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<RegisterFormData>({
    resolver: zodResolver(registerSchema),
  });

  const onSubmit = async (data: RegisterFormData) => {
    try {
      await registerUser(data.username, data.password, data.email);
      navigate('/login');
    } catch (error) {
      console.error('Registration failed:', error);
    }
  };

  return (
    <Box component="form" onSubmit={handleSubmit(onSubmit)} sx={{ mt: 1 }}>
      <Typography variant="h4" component="h1" gutterBottom>
        Create Account
      </Typography>
      <TextField
        label="Username"
        fullWidth
        margin="normal"
        {...register('username')}
        error={!!errors.username}
        helperText={errors.username?.message}
      />
      <TextField
        label="Email"
        fullWidth
        margin="normal"
        {...register('email')}
        error={!!errors.email}
        helperText={errors.email?.message}
      />
      <TextField
        label="Password"
        type="password"
        fullWidth
        margin="normal"
        {...register('password')}
        error={!!errors.password}
        helperText={errors.password?.message}
      />
      <TextField
        label="Confirm Password"
        type="password"
        fullWidth
        margin="normal"
        {...register('confirmPassword')}
        error={!!errors.confirmPassword}
        helperText={errors.confirmPassword?.message}
      />
      <Button
        type="submit"
        fullWidth
        variant="contained"
        sx={{ mt: 3, mb: 2 }}
        disabled={isSubmitting}
      >
        {isSubmitting ? 'Registering...' : 'Register'}
      </Button>
      <Box sx={{ textAlign: 'center' }}>
        <Link href="/login" color="inherit">
          Already have an account? Login
        </Link>
      </Box>
    </Box>
  );
};
```

---

## **8. Exemples de Stores et Services**

### **A. `authStore.ts` (Zustand)**

```typescript
import { create } from 'zustand';
import { authService } from '@services/authService';

interface AuthState {
  user: { id: string; username: string; email?: string } | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  error: string | null;
  login: (username: string, password: string) => Promise<void>;
  register: (username: string, password: string, email?: string) => Promise<void>;
  logout: () => void;
  clearError: () => void;
}

export const useAuthStore = create<AuthState>((set) => ({
  user: null,
  isAuthenticated: false,
  isLoading: false,
  error: null,

  login: async (username, password) => {
    set({ isLoading: true, error: null });
    try {
      const user = await authService.login(username, password);
      set({ user, isAuthenticated: true, isLoading: false });
    } catch (error) {
      set({ error: error.message || 'Login failed', isLoading: false });
      throw error;
    }
  },

  register: async (username, password, email) => {
    set({ isLoading: true, error: null });
    try {
      const user = await authService.register(username, password, email);
      set({ user, isAuthenticated: true, isLoading: false });
    } catch (error) {
      set({ error: error.message || 'Registration failed', isLoading: false });
      throw error;
    }
  },

  logout: () => {
    authService.logout();
    set({ user: null, isAuthenticated: false });
  },

  clearError: () => {
    set({ error: null });
  },
}));
```

---

### **B. `authService.ts**`

```typescript
import axios from 'axios';
import { storage } from '@utils/storage';

const API_BASE_URL = '/api/auth';

interface User {
  id: string;
  username: string;
  email?: string;
}

interface AuthResponse {
  user: User;
  accessToken: string;
}

export const authService = {
  async login(username: string, password: string): Promise<User> {
    const response = await axios.post<AuthResponse>(`${API_BASE_URL}/login`, {
      username,
      password,
    });
    const { user, accessToken } = response.data;
    storage.setToken(accessToken);
    return user;
  },

  async register(username: string, password: string, email?: string): Promise<User> {
    const response = await axios.post<AuthResponse>(`${API_BASE_URL}/register`, {
      username,
      password,
      email,
    });
    const { user, accessToken } = response.data;
    storage.setToken(accessToken);
    return user;
  },

  logout(): void {
    storage.removeToken();
  },

  async forgotPassword(email: string): Promise<void> {
    await axios.post(`${API_BASE_URL}/forgot-password`, { email });
  },

  async getCurrentUser(): Promise<User | null> {
    try {
      const token = storage.getToken();
      if (!token) return null;

      const response = await axios.get<User>(`${API_BASE_URL}/me`, {
        headers: { Authorization: `Bearer ${token}` },
      });
      return response.data;
    } catch (error) {
      storage.removeToken();
      return null;
    }
  },
};
```

---

### **C. `storage.ts` (Utilitaire)**

```typescript
const TOKEN_KEY = 'infinity_token';

export const storage = {
  getToken(): string | null {
    return localStorage.getItem(TOKEN_KEY);
  },

  setToken(token: string): void {
    localStorage.setItem(TOKEN_KEY, token);
  },

  removeToken(): void {
    localStorage.removeItem(TOKEN_KEY);
  },
};
```

---

## **9. Types TypeScript**

### **A. `types/auth.ts**`

```typescript
export interface User {
  id: string;
  username: string;
  email?: string;
  createdAt?: string;
}

export interface LoginCredentials {
  username: string;
  password: string;
}

export interface RegisterCredentials extends LoginCredentials {
  email?: string;
}

export interface AuthResponse {
  user: User;
  accessToken: string;
}

export interface AuthError {
  message: string;
  code?: string;
}
```

---

## **10. Étapes pour Démarrer le Projet**

1. **Initialiser le projet** :
  ```bash
   npm create vite@latest stellargate -- --template react-ts
   cd stellargate
  ```
2. **Installer les dépendances** :
  ```bash
   npm install react-router-dom zustand @hookform/resolvers react-hook-form zod axios socket.io-client @mui/material @emotion/react @emotion/styled framer-motion
   npm install --save-dev @types/react @types/react-dom @vitejs/plugin-react typescript vite eslint prettier tailwindcss postcss autoprefixer
  ```
3. **Configurer Vite et TypeScript** :
  - Copier les fichiers `vite.config.ts` et `tsconfig.json` depuis ce document.
4. **Créer la structure de dossiers** :
  - Suivre la structure décrite dans la section **3. Structure du Projet**.
5. **Configurer Tailwind CSS (optionnel)** :
  - Si tu utilises Tailwind, crée un fichier `tailwind.config.js` et `postcss.config.js`.
6. **Ajouter les assets** :
  - Placer les images (logo, fond d’écran) dans `public/images/`.
7. **Démarrer le développement** :
  ```bash
   npm run dev
  ```

---

- **Finaliser les formulaires** : Ajouter la validation côté client et serveur.
- **Intégrer avec le serveur** : Connecter les appels API à ton backend NestJS.
- **Ajouter des animations** : Utiliser Framer Motion pour des transitions fluides.
- **Gérer les erreurs** : Afficher des messages d’erreur clairs pour l’utilisateur.
- **Sécuriser le stockage** : Utiliser `httpOnly` cookies pour le token (si possible).
- **Ajouter un thème sombre/clair** : Utiliser MUI ou Tailwind pour un design adaptable.

---

- [React Documentation](https://react.dev/)
- [React Router Documentation](https://reactrouter.com/)
- [Zustand Documentation](https://github.com/pmndrs/zustand)
- [React Hook Form Documentation](https://react-hook-form.com/)
- [Zod Documentation](https://zod.dev/)
- [MUI Documentation](https://mui.com/)
- [Framer Motion Documentation](https://www.framer.com/motion/)
- [Tailwind CSS Documentation](https://tailwindcss.com/)

---

- Ce document est une **base de départ** : adapte-le selon tes besoins spécifiques.
- Pour une **meilleure organisation**, utilise des **branches Git** pour chaque fonctionnalité majeure.
- Pense à **documenter** chaque composant et service pour faciliter la maintenance.
- Si tu utilises **Socket.IO** pour des notifications en temps réel (ex: