# How to setup on GoogleCloudPlatform

## Step by step guide

1. Create a new instance

   ```shell
   gcloud compute instances create instance-20240618-090236 \
        --project=ai-sandbox-company-25 \
        --zone=asia-southeast1-b \
        --machine-type=n2d-standard-8 \
        --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=subnet-asia \
        --metadata=enable-osconfig=TRUE \
        --can-ip-forward \
        --maintenance-policy=MIGRATE \
        --provisioning-model=STANDARD \
        --service-account=512612803435-compute@developer.gserviceaccount.com \
        --scopes=https://www.googleapis.com/auth/cloud-platform \
        --tags=http-server,https-server,lb-health-check \
        --create-disk=auto-delete=yes,boot=yes,device-name=instance-20240618-090236,image=projects/debian-cloud/global/images/debian-12-bookworm-v20240617,mode=rw,size=512,type=projects/ai-sandbox-company-25/zones/asia-southeast1-c/diskTypes/pd-balanced \
        --no-shielded-secure-boot \
        --shielded-vtpm \
        --shielded-integrity-monitoring \
        --labels=goog-ops-agent-policy=v2-x86-template-1-2-0,goog-ec-src=vm_add-gcloud \
        --reservation-affinity=any
    ```

1. Setup `git`, `docker`

   ```shell
   sudo apt update
   sudo apt install git
   git --version
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

1. Clone git repo

   ```shell
   ssh-keygen -t ed25519 -C "jakelim070@gmailexample.com"
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ssh -T git@github.com
   cd ~
   mkdir apps
   git clone git@github.com:jakelime/ppcs-docker.git
   cd ppcs-docker
   ```
