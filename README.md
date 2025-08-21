#!/bin/bash

# URL del archivo de instalación de XAMPP
XAMPP_URL="https://sourceforge.net/projects/xampp/files/XAMPP%20Linux/8.1.25/xampp-linux-x64-8.1.25-0-installer.run"
XAMPP_FILE="xampp-linux-x64-8.1.25-0-installer.run"

# Descargar XAMPP Lite
echo "Intentando descargar XAMPP desde $XAMPP_URL"
wget "$XAMPP_URL"
if [ $? -ne 0 ]; then
  echo "Error: No se pudo descargar XAMPP. Verifica la URL y tu conexión a Internet."
  exit 1
fi

# Dar permisos de ejecución
echo "Dando permisos de ejecución a $XAMPP_FILE"
chmod +x "$XAMPP_FILE"
if [ $? -ne 0 ]; then
  echo "Error: No se pudieron dar permisos de ejecución al archivo."
  exit 1
fi

# Ejecutar el instalador
echo "Ejecutando el instalador..."
sudo ./"$XAMPP_FILE"
if [ $? -ne 0 ]; then
  echo "Error: Falló la ejecución del instalador. Revisa los mensajes de error."
  exit 1
fi

# Iniciar el servicio XAMPP
echo "Iniciando el servicio XAMPP..."
sudo /opt/lampp/lampp start
if [ $? -ne 0 ]; then
  echo "Error: No se pudo iniciar XAMPP.  Verifica la instalación."
  exit 1
fi

# Habilitar el inicio automático de XAMPP
echo "Configurando el inicio automático..."
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
if [ $? -ne 0 ]; then
    echo "Error al habilitar el servicio XAMPP."
    exit 1
fi
# Reiniciar el sistema para aplicar los cambios
echo "Reiniciando el sistema..."
sudo reboot

# Verificar que XAMPP se está ejecutando
echo "Verificando el estado de XAMPP..."
sudo /opt/lampp/lampp status

echo "XAMPP Lite instalado correctamente en /opt/lampp con puertos predeterminados (80 y 3306)"
