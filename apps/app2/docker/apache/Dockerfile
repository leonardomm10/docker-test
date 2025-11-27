FROM httpd:2.4

RUN sed -i \
    # -e 's/^#\(Include .*httpd-vhosts.conf\)/\1/' \
    -e 's/^#\(LoadModule .*mod_proxy.so\)/\1/' \
    -e 's/^#\(LoadModule .*mod_rewrite.so\)/\1/' \
    -e 's/^#\(LoadModule .*mod_proxy_fcgi.so\)/\1/' \
    -e 's/DirectoryIndex index.html/DirectoryIndex index.html index.php/g' \
    conf/httpd.conf

RUN apt-get update && apt-get install -y \
    wget \
    vim \
    unzip \
    htop

RUN mkdir -p /usr/local/apache2/conf/vhosts
RUN echo "Include conf/vhosts" >> /usr/local/apache2/conf/httpd.conf
COPY ./vhost.conf /usr/local/apache2/conf/vhosts

EXPOSE 80