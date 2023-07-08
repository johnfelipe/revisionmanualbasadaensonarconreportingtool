# Imagen SonarQube del CNES \[servidor\]

![](https://github.com/cnescatlab/sonarqube/workflows/CI/badge.svg)
![](https://github.com/cnescatlab/sonarqube/workflows/CD/badge.svg)
[![Insignia de Codacy](https://app.codacy.com/project/badge/Grade/2a4a53f54ae94bd69d66a7690b95612f)](https://www.codacy.com/gh/cnescatlab/sonarqube?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=lequal/sonarqube&amp;utm_campaign=Badge_Grade)

> Imagen Docker para SonarQube con plugins preconfigurados y ajustes del CNES dedicados a la integración continua.

Esta imagen es una imagen de servidor SonarQube preconfigurada derivada de [Docker-CAT](https://github.com/cnescatlab/docker-cat). Contiene los mismos plugins y las mismas reglas para el análisis de código. Está basado en la versión LTS de SonarQube.

SonarQube en sí es un proyecto de código abierto en GitHub: [SonarSource/sonarqube](https://github.com/SonarSource/sonarqube).

Para versiones y registro de cambios: [Versiones de GitHub](https://github.com/cnescatlab/sonarqube/releases).

## Funciones

Esta imagen se basa en la imagen oficial de SonarQube LTS, a saber, [sonarqube: 8.9.6-community](https://hub.docker.com/_/sonarqube), y ofrece características adicionales.

Las características adicionales son:

* Modificación obligatoria de la contraseña de administrador predeterminada para ejecutar un contenedor.
* Comprobación del estado del contenedor.
* Más plugins (ver [la lista](#sonarqube-plugins-included))
* Configuración CNES
    * Reglas Java del CNES
    * Perfiles de calidad CNES para Java, Python, C, C++ y VHDL
    * CNES Quality Gate (establecido como predeterminado)

_Esta imagen está hecha para ser utilizada junto con una imagen de sonar-escáner preconfigurada que incorpora todas las herramientas necesarias: [cnescatlab/sonar-scanner](https://github.com/cnescatlab/sonar-scanner). Sin embargo, no es obligatorio usarlo._

## Guía del desarrollador

### Cómo construir la imagen

Es una imagen docker normal. Por lo tanto, se puede construir con los siguientes comandos.

```sh
# from the root of the project
$ docker build -t lequal/sonarqube .
```

Para luego ejecutar un contenedor con esta imagen, consulte la guía del [usuario](#user-guide).

Para ejecutar las pruebas y crear las suyas propias, consulte la documentación de la [prueba](https://github.com/cnescatlab/sonarqube/tree/develop/tests).

## Guía del usuario

Esta imagen está disponible en Docker Hub: [lequal/sonarqube](https://hub.docker.com/r/lequal/sonarqube/).

Desde su creación, esta imagen ha sido diseñada para ser utilizada en producción. Por lo tanto, dejar la contraseña de administrador predeterminada (es decir, "admin") nunca será una opción. En este sentido, se proporcionará una nueva contraseña para la cuenta de administrador estableciendo la variable de entorno `SONARQUBE_ADMIN_PASSWORD`.

:warning: :rotating_light: El contenedor no se ejecutará si `SONARQUBE_ADMIN_PASSWORD` está vacío o es igual a "admin".

Para ejecutar la imagen localmente:

```sh
# Recommended options
$ docker run --name lequalsonarqube \
             --rm \
             -p 9000:9000 \
             -e SONARQUBE_ADMIN_PASSWORD="admin password of your choice" \
             lequal/sonarqube:latest

# To stop (and remove) the container
Ctrl-C
# or
$ docker container stop lequalsonarqube
```

### Usar una base de datos externa

De forma predeterminada, SonarQube utiliza una base de datos integrada que se puede utilizar para pruebas, pero en producción el uso de una base de datos externa para la persistencia de datos es obligatorio. El `docker-compose.yml` archivo muestra un ejemplo de cómo configurar una base de datos postgres externa. Se puede ejecutar con:

```sh
$ docker-compose up -d

# To set variables when running the containers
$ LEQUAL_SONARQUBE_VERSION=1.0.0 POSTGRES_PASSWD=secret-passwd SONARQUBE_ADMIN_PASSWORD="a password" docker-compose up -d
```

Con una base de datos externa, los datos utilizados por SonarQube se almacenan fuera del contenedor. Significa que el contenedor puede detenerse, reiniciarse, retirarse y recrearse a voluntad.

## Plugins de SonarQube incluidos

| Plugin SonarQube                                  | Versión                  | URL                                                                        |
|---------------------------------------------------|--------------------------|----------------------------------------------------------------------------|
| Ansible Lint                                      | 2.5.1                    | https://github.com/sbaudoin/sonar-ansible/sonar-ansible-plugin             |
| Calidad y seguridad del código C#                      | 8.22 (compilación 31243)       | http://redirect.sonarsource.com/plugins/csharp.html                        |
| C++ (Comunidad)                                   | 2.0.7 (compilación 3119)       | https://github.com/SonarOpenCommunity/sonar-cxx/wiki                       |
| Calidad y seguridad del código CSS                     | 1.4.2 (compilación 2002)       | http://redirect.sonarsource.com/plugins/css.html                           |
| Estilo de comprobación                                        | 8.40                     | n/d                                                                        |
| Trébol                                            | 4.1                      | https://github.com/sfeir-open-source/sonar-clover                          |
| Cobertura                                         | 2.0                      | https://github.com/galexandre/sonar-cobertura                              |
| Complemento de sucursal comunitaria                           | 1.8.1                    | https://github.com/mc1arke/sonarqube-community-branch-plugin               |
| Métricas de FPGA                                      | 1.3.0                    | https://www.linty-services.com                                             |
| Findbugs                                          | 4.0.4                    | https://github.com/spotbugs/sonar-findbugs/                                |
| Calidad y seguridad del código flexible                    | 2.6.1 (compilación 2564)       | http://redirect.sonarsource.com/plugins/flex.html                          |
| Calidad y seguridad de Go Code                      | 1.8.3 (compilación 2219)       | http://redirect.sonarsource.com/plugins/go.html                            |
| Calidad y seguridad del código HTML                    | 3.4 (compilación 2754)         | http://redirect.sonarsource.com/plugins/web.html                           |
| JaCoCo                                            | 1.1.1 (compilación 1157)       | n/d                                                                        |
| Calidad y seguridad del código Java                    | 6.15.1 (compilación 26025)     | http://redirect.sonarsource.com/plugins/java.html                          |
| Calidad y seguridad del código JavaScript/TypeScript   | 7.4.4 (compilación 15624)      | http://redirect.sonarsource.com/plugins/javascript.html                    |
| Calidad y seguridad del código Kotlin                  | 1.8.3 (compilación 2219)       | http://redirect.sonarsource.com/plugins/kotlin.html                        |
| ModelSim                                          | 1.6.0                    | https://www.linty-services.com                                             |
| Calidad y seguridad del código PHP                     | 3.17.0.7439              | http://redirect.sonarsource.com/plugins/php.html                           |
| PMD                                               | 3.3.1                    | https://github.com/jensgerdes/sonar-pmd                                    |
| Calidad y seguridad del código Python                  | 3.4.1 (compilación 8066)       | http://redirect.sonarsource.com/plugins/python.html                        |
| Calidad y seguridad del código Ruby                    | 1.8.3 (compilación 2219)       | http://redirect.sonarsource.com/plugins/ruby.html                          |
| Calidad y seguridad del código de Scala                   | 1.8.3 (compilación 2219)       | http://redirect.sonarsource.com/plugins/scala.html                         |
| Analizador ShellCheck                               | 2.5.0                    | https://github.com/sbaudoin/sonar-shellcheck                               |
| Plugin Sonar Frama-C                              | 2.1.1                    | https://github.com/lequal/sonar-frama-c-plugin                             |
| Complemento Sonar i-Code CNES                          | 3.0.0                    | https://github.com/cnescatlab/sonar-icode-cnes-plugin                      |
| Informe SonarQube CNES                             | 4.1.3                    | https://github.com/cnescatlab/sonar-cnes-report                            |
| SonarTS                                           | 2.1 (compilación 4362)         | http://redirect.sonarsource.com/plugins/typescript.html                    |
| VB.NET Calidad y seguridad del código                  | 8.22 (compilación 31243)       | http://redirect.sonarsource.com/plugins/vbnet.html                         |
| VHDLRC                                            | 3.4                      | https://www.linty-services.com                                             |
| Calidad y seguridad del código XML                     | 2.2 (compilación 2973)         | http://redirect.sonarsource.com/plugins/xml.html                           |
| Analizador YAML                                     | 1.7.0                    | https://github.com/sbaudoin/sonar-yaml                                     |

Para actualizar esta lista, ejecute:
```sh                                                                   "
while IFS='|' read -r plugin version url
do
    if [ "$url" = "null" ]; then url="n/a"; fi
    printf "| %.50s| %.25s| %.75s|\n" "$plugin                                                  " "$version                         " "$url                                                                           "
done < <(curl -u MY_TOKEN: -s http://localhost:9000/api/plugins/installed | jq -r '.plugins[] | "\(.name)|\(.version)|\(.homepageUrl)"')

# With `MY_TOKEN` your SonarQube personal token.
```

### Información adicional para el complemento Community Branch

Se recomienda establecer la propiedad `sonar.core.serverBaseURL` en [/admin/settings](http://localhost:9000/admin/settings) para que los enlaces publicados en los comentarios de relaciones públicas y el correo funcionen.

## Cómo contribuir

Si experimentó un problema con la imagen, abra un problema. Dentro de este número, explíquenos cómo reproducir este problema y pegue el registro. 

Si desea hacer un PR, por favor ponga dentro de él el motivo de esta solicitud de extracción. Si esta solicitud de extracción soluciona un problema, inserte el número del problema o explique dentro del PR cómo reproducir este problema.

Todos los detalles están disponibles en [CONTRIBUTING.](https://github.com/cnescatlab/.github/blob/master/CONTRIBUTING.md)

Errores y solicitudes de funciones: [problemas](https://github.com/cnescatlab/sonarqube/issues)

Para contribuir al proyecto, lea [esto](https://github.com/cnescatlab/.github/wiki/CATLab's-Workflows) sobre los flujos de trabajo de CATLab para imágenes de Docker.

## Licencia

Licenciado bajo la [Licencia Pública General GNU, Versión 3.0](https://www.gnu.org/licenses/gpl.txt)

Este proyecto es software libre; puede redistribuirlo y/o modificarlo bajo los términos de la Licencia Pública General GNU publicada por la Free Software Foundation; ya sea la versión 3 de la Licencia o (a su elección) cualquier versión posterior.
