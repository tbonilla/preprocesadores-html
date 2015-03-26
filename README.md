# Twig
























































### ¿Cómo funciona Twig?

Se puede resumir en cuatro pasos fundamentales:

1. **Cargar la plantilla**: Si la plantilla ya está compilada, la carga y va al paso evaluación, de lo contrario:
  * El analizador léxico reduce el código fuente de la plantilla a pequeñas piezas para facilitar su procesamiento;
  * El analizador convierte el flujo del segmento en un árbol de nodos significativo (el árbol de sintaxis        abstracta);
  * El compilador transforma el árbol de sintaxis abstracta en código PHP;
2. **Evaluar la plantilla**: Básicamente significa llamar al método display() de la plantilla compilada adjuntando el contexto.

### El analizador léxico

El analizador léxico acorta el código fuente de una plantilla hasta una secuencia de símbolos (cada símbolo es una instancia de `Twig_Token`, y la secuencia es una instancia de `Twig_TokenStream`). El analizador léxico por omisión reconoce 13 diferentes tipos de símbolos:

* `Twig_Token::BLOCK_START_TYPE, Twig_Token::BLOCK_END_TYP`: Delimitadores para bloques ({% %})
* `Twig_Token::VAR_START_TYPE, Twig_Token::VAR_END_TYPE`: Delimitadores para variables ({{ }})
* `Twig_Token::TEXT_TYPE`: Un texto fuera de una expresión;
* `Twig_Token::NAME_TYPE`: Un nombre en una expresión;
* `Twig_Token::NUMBER_TYPE`: Un número en una expresión;
* `Twig_Token::STRING_TYPE`: Una cadena en una expresión;
* `Twig_Token::OPERATOR_TYPE`: Un operador;
* `Twig_Token::PUNCTUATION_TYPE`: Un signo de puntuacion;
* `Twig_Token::INTERPOLATION_START_TYPE`, `Twig_Token::INTERPOLATION_END_TYPE` (a partir de la ramita 1,5):
  Los delimitadores para la interpolación de cadenas;
* `Twig_Token::EOF_TYPE`: Extremos de la plantilla.

Puedes convertir manualmente un código fuente en una secuencia de segmentos llamando al método tokenize() de un entorno:
```
$stream = $twig->tokenize($source, $identifier);
```
Dado que el flujo tiene un método `__toString()`, puedes tener una representación textual del mismo haciendo eco del objeto:
```
echo $stream."\n";
```
Aquí está la salida para la plantilla `Hello {{ name }}`:
```
TEXT_TYPE(Hello )
VAR_START_TYPE()
NAME_TYPE(name)
VAR_END_TYPE()
EOF_TYPE()
```
**Nota:**
Puedes cambiar el analizador léxico predeterminado usado por Twig (`Twig_Lexer`) llamando al método `setLexer()`:
```
$twig->setLexer($lexer);
```

### El analizador sintáctico

Convierte la secuencia de símbolos en un ASA (árbol de sintaxis abstracta), o un árbol de nodos (una instancia de `Twig_Node_Module`). La extensión del núcleo define los nodos básicos como: `for, if,` ... y la expresión nodos.

 Convertir manualmente una secuencia de símbolos en un nodo del árbol llamando al método `parse()` de un entorno:
 ```
 $nodes = $twig->parse($stream);
 ```
 Al hacer eco del objeto nodo te da una buena representación del árbol:
 ```
 echo $nodes."\n";
 ```
 Aquí está la salida para la plantilla `Hello {{ name }}`:
 ```
 Twig_Node_Module(
  Twig_Node_Text(Hello )
  Twig_Node_Print(
    Twig_Node_Expression_Name(name)
  )
)
```
**Nota:**
También puedes cambiar el analizador predeterminado (`Twig_TokenParser`) llamando al método `setParser()`:
```
$twig->setParser($analizador);
```

### El compilador

El último paso lo lleva a cabo el compilador. Este necesita un árbol de nodos como entrada y genera código PHP que puedes emplear para ejecutar las plantillas en tiempo de ejecución.

Puedes llamar al compilador manualmente con el método `compile()` de un entorno:
```
$php = $twig->compile($nodes);
```
El método `compile()` devuelve el código fuente PHP que representa al nodo.

La plantilla generada por un patrón Hello `{{ name }}` es la siguiente (la salida real puede diferir dependiendo de la versión de Twig que estés usando):
```
/* Hello {{ name }} */
class __TwigTemplate_1121b6f109fe93ebe8c6e22e3712bceb extends Twig_Template
{
    protected function doDisplay(array $context, array $blocks = array())
    {
        // line 1
        echo "Hello ";
        echo twig_escape_filter($this->env, $this->getContext($context, "name"), "ndex", null, true);
    }

    // algún código adicional
}
```
**Nota:**
En cuanto a los analizadores léxico y sintáctico, el compilador predeterminado (`Twig_Compiler`) se puede cambiar mediante una llamada al método `setCompiler()`:
```
$twig->setCompiler($compilador);
```

### Plantillas 

Son muy concisas y fáciles de leer, por lo que además son fáciles de entender por parte de los diseñadores web. Observa el mismo ejemplo anterior definido como plantilla Twig:
```
<!DOCTYPE html>
    <html>
        <head>
        <title>¡Bienvenido a Symfony!</title>
        </head>
        <body>
        <h1>{{ page_title }}</h1>
 
        <ul id="navigation">
            {% for item in navigation %}
                <li><a href="{{ item.href }}">{{ item.caption }}</a></li>
            {% endfor %}
        </ul>
    </body>
</html>
```
La sintaxis de Twig se basa en dos etiquetas especiales:
* `{{ ... }}`: sirve para mostrar el contenido de una variable o el resultado de realizar alguna operación o procesar alguna expresión. En PHP la construcción equivalente es `echo` o `print`.
* `{% ... %}`: sirve para definir la lógica de la plantilla, es decir, la parte de programación que controla cómo se muestran los contenidos de la plantilla. Entre otros, esta etiqueta se emplea para las instrucciones `if` y para los bucles `for`.

**Nota:**
Twig define una tercera etiqueta para crear comentarios: `{# esto es un comentario #}`. Su sintaxis es equivalente a la de PHP (`/* comentario */`) por lo que puedes utilizarla para crear comentarios de varias líneas.

Twig también define **filtros** y **etiquetas**, que modifican los contenidos antes de mostrarlos. El siguiente ejemplo convierte a mayúsculas la variable title antes de mostrarla:
```
{{ title|upper }}
```
También incluye varias funciones e incluso te permite definir tus propias funciones. En el siguiente ejemplo, la etiqueta `for` utiliza la función `cycle` para imprimir diez etiquetas `<div>` cuyas clases CSS alternan entre los valores `par` e `impar`:
```
{% for i in 0..10 %}
    <div class="{{ cycle(['odd', 'even'], i) }}">
        {# ... #}
    </div>
{% endfor %}
```

### ¿Por qué Twig?

Las plantillas Twig están pensadas para que sean sencillas y por eso no permiten incluir código PHP. Esta limitación se ha añadido a propósito, ya que las plantillas sólo deberían encargarse de mostrar información, no de programar parte de la aplicación.

Twig también es capaz de hacer cosas que PHP no puede, como controlar los espacios en blanco generados por el código, renderizar las plantillas dentro de un entorno de ejecución seguro y controlado (llamado *sandbox*) y la aplicación automática del mecanismo de escape. Twig incluye muchas pequeñas funcionalidades que facilitan la creación de plantillas muy concisas. Considera por ejemplo el siguiente ejemplo, que combina un bucle `for` con una condición `if`:
```
   <ul>
      {% for user in users if user.active %}
          <li>{{ user.username }}</li>
      {% else %}
          <li>No existe ningún usuario.</li>
      {% endfor %}
  </ul>
```
