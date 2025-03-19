# Formato de los mensajes e inclusion
Ahora mismo todos lo mensajes sé encuentra én [es.yml](https://github.com/cerepapaloca/XBXT-Translations/blob/main/es.yml).

> [!WARNING]
> Estos mensajes están sujetos a cambios, semánticos, inclusion, eliminación
> o cambios en sus rutas

## Inclusion de mensajes no existente
Para incluir un mensaje traducido tienes que copiar o saber la ruta del mensaje en [es.yml](https://github.com/cerepapaloca/XBXT-Translations/blob/main/es.yml) y 
añadirlo en el yml que quieras traducir. Un ejemplo de esto:

En este caso quiero traducir el hover del chat. Lo primero ver donde se ubica en `es.yml`
```yml
event:
  quit: '&8[&4-&8]|!> El jugador <|%s|> se a desconecto'
  join: '&8[&a+&8]|!> El jugador<click:suggest_command:/w %1$s> <|%1$s|> se a unido'
  chat:
    format: '%1$s&r » %2$s'
    # El hover se va a traducir
    hover: |
      Distancia recorrida: <|%sKM|>
      Tiempo jugado: <|%sH|>
      Nombre Real: <|%s|>
      Idioma: <|%s|>
```
En este caso se ve que se ubica en `event.chat.hover`. Con esa ruta se tiene pasar en al yml que quieres traducir
en este caso al `en.yml` de esta manera
```yml
event:
  chat:
    hover: |
      Distance traveled: <|%sKM|>
      Time played: <|%sH|>
      name: <|%s|>
      Language: <|%s|>
```
> [!NOTE]
> En caso quieres incluir otro mensaje que ya tiene una ruta tienes que añadir a partir de esa ruta un 
> ejemplo de esto:
> ```yml
> event:
>  # Se añade solo el quit por qué ya existe la sección de event 
>  quit: '&8[&4-&8]|!> Player <|%s|> has disconnected'
>  chat:
>    hover: |
>      Distance traveled: <|%sKM|>
>      Time played: <|%sH|>
>      name: <|%s|>
>      Language: <|%s|>
>```
De esta manera se habrá incluido un mensaje traducido correctamente

## Inclusion de mensajes existente
En caso de que quieras incluir un mensaje, pero ya existe puedes añadir varios esto es util para los
mensajes de muerte. Estos mensajes van a ir apareciendo de manera totalmente aleatoria.

Para hacer esto tienes que crear una **lista** si no sabes aquí está la explicación:
lo que tienes que hacer es un salto de línea al texto y añadir tu guion al principio del mensaje nuevo y
al viejo. Un ejemplo:

Con un solo mensaje
```yml
death-cause:
  entity:
    creeper: un <|%2$s|> se inmoló matando a <|%1$s|>
```

Con varios mensajes
```yml
death-cause:
  entity:
    creeper:
      - 'un <|%2$s|> se inmoló matando a <|%1$s|>'
      - 'un <|%2$s|> le exploto en la cara a <|%1$s|>'
```
> [!TIP]
> Es recomendable usar un programa para editar yml sin equivocarte como Notepad++ o Visual Studio

## Formato de mensajes
### ¿Que son?
Es un formato usado en java para remplazar texto de manera más cómoda suelen ser asi `%1$s` o `%s`. 
Es decir donde veas un símbolo de estos va una variable que puede ser el nombre de un jugador, un
número u otro tipo de texto.

### ¿Cuál es la diferencia entré `%1$s` y `%s`?
Tiene una única diferencia y es el orden de las variables. En `%s` todas las variables salen en el orden
por defecto mientras en `%1$s` puedes cambiar el orden cambiando su número un ejemplo de esto

En este caso solo se está usando `%s`
```yml
death-cause:
  entity:
    creeper: 'un <|%s|> le exploto en la cara a <|%s|>'
```
```
Un cerespaploca le exploto en la cara a Creeper
```
Como se ve el orden no es correcto, por eso se tiene usar `%1$s` para cambiar el orden las variables
```yml
death-cause:
  entity:
    creeper: 'un <|%2$s|> le exploto en la cara a <|%1$s|>'
```
```
Un Creeper le exploto en la cara a cerespaploca
```
En este caso esta correcto el mensaje.

> [!CAUTION]
> No usar `%1$s` o `%s` de más. Es decir si ves que el mensaje solo tiene dos `%s` no añadas más 
> porque esto producirá un error

## Colores en los mensajes
Actualmente el plugin tiene soporte de MiniMessage para los colores y estilos. [Aquí](https://docs.advntr.dev/minimessage/format.html)
está la documentation de MiniMessage.

### Tipos de mensajes

El plugin tiene una propiedad especial para facilitar el asignado de colores todos los mensajes del plugin tiene una 
"tipo" que pertenece

```java
package net.atcore.messages;

import lombok.Getter;
import lombok.RequiredArgsConstructor;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@RequiredArgsConstructor
public enum TypeMessages {
    SUCCESS(new Tags("dark_green"), new Tags("green")),
    INFO(new Tags("dark_aqua"), new Tags("aqua")),
    WARNING(new Tags("yellow"), new Tags("gold")),
    ERROR(new Tags("red"), new Tags("dark_red")),
    KICK(new Tags("red"), new Tags("dark_red>", "b")),
    NULL(new Tags("gray"), new Tags("dark_red"));

    private final Tags mainColor;
    private final Tags secondColor;

    @Getter
    private static class Tags {

        Tags(String... tag) {
            tags.addAll(Arrays.stream(tag).toList());
        }

        private final List<String> tags = new ArrayList<>();
    }

    public String getMainColor() {
        return String.join("", mainColor.tags.stream().map(s -> "<" + s + ">").toList());
    }

    public String getSecondColor() {
        return String.join("", secondColor.tags.stream().map(s -> "<" + s + ">").toList());
    }

    public String getMainColorClose() {
        return String.join("", mainColor.tags.reversed().stream().map(s -> "</" + s + ">").toList());
    }

    public String getSecondColorClose() {
        return String.join("", secondColor.tags.reversed().stream().map(s -> "</" + s + ">").toList());
    }
}

```
Con esto, todos los mensajes informativos salen con `dark_aqua` los de advertencia con `yellow`
asi con todos los tipos.

### Uso de `<|`, `|>` y `|!>`

Para dar énfasis a un mensaje se usa los color segundario. Para usarse se tiene que abrir con `<|`  y para cerrar con `|>`.
Por último esta `|!>` que se utlizaria cuando cambias a un color de manera arbitraria y quieres pasar al color original 
aunque se intenta dejar de usar por qué los formatos de MiniMessage Permite cerrar los tag dejando el color original
asiendo que no sea tan util. Ejemplo de uso

**Sin resalte**
```yml
event:
  quit: '&8[&4-&8]|!> El jugador %s se a desconecto'
```
![](https://xbxt.xyz/img/docu/ejemplo1.png)

**Con resalte**
```yml
event:
  quit: '&8[&4-&8]|!> El jugador <|%s|> se a desconecto'
```
![](https://xbxt.xyz/img/docu/ejemplo2.png)

> [!TIP]
> Para tener una preview de como puede quedar el mensaje usa [Mini Message Viewer](https://webui.advntr.dev/)

> [!NOTE]
> Se recomienda usar `<|` y `|>` para nombres de jugadores o datos importantes

-----------

### ¿Qué es esto?
Este repositorio estará todos los mensajes del servidor traducido a varios idiomas, El plugin principal
del servidor tiene la capacidad de traducción dinámicamente según el idioma que tenga el cliente
### ¿Comó Puedo añadir Traducciones?
No se puede. Actualmente solo hay 3 idiomas disponibles, pero en un futuro se va a ir añadiendo más idiomas
### ¿Tengo traducir todo?
No hace falta traducir todo puedes traducir un solo mensajes o todos, no hay ni un mínimo o un máximo de mensajes
que hay que traducir
### ¿Puedo poner colores personalizados a los mensajes?
No, por qué todos los mensajes tienes un color específico según su tipo. Solo se puede cambiar de color
para casos muy puntuales
