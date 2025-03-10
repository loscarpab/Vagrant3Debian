
### 1. Instalación de VMware Workstation Pro 17

Antes de comenzar, es necesario instalar VMware Workstation Pro 17. Recuerda que en la BIOS de tu equipo debe estar activada la virtualización para que VMware pueda crear y gestionar máquinas virtuales.

---

### 2. Instalación de Vagrant

En la máquina Ubuntu, abre una terminal y ejecuta el siguiente comando para instalar Vagrant:

```bash
sudo apt install vagrant
```

Este comando instalará la herramienta que te permitirá gestionar entornos virtuales de manera sencilla.

---

### 3. Instalación del plugin para VMware

Para que Vagrant se comunique correctamente con VMware, es imprescindible instalar el plugin específico. Ejecuta:

```bash
vagrant plugin install vagrant-vmware-desktop
```

Este plugin sirve como puente entre Vagrant y VMware, permitiéndote controlar las máquinas virtuales desde Vagrant.

---

### 4. Creación del directorio y configuración inicial

Crea un directorio para tu proyecto y sitúate en él:

```bash
mkdir ~/vagrant-debian-cluster && cd ~/vagrant-debian-cluster
```

Dentro de este directorio, inicializa Vagrant para generar el archivo de configuración básico:

```bash
vagrant init
```

---

### 5. Configuración del entorno virtual

Abre el archivo `Vagrantfile` que se acaba de crear y modifícalo para definir las características de las máquinas. Por ejemplo, para crear un clúster de 3 máquinas basadas en Debian 12 (versión 4.3.12), puedes usar el siguiente contenido:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "generic/debian12"
  config.vm.box_version = "4.3.12"

  num_machines = 3  # Número de máquinas a crear

  (1..num_machines).each do |i|
    config.vm.define "debian-#{i}" do |node|
      node.vm.hostname = "debian-#{i}"
      node.vm.provider "vmware_desktop" do |vmware|
        vmware.linked_clone = false
        vmware.memory = 256
        vmware.cpus = 1
      end
    end
  end
end
```

En este archivo se configura la caja base (la imagen de Debian 12) y se definen tres máquinas virtuales, cada una con 256 MB de memoria, una CPU y un nombre único (`debian-1`, `debian-2`, `debian-3`).

---

### 6. Levantar el entorno virtual

Con el entorno configurado, puedes iniciar todas las máquinas ejecutando:

```bash
vagrant up
```

Este comando se encargará de descargar la imagen de Debian (si aún no está disponible localmente), crear el "box" y clonar la imagen para levantar cada una de las máquinas.

---

### 7. Acceso a las máquinas

Para conectarte a cada máquina a través de SSH, utiliza los siguientes comandos según la máquina deseada:

```bash
vagrant ssh debian-1
vagrant ssh debian-2
vagrant ssh debian-3
```

De este modo, podrás acceder a cada entorno virtual individualmente.

---

### 8. Gestión del ciclo de vida de las máquinas

- **Detener las máquinas:**  
  Para apagar todas las máquinas sin eliminarlas, ejecuta:

  ```bash
  vagrant halt
  ```

- **Eliminar las máquinas:**  
  Si deseas borrar completamente el entorno, utiliza:

  ```bash
  vagrant destroy
  ```

  Al ejecutar este comando, se te pedirá confirmar qué máquinas deseas eliminar.
