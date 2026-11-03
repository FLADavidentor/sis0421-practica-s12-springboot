# Práctica guiada Sesión 12 · React + Spring Boot

SIS0421 Servicios Web · UAM

## Estructura

- `solicitudes-api`: Spring Boot (Spring Web), lista en memoria.
- `solicitudes-web`: React + Vite + Axios + Context + useReducer + Bootstrap.
- `bruno`: colección Bruno con GET, POST, PUT y DELETE.
- `evidencias`: capturas reales.

## Ejecución

Backend (puerto 8080):

```bash
cd solicitudes-api
./mvnw spring-boot:run
```

Frontend (puerto 5173):

```bash
cd solicitudes-web
npm install
npm run dev
```

Abrir http://localhost:5173. API en http://localhost:8080/api/solicitudes.

## Pruebas de API

| Método | URL | Cuerpo | Resultado | Evidencia |
|---|---|---|---|---|
| GET | /api/solicitudes | — | 200 | `evidencias/02-bruno-get.png` |
| POST | /api/solicitudes | `{"descripcion":"Revisar red"}` | 201 | `evidencias/03-bruno-post.png` |
| PUT | /api/solicitudes/1 | `{"estado":"RESUELTA"}` | 200 | `evidencias/04-bruno-put.png` |
| DELETE | /api/solicitudes/2 | — | 204 | `evidencias/05-bruno-delete.png` |

## Evidencias de React

- `06-react-lista.png`: datos de Spring Boot y contador de pendientes.
- `07-react-crear.png`, `08-react-resolver.png`, `09-react-eliminar.png`: CRUD desde la interfaz.
- `10-react-error-api-detenida.png`: error con la API detenida.
- `01-spring-boot-run.png`: Spring Boot en el puerto 8080.

## Preguntas de reflexión

1. **¿Por qué el arreglo compartido pertenece al proveedor y no a cada tarjeta?** Porque es una sola fuente de verdad: el contador, la lista y las acciones leen el mismo estado. Si cada tarjeta tuviera su copia, se desincronizarían.
2. **¿Qué cambia entre useState y useReducer en este caso?** Con useReducer todas las transiciones (cargando, cargadas, error, agregada, actualizada, eliminada) quedan centralizadas en una función pura con acciones con nombre, en vez de varios setState dispersos.
3. **¿Para qué se utiliza AbortController al cargar solicitudes?** Para cancelar el GET cuando el proveedor se desmonta (o en la doble ejecución de Strict Mode) y no despachar resultados de una petición obsoleta.
4. **¿Por qué useEffect no equivale exactamente a los métodos de ciclo de vida de clase?** Porque useEffect sincroniza el componente con un sistema externo según sus dependencias; se ejecuta tras el render, puede repetirse y su cleanup corre antes de cada nueva ejecución, no solo al desmontar.
5. **¿Qué limitación tiene guardar los datos en una lista en memoria?** Los datos se pierden al reiniciar Spring Boot, no se comparten entre instancias y no escalan; se requiere una base de datos para persistir.
