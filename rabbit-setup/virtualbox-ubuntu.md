# Hints for setting up new VM


### Network
Setup two network adapters: one for communication VM <-> Host, otder for VM <-> Internet.
See [here](https://gist.github.com/pjdietz/5768124) for additional info.

- Adapter 1:
  Switch guest VM to **host-only adapter**.
  Optional: Set a static ip-address in guest-vm so that it's stable across restarts (see link above)

- Adapter 2:
   **NAT**

---

### Copy & Paste 
Enable Copy & Paste:
  Inside VM > Geräte > Gemeinsame Zwischenablage > Bi-Directional
  Inside VM > Geräte > Gasterweiterungen einlegen…

---

### Shared-Folder 

- Add "Shared Folder" in VirtualBox while VM shut out. (e.g. name: "myShare")
- Maybe "myShare" is already mounted as "sf_myShare" in directory /media
  - if so: use it as is
  - or: ````sudo umount /media/sf_myShare ````
- Mount the shared folder to a directory of choice:
  - ````sudo mount -t vboxsf -o rw,uid=1000,gid=1000 myShare /home/mats/somedir```` where 'somedir' must exist
  - replace '1000' with values retrieved from running 'id'
  - maybe add user to group vboxsf: ````sudo adduser [username] vboxsf```` (Background: When VirtualBox installed the Ubuntu operating system, it added a group called “vboxsf”. Before you can access any shared folders, you must add yourself to the vboxsf group.)
  
---
