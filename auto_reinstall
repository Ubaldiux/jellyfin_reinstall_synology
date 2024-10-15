#!/bin/bash

# Ruta del paquete Jellyfin .spk (ajusta según sea necesario)
SPK_PATH="/path/to/your/jellyfin.spk"

# Función para verificar el estado de Jellyfin
check_jellyfin_status() {
    status=$(synopkg status jellyfin | grep '"status":"running"')
    if [ -n "$status" ]; then
        echo "Jellyfin está ejecutándose."
        return 0
    else
        echo "Jellyfin está detenido."
        return 1
    fi
}

# Intentar detener Jellyfin
echo "Intentando detener Jellyfin..."
synopkg stop jellyfin
sleep 5

# Verificar si Jellyfin se detuvo
check_jellyfin_status
if [ $? -eq 0 ]; then
    echo "Error: No se pudo detener Jellyfin."
    exit 1
fi

# Desinstalar Jellyfin
echo "Desinstalando Jellyfin..."
synopkg uninstall --force jellyfin
if [ $? -ne 0 ]; then
    echo "Error: No se pudo desinstalar Jellyfin."
    exit 1
fi

# Limpiar caché de paquetes
echo "Limpiando caché de paquetes..."
sleep 5

# Instalar Jellyfin desde el archivo .spk manualmente
echo "Instalando Jellyfin desde el paquete .spk..."
synopkg install $SPK_PATH
if [ $? -ne 0 ]; then
    echo "Error: No se pudo instalar Jellyfin desde el archivo .spk."
    exit 1
fi

echo "Jellyfin fue desinstalado e instalado correctamente desde el archivo .spk."

# Iniciar Jellyfin
echo "Iniciando Jellyfin..."
synopkg start jellyfin
if [ $? -ne 0 ]; then
    echo "Error: No se pudo iniciar Jellyfin."
    exit 1
fi

# Verificar el estado de Jellyfin nuevamente
check_jellyfin_status
if [ $? -eq 0 ]; then
    echo "Jellyfin se ha iniciado correctamente."
else
    echo "Error: Jellyfin no se pudo iniciar."
    exit 1
fi
