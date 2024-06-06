[Duck DNS]: https://www.duckdns.org/
[Duck DNS/install]: https://www.duckdns.org/install.jsp

[docker-compose.yml]: /docker-compose.yml


**Contenidos**
- [1. Duck DNS](#1-duck-dns)
- [2. Actualizar automáticamente](#2-actualizar-automáticamente)
    - [2.1. Script](#21-script)
    - [2.2. Docker](#22-docker)


# 1. [Duck DNS]
Permite la asignar un **dominio** con una **IP dinámica**


# 2. Actualizar automáticamente
## 2.1. Script
Actualizar automáticamente Duck DNS mediante un **Script**:

- Iniciar sesión en [Duck DNS]
    - **Acceder** a [Duck DNS/install]
        - Seleccionar **Sistema Operativo**: En este caso `pi` (**Raspberry Pi**)
        - Seleccionar **dominio**
- **Conectarse** a la Raspberry Pi (Los pasos siguientes están disponibles en [Duck DNS/install])
    - **Crear carpeta** y acceder a ella: `mkdir duckdns; cd duckdns`
    - **Escribir** script: `nano duck.sh`
        - Contenido del archivo:
            ```bash
            echo url="<url_que_aparece_en_duck_dns>" | curl -k -o ~/duckdns/duck.log -K -
            ```
            > [!NOTE]
            > `Ctrl` + `O` para guardar y `Ctrl` + `X` para cerrar el archivo

    - Añadir **permisos de ejecución** al Script: `chmod 700 duck.sh`
    - **Crear automátización**: `crontab -e`
        - **Añadir** en la **parte inferior** del archivo: `*/5 * * * * ~/duckdns/duck.sh >/dev/null 2>&1` (Se ejecuta cada 5 minutos)
            > [!NOTE]
            > `Ctrl` + `O` para guardar y `Ctrl` + `X` para cerrar Cron Tab
    - **Ejecutar script**: `./duck.sh`
    - **Comprobar log**: `cat duck.log`
        - **OK**: Funcionamiento correcto
        - **KO**: Error
    - **Inicio automático** al encender la Raspberry: `sudo service cron start`

## 2.2. Docker
Actualizar automáticamente Duck DNS mediante **Docker**:

- Modificar las variables de [docker-compose.yml] (marcadas con `<>`)
    - `<sub_dominio>`: **Únicamente** el nombre del **sub dominio**, sin `.duckdns.org`
    - `<token>`: Token que aparece en la página de [Duck DNS] cuando inicias sesión
- Ejecutar `docker compose up -d --build`
- Comprobar log: `cat logs/duck.log`
    - Debe aparecer `successful`
