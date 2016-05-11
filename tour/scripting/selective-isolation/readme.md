**Poking Holes**

Turbo containers are not required to be completely isolated from the host device resources. Users can fully or partially isolate objects as needed at a fine granularity.

    # Mount a host folder into the container
    > turbo run clean --mount="C:\Installers"
    
    # Directly access files in mounted folder
    (n256vmko) C:\> "C:\Installers\Sublime Text 2.0.2 Setup.exe"
