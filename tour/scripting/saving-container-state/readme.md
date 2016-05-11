**Creating Images**

Committing changes made to a container takes a snapshot of the current state of the container and saves it to a Turbo image file. Image files can then be used to revert back to the current state of the container in the future. Images can also be shared and used to re-create the container on multiple machines.

    # Save changes to the container to an image
    > turbo commit n256vmko sublimetext --startup-file="C:\Program Files (x86)\Sublime Text 2\sublime_text.exe"
    
    Committing container n256vmko to image sublimetext
    Commit complete
    
    # Run the new image
    > turbo run sublimetext