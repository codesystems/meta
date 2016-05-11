**TurboScript Automation**

Powerful TurboScript primitives allow automated configuration of containers and integration into continuous integration processes. Higher-order operators such as "using" allow transient consumption of containers within scripted build environments.

    # Use git and sbt to download and build the project
    using git,sbt
      git clone https://github.com/scala/async.git
      cd async && sbt clean test
    
    # git and sbt are no longer present in the image
    commit async
    push async