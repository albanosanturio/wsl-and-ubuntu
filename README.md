# wsl-and-ubuntu



# Readme file

Readme file edited with markdown, testing md features.


## Code format

```python
import foobar

# returns 'words'
foobar.pluralize('word')

# returns 'geese'
foobar.pluralize('goose')

# returns 'phenomenon'
foobar.singularize('phenomena')
```

# TITLE1
## TITLE2
### TITLE3
Adding a # symbol at the begining, you can get different title formats Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.



## License
[MIT](https://choosealicense.com/licenses/mit/)

## Reference
https://www.makeareadme.com/ 
https://www.markdownguide.org/basic-syntax/#reference-style-links


<body>
<header id="title-block-header">
<h1 class="title">Guide for local training environment</h1>
</header>
<div id="content">
<h1 class="title" id="local-training-environment">local-training-environment</h1>
<div id="outline-container-org17b60fb" class="outline-2">
<h2 id="org17b60fb">Pages to check</h2>
<div id="text-org17b60fb" class="outline-text-2">
<ul>
<li><a href="https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9">https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9</a></li>
<li><a href="https://learn.microsoft.com/en-us/windows/wsl/install#change-the-default-linux-distribution-installed">https://learn.microsoft.com/en-us/windows/wsl/install#change-the-default-linux-distribution-installed</a></li>
<li><a href="https://learn.microsoft.com/en-us/windows/wsl/setup/environment">https://learn.microsoft.com/en-us/windows/wsl/setup/environment</a></li>
</ul>
</div>
</div>
<div id="outline-container-org08cd3fd" class="outline-2">
<h2 id="org08cd3fd">wsl machine</h2>
<div id="text-org08cd3fd" class="outline-text-2">
<p>In Power Shell</p>
<p><code>wsl --set-default-version 2</code></p>
<p><code>wsl --install -d Ubuntu</code></p>
<p>If you have a machine which you would like to use confirm that it is
version 2</p>
<p>Check the version with</p>
<p><code>wsl -l -v</code></p>
<p>If it is version 1, you can convert it with</p>
<p><code>wsl --set-version Ubuntu 2</code></p>
<p>Check that your user is in sudoers. As root.</p>
<p><code>grep -E &#39;%sudo|%wheel&#39; /etc/sudoers</code></p>
<p><code>sudo apt update &amp;&amp; sudo apt upgrade</code></p>
</div>
</div>
<div id="outline-container-org8e53356" class="outline-2">
<h2 id="org8e53356">docker</h2>
<div id="text-org8e53356" class="outline-text-2">
<p>In the Ubuntu created machine</p>
<p><code>apt remove docker docker-engine.io containerd runc</code></p>
<p><code>sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2</code></p>
<p><code>update-alternatives --config iptables</code></p>
<p><code>. /etc/os-release</code></p>
<p><code>curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc</code></p>
<p><code>echo &quot;deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable&quot; | sudo tee /etc/apt/sources.list.d/docker.list</code></p>
<p><code>sudo apt update</code></p>
<p><code>sudo apt install docker-ce docker-ce-cli containerd.io</code></p>
<p><code>sudo groupmod -g 36257 docker</code></p>
<p><code>sudo usermod -aG docker $USER</code></p>
<p>exit (to PowerShell)</p>
<p>wsl (to return with the group applied)</p>
<p><code>DOCKER_DIR=/mnt/wsl/shared-docker</code></p>
<p><code>mkdir -pm o=,ug=rwx &quot;$DOCKER_DIR&quot;</code></p>
<p><code>sudo chgrp docker &quot;$DOCKER_DIR&quot;</code></p>
<p><code>sudo mkdir /etc/docker/</code></p>
<p><code>sudo su</code></p>
<pre id="org51f02ff" class="example"><code>cat &gt;&gt; /etc/docker/daemon.json &lt;&lt;EOF
{
  &quot;hosts&quot;: [&quot;unix:///mnt/wsl/shared-docker/docker.sock&quot;]
}
EOF</code></pre>
<p><code>exit</code></p>
<p><code>sudo apt install screen</code></p>
<p><code>sudo dockerd</code></p>
<p><code>Ctrl-A Ctrl-C (new screen window)</code></p>
<p><code>export DOCKER_HOST=&quot;unix:///mnt/wsl/shared-docker/docker.sock&quot;</code></p>
<p><code>docker run --rm hello-world</code></p>
<p>Add to ~/.bashrc</p>
<pre id="orgc012804" class="example"><code>DOCKER_DISTRO=&quot;Ubuntu&quot;
DOCKER_DIR=/mnt/wsl/shared-docker
DOCKER_SOCK=&quot;$DOCKER_DIR/docker.sock&quot;
export DOCKER_HOST=&quot;unix://$DOCKER_SOCK&quot;
if [ ! -S &quot;$DOCKER_SOCK&quot; ]; then
    mkdir -pm o=,ug=rwx &quot;$DOCKER_DIR&quot;
    chgrp docker &quot;$DOCKER_DIR&quot;
    /mnt/c/Windows/System32/wsl.exe -d $DOCKER_DISTRO sh -c &quot;nohup sudo -b dockerd &lt; /dev/null &gt; $DOCKER_DIR/dockerd.log 2&gt;&amp;1&quot;
fi</code></pre>
<p><code>sudo visudo</code></p>
<p>Add at the end of the file:</p>
<p><code>%docker ALL=(ALL)  NOPASSWD: /usr/bin/dockerd</code></p>
<p>Save and exit</p>
</div>
</div>
<div id="outline-container-orgce6a433" class="outline-2">
<h2 id="orgce6a433">docker-compose</h2>
<div id="text-orgce6a433" class="outline-text-2">
<p><code>sudo curl -L https://github.com/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose</code></p>
<p><code>sudo chmod +x /usr/local/bin/docker-compose</code></p>
<p>Check with</p>
<p><code>docker-compose --version</code></p>
</div>
</div>
</div>
<div id="postamble" class="status">
<p>Date: 2022-11</p>
<p>Created: 2022-11-02 Wed 03:00</p>
<p><a href="https://validator.w3.org/check?uri=referer">Validate</a></p>
</div>
</body>


