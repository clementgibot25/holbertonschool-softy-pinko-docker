# Étape 1 : utiliser nginx latest comme base
FROM nginx:latest

# Étape 2 : copier les fichiers front-end dans /var/www/html/softy-pinko-front-end
COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end

# Étape 3 : copier le fichier de configuration nginx
COPY ./softy-pinko-front-end.conf /etc/nginx/conf.d/default.conf