Let's create a new container using the turbo run command, which will bootstrap a new container from any specified image. Any parameters specified after the <image> will be passed to the startup file.

    > turbo run clean echo Hello World!
    
    # A new, containerized command prompt will appear with your output:
    
    Hello World! 