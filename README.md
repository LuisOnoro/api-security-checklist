# API Security Checklist
 
Checklist práctica para revisar la seguridad de APIs REST durante las pruebas y las revisiones de diseño y código dentro de un SSDLC. Está organizada según el **OWASP API Security Top 10 (edición 2023)**.
 
Cada punto incluye: qué comprobar, un ejemplo de prueba, qué evidencia guardar y cómo mitigarlo.
 
## Para quién es
- Perfiles de QA y testing que quieren añadir comprobaciones de seguridad a sus pruebas de API.
- Desarrollo, para revisar diseño y código antes de una entrega.
- Quien necesite una plantilla para tickets o revisiones de seguridad.
## Cómo usarla
1. Copia [`checklist.md`](checklist.md) en el ticket, la pull request o el documento de revisión.
2. Marca cada punto como **OK / KO / N/A** y anota el endpoint afectado.
3. Adjunta la evidencia (petición, respuesta, captura) y abre un ticket por cada KO.
4. Repite la revisión cuando cambie el diseño o se añadan endpoints.
## Alcance y aviso
- Prueba **únicamente sistemas propios o con autorización expresa**.
- Los ejemplos usan `https://api.example.com` como servidor ficticio.
- Para practicar de forma segura, usa aplicaciones vulnerables a propósito y en local, como OWASP crAPI o OWASP Juice Shop.
## Estructura del repositorio
```
api-security-checklist/
├── README.md
├── checklist.md
├── LICENSE
└── examples/          (ejemplos de tickets y colecciones de Postman, próximamente)
```
 
## Hoja de ruta
- [ ] Ejemplos de tickets de vulnerabilidad (formato para Jira)
- [ ] Colección de Postman con las pruebas base
- [ ] Versión en inglés
- [ ] Sección de SOAP
## Referencias
- [OWASP API Security Project](https://owasp.org/API-Security/)
## Autor
**Luis Oñoro**, Ingeniero de Telecomunicaciones con máster en ciberseguridad, con experiencia en QA, automatización y validaciones de seguridad.
[LinkedIn](https://www.linkedin.com/in/luisonoro-cyber/)
 
## Licencia
MIT
