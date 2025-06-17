## Installation of an SInfoE POP on Linux

As the SInfoE repo is private, we need to create a GitHub personal access token (CLASSIC) with the appropriate scope (repo).
(https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

Store the token in an environment variable:

export CR_PAT=my_token

sign-in to the Container registry service at ghcr.io with an authorised USERNAME:

echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin
> Login Succeeded

docker pull ghcr.io/scp-development/local-mdis-docker/pop:main

NOTE: docker pull will only work if "Linux post-installation steps for Docker Engine" have been undertaken (or if logged-in as root).
(https://docs.docker.com/engine/install/linux-postinstall/)

We now need to create a directory called pop_files:

mkdir pop_files

(I created it in a parent directory "sinfoe")

and then, within this "pop_files" directory, create a file called "pop_config.json" with the following contents:

{

    "listen_url": "http://0.0.0.0:19316",

    "transport": "httptransport",

    "db_path": "/pop_files/pop.db",
    "node": {

        "listen_url": "http://0.0.0.0:19316",
        "transport": "httptransport",
        "db_path": "/pop_files/pop.db",
        "registered_services_path": "/pop_files/registered_services"

    }
}

We can now run the POP container:

docker run --name pop --network host --user "$(id -u):$(id -g)" -v './pop_files:/pop_files' ghcr.io/scp-development/local-mdis-docker/pop:main pop /pop_files/pop_config.json

(Docker run needs a bit of care. The volume path is relative to where you are in the file structure, the json config file is where it appears in the mounted volume!) 

Should it fail, remove the container with:

docker rm pop

before trying again.

To check everything is working, we can open a shell on the running POP:

docker exec -it pop bash
