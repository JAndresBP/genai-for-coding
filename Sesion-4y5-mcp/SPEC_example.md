# Especificación del Producto: App Presupuesto Anual

## 1. Visión del Producto

**Nombre del Producto:** BudgetWise - Gestor de Presupuesto Anual

**Descripción:** Aplicación móvil multiplataforma (React Native) que ayuda a usuarios sin experiencia previa en finanzas personales a crear presupuestos mensuales para el año en curso, registrar sus gastos reales y analizar su comportamiento financiero para identificar oportunidades de ahorro.

**Público Objetivo:** Usuarios beginners que nunca han utilizado aplicaciones de presupuesto personal.

**Plataforma:** React Native (iOS y Android)

---

## 2. Requerimientos Funcionales

### 2.1 Gestión de Presupuesto Mensual

| ID | Requerimiento | Descripción |
|----|---------------|-------------|
| RF01 | Crear presupuesto mensual | El usuario debe poder crear un presupuesto mensual estableciendo una cantidad máxima de dinero disponible para gastar en cada categoría del mes. |
| RF02 | Editar presupuesto mensual | El usuario debe poder modificar un presupuesto mensual existente. |
| RF03 | Eliminar presupuesto mensual | El usuario debe poder eliminar un presupuesto mensual si decide no continuar con él. |
| RF04 | Categorías predefinidas | El sistema debe proporcionar categorías predefinidas de gasto (alimentación, transporte, entretenimiento, servicios, etc.) para facilitar la creación del presupuesto. |
| RF05 | Crear categorías personalizadas | El usuario debe poder crear sus propias categorías de gasto además de las predefinidas. |

### 2.2 Registro de Gastos Reales

| ID | Requerimiento | Descripción |
|----|---------------|-------------|
| RF06 | Registrar gasto | El usuario debe poder registrar un gasto indicando: monto, categoría, fecha, descripción (opcional). |
| RF07 | Editar gasto | El usuario debe poder modificar los datos de un gasto registrado. |
| RF08 | Eliminar gasto | El usuario debe poder eliminar un gasto registrado. |
| RF09 | Vista de historial de gastos | El usuario debe poder ver un historial de todos los gastos registrados, filtrable por mes y categoría. |

### 2.3 Análisis de Comportamiento de Gastos

| ID | Requerimiento | Descripción |
|----|---------------|-------------|
| RF10 | Comparación presupuesto vs real | El sistema debe mostrar una comparación visual entre el presupuesto asignado y el gasto real por categoría. |
| RF11 | Resumen mensual | El usuario debe poder ver un resumen mensual que muestre: total presupuestado, total gastado, diferencia (ahorro/sobregasto). |
| RF12 | Tendencia de gastos | El sistema debe mostrar la tendencia de gastos a lo largo de los meses (gráfico simple). |
| RF13 | Alertas de sobregasto | El sistema debe notificar al usuario cuando esté a punto de exceder o haya excedido el presupuesto de una categoría. |

### 2.4 Identificación de Oportunidades de Ahorro

| ID | Requerimiento | Descripción |
|----|---------------|-------------|
| RF14 | Recomendaciones de ahorro | El sistema debe analizar los patrones de gasto y proporcionar recomendaciones personalizadas de ahorro (ej: "Has gastado 20% más en entretenimiento que el mes anterior"). |
| RF15 | Top categorías de gasto | El usuario debe poder identificar cuáles son las categorías donde más gasta dinero. |

### 2.5 Gestión del Año Fiscal

| ID | Requerimiento | Descripción |
|----|---------------|-------------|
| RF16 | Configuración del año actual | La aplicación debe estar configurada por defecto para el año actual (2026). |
| RF17 | Navegación entre meses | El usuario debe poder navegar entre los meses del año para ver y editar información de cualquier mes. |

---

## 3. Incrementos de Desarrollo

### Incremento 1: Funcionalidad Base (MVP) 
**Objetivo:** Permitir al usuario crear un presupuesto mensual básico y registrar gastos.

- RF01 - Crear presupuesto mensual
- RF04 - Categorías predefinidas
- RF06 - Registrar gasto
- RF10 - Comparación presupuesto vs real (vista básica)
- RF16 - Configuración del año actual
- RF17 - Navegación entre meses

### Incremento 2: Gestión de Datos 
**Objetivo:** Completar las operaciones CRUD para presupuestos y gastos.

- RF02 - Editar presupuesto mensual
- RF03 - Eliminar presupuesto mensual
- RF05 - Crear categorías personalizadas
- RF07 - Editar gasto
- RF08 - Eliminar gasto
- RF09 - Vista de historial de gastos

### Incremento 3: Diseño Stitch 
**Objetivo:** Aplicar sistema de diseño premium al proyecto.

- Diseño tokens (colores, tipografía, spacing)
- Componentes actualizados con theme
- Screens con sistema "The Editorial Ledger"
- RF11 - Resumen mensual (MonthlySummaryScreen)
- MonthlySummaryScreen y SettingsScreen

### Incremento 4: Análisis y Visualización
**Objetivo:** Proporcionar herramientas de análisis para evaluar el comportamiento de gastos.

- RF12 - Tendencia de gastos (gráfico de evolución)
- RF13 - Alertas de sobregasto (notificaciones)
- RF15 - Top categorías de gasto (ranking de categorías)

### Incremento 5: Optimización de Ahorro
**Objetivo:** Ayudar al usuario a encontrar oportunidades de ahorro.

- RF14 - Recomendaciones de ahorro personalizadas

---

## 4. Historias de Usuario Priorizadas

### Incremento 1 - MVP

**HU01: Como usuario, quiero crear un presupuesto mensual para planificar mis gastos**
- Criterios de aceptación:
  - Puedo seleccionar un mes del año actual
  - Puedo asignar un monto máximo a cada categoría predefinida
  - Puedo guardar el presupuesto
  - El sistema me muestra confirmación de guardado

**HU02: Como usuario, quiero registrar un gasto para rastrear mis compras**
- Criterios de aceptación:
  - Puedo seleccionar una categoría del presupuesto
  - Puedo ingresar el monto del gasto
  - La fecha se selecciona automáticamente (puede modificarse)
  - Puedo agregar una descripción opcional
  - El gasto se guarda y se descuenta del presupuesto disponible

**HU03: Como usuario, quiero ver cuánto he gastado vs mi presupuesto**
- Criterios de aceptación:
  - Puedo ver una lista de categorías con presupuesto asignado
  - Puedo ver el monto gastado en cada categoría
  - Puedo ver visualmente (barra o indicador) si estoy dentro o fuera del presupuesto

---

## 5. Supuestos y Restricciones

1. **Almacenamiento local:** La aplicación utilizará almacenamiento local (AsyncStorage o SQLite) sin backend.
2. **Sin autenticación:** No se requerirá login o registro para simplificar la experiencia inicial.
3. **Moneda única:** La aplicación estará configurada para una moneda (COP - Peso Colombiano) por defecto.
4. **Sin sincronización en la nube:** Los datos se almacenan únicamente en el dispositivo.

---

## 6. Definición de Éxito del Producto

El producto será exitoso cuando:
1. Un usuario sin experiencia previa pueda crear su primer presupuesto mensual en menos de 3 minutos.
2. El usuario pueda registrar un gasto en menos de 30 segundos.
3. El usuario pueda identificar claramente si está superando su presupuesto en una categoría específica.
4. El usuario reciba al menos una recomendación de ahorro útil después de 2 semanas de uso.
