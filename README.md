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

# Mensaje final
echo "¡XAMPP ha sido instalado y está en ejecución!"
echo "Accede al panel de control en: http://tu-direccion-ip/xampp/"
