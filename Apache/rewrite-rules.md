# Apache mod_rewrite collection
#### Set rewrite base (for e.g. subdirectory as base)
```
RewriteBase /
```
#### Force https
```
RewriteEngine on
RewriteCond %{HTTPS} off
RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```
#### Force www
```
RewriteEngine on
RewriteCond %{HTTP_HOST} !^www\.
RewriteRule .* https://www.%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```
#### Remove www
```
RewriteEngine on
RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
RewriteRule ^(.*)$ https://%1%{REQUEST_URI} [R=301,QSA,NC,L]
```
#### Remove trailing slash
```
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)/$ /$1 [L,R=301]
```
#### Redirect all non physical matches to front-controller
```
RewriteEngine on
RewriteCond %{REQUEST_URI} !^/index\.php
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule .* index.php [L]
```
#### Pseudo prefixes (for e.g. languages)
```
RewriteEngine on
RewriteRule ^(?:en|de)/source/(.*)$  /source/$1 [L,QSA]
```
#### Serve content from other source
```
RewriteEngine on
RewriteRule ^(?:en|de)/(?:this|that)/css/(.*)((.*).(css))$  /target/$1 [L,QSA]
```
#### Rewrite SEF request (and match a pattern) to file with arguments
```
RewriteEngine on
RewriteRule ^sef/([a-zA-Z0-9]+)$ /old-folder/file.php?arg=$1 [L,QSA]
# additionally rewrite requests for old url to SEF
RewriteCond %{THE_REQUEST} /old-folder/file\.php\?arg=([a-zA-Z0-9]+) [NC]
RewriteRule ^ /sef/%1? [R=301,L]
```
#### Rewrite URIS containing a query string
```
RewriteEngine on
RewriteCond %{REQUEST_URI} ^/folder/sub-folder$ [NC]
RewriteCond %{QUERY_STRING} argument=0&sort=asc [OR]
RewriteCond %{QUERY_STRING} argument=0&sort=desc [OR]
RewriteRule ^(.*)$ /target? [R=301,L]
```
