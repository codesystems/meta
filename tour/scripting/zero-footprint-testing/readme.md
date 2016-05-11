**Zero Footprint Testing**

Try out applications in temporary containers using the try primitive.

    # Remove all containers
    > turbo rm -a
    All containers have been removed
    
    # 'try' works like 'run' except the resulting
    # container is automatically deleted.
    > turbo try jdk:7.65,mongo,node:0.10.29
    
    (n256vmko) C:\> exit
    
    # List containers to verify the container was
    # deleted
    > turbo containers
    There are no containers on this machine
    