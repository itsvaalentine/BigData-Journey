#  Docker + Jupyter + AWS 

##  DOCKER – Instalación y verificación

```powershell
# Verificar versión instalada
docker --version

# Docker Desktop se inicia automáticamente en Windows (desde la app)
# Verificar que está corriendo
docker info

# Probar instalación
docker run hello-world
```

---

##  DOCKER – Gestión de imágenes

```powershell
# Descargar imagen de Jupyter Notebook
docker pull jupyter/base-notebook

# Descargar imagen de AWS CLI
docker pull amazon/aws-cli

# Listar imágenes descargadas
docker images
```

---

##  EJECUTAR JUPYTER NOTEBOOK CON DOCKER

```powershell
# Modo básico
docker run -p 8888:8888 jupyter/base-notebook

# Modo detached (en segundo plano)
docker run -d -p 8888:8888 jupyter/base-notebook

# Montar directorio actual — PowerShell
docker run -p 8888:8888 -v ${PWD}:/home/jovyan/work jupyter/base-notebook

# Montar directorio actual — CMD (símbolo del sistema)
docker run -p 8888:8888 -v %cd%:/home/jovyan/work jupyter/base-notebook
```

>  `-p 8888:8888` → mapea el puerto local al del contenedor  
>  `-v ${PWD}:/home/jovyan/work` → monta la carpeta actual dentro del contenedor  

---

##  DOCKER – Gestión de contenedores

```powershell
# Listar contenedores en ejecución
docker ps

# Listar todos los contenedores (incluyendo detenidos)
docker ps -a

# Detener un contenedor
docker stop <container_id>

# Eliminar un contenedor
docker rm <container_id>

# Eliminar una imagen
docker rmi <image_id>
```

---

##  ACCEDER A UN CONTENEDOR EN EJECUCIÓN

```powershell
# Abrir shell interactivo dentro del contenedor
docker exec -it <container_id> bash
```

---

##  AWS CLI CON DOCKER EN WINDOWS

```powershell
# Verificar versión de AWS CLI
docker run --rm -it amazon/aws-cli --version

# Configurar credenciales AWS — PowerShell
docker run --rm -it -v ${HOME}/.aws:/root/.aws amazon/aws-cli configure

# Configurar credenciales AWS — CMD
docker run --rm -it -v %USERPROFILE%\.aws:/root/.aws amazon/aws-cli configure
```


---

##  COMANDOS AWS S3 EN WINDOWS

```powershell
# Listar buckets S3
docker run --rm -it -v ${HOME}/.aws:/root/.aws amazon/aws-cli s3 ls

# Subir archivo a S3 — PowerShell
docker run --rm -it `
  -v ${HOME}/.aws:/root/.aws `
  -v ${PWD}:/data `
  amazon/aws-cli s3 cp /data/file.csv s3://tu-bucket/

# Subir archivo a S3 — CMD
docker run --rm -it ^
  -v %USERPROFILE%\.aws:/root/.aws ^
  -v %cd%:/data ^
  amazon/aws-cli s3 cp /data/file.csv s3://tu-bucket/

# Descargar archivo de S3 — PowerShell
docker run --rm -it `
  -v ${HOME}/.aws:/root/.aws `
  -v ${PWD}:/data `
  amazon/aws-cli s3 cp s3://tu-bucket/file.csv /data/

# Descargar archivo de S3 — CMD
docker run --rm -it ^
  -v %USERPROFILE%\.aws:/root/.aws ^
  -v %cd%:/data ^
  amazon/aws-cli s3 cp s3://tu-bucket/file.csv /data/
```


##  EJECUTAR CONTENEDOR CON NOMBRE PERSONALIZADO

```powershell
docker run -d -p 8888:8888 --name jupyter-lab jupyter/base-notebook
```

---

##  LIMPIEZA

```powershell
# Eliminar contenedores detenidos
docker container prune

# Eliminar imágenes no utilizadas
docker image prune
```

---

##  Propósito

Estos comandos permiten:
- Entornos de desarrollo contenedorizados en Windows
- Flujos de trabajo reproducibles sin instalar dependencias locales
- Interacción con AWS desde la línea de comandos
- Experimentación con Big Data usando Jupyter Notebook