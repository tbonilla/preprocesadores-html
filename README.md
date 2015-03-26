# Twig

### Requisitos previos
*Twig* necesita por lo menos **PHP 5.2.4** para funcionar.

### Instalar vía Composer (recomendado)

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
require_once '/ruta/a/vendor/autoload.php';

$loader = new Twig_Loader_String();
$twig = new Twig_Environment($loader);

echo $twig->render('Hello {{ name }}!', array('name' => 'Fabien'));
```
*Twig* utiliza un cargador (```Twig_Loader_String```) para buscar las plantillas, y un entorno (```Twig_Environment```) para almacenar la configuración.

El método ```render()``` carga la plantilla pasada como primer argumento y la reproduce con las variables pasadas como segundo argumento.

Debido a que las plantillas generalmente se guardan en el sistema de archivos, *Twig* también viene con un cargador del sistema de archivos:
```
$loader = new Twig_Loader_Filesystem('/ruta/a/templates');
$twig = new Twig_Environment($loader, array(
    'cache' => '/ruta/a/compilation_cache',
));

echo $twig->render('index.html', array('name' => 'Fabien'));
```
Si no estás usando ```Composer```, usa el cargador automático de *Twig*:
```
require_once '/path/to/lib/Twig/Autoloader.php';
Twig_Autoloader::register();
```
Sustituye /path/to/lib/ con la ruta que utilizaste en la instalación de *Twig*.
