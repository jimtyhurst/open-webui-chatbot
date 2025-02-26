# open-webui-chatbot

This project specifies the development environment for
using [DeepSeek R1](https://www.deepseek.com/)
with [Ollama](https://ollama.com/)
and [Open-WebUI](https://www.openwebui.com/)
on your local workstation.

There is no source code! The sole purpose of this project is to set up
a virtual environment in which to run `Open-WebUI`.

## Contents

- [Step 1: Install ollama](#step-1-install-ollama)
- [Step 2: Run Open-WebUI](#step-2-run-open-webui)
    - [Option 1: Run Open-WebUI in a Docker container](#option-1-run-open-webui-in-a-docker-container)
    - [Option 2: Install Open-WebUI in a Python virtual environment](#option-2-install-open-webui-in-a-python-virtual-environment)
- [Step 3: Query models from Open-WebUI](#step-3-query-models-from-open-webui)
- [License](#license)

## Step 1: Install ollama

Download the installer from: https://ollama.com/

See the [documentation for ollama](https://github.com/ollama/ollama) for more information.
The `ollama` app is used to run LLM models on your local machine.
For this project, I compared [DeepSeek-R1 7B](https://huggingface.co/deepseek-ai/DeepSeek-R1)
and [Gemma-2 27B](https://huggingface.co/google/gemma-2-27b).

It is possible to run `ollama` as a stand-alone app, but in this exercise, it gets called from
[Open-WebUI](https://www.openwebui.com/). So you need to install `ollama`, but then you only run it indirectly
through the `Open-WebUI` interface.

## Step 2: Run Open-WebUI

I found it easiest to run `Open-WebUI` from a Docker container,
but it is also possible to run `Open-WebUI` from the command line.

### Option 1: Run Open-WebUI in a Docker container

This command starts an image running in the background:

```shell
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v \
  open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
```

Then open a browser to http://localhost:3000, where you will be prompted to create an admin account
or login if you created an admin account previously.
See the [Quick Start for Open-WebUI](https://docs.openwebui.com/getting-started/quick-start/)
for more information.

### Option 2: Install Open-WebUI in a Python virtual environment

#### 1. Create a virtual environment for Python

I use the [uv](https://docs.astral.sh/uv/) tool, which makes it easy to
manage the version of Python and the packages needed for this environment.
You can use whatever method that you typically use to create a Python virtual environment.

```shell
uv venv --python 3.11
```

Note: I had to use Python 3.11, in order for all of the package dependencies
to install correctly. I am using macOS Sequoia 15.3.1 with an Apple M4 Pro chip.
When I tried to use Python 3.12 or 3.13, the package installation failed, even though
the [open-webui documentation](https://pypi.org/project/open-webui/) says that it
is compatible with Python 3.12.
It failed to install [open-webui](https://docs.openwebui.com/)'s dependencies due to some packages
not being available for my environment.

#### 2. Install Open-WebUI

```shell
uv sync
```

If you are not using `uv`, you will need to `pip` install the `Open-WebUI` package:

```shell
source .venv/bin/activate
python -m pip install open-webui
```
#### 3. Run the server from the command line

```shell
uv run open-webui serve
```
-or-
```shell
open-webui serve
```

Then open a browser to http://localhost:8080, where you will be prompted to create an admin account
or login if you created an admin account previously.

## Step 3: Query models from Open-WebUI

After logging in to `Open-WebUI`, you can:

1. Install models;
2. Create knowledge bases to use for Retrieval Augmented Generation (RAG); and
3. Submit prompts to a model.

See the [documentation for Open-WebUI](https://docs.openwebui.com/getting-started/)
for instructions on how to perform those tasks.

## License

MIT License

Copyright (c) 2025 [Jim Tyhurst](https://jimtyhurst.com)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
