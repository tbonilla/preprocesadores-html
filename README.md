# Twig

### Requisitos previos
*Twig* necesita por lo menos PHP 5.2.4 para funcionar.

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

*Twig* viene con una extensión C que mejora el rendimiento del motor Twig en tiempo de ejecución. La puedes instalar como cualquier otra extensión de PHP:

```
$ cd ext/twig
$ phpize
$ ./configure
$ make
$ make install
```

Por último, activa la extensión en tu archivo de configuración php.ini:

```
extension=twig.so
```

Y a partir de ahora, Twig compilará automáticamente tus plantillas para tomar ventaja de la extensión C. Ten en cuenta que esta extensión no sustituye el código PHP, solamente proporciona una versión optimizada del método ```Twig_Template::getAttribute()```.
