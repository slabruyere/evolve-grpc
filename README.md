# Development Setup #

Prerequisites:

1. IntelliJ (other IDE's will work, but not pre-configured)
1. Protobuf support in your IDE
1. Gradle (>5.2 for building java project)
1. Clone of [evolve-grpc](https://bitbucket.org/zepben/evolve-grpc) if not using the git submodule.

Clone the repo and optionally the submodules:

```
git clone git@bitbucket.org:zepben/evolve-grpc-nc.git
# or to include evolve-grpc:
git clone --recurse-submodules -j2 git@bitbucket.org:zepben/evolve-grpc-nc.git
```
    
If you included the submodule, open the project in IntelliJ and add the `evolve-grpc/proto` directory to the Protobuf plugin include paths. This will make imports be resolved within the IDE.
 
![](images/plugin-config.png)

Otherwise, you will require a symlink to the cim package in the [evolve-grpc](https://bitbucket.org/zepben/evolve-grpc) repository: 

On Linux:

```
ln -s path/to/evolve-grpc/proto/zepben/cim/ proto/zepben/cim
``` 

On Windows:
    
```
mklink /J proto\zepben\protobuf\cim path\to\evolve-grpc\proto\zepben\protobuf\cim\
mklink /J proto\zepben\protobuf\network path\to\evolve-grpc\proto\zepben\protobuf\network\
```

To build java:

```
cd java
mvn package
```

To build and package python:

```
cd python
python3 build.py --package
```
 
## Protolock ##

This repository uses protolock to ensure there are no breaking changes between releases.

The proto.lock file can be found at `proto/zepben/proto.lock`. This file will be automatically kept up to date by the build pipeline - PR's **_should not_** update this file. 

For testing, the protolock binary can be retrieved from https://github.com/nilslice/protolock/releases/. Download, extract, and add it to your `PATH`. 

Prior to merging, protolock is run automatically by the build pipeline, and will ensure no breaking changes have been made. You can test this prior to committing by running:

```
cd proto/zepben
protolock status
```

### Breaking changes ###

If you need to merge breaking changes your patch must:

1. Update the major version to the next release for each library

1. Force update the `proto.lock` file prior to committing.

    ```
    cd proto/zepben
    protolock commit --force
    git add proto.lock
    ```
    
1. Commit `proto.lock` changes with version updates.