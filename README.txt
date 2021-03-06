Cosas destacadas que podría configurar en Maven:
  X- Añadir el MANIFEST a mi jar
         Antes de usar, actualizo la Main-Class en pom.xml 
  X- Uso del plugin assembly:single, para empaquetar mis proyectos
         $mvn package assembly:single
  X- Firmar un jar con jarsigner (Hecho, contraseña del almacén de claves: si...h, contraseña de la clave: si...h)
         Crear la keystore con el comando: $keytool -genkey -v -keystore developer.keystore -alias developer -keyalg RSA -keysize 2048 -validity 10000 
         $mvn package jarsigner:sign (hay que poner el elemento <keypass> en el pom.xml)
         OJO! no usar maven para firmar, ya que hay que poner la clave del keystore en el pom.xml, y no es seguro. Para firmar un fichero, usar:
         $jarsigner -keystore /Java/work/workspace_maven/developer.keystore /Java/work/workspace_maven/maven_git/target/pruebas_maven-1.0-SNAPSHOT.jar developer
         OJO! no pasa nada si firmo un fichero dos veces
  - Hacer el release de un proyecto
  X- Uso de un SCM (git) 
         $mvn scm:checkin para commits y $mvn scm:update para pulls, scm:diff para comparar versiones
  - Hacer un empaquetado .apk (ver un proyecto de $mvn archetype:generate)
  - Hacer un empaquetado .ear, .war (ver un proyecto de archetype)
  - Hacer un empaquetado .app (ver un proyecto de archetype)
  - Mavenizar mis proyectos
  - Creación de nuevos proyectos a partir de archetype

Coordenadas de un artifact:
  groupId:artifactId:packaging:version:scope

Usando archetype para crear una plantilla para mi proyecto:
  $mvn archetype:generate
  $mvn archetype:create-from-project     # para crear una nueva plantilla a partir de mi proyecto
  $mvn archetype:generate -DarchetypeCatalog=<catalog>
    http://repo.fusesource.com/maven2
    http://cocoon.apache.org
    http://download.java.net/maven/2
    http://myfaces.apache.org
    http://tapestry.formos.com/maven-repository
    http://scala-tools.org
    http://www.terracotta.org/download/reflector/maven2/

Obtener ayuda y ver las fases del lifecycle y los goals asociados:
  $mvn help:help
  $mvn help:describe -Dcmd=validate
  $mvn help:describe -Dplugin=org.apache.maven.plugins:maven-compile-plugin

Cuando ejecutamos un goal específico, sólo ese goal es ejecutado
  $mvn compile:compile jar:jar
Cuando ejecutamos una fase, todas las fases anteriores y todos goals de los plugins:
  $mvn deploy

Ver dependencias
  $mvn dependency:[tree,resolve,resolve-plugins,analyze]
Online y offline:
  $mvn dependency:go-offline
Forzar ejecución offline:
  $mvn <phase or goal> -o

Plugins importantes:
  Listado completo: http://maven.apache.org/plugins/index.html
  assembly: crea ZIPs y otros paquetes de distribución de aplicaciones y sus dependencias JAR transitivas

Usando el SCM plugin:
  Las dos operaciones más habituales con SCM son checkin (git commit) y update (git pull)
  $mvn scm:diff
  $mvn scm:update-subprojects    # para actualizar sub-repositorios (git pull)

Haciendo el release del proyecto:
  $mvn release:prepare
  $mvn release:perform

DEBUG DE MAVEN: se puede usar con Eclipse
--------------
  $mvnDebug <goal>      # debug de un goal
  $mvn test -Dmaven.surefire.debug           # debug de un Unit test

CONFIGURACIÓN DE POM.XML
------------------------
El pom.xml más básico (o casi), está copiado en este directorio

Scopes de una dependencia: compile (hace falta para la compilación), test, provided (lo proporciona el container), system, import (me genera un artifact de tipo POM que facilita las dependencias)

Ver el super pom:
  $mvn help:effective-pom

Configuración de plugins: se hace dentro de <build><plugins><plugin>...<configuration> (hay que mirar los parámetros en la web de cada plugin)

Configuración de properties:
  <project>
    <properties>
      <mi.property>propiedad</mi.property>
    </properties>
  </project>

Configuración de SCM:
  <scm>
    <connection>scm://svn:http://myvendor.com/ourrepo/trunk</connection>
    <developerConnection>
      scm:svn:https://myvendor.com/ourrepo/trunk
    </developerConnection>
    <url>http://myvendor.com/viewsource.pl</url>
  </scm>
  Para referencia de los SCM's que podemos configurar: http://docs.codehaus.org/display/SCM/SCM+Matrix

Módulos. Dento de <project>, para construir sub-proyectos que estén en subcarpetas:
  <modules>
    <module>servlets</module>
    <module>ejbs</module>
    <module>ear</module>
  </modules>

Profiles (http://maven.apache.org/guides/introduction/introduction-to-profiles.html)
  <profiles>
    <profile>
      <id>MiProfile</id>
      ...
    </profile>
  </profiles>



