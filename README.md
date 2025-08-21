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

# Verificar si XAMPP está instalado
if [ ! -f "/opt/lampp/lampp" ]; then
  echo "Error: XAMPP no se instaló correctamente."
  exit 1
fi

# Dar permisos de ejecución a lampp
echo "Asignando permisos de ejecución a lampp..."
sudo chmod +x /opt/lampp/lampp

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
Type=forking
ExecStart=/opt/lampp/lampp start
ExecStop=/opt/lampp/lampp stop
User=root
Group=root
Restart=on-failure

[Install]
WantedBy=multi-user.target
EOF'

# Recargar systemd y habilitar el servicio
sudo systemctl daemon-reload
sudo systemctl enable xampp
sudo systemctl start xampp

# Verificar el estado del servicio
echo "Verificando el estado del servicio..."
systemctl status xampp.service

# Mensaje final
echo "¡XAMPP ha sido instalado, iniciado y configurado para inicio automático!"
echo "Accede al panel de control en: http://tu-direccion-ip/xampp/"
