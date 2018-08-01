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