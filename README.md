# Twig

*Twig* es un motor de creación de plantillas para utilizar con PHP

### Requisitos previos
*Twig* necesita por lo menos **PHP 5.2.4** para funcionar.

### Instalando vía Composer (recomendado)

1. Instala [Composer](https://getcomposer.org/download/) en tu proyecto:
```
curl -s http://getcomposer.org/installer | php
```

2. Crea un archivo ```composer.json``` en el directorio raíz de tu proyecto:
```
{
    "require": {
        "twig/twig": "1.*"
    }
}
```
3. Instala vía composer
```
php composer.phar install
```
Más sobre [Composer](https://getcomposer.org/doc/)

### Instalando desde la versión del ```tarball```

1. Descargar el ```tarball``` más reciente de la [página de descargar](https://github.com/twigphp/Twig/tags)
2. Verificar la integridad del ```tarball``` (http://fabien.potencier.org/article/73/signing-project-releases)
3. Descomprime el archivo
4. Mueve los archivos a algún lugar en tu proyecto

### Instalando la versión de desarrollo
```
git clone git://github.com/fabpot/Twig.git
```

### Instalando la extensión C

*Twig* viene con una extensión C que mejora el rendimiento del motor *Twig* en tiempo de ejecución. La puedes instalar como cualquier otra extensión de *PHP*:
```
$ cd ext/twig
$ phpize
$ ./configure
$ make
$ make install
```

Por último, activa la extensión en tu archivo de configuración ```php.ini```:
```
extension=twig.so
```
Y a partir de ahora, *Twig* compilará automáticamente tus plantillas para tomar ventaja de la extensión C. Ten en cuenta que esta extensión no sustituye el código *PHP*, solamente proporciona una versión optimizada del método ```Twig_Template::getAttribute()```.


































































































































































### Uso básico de la *API*

Esta sección te ofrece una breve introducción a la *API PHP* de *Twig*.
```
require_once '/path/to/vendor/autoload.php';

$loader = new Twig_Loader_Array(array(
    'index' => 'Hello {{ name }}!',
));
$twig = new Twig_Environment($loader);

echo $twig->render('index', array('name' => 'Fabien'));
```
*Twig* utiliza un cargador (```Twig_Loader_Array```) para buscar las plantillas, y un entorno (```Twig_Environment```) para almacenar la configuración.

El método ```render()``` carga la plantilla pasada como primer argumento y la reproduce con las variables pasadas como segundo argumento.

Debido a que las plantillas generalmente se guardan en el sistema de archivos, *Twig* también viene con un cargador del sistema de archivos:
```
$loader = new Twig_Loader_Filesystem('/path/to/templates');
$twig = new Twig_Environment($loader, array(
    'cache' => '/path/to/compilation_cache',
));

echo $twig->render('index.html', array('name' => 'Fabien'));
```
**Nota:** Si no estás usando ```Composer```, usa el cargador automático de *Twig*:
```
require_once '/path/to/lib/Twig/Autoloader.php';
Twig_Autoloader::register();
```
Sustituye /path/to/lib/ con la ruta que utilizaste en la instalación de *Twig*.

[Aquí](http://gitnacho.github.io/Twig/api.html) puedes encontrar más información sobre el API para *Twig*.

### Referencias

* [Página oficial de Twig](http://twig.sensiolabs.org/)
* [Documentación de Twig en español](http://gitnacho.github.io/Twig/index.html)
* [Plantillas](http://librosweb.es/libro/symfony_2_x/capitulo_7/plantillas.html)

### Ejemplo

* [Twig, plantillas para PHP Parte I](http://jhernandz.es/noticia/twig-plantillas-php-i)
* [Twig, plantillas para PHP Parte II](http://jhernandz.es/noticia/twig-plantillas-php-ii)
