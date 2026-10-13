# API de restaurantes y menús del día — La Pizarra

Especificación (OpenAPI 3) de la API que permite a un gestor externo dar de alta restaurantes en La Pizarra,
publicar su menú del día y retirarlos. Está en [`openapi.yaml`](openapi.yaml); se puede abrir en
<https://editor.swagger.io> o importar en Postman.

Contiene solo la API: ningún código ni dato de la plataforma que la usa.

## Operaciones
| Operación | Ruta |
|---|---|
| Crear perfil | `POST /restaurantes` |
| Consultar / listar | `GET /restaurantes?id_altiora=…` |
| Actualizar | `PATCH /restaurantes/{id_externo}` |
| Publicar menú del día | `PUT /restaurantes/{id_externo}/menus/{fecha}` |
| Borrar perfil | `DELETE /restaurantes/{id_externo}` |
| Sustituir (opcional, atómico) | `POST /restaurantes/{id_externo}/sustituir` |

## Reglas
- Autenticación con clave de API (`Authorization: Bearer …`) y un entorno de pruebas aparte.
- Idempotencia: cada recurso lleva `id_altiora`; repetir una petición no duplica nada.
- Mismo restaurante y fecha en el menú = se sobrescribe.
- Si el dueño ya reclamó el perfil, no se puede borrar: `409 perfil_reclamado`.
- Errores en JSON: `{"error": {"codigo": "...", "mensaje": "..."}}`.
