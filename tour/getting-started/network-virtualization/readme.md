**Network Virtualization**

Develop and test client/server applications on a single developer box. Containerize multi-server applications and execute in virtualized production network environments. Test with actual domain names and IP configurations. No more "localhost:8080".

    # Launch WordPress server, block external network
    > turbo run -d --name=web --route-block="tcp,udp" wordpress:4.3
    
    # Connect a Firefox browser instance to the
    # WordPress server and map the domain awesome.com
    # to the server container's port 8080
    > turbo run --link=web:awesome.com firefox http://awesome.com:8080