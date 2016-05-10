**Stopping a Container**

To stop a container, simply exit all processes that were started in the container. In our case, only the command prompt process is running in the container.

    # Exit the new containerized command prompt
    (n256vmko) C:\> exit
    
    # The container ID will appear in the command
    # prompt
    n256vmko
    
    # Optionally, delete the container to remove all
    # traces of its execution
    
    # Remove a single container by specifying the ID
    > turbo rm n256vmko
    Container n256vmko has been removed
