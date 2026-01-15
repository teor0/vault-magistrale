``` bash
# snapshot interno con VM offline
#create
sudo qemu-img -c <name> <image>
#list
sudo qemu-img -l <image>
#apply snapshot
sudo qemu-img -a <name> <image>
#delete
sudo qemu-img -d <name> <image>

# start in snapshot mode
qeum-system-x86_64 -snapshot 

# snapshot interno con VM online 
savevm <name>
loadvm <name>

# copia del immagine
cp <og image> <snap image>

# snapshot esterno, utile per avere piccole varianti dell'immagine
qemu-img create -f qcow2 -b <snapshot image> -F qcow2 <original image>

```

