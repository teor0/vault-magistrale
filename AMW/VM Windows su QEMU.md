Scaricare l'iso di Windows 10 o 11.
Scaricare l'iso di virtio-win da https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/?C=M;O=A
Creare un'immagine per la VM tramite `qemu-img create -f qcow2 /path/to/file [gigabytesize]G`
Utilizzare il file `bootvm.sh` per lanciare la VM.
Al primo lancio effettuare l'installazione di Windows e quando serve scegliere i driver sceglie l'opzione browse e seguire il path nel CD virtio che viene proposto e scegliere `E:\amd\w10` ad esempio per Windows 10. Aggiungere una partizione nuova e continuare la procedura. Al termine dell'installazione occorre aprire il device manager e aggiungere il driver di virtio per la connessione questa volta nel path `E:\NetKVM\w10\amd64` sempre per windows 10. Mentre per il display adapter selezionare il path `E:\viogpudo\w10\amd64`.
Per elencare gli snapshot, indipendentemente da come sono stati realizzati bisogna utilizzare il comando `qemu-img snapshot -l file_immagine`