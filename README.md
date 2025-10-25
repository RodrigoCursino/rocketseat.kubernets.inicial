### LENS

1. Obtenha a chave de segurança pública do Lens:
    
    ```bash
    curl -fsSL https://downloads.k8slens.dev/keys/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/lens-archive-keyring.gpg > /dev/null
    ```
    
2. Especifique o canal `stable`
    
    ```bash
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/lens-archive-keyring.gpg] https://downloads.k8slens.dev/apt/debian stable main" | sudo tee /etc/apt/sources.list.d/lens.list > /dev/null
    ```
    
3. Instale o Lens Desktop:
    
    ```bash
    sudo apt update
    sudo apt install lens
    ```

### KIND

1. Instale com o comando:

    ```bash
    # PARA AMD64 / x86_64
    [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.24.0/kind-linux-amd64
    # PARA ARM64
    [ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.24.0/kind-linux-arm64
    ```

2. Adicione permissão de execução e o caminho para o comando Kind:

    ```bash
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind
    ```

3. Teste o funcionamento:

    ```bash
    kind --version
    ```

### kubectl

1. Download the latest release with the command:

   ```bash
    # PARA AMD64 / x86_64
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    # PARA ARM64
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl"
    ```

2. Validate the binary (optional):

    ```bash
    # PARA AMD64 / x86_64
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    # PARA ARM64
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl.sha256"
    ```
    - Validate the kubectl binary against the checksum file:

    ```bash
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    ```
    
    - If valid, the output is:

    ```bash
    kubectl: OK
    ```

    - If the check fails, sha256 exits with nonzero status and prints output similar to:

    ```bash
    kubectl: FAILED
    sha256sum: WARNING: 1 computed checksum did NOT match
    ```

3. Install kubectl:

    ```bash
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

    - Note:

        If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:

        ```bash
        chmod +x kubectl
        mkdir -p ~/.local/bin
        mv ./kubectl ~/.local/bin/kubectl
        # and then append (or prepend) ~/.local/bin to $PATH
        ```

4. Test to ensure the version you installed is up-to-date:

    ```bash
    kubectl version --client
    ```

5. Install metrics server

    - Metrics Server is a scalable, efficient source of container resource metrics for Kubernetes built-in autoscaling pipelines.
    - link: https://github.com/kubernetes-sigs/metrics-server
    - Kubelet certificate needs to be signed by cluster Certificate Authority (or disable certificate validation by passing --kubelet-insecure-tls to  Metrics Server)
    ```bash
    #local - configure o arquivo com o --kubelet-insecure-tls - segue o exemplo no arquivo metrics-server.yml - depois rode kubectl apply -f metrics-server.yml
    wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
    #env
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
    ```

    - Verify metrics:
    ```bash
    kubectl get po -n kube-system
    ```

