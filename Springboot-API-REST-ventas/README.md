# build contenedor

sudo docker build -t ventas-microservice:v1 .

# correr contenedor
sudo docker run -p 8080:8080 \
  -e DB_ENDPOINT=localhost \
  -e DB_PORT=3306 \
  -e DB_NAME=test_db \
  -e DB_USERNAME=root \
  -e DB_PASSWORD=root \
  ventas-microservice:v1
