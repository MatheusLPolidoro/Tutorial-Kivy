# KIVY Windows 10
Configuração do ambiente e construção do APK.  

- [KIVY Windows 10](#kivy-windows-10)
  - [WSL2](#wsl2)
  - [Dependências](#dependências)
    - [Linux](#linux)
    - [Windows](#windows)
  - [Desenvolvimento do APK](#desenvolvimento-do-apk)
    - [Instalação do Kivy](#instalação-do-kivy)
      - [**pip**](#pip)
      - [**Poetry**](#poetry)
  - [Build do Android APK](#build-do-android-apk)
  - [Instalação do APK no aparelho.](#instalação-do-apk-no-aparelho)
___
## WSL2
Realizar a instalação do wsl2 conforme o passo a passo:

[Instalar o Linux no Windows com o WSL](https://learn.microsoft.com/pt-br/windows/wsl/install)

1. Habilitar o Subsistema do Windows para Linux
    ```PowerShell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

2. Comando de instalação: 
    ```PowerShell
    wsl --install
    ```
3. Habilitar o recurso de Máquina Virtual:
    ```PowerShell
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```
4. **Baixar e instalar** o pacote de atualização do kernel do Linux:  
    [Pacote de atualização do kernel do Linux do WSL2 para computadores x64](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

5. Definir o WSL 2 como a sua versão padrão:
    ```PowerShell
    wsl --set-default-version 2
    ```

6. Instalar a distribuição do Linux:
    ```PowerShell
    wsl --install -d ubunto
    ```

7. Comando para verificar a versão:
    ```PowerShell
    wsl -l -v
    ```
## Dependências
### Linux
1. Atualizar o update:
   ```bash
   sudo apt-get update
   ```
2. Instalação do buildozer:
   ```bash
   sudo apt-get install git
   git clone https://github.com/kivy/buildozer.git
   cd buildozer
   sudo apt install python3-pip
   python3 -m pip install --upgrade pip      setuptools virtualenv
   sudo python3 setup.py install
   ```
3. Instalação do Cython:
   ```bash
   sudo apt-get install cython
   sudo apt-get install --reinstall gedit
   cd /bin/ && sudo gedit cython
   sudo apt-get install cython3
   sudo pip3 install --upgrade Cython==0.29.19 virtualenv
   ```
4. Demais instalações necessarias:
   ```bash
   sudo apt install zip
   sudo apt install unzip
   sudo apt install openjdk-11-jdk
   sudo apt install autoconf
   sudo apt install libtool
   sudo apt install pkg-config
   sudo apt install zlib1g-dev
   sudo apt install libncurses5-dev
   sudo apt install libncursesw5-dev
   sudo apt install libtinfo5
   sudo apt install cmake
   sudo apt install build-essential libltdl-dev libffi-dev libssl-dev python-dev
   sudo apt install adb
   ```
5. Definição da variavel de ambiente do Java:
   ```bash
   export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64/
   ```
### Windows
1. Baixar e descompactar o zip do platform-tools:  
   [platform-tools_r33.0.0-windows](https://dl.google.com/android/repository/platform-tools_r27.0.0-windows.zip)

## Desenvolvimento do APK
Em seu ambiente Windows iniciar o projeto (preferencialmente utilizando **ambiente virtual**) e seguir com a instalação do **Kivy**.

### Instalação do Kivy 
**Escolha apenas um tipo de instalação:**
#### **pip**
```PowerShell
python -m pip install --upgrade pip setuptools virtualenv
pip install kivy[base] kivy_examples --pre --extra-index-url https://kivy.org/downloads/simple/
```
#### **Poetry**
```PowerShell
poetry source add --secondary kivy https://kivy.org/downloads/simple/
poetry add --allow-prereleases --source kivy kivy
```
## Build do Android APK
1. Copiar o projeto já desenvolvido em seu ambiente Windows para seu ambiente Linux.
2. Inicializar o buildozer na mesma pasta do projeto:
    ```PowerShell
    buildozer init
    ```
3. Realizar as devidas configurações no arquivo build.spec
4. Comando para efetuar o build:
   ```PowerShell
   sudo buildozer android debug
   ```
## Instalação do APK no aparelho.
A instalação também pode ser realiza manualmente passando o arquivo APK no aparelho.

1. Ligar o smartfone no modo desenvolvedor em depurador USB PTP (transferência de imagens).
2. Ligar o depurador USB em seu android.
3. Iniciar o server no ambiente Windows:
4. Executar como adm no PowerShell no caminho do platform-tools
    ```PowerShell
    adb tcpip 5555 
    ```
5. Pegar o IPv4 do sistema Android (configs>sobre>endereço ip), e executar o comando:
   ```PowerShell
   adb connect <android ip>:5555
   ```
6. Efetivar a transferencia do APK:
   ```PowerShell
   adb -s <android ip>:5555 install <apk file>
   ```