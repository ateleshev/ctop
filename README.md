<p align="center"><img width="200px" src="/_docs/img/logo.png" alt="ctop"/></p>

#

Top-like interface for container metrics

`ctop` provides a concise and condensed overview of real-time metrics for multiple containers:
<p align="center"><img src="_docs/img/grid.gif" alt="ctop"/></p>

as well as an [expanded view][expanded_view] for inspecting a specific container.

`ctop` currently comes with built-in support for Docker; connectors for other container and cluster systems are planned for future releases.

## Install

Fetch the [latest release](https://github.com/bcicen/ctop/releases) for your platform:

#### Linux

```bash
wget https://github.com/bcicen/ctop/releases/download/v0.5/ctop-0.5-linux-amd64 -O ctop
sudo mv ctop /usr/local/bin/
sudo chmod +x /usr/local/bin/ctop
```

#### OS X

```bash
brew install ctop
```
or
```bash
curl -Lo ctop https://github.com/bcicen/ctop/releases/download/v0.5/ctop-0.5-darwin-amd64
sudo mv ctop /usr/local/bin/
sudo chmod +x /usr/local/bin/ctop
```

or run via Docker:
```bash
docker run -ti --name ctop --rm -v /var/run/docker.sock:/var/run/docker.sock quay.io/vektorlab/ctop:latest
```

`ctop` is also available for Arch in the [AUR](https://aur.archlinux.org/packages/ctop-bin/)

## Building

To build `ctop` from source, ensure you have a recent version of [glide](http://glide.sh/) installed and run:

```bash
git clone https://github.com/bcicen/ctop.git $GOPATH/src/github.com/bcicen/ctop && \
cd $GOPATH/src/github.com/bcicen/ctop && \
glide install && \
go build
```

To build a minimal Docker image containing only `ctop`, follow the build instructions above up through `glide install`, then:

```bash
CGO_ENABLED=0 go build -a
[[ ! -d docker-build ]] && mkdir docker-build
cd docker-build && cp ../ctop ./
cat > Dockerfile <<- "EOF"
FROM scratch
COPY ./ctop /ctop
ENTRYPOINT ["/ctop"]
EOF
docker build -t ctop .
```

Now you can run ctop as above:

```bash
docker run -ti --name ctop --rm -v /var/run/docker.sock:/var/run/docker.sock ctop
```

## Usage

`ctop` requires no arguments and will configure itself using the `DOCKER_HOST` environment variable
```bash
export DOCKER_HOST=tcp://127.0.0.1:4243
ctop
```

### Options

Option | Description
--- | ---
-a	| show active containers only
-f <string> | set an initial filter string
-h	| display help dialog
-i  | invert default colors
-r	| reverse container sort order
-s  | select initial container sort field
-v	| output version information and exit

### Keybindings

Key | Action
--- | ---
a | Toggle display of all (running and non-running) containers
f | Filter displayed containers (`esc` to clear when open)
H | Toggle ctop header
h | Open help dialog
s | Select container sort field
r | Reverse container sort order
q | Quit ctop

[expanded_view]: _docs/expanded.md
