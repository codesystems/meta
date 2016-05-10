**Migrating Containers**

Save application state and continue execution on another device -- even on a different Windows OS.

    # Begin work in container on device A
    > turbo run python
    (98348bi3) C:\> exit
    
    # Continue execution on device B
    > turbo continue 1b7f5707
    (98348bi3) C:\>