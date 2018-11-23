# Hints for working in Ubuntu

Do everything as **root**:
``` sudo -s ```

---
Keyboard shortcut to **open terminal**:
``` ctrl + alt + T ```

---
**Nano Editor:**

Copy a line: 
````
Ctrl + K   // -> cut lline
Ctrl + U   // -> insert line
````

Select text: 
````
Alt + A   // -> starts selection - then move with cursor
// then use Ctrl + K to cut and Ctrl + U to paste.
````

---
Find path of app with **which**. 
E.g.:

````which dotnet````

````which docker````

---

**dpkg**

- Install a deb: ````dpkg -i .\{mydeb}````
- List all installed packages: ````dpkg -l ```` (use with | grep)
- See the locations of all the files installed as part of the package:
 ````dpkg -L <packagename>````

