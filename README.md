Customer

# Como crear una aplicaci&oacute;n que consuma distintas versiones de un M&oacute;dulos en GO 

## Creaci&oacute;n de modulo con distintas versiones

Para ver el manejo de versiones en modulos de GO vamos a crear un modulo con una unica funcion que regrese
la versi&oacute;n del modulo. A continuacion los pasos.

### Crear un proyecto en github. 

Por ejemplo puede crear un repositorio gomodulo en la ruta [https://github.com/propietario/gomodulo/](https://github.com/propietario/gomodulo/)

### Clonar el proyecto en un directorio local

Ejecute el siguiente comando para clonar el proyecto
```console
git clone https://github.com/propietario/gomodulo.git
```
   
### Inicializar el modulo

Para inicializar el modulo ejecute el comando en el directorio del proyecto que clono:

```console
go mod init github.com/propietario/gomodulo/
```

Cambie el valor de github.com/propietario/gomodulo/ segun el repositorio que cree. Verifique que este creado
el archivo go.mod con este contenido

```console
module github.com/propietario/gomodulo

go 1.14
```

### Crear c&oacute;digo inicial

Dentro del directorio del proyecto cree el archivo gomodulo.go

```console
package gomodulo
import "fmt"
func Version(){
   fmt.Println("Version 1.0.0");
}
```

### Crear V1.0.0

Ejecute los siguientes comando dentro del directorio del proyecto:

```console
git add .
git commit -m "Version 1.0.0"
git tag v1.0.0
git push --tags origin v1.0.0
```

### Crear v1.1.0

Edite el archivo gomodulo.go con el siguiente c&oacute;digo

```console
package gomodulo
import "fmt"
func Version(){
   fmt.Println("Version 1.1.0");
}
```

Ejecute los siguientes comandos:

```console
git add .
git commit -m "Version v1.1.0"
git tag v1.1.0
git push --tags origin v1.1.0
```

### Crear v2.0.0

Para crear la version v2.0.0 debe crear una rama y cambiar el archivo go.mod . Para esto primero cree la rama
v2 con los siguientes comandos:

```console
git checkout -b v2
git push --set-upstream origin v2
```

Para esto cambie el archivo go.mod para que quede as&iacute;:

```console
module github.com/propietario/gomodulo/v2

go 1.14
```

Note como se agrega v2 al final de la ruta con el proyecto. Ahora edite el archivo gomodules.go para que quede 
de esta forma:

```console
package gomodulo
import "fmt"
func Version(){
   fmt.Println("Version 2.0.0");
}
```

Ahora ejecute los siguientes cambios para hacer commit y crea el tag con la versi&oacute;n

```console
 git add .
 git commit -m "Version v2.0.0"
 git tag v2.0.0
 git push --tags origin v2.0.0
```

### Crear v2.1.0

Edite el archivo gomodulo.go con el siguiente c&oacute;digo

```console
package gomodulo
import "fmt"
func Version(){
   fmt.Println("Version 2.1.0");
}
```

Ejecute los siguientes comandos:

```console
git add .
git commit -m "Version v2.1.0"
git tag v2.1.0
git push --tags origin v2.1.0
```

## Proyecto que consume versiones del modulo

### Cree proyecto que usa el modulo v1.0.0

Ejecute el siguiente comando para crear un directorio e inicializar el proyecto

```console
mkdir usogomodulo
cd usogomodulo
go mod init github.com/propietario/usogomodulo
```

Note que el valor github.com/propietario/repositorio debe ser el que usted desee usar y adicionalmente
no es necesario crear el proyecto en github. Note que en directorio se crea el archivo go.mod con el contenido

```console
module github.com/propietario/usogomodulo/

go 1.14
```

Edite el archivo go.mod para que quede de esta forma:

```console
module github.com/propietario/usogomodulo/
require github.com/propietario/gomodulo v1.0.0
go 1.14
```

Ahora cree el archivo usogomodulo.go con el siguiente contenido:

```console
package main
import "github.com/propietario/gomodulo"
func main(){
  gomodulo.Version()
}
```

Ahora compile y ejecute el proyecto con los comandos:

```console
go build
./usogomodulo
```

Debera ver la salida Version 1.0.0

### Ajustar proyecto que usa el modulo v1.1.0

Para usar la versi&oacute;n v1.1.0 cambie el archivo go.mod para que quede de la siguiente forma:

```console
module github.com/propietario/usogomodulo/
require github.com/propietario/gomodulo v1.1.0
go 1.14
```

Borre el archivo go.sum con el comando

```console
rm go.sum
```

Compile y ejecute el proyecto para ver los cambios 

```console
go build
./usogomodulo
```

Debera ver la salida Version 1.1.0

### Ajustar proyecto que usa el modulo v2.0.0

Para usar la versi&oacute;n v2.0.0 cambie el archivo go.mod para que quede de la siguiente forma:

```console
module github.com/propietario/usogomodulo/
require github.com/propietario/gomodulo/v2 v2.0.0
go 1.14
```

Borre el archivo go.sum con el comando

```console
rm go.sum
```

Ajuste el archivo usogomodulo.go para que quede de esta forma:

```console
package main
import "github.com/propietario/gomodulo/v2"
func main(){
  gomodulo.Version()
}
```

Compile y ejecute el proyecto para ver los cambios 

```console
go build
./usogomodulo
```

Debera ver la salida Version 2.0.0

### Ajustar proyecto que usa el modulo v2.1.0

Para usar la versi&oacute;n v2.1.0 cambie el archivo go.mod para que quede de la siguiente forma:

```console
module github.com/propietario/usogomodulo/
require github.com/propietario/gomodulo/v2 v2.1.0
go 1.14
```

Borre el archivo go.sum con el comando

```console
rm go.sum
```

Compile y ejecute el proyecto para ver los cambios 

```console
go build
./usogomodulo
```

Debera ver la salida Version 2.1.0
