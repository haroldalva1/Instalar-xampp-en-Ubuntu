#!/bin/bash

# Actualizar el sistema
echo "Actualizando el sistema..."
sudo apt update && sudo apt upgrade -y

# Descargar XAMPP (versión 8.2.4)
echo "Descargando XAMPP..."
wget https://www.apachefriends.org/xampp-files/8.2.4/xampp-linux-x64-8.2.4-0-installer.run

# Hacer el instalador ejecutable
echo "Haciendo el instalador ejecutable..."
chmod +x xampp-linux-x64-8.2.4-0-installer.run

# Instalar XAMPP
echo "Instalando XAMPP..."
sudo ./xampp-linux-x64-8.2.4-0-installer.run

# Iniciar XAMPP
echo "Iniciando XAMPP..."
sudo /opt/lampp/lampp start

# Configurar inicio automático de XAMPP
echo "Configurando inicio automático de XAMPP..."

# Crear un archivo de servicio para systemd
sudo bash -c 'cat > /etc/systemd/system/xampp.service <<EOF
[Unit]
Description=XAMPP
After=network.target

[Service]
ExecStart=/opt/lampp/lampp start
ExecStop=/opt/lampp/lampp stop
Type=forking
Restart=always

[Install]
WantedBy=multi-user.target
EOF'

# Habilitar y iniciar el servicio
sudo systemctl daemon-reload
sudo systemctl enable xampp
sudo systemctl start xampp

# Mensaje final
echo "¡XAMPP ha sido instalado, iniciado y configurado para inicio automático!"
echo "Accede al panel de control en: http://tu-direccion-ip/xampp/"
