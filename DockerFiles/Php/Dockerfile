# PHP5 in longer support on newer apline linux but on 3.8
FROM alpine:3.8
LABEL author="Leroy Leow"
LABEL description="Specify php version using ENV"
ENV PHPVERSION=7
RUN apk add --update apache2 php${PHPVERSION}-apache2 php${PHPVERSION}
RUN rm -rf /var/cache/apk/*
RUN mkdir /run/apache2/
RUN rm -rf /var/www/localhost/htdocs/index.html
RUN echo "<?php phpinfo(); ?>" > /var/www/localhost/htdocs/index.php
RUN chmod 755 /var/www/localhost/htdocs/index.php
EXPOSE 80/tcp
ENTRYPOINT ["httpd"]
CMD ["-D","FOREGROUND"]