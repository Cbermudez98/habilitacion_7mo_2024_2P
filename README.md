### Enunciado

Diseña una API utilizando **Nest.js**, **TypeORM**, y **class-validator** para gestionar **usuarios** y sus **tarjetas de crédito**. Un **usuario** puede tener una o más **tarjetas de crédito**, y se deben permitir las siguientes operaciones a través de los endpoints:

![Diagrama relacional](https://github.com/user-attachments/assets/04f05086-9b21-4886-a9a6-9bb63f9e98a2)


#### Requerimientos:

1. **Gestión de Usuarios**:
   - Un usuario puede registrarse en el sistema.
   - Un **usuario** tiene un conjunto de **tarjetas de crédito** asociadas.
   - Los datos del usuario incluyen:
     - ID único.
     - Nombre completo.
     - Correo electrónico.

2. **Gestión de Tarjetas de Crédito**:
   - Cada **tarjeta de crédito** debe tener:
     - ID único.
     - Número de tarjeta.
     - Límite de crédito.
     - Saldo actual.
   - Un usuario puede tener múltiples tarjetas de crédito.
   - Se deben permitir las siguientes operaciones:
     - Crear una nueva tarjeta y asociarla a un usuario.
     - Consultar el saldo de una tarjeta específica.
     - Realizar pagos para reducir el saldo de la tarjeta.
     - Obtener una lista de todas las tarjetas asociadas a un usuario.
     - Ver la información del usuario.
     - Realizar compras con las tarjetas y aumentar el saldo (simulando el gasto de la tarjeta).
     - Ver el saldo disponible de una tarjeta tras realizar compras.

---

### **Endpoints con JSON de entrada**:

1. **Obtener todas las tarjetas del sistema**  
   - **Método**: `GET`  
   - **Endpoint**: `/cards/all`  
   - **Datos de entrada**: No requiere datos de entrada.  

2. **Obtener las tarjetas asociadas a un usuario específico**  
   - **Método**: `GET`  
   - **Endpoint**: `/user/:id/cards`  
   - **Datos de entrada**:  
     - **Parámetro de ruta**:  
       - `id`: ID del usuario (número entero o UUID).  

3. **Obtener la información de un usuario**  
   - **Método**: `GET`  
   - **Endpoint**: `/user/:id`  
   - **Datos de entrada**:  
     - **Parámetro de ruta**:  
       - `id`: ID del usuario (número entero o UUID).

4. **Consultar el saldo de una tarjeta específica**  
   - **Método**: `GET`  
   - **Endpoint**: `/card/:id/saldo`  
   - **Datos de entrada**:  
     - **Parámetro de ruta**:  
       - `id`: ID de la tarjeta (número entero o UUID).

5. **Pagar una tarjeta de crédito**  
   - **Método**: `POST`  
   - **Endpoint**: `/card/:id/pagar`  
   - **Datos de entrada**:  
     - **Parámetro de ruta**:  
       - `id`: ID de la tarjeta (número entero o UUID).  
     - **Cuerpo de la solicitud**:  
       ```json
       {
         "monto": 200.00
       }
       ```  
       - `monto`: Monto que se desea pagar (número decimal mayor a 0 y menor o igual al saldo actual).  

6. **Crear una nueva tarjeta de crédito y asociarla a un usuario**  
   - **Método**: `POST`  
   - **Endpoint**: `/user/:id/cards`  
   - **Datos de entrada**:  
     - **Parámetro de ruta**:  
       - `id`: ID del usuario (número entero o UUID).  
     - **Cuerpo de la solicitud**:  
       ```json
       {
         "numero_tarjeta": "1234567812345678",
         "limite_credito": 5000.00
       }
       ```  
       - `numero_tarjeta`: Número de tarjeta de crédito (cadena de 16 dígitos).  
       - `limite_credito`: Límite de crédito de la tarjeta (número decimal).  

7. **Realizar una compra con una tarjeta de crédito**  
   - **Método**: `POST`  
   - **Endpoint**: `/card/:id/comprar`  
   - **Datos de entrada**:  
     - **Parámetro de ruta**:  
       - `id`: ID de la tarjeta (número entero o UUID).  
     - **Cuerpo de la solicitud**:  
       ```json
       {
         "monto": 150.00
       }
       ```  
       - `monto`: Monto de la compra (número decimal que debe ser menor o igual al saldo disponible de la tarjeta).  
       - La operación aumentará el saldo de la tarjeta al realizar la compra.  

---

### **Ejemplo de Solicitudes**:

1. **Obtener las tarjetas de un usuario con ID 1**:  
   - **Solicitud**:  
     ```http
     GET /user/1/cards
     ```

2. **Consultar el saldo de una tarjeta con ID 5**:  
   - **Solicitud**:  
     ```http
     GET /card/5/saldo
     ```

3. **Pagar una tarjeta con ID 5**:  
   - **Solicitud**:  
     ```http
     POST /card/5/pagar
     Content-Type: application/json

     {
       "monto": 200.00
     }
     ```

4. **Crear una nueva tarjeta para un usuario con ID 1**:  
   - **Solicitud**:  
     ```http
     POST /user/1/cards
     Content-Type: application/json

     {
       "numero_tarjeta": "1234567812345678",
       "limite_credito": 5000.00
     }
     ```

5. **Realizar una compra con una tarjeta de crédito (ID 5)**:  
   - **Solicitud**:  
     ```http
     POST /card/5/comprar
     Content-Type: application/json

     {
       "monto": 150.00
     }
     ```
