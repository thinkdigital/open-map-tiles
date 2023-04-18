# Servidor local de OpenStreetMap (MapTiler)

### Ambiente
- Sistema Operativo: Arch Linux 6.2.11
- Desktop Enviroment: GNOME 43
- Package manager: pacman
- Engine de renderização: TileServer GL
- Gestor gráfico docker engine (não Docker Desktop): Whaler (flatpak)
- Accesso ao MapTiler

### Requesitos
Requer:
* Cliente DHCP, 
* Locales em UTF-8 ou equivalente, 
* NetworkManager, 
* systemd [^1], 
* qemu, 
* modulo kernel KVM, 
* gnome-terminal
* [AppIndicator](https://extensions.gnome.org/extension/615/appindicator-support/)

### Instalação do Docker
1. Fazer atualização do sistema: `sudo pacman -Syu`
2. Verificar se o utilizador tem permissões para modulo KVM `ls -al /dev/kvm`
    - Caso não tenha permissões, executar `sudo usermod -aG kvm $USER`
3. Descarregar e instalar o docker engine: `sudo pacman -S docker`
4. Testar com `docker info` e `docker run hello-world`
5. Instalar um frontend GUI para o engine, neste caso o Whaler: `flatpak install whaler`

(Se optar por não usar o whaler)
- [Descarregar](https://docs.docker.com/desktop/install/archlinux/) o pacote do Docker Desktop para arch
- No local onde está guardado, executar: `sudo pacman -U ./docker-desktop-<version>-<arch>.pkg.tar.zst` 

### Importar tiles Openstreetmap e corre-las no container
1. Descarregar tiles do MapTiler e colocar num local com permissões de leitura e escrita
    - https://data.maptiler.com/downloads/dataset/osm/europe/portugal/#4.11/37.25/-18.84
2. Puxar a imagem do renderizador TileServer GL: `docker pull maptiler/tileserver-gl`
3. No local, dentro da pasta com o ficheiro osm*.mbtiles. Executar: `docker run -it -v $(pwd):/data -p 8080:80 maptiler/tileserver-gl`
4. No web-browser, aceder ao local `http://localhost:8080` ou `http://[o ip do docker bridge]:8080`

[^1]: Não pode ser outro tipo de sistema init

### Consultas e referências
- https://docs.docker.com/desktop/install/archlinux/
- https://wiki.archlinux.org/title/Docker
- https://docs.docker.com/desktop/install/linux-install/#kvm-virtualization-support
- https://openmaptiles.org/docs/host/tileserver-gl/
