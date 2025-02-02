# Cisco IOS Switch configuración

## Configuración básica de dispositivos

### 1. Nombre de los dispositivos

1. Entramos en modo EXEC privilegiado `enable`
2. Vamos a darle nombre a un switch Cisco desde IOS.
3. Con `hostname` damos nombre a nuestro dispositivo
4. Si queremos devolver el switch a predeterminado indicaremos `no hostname`

```shell
Switch> enable
Switch# configure terminal
Switch(config)# hostname Sw-Planta-1
Sw-Planta-1(config)#
```

---

### 2. Asignación de contraseñas

Puntos clave al elegir las contraseñas

- Contraseñas de mas de 8 caracteres de longitud
- Combinación de letras mayúsculas y minúsculas, números, caracteres especiales o secuencias numéricas.

<br>

<aside>
🔑Protección modo EXEC del usuario.

</aside>

```bash
Sw-Planta-1# configure terminal
Sw-Planta-1(config)# line console 0
Sw-Planta-1(config-line)# password cisco
Sw-Planta-1(config-line)# login
Sw-Planta-1(config-line)# end
Sw-Planta-1#
```

El comando `line console 0` se utiliza para acceder a la configuración de la línea de consola física del switch. El "0" representa el primer (y generalmente único) puerto de consola del dispositivo.

La línea de consola es el puerto físico al que conectamos nuestro cable de consola para realizar la configuración inicial del switch. Es importante proteger este acceso ya que cualquier persona con acceso físico al switch podría conectarse a través de este puerto.

<aside>
🔑Protección modo EXEC privilegiado (acceso total al dispositivo)

</aside>

```bash
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# enable secret class
Sw-Floor-1(config)# exit
Sw-Floor-1#
```

> Con `enable secret password` asignamos una contraseña.
>

---

<aside>
🔑Protección lineas VTY (virtual teletype)Telnet o SSH

</aside>

```bash
Sw-Planta-1# configure terminal
Sw-Planta-1(config)# line vty 0 15
Sw-Planta-1(config-line)# password cisco
Sw-Planta-1(config-line)# login
Sw-Planta-1(config-line)# end
```

Al configurar las líneas VTY:

- Se especifica el rango (0 15) para configurar todas las líneas de una vez
- El comando `password` establece la contraseña para el acceso remoto
- El comando `login` activa la autenticación

<aside>
Es crucial proteger las líneas VTY ya que son un punto de entrada remoto al switch y podrían ser objetivo de ataques desde la red.

</aside>

### 3.  Encriptación de las contraseñas

Para encriptar todas las contraseñas utilizaremos

```bash
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# service password-encryption
Sw-Floor-1(config)#
```

Este comando aplica un cifrado básico a todas las contraseñas que no están encriptadas. La encriptación solamente protege las contraseñas almacenadas en el archivo de configuración, no durante su transmisión por la red. Su objetivo principal es impedir que personas no autorizadas puedan ver las contraseñas al acceder al archivo de configuración.

**show running-config** Use el comando para verificar que las contraseñas estén ahora encriptadas.

### 4. Mensajes de aviso

Use el siguiente comando para crear un mensaje de banner

```bash
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# banner motd #Authorized Access Only#
```

> Este mensaje aparecerá en todos los intentos posteriores, hasta que el mensaje sea eliminado.


Solo valor académico.