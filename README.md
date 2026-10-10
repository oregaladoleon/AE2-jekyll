# AE2-jekyll
> Autor: Óscar Regalado León  
> Fecha: Octubre 2026 

El objeto de este repositorio es desarrollar un site/ a través de **Jekyll**, emplearemos el contenedor docker laboratorio del módulo **dpl-lab** para lanzar a través de **nginx** el _site/.

## Índice

- [Entorno e instalación](#entorno-e-instalacion)
- [Configuración](#configuracion)
- [Comprobación](#comprobacion)
- [Problemas encontrados y solución](#problemas-encontrados-y-solucion)
- [Repositorio remoto](#repositorio-remoto)

## Entorno e instalación
Creamos el respoitorio:
~~~bash
gh repo create AE2-jekyll --public --add-readme --license mit
~~~
Clonamos el repositorio de github en local en el directorio ~/dpl/AE2-jekyll/:
~~~bash
git clone git@github.com:oregaladoleon/AE2-jekyll.git AE2-jekyll
~~~
Creamos el **.gitignore** para un proyecto jekyll:
~~~bash
touch .gitignore

nano .gitignore 
~~~
El contenido del **.gitignore** es:
~~~
# 1. Archivos generados por Jekyll (BUILD & CACHE)
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata

# 2. Gestión de dependencias (Ruby & Bundler)
.bundle/
vendor/

# 3. Archivos del sistema y de entorno
.DS_Store
*.log
*.swp
.env
~~~
---
Instalamos los prerequisitos de **jekyll**:
~~~bash
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
gem install jekyll bundler
~~~
---
Creamos el entorno con **Gemfile** y editamos el fichero 
~~~bash
bundle init 
nano Gemfile
~~~
Estructura propuesta del fichero **Gemfile**
~~~
source "https://rubygems.org"

# Gema principal de Jekyll
gem "jekyll", "~> 4.3"

# Tema por defecto para la estructura visual inicial
gem "minima", "~> 2.5"

# Plugins recomendados para rendimiento y SEO
group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
end
~~~
Ejecutamos el instalador de **bundler**
~~~bash
bundle install
~~~ 
Creamos el archivo de configuración **_config.yml**
~~~bash
nano _config.yml
~~~
---

Contenido del fichero
~~~
title: Blog de Oscar Regalado
email: oregaladoleon@gmail.com
description: "Despliegue con Jekyll"
baseurl: "" # Déjalo vacío si serviremos en la raíz de /ae2
url: "http://localhost:8080"

theme: minima
plugins:
  - jekyll-feed
  - jekyll-seo-tag

# Excluir archivos innecesarios al compilar
exclude:
  - Gemfile
  - Gemfile.lock
  - README.md
~~~
## Configuración
Creamos el directorio **_post** con contenido:
~~~bash
mkdir _posts
touch _posts/2026-10-09-despliegue-jekyll-docker.md
nano _posts/2026-10-09-despliegue-jekyll-docker.md
~~~
Procedemos a compilar para verificar que todo funciona correctamente:
~~~bash
bundle exec jekyll build
~~~
Observamos que se ha creado la carpeta **_site** con todo el contenido compilado.

Procedemos a copiar los archivos en el contendor Docker a nginx
~~~bash
sudo docker cp _site/. dpl-lab:/var/www/html/AE2-jekyll
~~~
Configuramos el fichero original de la ae2 de la práctica con zénsical para que nginx pueda leer la ruta del blog con jekyll con otro bloque server
~~~bash

~~~
nano /etc/nginx/sites-available/ae2
~~~
# Server 1: Zensical (Escucha en el puerto 80 interno)
server {
    listen 80;
    server_name localhost;

    root /var/www/html/ae2;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# Server 2: Jekyll Blog (Escucha en el puerto 8000 interno)
server {
    listen 8000;
    server_name localhost;

    root /var/www/html/AE2-jekyll;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
~~~
## Problemas encontrados y solución
No aplica en este documento.
## Repositorio remoto
Enlace público: https://github.com/oregaladoleon/AE2-jekyll.git


