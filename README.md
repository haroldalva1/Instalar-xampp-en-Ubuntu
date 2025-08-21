#!/bin/bash

# 1. Descargar XAMPP
echo "Descargando XAMPP..."
wget https://www.apachefriends.org/xampp-files/8.2.4/xampp-linux-x64-8.2.4-0-installer.run
#Nota: remplaza la versión con la ultima version en la pagina oficial apachefriends.org

# 2. Hacer el instalador ejecutable
echo "Haciendo el instalador ejecutable..."
chmod +x xampp-linux-x64-8.2.4-0-installer.run

# 3. Ejecutar el instalador
echo "Ejecutando el instalador..."
sudo ./xampp-linux-x64-8.2.4-0-installer.run

# 4. Iniciar XAMPP
echo "Iniciando XAMPP..."
sudo /opt/lampp/lampp start

# 5. Habilitar el inicio automático (usando systemd)
echo "Habilitando el inicio automático..."

# Crear un archivo de servicio systemd
cat <<EOF | sudo tee /etc/systemd/system/xampp.service
[Unit]
Description=XAMPP
After=network.target

[Service]
Type=forking
PIDFile=/opt/lampp/var/run/lampp.pid
ExecStart=/opt/lampp/lampp start
ExecStop=/opt/lampp/lampp stop

[Install]
WantedBy=multi-user.target
EOF

# Recargar systemd
sudo systemctl daemon-reload

# Habilitar el servicio
sudo systemctl enable xampp.service

echo "¡XAMPP instalado, iniciado y configurado para inicio automático!"
