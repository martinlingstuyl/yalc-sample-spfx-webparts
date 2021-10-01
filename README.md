# yalc-sample-spfx-webparts

## Summary

This project contains an SPFx Webpart solution. 
You can use this repo to test the use of yalc with dev containers and SPFx Library Components. 
There is another repo which contains the library component solution. [That can be found here](https://github.com/martinlingstuyl/yalc-sample-spfx-library-component).

For the purpose of this tutorial you need to start with the readme.md in the library component repo.

## Used SharePoint Framework Version

![version](https://img.shields.io/npm/v/@microsoft/sp-component-base/latest?color=green)


## Prerequisites

You need the following prerequisites:
- Docker Desktop with WSL integration enabled
- Visual Studio Code with the following extensions
  - ms-azuretools.vscode-docker
  - ms-vscode-remote.remote-containers


## Version history

Version|Date|Comments
-------|----|--------
1.0|Oktober 1st, 2021|Initial release


## Getting started
> First open the library component github repo and follow the readme. After that, come back here.

I'm assuming you now have a _shared folder with a published package. Follow the next instructions to continue: 

1. Copy the git clone url for this repo from GitHub
2. Open a new window of VS Code
3. Use the command palette to execute 'Remote-Containers: Clone repository in container volume'. This will clone the repo and build the devcontainer
4. Ensure that you are at the solution folder
5. in the command-line run the following script:
  
    ```bash
    npm install    
    ```    

   If all is well, you should see a .yalc folder in the root of your explorer. This folder contains the built library package. You can reference using:

    ```bash
    yalc link yalc-sample-spfx-library-component --store-folder /_shared
    ```    

6. You can now access the code that is in the library component repo and use it in your webpart.   

### Republishing and updating linked code
The downside of the above way of working is that the linked library is not automatically updated on changes. 

After making changes to the library component code, you should need to re-run the following script on the library component dev container:

```bash
gulp bundle
yalc publish . --store-folder /_shared
```

And then you should run the following script on the depending devcontainers if you want to update the linked version.

```bash 
yalc update --store-folder /_shared
``` 


### With change automation
It should be possible to run the following command as postbuild task to gulp serve. This would push the package to the dependencies when changing code:
    
```bash
yalc publish . --store-folder /_shared --push
```

But this does not work with `yalc link`. You would need to use `yalc add` on the webpart solution:

    ```bash
    yalc add yalc-sample-spfx-library-component --store-folder /_shared
    ```    

The package will now be added to the `package.json` file as `"yalc-sample-spfx-library-component": "file:.yalc/yalc-sample-spfx-library-component"`. 

> Take care to remove this package.json line when your start building for production.

## References

- [Getting started with SharePoint Framework](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant)

- [Library Component tutorial](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/library-component-tutorial)

- [Where are docker images stored](https://www.freecodecamp.org/news/where-are-docker-images-stored-docker-container-paths-explained/)

- [Link devcontainers using yalc](https://stackoverflow.com/questions/68670700/npm-link-dev-packages-when-using-docker-dev-containers)

- [yalc documentation](https://www.npmjs.com/package/yalc)