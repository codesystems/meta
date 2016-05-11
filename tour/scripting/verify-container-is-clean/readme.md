**Isolated File System**

Create a directory in the container and verify that the directory does not exist on host machine.

    # In the container, make a new directory called
    # "Turbo"
    (n256vmko) C:\> mkdir Turbo
    
    # In the native command prompt:
    C:\> dir
    Directory of C:\
    <DIR>         Windows
    
    # In the container:
    (n256vmko) C:\> dir
    Directory of C:\
    <DIR>         Turbo
    <DIR>         Windows
