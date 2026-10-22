# Configuración de SSL/TLS con OpenSSL y Apache

![Categoría](https://img.shields.io/badge/Categor%C3%ADa-SSL%2FTLS%20%2F%20PKI-blue)
![Herramienta](https://img.shields.io/badge/Herramienta-OpenSSL%20%2B%20Apache-green)
![Plataforma](https://img.shields.io/badge/Plataforma-Debian%20%2B%20VirtualBox-orange)
![Bootcamp](https://img.shields.io/badge/4Geeks-Cybersecurity%20Bootcamp-blue)
![Estado](https://img.shields.io/badge/Estado-Completado-brightgreen)

Laboratorio de configuración de un **servidor HTTPS seguro** utilizando **OpenSSL** para la generación de certificados y **Apache** como servidor web, implementando comunicación cifrada con TLS en un entorno virtualizado.

---

## Aviso ético

> Este ejercicio se realizó sobre una máquina virtual de prueba en un entorno aislado. El certificado generado es autofirmado y de uso exclusivamente educativo. Las técnicas documentadas tienen fines **exclusivamente educativos, defensivos y de portafolio profesional**.

---

## Resumen

| Indicador | Valor |
|-----------|-------|
| Objetivo | Configurar un servidor Apache con HTTPS usando un certificado autofirmado |
| Entorno | Debian VM en VirtualBox con Apache y OpenSSL |
| Clave generada | RSA 2048 bits |
| Certificado | Autofirmado, validez 365 días |
| Dominio configurado | `my-domain.com` (resolución local vía `/etc/hosts`) |
| Entregable | `report.json` (generado por `check_ssl.sh`) |
| Contexto | Práctica del Bootcamp de Ciberseguridad de 4Geeks Academy |

---

## Metodología

1. **Generación de clave privada y CSR** — creación de una clave RSA de 2048 bits y una solicitud de firma de certificado (CSR).
2. **Firma del certificado** — autofirmado del CSR para obtener un certificado X.509 con validez de 365 días.
3. **Configuración de Apache** — modificación de `default-ssl.conf` con las rutas del certificado y la clave, y configuración del `ServerName`.
4. **Habilitación de HTTPS** — activación del módulo SSL y del sitio SSL en Apache.
5. **Resolución DNS local** — mapeo del dominio `my-domain.com` a `127.0.0.1` en `/etc/hosts`.
6. **Verificación** — acceso a `https://my-domain.com` y confirmación de la conexión cifrada.

---

## Comandos principales

```bash
# Generar clave privada RSA de 2048 bits
openssl genrsa -out server.key 2048

# Generar CSR (Certificate Signing Request)
openssl req -new -key server.key -out server.csr

# Autofirmar el certificado (validez 365 días)
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

# Habilitar módulo SSL en Apache
sudo a2enmod ssl

# Habilitar sitio SSL
sudo a2ensite default-ssl

# Recargar Apache
sudo systemctl reload apache2

# Configurar resolución local
echo "127.0.0.1 my-domain.com" | sudo tee -a /etc/hosts

# Verificar la configuración
bash check_ssl.sh
```

---

## Configuración de Apache (`default-ssl.conf`)

```apache
SSLCertificateFile    /ruta/al/server.crt
SSLCertificateKeyFile /ruta/al/server.key
ServerName            my-domain.com
```

---

## Resultado

Al acceder a `https://my-domain.com` desde el navegador de la VM, se muestra la advertencia del certificado autofirmado. Al aceptarla, se carga la página predeterminada de Apache sobre una conexión HTTPS cifrada.

---

## Estructura del repositorio

```
set-up-an-SSL-in-openSSL-with-a-secure-server/
├── README.md                 # Documentación del ejercicio (este archivo)
├── README.es.md              # Instrucciones originales del ejercicio (español)
├── report.json               # Resultado de la verificación (check_ssl.sh)
├── assets/
│   ├── check_ssl.sh          # Script de verificación proporcionado
│   └── https.png             # Captura del acceso HTTPS
└── learn.json                # Metadatos del ejercicio
```

---

## Conceptos aplicados

- **PKI (Public Key Infrastructure)** — flujo completo: clave privada → CSR → certificado
- **TLS/SSL** — cifrado de comunicaciones web
- **Certificados X.509** — formato estándar de certificados digitales
- **Apache mod_ssl** — módulo de Apache para HTTPS
- **Resolución DNS local** — uso de `/etc/hosts` para pruebas

---

## Herramientas utilizadas

- **OpenSSL** — generación de claves, CSR y certificados
- **Apache** — servidor web con módulo SSL
- **Debian** — sistema operativo del servidor
- **VirtualBox** — virtualización del entorno

---

## Referencias

- [OpenSSL — Documentación oficial](https://www.openssl.org/docs/)
- [Apache — Módulo mod_ssl](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html)
- [Let's Encrypt — Cómo funciona HTTPS](https://letsencrypt.org/how-it-works/)

---

## Créditos

Práctica basada en un ejercicio del **Bootcamp de Ciberseguridad de 4Geeks Academy**. La configuración, la verificación y la documentación de este repositorio son de elaboración propia.

## Autor

**Bryan Calderón Herrera** · Ciberseguridad | Redes y Telecomunicaciones
[GitHub: cbry-96](https://github.com/cbry-96)
