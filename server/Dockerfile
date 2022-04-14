FROM nginx:1.20.2-alpine

COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY ./keys/serverA.crt /etc/ssl/serverA.crt
COPY ./keys/serverA.key /etc/ssl/serverA.key

# Append list of client CA for mutual SSL
COPY ./keys/allClientCA.pem /etc/ssl/allClientCA.pem

# Allowing nginx user to run nginx on <1024 ports
RUN apk add libcap && setcap 'cap_net_bind_service=+ep' /usr/sbin/nginx

# Change ownership of nginx directories to default nginx user.
RUN chown -R nginx:nginx /var/cache/nginx && \
        chown -R nginx:nginx /var/log/nginx && \
        chown -R nginx:nginx /etc/nginx/conf.d
RUN touch /var/run/nginx.pid && \
        chown -R nginx:nginx /var/run/nginx.pid

# Switch to non-root user
USER nginx

EXPOSE 443
CMD ["nginx", "-g", "daemon off;"]