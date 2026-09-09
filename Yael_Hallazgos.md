Hallazgos individuales — Día 1
Proyecto:
ProyectoLyEP2026
Autora: Yael
Resumen ejecutivo (borrador)
El prototipo es un panel de gestión de clientes en React que consume la API pública FakeStoreAPI. Funciona a nivel demo, pero tiene fallas de seguridad visibles a simple vista (credenciales en texto plano, tanto de los administradores como de los clientes) y al menos una falla de arquitectura más sutil en el manejo de permisos, que solo aparece al comparar cómo distintos archivos leen el rol del usuario logueado.
Tabla de hallazgos
| # | Problema detectado                                                                                                                                                                       | Dimensión             |Impacto| Dónde está                             | Propuesta de solución 
| 1 |Los usuarios administradores están hard codeados en el código, con contraseñas en texto plano (Admin123 para todos)                                                                       |Seguridad              |Alto   |src/services/autorizacionesServices.js  |Sacar los usuarios del código; usar backend real con contraseñas hasheadas. 
| 2 |La ficha de cliente muestra la contraseña del cliente directamente en pantalla                                                                                                            |Seguridad / Privacidad |Alto   |src/pages/DetalleCliente.jsx (línea ~92)|Nunca mostrar contraseñas en la UI.
| 3 |El permiso para eliminar un cliente se decide leyendo localStorage.getItem("role") directo, en vez de usar el AutorizacionesContext que el resto de la app usa correctamente (Header.jsx) |Arquitectura           |Medio  |src/pages/DetalleCliente.jsx            |Usar siempre el contexto como única fuente de verdad para el rol del usuario.
Justificación de la selección
Elegí estos tres porque juntos muestran dos niveles de análisis distintos:los primeros dos son fallas de seguridad que quedan expuestas cuando lees el archivo correspondiente, mientras que el tercero exige leer y comparar el flujo de datos entre componentes.
