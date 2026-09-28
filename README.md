# Aplicación de práctica — Sistema de Gestión de Pedidos y Reservas para Restaurantes

Aplicación de dos caras (vulnerable / corregida) construida para la actividad AA2
(Proyecto Integrador: Evaluación de Seguridad con OWASP ZAP). Se usa Python 3
y Flask, sin dependencias externas más allá de Flask.

## Qué es real y qué debes hacer tú

- **Real, ya construido y verificado:** el código de las dos versiones, las 14
  comprobaciones automáticas (`pruebas/verificar_vulnerabilidades.py`) y las
  capturas de pantalla en `capturas/`, que muestran el bypass de login y el XSS
  ejecutándose en la versión vulnerable, y bloqueados en la versión corregida.
- **Pendiente, debes hacerlo tú:** instalar OWASP ZAP y ejecutar el escaneo real
  contra la versión vulnerable (puerto 5050). El informe (`AA2_ProyectoSeguridad_OWASPZAP.docx/.pdf`)
  deja marcada con claridad la sección que debes completar con tus propias
  capturas y el reporte que genera ZAP (menú **Report → Generate Report...**).

## Cómo ejecutar las aplicaciones

En dos terminales distintas (Windows, PowerShell):

```powershell
cd vulnerable
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r ..\requirements.txt
python app.py          # http://127.0.0.1:5050
```

```powershell
cd corregida
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r ..\requirements.txt
python app.py          # http://127.0.0.1:8060
```

Usuarios de prueba en ambas: `admin` / `admin123` y `cliente1` / `cliente123`.

## Cómo escanear con OWASP ZAP (lo que falta para el entregable)

1. Instala ZAP desde https://www.zaproxy.org/download/ (instalador de Windows).
2. Con la aplicación **vulnerable** corriendo en `http://127.0.0.1:5050`, abre ZAP.
3. En la pestaña **Automated Scan**, pega esa URL en "URL to attack" y pulsa **Attack**.
4. Cuando termine, revisa el panel **Alerts** (a la izquierda, agrupado por severidad).
5. Genera el reporte: menú **Report → Generate Report...** → elige HTML o PDF.
6. Toma capturas del panel de alertas y de al menos dos hallazgos (por ejemplo,
   la inyección SQL y el XSS) y pégalas en la sección 3 del informe, junto con
   el reporte generado.

## Cómo reproducir la verificación manual (opcional)

Con las dos aplicaciones corriendo (puertos 5050 y 8060):

```powershell
pip install requests
python pruebas\verificar_vulnerabilidades.py
```

Debe imprimir 14 de 14 verificaciones en el estado esperado.

## Estructura

```
vulnerable/app.py      aplicación con las 5 fallas a propósito
corregida/app.py       misma aplicación, con las 5 fallas corregidas
pruebas/verificar_vulnerabilidades.py   verificación automática (no reemplaza ZAP)
pruebas/salida_verificacion.txt         salida real de la última ejecución
capturas/               capturas de pantalla reales (antes/después)
```

## Vulnerabilidades incluidas a propósito (versión vulnerable)

1. Inyección SQL en el login (`admin' -- ` como usuario entra sin contraseña).
2. Inyección SQL en el buscador del menú.
3. XSS almacenado en las reseñas de un plato.
4. Cabeceras de seguridad ausentes (X-Frame-Options, X-Content-Type-Options, CSP).
5. La cabecera `Server` revela la versión de Werkzeug y de Python.

Todas están corregidas en `corregida/app.py`, con un comentario en el código
que explica cada corrección.
