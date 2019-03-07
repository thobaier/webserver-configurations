# Apache mod_rewrite collection
#### Set rewrite base (for e.g. subdirectory as base)
```
RewriteBase /
```
#### Force https
```
RewriteCond %{HTTPS} off
RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```
#### Force www
```
RewriteCond %{HTTP_HOST} !^www\.
RewriteRule .* https://www.%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```
#### Remove trailing slash
```
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)/$ /$1 [L,R=301]
```
#### Redirect all non physical matches to front-controller
```
RewriteCond %{REQUEST_URI} !^/index\.php
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule .* index.php [L]
```
#### Pseudo prefixes (for e.g. languages)
```
RewriteRule ^(?:en|de)/source/(.*)$  /source/$1 [L,QSA]
```
#### Serve content from other source
```
RewriteRule ^(?:en|de)/(?:this|that)/css/(.*)((.*).(css))$  /target/$1 [L,QSA]
```
#### Rewrite SEF request (and match a pattern) to file with arguments
```
RewriteRule ^sef/([a-zA-Z0-9]+)$ /old-folder/file.php?arg=$1 [L,QSA]
# additionally rewrite requests for old url to SEF
RewriteCond %{THE_REQUEST} /old-folder/file\.php\?arg=([a-zA-Z0-9]+) [NC]
RewriteRule ^ /sef/%1? [R=301,L]
```
#### Rewrite URIS containing a query string
```
RewriteCond %{REQUEST_URI} ^/folder/sub-folder$ [NC]
RewriteCond %{QUERY_STRING} argument=0&sort=asc [OR]
RewriteCond %{QUERY_STRING} argument=0&sort=desc [OR]
RewriteRule ^(.*)$ /target? [R=301,L]
```
