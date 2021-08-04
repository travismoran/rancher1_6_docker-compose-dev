FROM nginx:stable-alpine
COPY ./conf/rancher.conf /etc/nginx/conf.d/rancher.conf
RUN sed -i "s/FQDN/YOURURLHERE/g" /etc/nginx/conf.d/rancher.conf
