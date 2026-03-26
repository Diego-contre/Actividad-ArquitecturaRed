# Actividad-ArquitecturaRed

# 🌐 Guía de Arquitectura de Red e Infraestructura Cloud

---

## I. Glosario de Conceptos Críticos

| Concepto | Definición Simple | Nivel Técnico Breve | Ejemplo Real |
|----------|------------------|--------------------|--------------|
| **Modelo OSI** | El "mapa" de 7 capas del viaje de los datos. | Framework conceptual que divide la comunicación desde el Hardware (Física) hasta el Software (Aplicación). | Al abrir una web: Capa 7 (HTTP) pide los datos → Capa 4 (TCP) los segmenta → Capa 3 (IP) los enruta. |
| **DNS** | La "guía telefónica" de Internet. | Sistema jerárquico que traduce nombres de dominio (google.com) en direcciones IP (142.250.x.x). | Escribes un nombre en el navegador → El DNS resuelve la IP → Tu PC sabe a qué servidor conectarse. |
| **Subnet (Subred)** | Una red pequeña dentro de una grande. | Segmentación lógica de una red física para organizar tráfico y aplicar reglas de seguridad. | Una empresa separa el WiFi de "Invitados" de la red de "Servidores Críticos". |
| **VPC** | Tu parcela privada en la nube. | Virtual Private Cloud: Red aislada en AWS/Azure donde controlas IPs, rutas y firewalls. | Es tu "barrio privado" dentro del centro de datos del proveedor cloud. |
| **Load Balancer** | El director de tráfico. | Dispositivo que distribuye peticiones desde una IP pública a múltiples servidores internos. | 1,000 usuarios → 500 al servidor A y 500 al servidor B. |

---

## II. El Modelo OSI y la Encapsulación

- El modelo OSI **no es un protocolo físico**, es un estándar conceptual.
  
### 🔹 Encapsulación
Cada capa agrega un "header" (como sobres):
- Datos → TCP → IP → Ethernet

### 🔹 Desencapsulación
El receptor elimina cada capa hasta obtener el mensaje original.

### 🔹 Utilidad práctica
- Si el `ping` funciona pero la web no carga:
  - ❌ No es problema de red (Capa 3)
  - ✅ Puede ser:
    - Capa 4 (puertos)
    - Capa 7 (aplicación)

---

## III. Flujo de Resolución DNS

1. **Browser Cache**
   - ¿Ya visitaste la web antes?

2. **OS Cache**
   - ¿El sistema operativo conoce la IP?

3. **Resolver (ISP)**
   - Tu proveedor intenta resolver la dirección.

4. **Servidores Root / TLD / Authoritative**
   - Si nadie sabe, se consulta a nivel global.

5. **TTL (Time To Live)**
   - Tiempo que la IP se guarda en caché.

---

## IV. Arquitectura de Subredes en la Nube

### 🌍 Subred Pública
- Tiene acceso a Internet
- Contiene:
  - Internet Gateway
  - Load Balancer

### 🔒 Subred Privada
- No tiene acceso directo a Internet
- Contiene:
  - Backend (servidores)
  - Bases de datos

✅ Solo acepta tráfico desde la subred pública (ej: Load Balancer)

---

## V. Troubleshooting (CRÍTICO)

### 1. ¿Es el DNS?
```bash
nslookup tu-app.com
