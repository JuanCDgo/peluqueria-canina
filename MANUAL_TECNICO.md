# 🛠️ Manual Técnico · Peluquería Canina & Spa

**Autor:** Juan & Antigravity  
**Stack:** HTML5, Tailwind CSS (CDN), Vanilla JavaScript (ES6+), LocalStorage, QRCode.js (con fallback QuickChart.io), Canvas Confetti.  
**Despliegue:** GitHub Pages (`https://juancdgo.github.io/peluqueria-canina/`).  
**Copia de Seguridad:** Google Drive (`Mini Apps/peluqueria-canina`).

---

## 1. Arquitectura y Vistas

La aplicación está diseñada como una SPA modular con 3 vistas especializadas:
* **Vista 1 (Panel Administrador/Groomer):** Fichas caninas, historial de baños/cortes, control de saldos y recordatorios WhatsApp.
* **Vista 2 (Turnero Online para Tutores):** Activada con `?booking=true`, permite a los dueños solicitar turno según disponibilidad del negocio.
* **Vista 3 (Tarjeta Digital de Fidelidad):** Activada con `?pass=ID_PET` o `?card=ID_PET`, interfaz pública aislada y segura para los tutores con patitas 🐾 interactivas, meta de visitas, premio desbloqueable y código QR para mostrador.

---

## 2. Esquema de Persistencia (LocalStorage)

### 1. `pelucanina_settings` (Object)
```typescript
{
  salonName: string;          // "Grooming · Huellitas"
  salonLogo: string;          // Base64 Data URL (optimizado a 280px)
  salonPhone: string;         // WhatsApp de contacto
  alias: string;              // Alias de cobro MP/MODO
  cbu: string;                // CBU/CVU para transferencias
  titular: string;            // Nombre del titular de la cuenta
  targetStamps: number;       // Meta de patitas (default: 5)
  rewardDescription: string;  // "1 Baño & Secado con Perfumado Gratis"
  template: string;           // Plantilla de aviso WhatsApp
  bookingEnabled: boolean;    // Turnero online habilitado
  workDays: number[];         // [1, 2, 3, 4, 5, 6]
  bookingStartHour: number;   // 9
  bookingEndHour: number;     // 19
}
```

### 2. `pelucanina_clients` (Array<Object>)
```typescript
{
  id: string;                 // "pet-1", "pet-179374..."
  name: string;               // "Milo"
  owner: string;              // "Familia Rossi"
  phone: string;              // "54911..."
  breed: string;              // "Golden Retriever"
  size: string;               // "Grande" | "Mediano" | "Chico"
  service: string;            // "Baño y Corte de Raza"
  temperament: string;        // "Tranquilo", "Inquieto con secador"
  lastVisit: string;          // "YYYY-MM-DD"
  intervalWeeks: number;      // 3
  debt: number;               // ARS $ (0 si está al día)
  stamps: number;             // Patitas acumuladas (0..N)
  notes: string;              // Ficha técnica veterinaria/grooming
  lastReminderSent: string;   // ISO string de último recordatorio
}
```

---

## 3. Cobro QR Infalible (Doble Motor)

Generación de código QR con motor local y fallback garantizado a imagen:
```javascript
let success = false;
try {
  if (typeof QRCode !== 'undefined') {
    new QRCode(container, { text: qrPayload, width: 170, height: 170, colorDark: '#78350f', colorLight: '#ffffff' });
    success = true;
  }
} catch (e) { console.warn(e); }
if (!success) {
  container.innerHTML = '<img src="https://quickchart.io/qr?text=' + encodeURIComponent(qrPayload) + '&size=170" alt="QR Cobro" class="w-[170px] h-[170px] rounded-xl shadow-sm mx-auto block">';
}
```

---

## 4. Despliegue y Sincronización

* Repositorio: `https://github.com/JuanCDgo/peluqueria-canina.git`
* Rama: `main`
* Despliegue automático vía GitHub Pages sobre `/root`.
