# Docsify

## Instalación desde SNAPS

1. Instalar node 
```bash
sudo snap install node --channel=14/stable --classic
```

2. Adecuar paths de ejecutables para npm
```bash
sudo -s
npm config set scripts-prepend-node-path true
```

3. Instalar docsify
```bash
sudo npm i docsify-cli -g 
```

## Uso

1. Inicializar un proyecto de documentación
```bash
#PENDIENTE DOCUMENTAR
```

2. Habilitar vista previa
```bash
docsify serve  ~/Projects/rdocs/
```
