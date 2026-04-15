# Contexto del Proyecto: BudgetWise - App de Presupuesto Anual

## 1. Stack Tecnológico

| Tecnología | Propósito |
|------------|-----------|
| **React Native** | Framework principal para el desarrollo de la aplicación móvil multiplataforma |
| **Expo** | Herramienta de desarrollo y deployment para simplificar el proceso de build y distribución |
| **TypeScript** | Lenguaje de programación tipado para mayor robustez y mantenibilidad del código |
| **AsyncStorage** | Almacenamiento local para simular persistencia de datos |
| **Zustand / Context API** | Gestión del estado global de la aplicación |

## 2. Estructura Monorepo

El proyecto utiliza una estructura monorepo para organizar el código de manera modular:

```
app-presupuesto-v1/
├── apps/
│   └── mobile/                 # Aplicación React Native (Expo)
│       ├── src/
│       │   ├── components/     # Componentes reutilizables de UI
│       │   ├── screens/        # Pantallas de la aplicación
│       │   ├── navigation/     # Configuración de navegación
│       │   ├── adapters/       # Implementaciones del Patrón Adaptador
│       │   ├── services/       # Lógica de negocio
│       │   ├── hooks/          # Custom hooks
│       │   ├── types/          # Definiciones de tipos TypeScript
│       │   └── utils/          # Utilidades helper
│       ├── app.json
│       └── package.json
├── packages/
│   └── shared/                 # Código compartido entre aplicaciones
│       ├── types/              # Tipos y entidades compartidas
│       ├── constants/          # Constantes globales
│       └── utils/              # Utilidades compartidas
├── package.json                # Root package.json para scripts
└── README.md
```

### Beneficios de la Estructura Monorepo

- **Reutilización de código**: Los tipos y utilidades compartidas se definen una sola vez
- **Consistencia**: Facilita mantener patrones de código consistentes en toda la aplicación
- **Mantenibilidad**: Organización clara separa las responsabilidades del proyecto
- **Escalabilidad**: Permite agregar nuevas aplicaciones (ej: web, backend) sin cambios estructurales

## 3. Desarrollo Basado en Experiencia de Usuario (UX)

### Principios de Diseño

| Principio | Descripción |
|-----------|-------------|
| **Simplicidad** | Interfaces minimalistas pensando en usuarios beginners |
| **Feedback inmediato** | El usuario recibe confirmación visual de todas las acciones |
| **Curva de aprendizaje gradual** | Las funcionalidades complejas se introducen progresivamente |
| **Guided UX** | El usuario es guiado paso a paso mediante flujos claros |

### Flujo de Usuario Principal

```
Onboarding → Crear Presupuesto → Registrar Gasto → Ver Análisis → Recibir Recomendaciones
```

### Consideraciones UX

- **Tiempo de tarea**: Registrar un gasto debe tomar menos de 30 segundos
- **Visuales claros**: Indicadores de color (verde/rojo) para mostrar estado del presupuesto
- **Mensajes amigables**: Lenguaje accesible sin jerga financiera técnica
- **Acciones rápidas**: Botones de acción principal siempre visibles

## 4. Patrón Adaptador para Integración de Backend

### Propósito

El patrón Adaptador permite abstraer la fuente de datos, facilitando:
- El desarrollo inicial con datos locales (LocalStorage)
- La futura integración con un backend real
- El cambio entre fuentes de datos sin modificar la lógica de negocio

### Estructura del Adaptador

```typescript
// packages/shared/types/storage.types.ts
export interface IBudget {
  id: string;
  month: number;
  year: number;
  categories: ICategoryBudget[];
  createdAt: Date;
  updatedAt: Date;
}

export interface IExpense {
  id: string;
  amount: number;
  categoryId: string;
  date: Date;
  description?: string;
  createdAt: Date;
}

// apps/mobile/src/adapters/index.ts
export interface IDataAdapter {
  // Budget operations
  createBudget(budget: Omit<IBudget, 'id'>): Promise<IBudget>;
  getBudget(month: number, year: number): Promise<IBudget | null>;
  updateBudget(id: string, data: Partial<IBudget>): Promise<IBudget>;
  deleteBudget(id: string): Promise<void>;

  // Expense operations
  createExpense(expense: Omit<IExpense, 'id'>): Promise<IExpense>;
  getExpenses(filters?: ExpenseFilters): Promise<IExpense[]>;
  updateExpense(id: string, data: Partial<IExpense>): Promise<IExpense>;
  deleteExpense(id: string): Promise<void>;

  // Category operations
  getCategories(): Promise<ICategory[]>;
  createCategory(category: Omit<ICategory, 'id'>): Promise<ICategory>;
}
```

### Implementación Actual (LocalStorage)

```typescript
// apps/mobile/src/adapters/LocalStorageAdapter.ts
import AsyncStorage from '@react-native-async-storage/async-storage';
import { IDataAdapter } from './interfaces';

export class LocalStorageAdapter implements IDataAdapter {
  private readonly BUDGETS_KEY = '@budgets';
  private readonly EXPENSES_KEY = '@expenses';
  private readonly CATEGORIES_KEY = '@categories';

  async createBudget(budget: Omit<IBudget, 'id'>): Promise<IBudget> {
    const budgets = await this.getAllBudgets();
    const newBudget = { ...budget, id: this.generateId() };
    budgets.push(newBudget);
    await AsyncStorage.setItem(this.BUDGETS_KEY, JSON.stringify(budgets));
    return newBudget;
  }

  // ... implementación de otros métodos
}
```

## 5. Simulación de Backend con LocalStorage

### Estrategia de Desarrollo

Para permitir el desarrollo de features sin necesidad de un backend real:

1. **Adaptador LocalStorage**: La implementación actual utiliza AsyncStorage para persistir todos los datos en el dispositivo
2. **Simulación de latencia**: Los adaptadores pueden incluir delays artificiales para simular llamadas de red
3. **Mock de respuestas**: Estructuras de datos que emulan respuestas de una API REST

### Implementación de Simulación

```typescript
// Ejemplo: Simulación de latencia de red
export class LocalStorageAdapter implements IDataAdapter {
  private readonly SIMULATED_DELAY_MS = 300;

  private async simulateNetworkDelay(): Promise<void> {
    return new Promise(resolve => 
      setTimeout(resolve, this.SIMULATED_DELAY_MS)
    );
  }

  async createBudget(budget: Omit<IBudget, 'id'>): Promise<IBudget> {
    await this.simulateNetworkDelay(); // Simula latencia
    // ... resto de la implementación
  }
}
```

### Transición a Backend Real

Cuando se requiera integrar un backend real:

1. Crear una nueva clase `ApiAdapter` que implemente `IDataAdapter`
2. Esta clase realizará llamadas HTTP a los endpoints del backend
3. Cambiar la inyección de dependencias para usar `ApiAdapter` en lugar de `LocalStorageAdapter`
4. No es necesario modificar la lógica de negocio ni los componentes de UI

```typescript
// apps/mobile/src/adapters/ApiAdapter.ts (futuro)
export class ApiAdapter implements IDataAdapter {
  private readonly baseUrl = 'https://api.budgetwise.com/v1';

  async createBudget(budget: Omit<IBudget, 'id'>): Promise<IBudget> {
    const response = await fetch(`${this.baseUrl}/budgets`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(budget)
    });
    return response.json();
  }
}
```

## 6. Notas para Desarrolladores

- **No modificar directamente los datos**: Siempre utilizar los métodos del adaptador
- **Tipado estricto**: Todos los tipos están definidos en `packages/shared/types`
- **Tests**: El patrón adaptador permite escribir tests unitarios fácilmente mockeando la interfaz
- **Documentación**: Actualizar este archivo cuando se agreguen nuevas tecnologías o patrones
