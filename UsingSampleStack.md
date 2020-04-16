# Using Sample Stack

## Using Sample Application stack

1. Clone or copy the "sample-stack" [git repository](https://github.com/application-stacks/sample-stack) 
1. Change directory to the `sample-stack` directory
1. Delete the `starter` directory (which contains the starter stack)
1. Create a new stack directory, within the base directory of the Git repository, to contain the stack to be published. For example:
    ```
    mkdir ./my-nodejs
    ```
1. Copy or create your stack in this new directory. Look at [Developing Stacks](https://appsody.dev/docs/stacks/develop) for information on how to create a stack. 
1. Package and test your stack. There are 2 ways that you can package and test your stack. These are:
    1. Using the Appsody CLI `stack` commands, see [Packaging stacks](https://appsody.dev/docs/stacks/package)
    1. Using the CI scripts provided within the sample-stack repo, see [Packaging a stack using CI scripts](#Packaging-a-stack-using-CI-scripts)
    > If using Travis CI within your git repository, it may be preferable to use the CI scripts as they are used when you push your stack to your git repository.
1. Once you have developed, packaged and tested your stack you then push your repository to your GitHub organization. i.e. `https://github.com/myorg/my-nodejs`

## Packaging a stack using CI scripts

1. Use environment variables to override default settings that are used by the build script, for example the namespace to use for the Docker images, and the URL to use to reference the template archive files. The main variables to override are:

   - `IMAGE_REGISTRY_ORG` this is the namespace to create the Docker images with.
   - `RELEASE_URL` this is the base URL to your web hosting service, and is used to reference the template archive files from within the repository index file.

   You can set these environment variables by exporting them. For example:

    ```
    export IMAGE_REGISTRY_ORG=myproject
    export RELEASE_URL=https://github.com/myorg/my-nodejs/releases/latest/download
    ```
1. Run the build script from the base directory of the git repository. For example:
    ```
    ./ci/build.sh
    ```
    This command creates the following artefacts in the `./ci/assets` directory: 
    * stack index file (containing remote URLs)
    * template archive files
    * stack source file

    and in the local docker registry:
    * stack container images

## Configure Git repository to use Travis to build stack

To configure your git repository to use Travis CI follow the "Get started with Travis using Github" [instructions](https://docs.travis-ci.com/user/tutorial/#to-get-started-with-travis-ci-using-github)

The "sample-stack" [git repository](https://github.com/application-stacks/sample-stack) contains the files that provide support for building and releasing of the stack. These file are:

* ./ci/* : the CI scripts that will perform the build and the release of the stack 
* ./travis.yml : the travis configuration file

Similar to when running the CI scripts locally, there are environment variables that need to be setup within your Travis environment. Use the "Defining Variables in Repository Setting" [Travis documentation](https://docs.travis-ci.com/user/environment-variables/#defining-variables-in-repository-settings) to define environment variables.

The environment variables you will need to override are:

   - `IMAGE_REGISTRY_ORG` this is the namespace to create the Docker images with.
   - `RELEASE_URL` this is the base URL to your web hosting service, and is used to reference the template archive files from within the repository index file.
   - `CODEWIND_INDEX` this specifies whether to build the indexfile for use with Codewind. Default=false.
   - `GITHUB_TOKEN` this is a user access token to allow access to your Github org.
   - `IMAGE_REGISTRY_USERNAME` this is the username that will be used to logon to your docker registry when publishing the docker images.
   - `IMAGE_REGISTRY_PASSWORD` this is the password that will be used to logon to your docker registry when publishing the docker images.
