# Request-Entity-Too-Large-Problem-Solved

````
[program:gunicorn8000]

directory=/home/ubuntu/Crunchscaled_linux
command=/home/ubuntu/myenv/bin/gunicorn --workers 4 --timeout 600 --limit-request-line 16384 --limit-request-fields 65536 --limit-request-field_size 32768 --bind 0.0.0.0:8000 Crunchscaled.wsgi:application
autostart=true
autorestart=true
stderr_logfile=/var/log/gunicorn/gunicorn8000.err.log
stdout_logfile=/var/log/gunicorn/gunicorn8000.out.log


[program:gunicorn9000]

directory=/home/ubuntu/Crunchscaled_linux
command=/home/ubuntu/myenv/bin/gunicorn --workers 4 --timeout 600 --limit-request-line 16384 --limit-request-fields 65536 --limit-request-field_size 32768 --bind 0.0.0.0:9000 Crunchscaled.wsgi:application
autostart=true
autorestart=true
stderr_logfile=/var/log/gunicorn/gunicorn9000.err.log
stdout_logfile=/var/log/gunicorn/gunicorn9000.out.log


[group:guni]
programs: gunicorn8000, gunicorn9000
````



# add this on nginx
````
server {
  server_name www.admin.krunchiq.com admin.krunchiq.com;
  
  location / {
  include proxy_params;
  #proxy_pass http://unix:/home/ubuntu/Crunchscaled_linux/app.sock;
  proxy_pass http://127.0.0.1:8000;
  
  client_max_body_size 100M;  # Increase the limit to 100MB
  }
}
````



