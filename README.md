# Local AI Chat Bot - Homelab Demo
Host your own AI on your local machine via Docker for research, coding and learning all in the privacy of your own machine.

## Setting expectations
Running AI, especially on a local desktop, is not the star-studded experience of chatGPT. What you might be gaining in privacy, you loose in speed and typically the knowledge of the models you can use. Millage will vary based on your machine hardware. Ideally you would setup and passthrough a GPU with tens of VRAM, however this demo is not focused on getting you the best experience with a GPU as there are way more complexities (IOMMU, drivers, correct docker images, OS, using via cli, etc). We are simply focused on getting you hosting a AI chat bot in this demo.

## Overview
We will be using two different docker open source projects, Ollama, for hosting, managing and downloading the public models, and Open-WebUI, for interacting with them via web-gui. We will also touch on how to interface with VScode via continue.dev extension to use our models from VScode.

## How to deploy
We will be deploying on a linux system that is up to date and has the following preinstalled: Git (version 28.2.2) & Docker Engine (version: 28.2.2) & sudo installed, account with sudo level permissions.

guide is build when the docker containers were at the following version:
    - Open-WebUI: v0.7.2
    - Ollama: v0.15.2

1) clone a copy of this git repo locally:

    `git clone https://github.com/SJW-sys/Local_AI_ChatBot-HomelabDemo.git`

2) Review hardware specs in the docker-compose.yaml file.

3) Always review external scripts, commands and tools before executing on your system.

3) navigate into docker deployment files:

    `cd DeploymentFiles`

4) run Docker Compose in detached mode, which will pull images and setup everything within the compose file:

    `sudo docker compose up -d`

5) navigate to open-webui, you will be prompted to login into account. Our compose will restart the container on boot:

    https://localhost:9000

6) You can now navigate to your user icon to click, follow path Settings > Admin Settings > Models > icon for download in top right.

7) you will now have a pop-up where you can now enter models to 'pull from ollama.com', then click the download icon. I would start with 2b sized models or smaller, then get larger models once you have played with the speed of responses.

8) Once complete you can exit the pop-up and select "new chat" on the left hand menu.

9) Once setup, I would advice you explore the settings. If the goal is privacy, I would check the settings for these features: Direct Connects, OpenAI API, and Community Sharing.


## How to setup a open-source code agent with continue.dev in VScode
We will be deploying on a linux system that is up to date and has the following preinstalled via deb package:  Microsoft VS code (Version: 1.108.2)

1) Ensure you have Ollama deployed first and its running, such as following the above steps

2) Open VS code and navigate to the Extensions tab, search for Continue.

3) select top option, check its a verified source and the creator is 'Continue.dev'. Click 'install' button. You will have to trust the publisher to install.

4) Once installed, select extension from side menu if it does not automatically open.

5) select the model in the bottom part of the chat window, here you can select "add langauge model". If ollama is running you can select it.

6) once selected choose auto gen, and it will pull in all your local models that you can choose to prompt. (I have attached in the repository an example file of a config you can use, if you choose to set different models for continue.)

7) Once setup, I would advice you explore the settings, in particular to see what the chat window can access. By default everything is set to "ask permission" so you have to feed it via context.


## Prompt Engineering
Interacting with AI is all about how you prompt it. There is a lot to be said about how the models are trained, and this can inherently build in flaws and complexities when using them that can provide a lot of the bad data you typically hear in the news, but we are going to focus on some simple standard tips.
- Start with a simple statement of what you need to set the tone. ie "I need help with troubleshooting proxmoxVE."
- Follow up with exactly what you need. "I am having an issue making a Virtual machine in proxmox." 
- Then start providing context, typically the more you provided the better. ie the steps you have tried to solve your problem, what you suspect is the culprit and why.
- Follow up with any rules of how you want the AI to return the information. ie "Explain this to me like I am five and just started using proxmoxVE."
- Finally asking the AI to explain its reasoning, and to provide resources, helps the AI generate better output.

There is another skill set emerging for interacting with AI called Context Engineering, which is focused not only on the rules of what you ask in output, but how and what you provide the AI. Typically this is seen when interacting with code, and allow the AI to see the exact code, the terminal and error logs, maybe even git history. We are not going to cover any sort of best use here.

## Example Prompt using Prompt Engineering tips above to help find models

please replace the hardware data with your information

    I need a recommendation for a AI model to selfhost via Ollama. Please provide your thought process as to why I should use your recommendation, and give me several external resources to backup your information. Please use the following rules when building recommendation:

    Rules:
        - My hardware limits are:
            - CPU 
                Threads: 4
                model: Intel Core i5-12400
            - RAM
                Amount: 24gb
            - GPU
                model: None
            - Disk
                type: SSD
                storage: not an issue
            - Network
                network card speed: 1gb
                internet connection speed: 200mbs
        - I will be hosting Ollama via docker, with Open WebUI for accessing on my local machine.
        - I will be throttling Ollama via the docker config with the provided hardware above
        - I am looking for a model focused on these capabilities:
            - Task:
                - write really bad puns
        - Models Needs to be capable with hosting on Ollama
        - Ideally downloadable from Ollama.com
        - "Trusted sources" for basing recommendations on, are:
            - Any well accolade AI research firm outside of the building and selling of AI products (Gemini, ChatGPT, Lumo, etc)
            - Any IVY-league or well award technical University (MIT, Brown, etc)
            - Any well accolade AI spokesman, not tied (financially or other means) to a AI product (Gemini, ChatGPT, Lumo, etc)
            - Ollama
        - Sources are allowed outside my "trusted sources", however these should be weighted less.
        - AI companies that sell AI products (Gemini, ChatGPT, Lumo, etc), can be considered as sources, but they should be weighted very little given inherit bias and any recommendation you give based on their reports, please provided WHY their recommendation is valid and that your using them as a source for the recommendation.
        - You may search the web
        - More recent recommendations are preferred (last month), however the model family should ideally have a reputable history (6 month+, with longer timeframes (ie 18months) considered better). 

## Resources:
### Ollama
- Github: https://github.com/ollama/ollama
- DockerHub: https://hub.docker.com/r/ollama/ollama
- website: https://ollama.com/
- Documentation: https://docs.ollama.com/
### Open-WebUI
- Github: https://github.com/open-webui/open-webui
- website: https://openwebui.com/
- Documentation: https://docs.openwebui.com/
### Additional for support, focused on passing through a Nvidia GPU
- NVIDIA Container Toolkit installation guide: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
- Docker Engine GPU support (--gpus): https://docs.docker.com/config/containers/resource_constraints/#gpu
- Rootless Docker – limitations (including not allowing GPU): https://docs.docker.com/engine/security/rootless/
- Ollama Docker quick‑start (official): https://github.com/ollama/ollama/blob/main/docs/docker.md
- Open‑WebUI Docker image (GPU variant): https://github.com/open-webui/open-webui#docker
- NVIDIA Github for toolkit: https://github.com/NVIDIA/nvidia-container-toolkit

## FAQ for repository visitors
### Why does the repository have 'Demo' in the title?
this "demo" repo is based on real world local deployments in my Homelab. Some settings may be changed, or different for privacy and safety. Typically in a real world scenario, you would use .gitignore to filter out potential sensitive files, as well has have pipeline jobs to find secrets. Typically you also consider storing variables within the git repository platform itself in a mask state to prevent jr devs from pushing secrets accidentally.

### Why multiple branches
I have a test and main branch to more demo a enterprise setup, where you might have people pushing changes to a protected test branch that is then has pipelines to stage tooling in a test space. Which once pulled into main, would deploy the same setup to PROD.

### Where can I find more about this project and your thought process?
I make it a habit that my files typically have dozens of in-line comments to better help anyone using them for the first time to understand what is happening, maybe not always why. Also please check out my blog, it typically has more information on my projects (sometimes the post is still being planned).

### Does ths connect to your other Homelab Demo repositories?
This one can, but its designed with the intent to be run locally on your machine.

### Was AI used to generate this?
No, but I have learned and expanded my knowledge of the tools within this project (and prior projects) with AI to design a better solution and skills for other deployments. AI I see as a tool and resource that is loose in the market place regardless of your stance, that you need to follow, know how to use, while educating others on its complexities and putting safeguards around it. I firmly design and deploy with my own brainstorming, knowledge, and troubleshooting as my first approach, but i have used AI to troubleshoot, help expand understanding, research, interpreter (ie. Bash to Go) and experimented with vibe coding. I have deeper thoughts and opinions, but those are better discussed rather than a few sentences in a git repo. 

## Issues to note with this "demo"
I wanted to do a proper code repo that could be poke around so you could see commits and pulls that you might normally see in a team production repo, unfortunately due to the overhead and this [issue](https://github.com/orgs/community/discussions/6292), I will be "cutting corners" and doing everything locally then pushing to main directly from my machine. However, I will still leave the demo "main" and "test" branches with there protections.

## Notes on Docker Rootless
Currently docker rootless does not support gpu pass through.

## Docker features, CI/CD tooling/skills, and other tools leveraged in this project.
- Docker:
    - image pull
    - compose
    - Networking
    - volumes
    - device passthrough
    - managing tooling multiple configurations files
- Yaml
- Prompt Engineering
- Context Engineering
- VScode and extensions
- Git
- Gitlab interface and secret management
- Pipelines (all in Gitlab formatting)
- Git CD/CI best practices (branches, branch protections, etc)
- Linux (general and permissions)
- Bash
- SSH
- Documentation