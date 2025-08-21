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

# 4. Encontrar la ruta de instalación de XAMPP
echo "Buscando la ruta de instalación de XAMPP..."
XAMPP_PATH=$(find / -name lampp -type d 2>/dev/null | head -n 1)

if [ -z "$XAMPP_PATH" ]; then
  echo "Error: No se pudo encontrar la instalación de XAMPP.  Asegúrate de que XAMPP esté instalado."
  exit 1
fi

echo "XAMPP encontrado en: $XAMPP_PATH"

# 5. Iniciar XAMPP
echo "Iniciando XAMPP..."
sudo "$XAMPP_PATH/lampp" start

# 6. Habilitar el inicio automático (usando systemd)
echo "Habilitando el inicio automático..."

# Crear un archivo de servicio systemd con la ruta correcta
cat <<EOF | sudo tee /etc/systemd/system/xampp.service
[Unit]
Description=XAMPP
After=network.target

[Service]
Type=forking
PIDFile=$XAMPP_PATH/var/run/lampp.pid
ExecStart=$XAMPP_PATH/lampp start
ExecStop=$XAMPP_PATH/lampp stop

[Install]
WantedBy=multi-user.target
EOF

# Recargar systemd
sudo systemctl daemon-reload

# Habilitar el servicio
sudo systemctl enable xampp.service

echo "¡XAMPP instalado, iniciado y configurado para inicio automático!"
