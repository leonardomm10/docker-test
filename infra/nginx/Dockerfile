FROM nginx:latest

# Add config padrão
# RUN rm -f /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/nginx.conf

# Copiar vhosts
COPY vhosts/ /etc/nginx/conf.d/

# Expor portas padrão
EXPOSE 80 443