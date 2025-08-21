#!/bin/bash

# Descargar XAMPP Lite
wget https://www.apachefriends.org/xampp-files/8.2.12/xampp-linux-x64-8.2.12-0-installer.run

# Dar permisos de ejecución
chmod +x xampp-linux-x64-8.2.12-0-installer.run

# Ejecutar el instalador
sudo ./xampp-linux-x64-8.2.12-0-installer.run

# Iniciar el servicio XAMPP
sudo /opt/lampp/lampp start

# Habilitar el inicio automático de XAMPP
sudo /opt/lampp/lampp start

# Crear un script para iniciar XAMPP al inicio del sistema
sudo nano /etc/systemd/system/xampp.service

# Añadir el siguiente contenido al archivo xampp.service:
# [Unit]
# Description=XAMPP
# After=network.target

# [Service]
# ExecStart=/opt/lampp/lampp start
# ExecStop=/opt/lampp/lampp stop
# Type=forking

# [Install]
# WantedBy=multi-user.target

# Guardar y cerrar el archivo

# Habilitar el servicio XAMPP
sudo systemctl enable xampp.service

# Reiniciar el sistema para aplicar los cambios
sudo reboot

# Verificar que XAMPP se está ejecutando
sudo /opt/lampp/lampp status

echo "XAMPP Lite instalado correctamente en /opt/lampp"
