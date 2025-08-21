# PDF to Audio RAG
## Introduction

This sample demonstrates a Retrieval-Augmented Generation (RAG) system using a PDF document as the knowledge base, with audio input and audio output.\
The PDF content is converted into embeddings using [**Teapot LLM**](https://huggingface.co/teapotai/teapotllm), forming the knowledge base for the RAG pipeline. The user provides an audio query, which is transcribed to text using the [**Whisper large-v3 model**](https://huggingface.co/openai/whisper-large-v3) via an Automatic Speech Recognition (ASR) pipeline. The transcribed text query is then processed using the Teapot LLM RAG system running on **Intel® Core™ Ultra Processors**. The resulting text response is converted back to speech output using the [**SpeechT5 model**](https://huggingface.co/microsoft/speecht5_tts) through a Text-to-Speech (TTS) pipeline.
This audio response can be played directly in the notebook and also saved locally. These models will leverage the [**PyTorch XPU backend**](https://docs.pytorch.org/docs/stable/notes/get_start_xpu.html) for Intel hardware acceleration.

---

## Table of Contents

- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installing Prerequisites && Setting Up the Environment](#installing-prerequisites--setting-up-the-environment)
   - [For Windows](#for-windows)
   - [For Linux](#for-linux)
- [Running the Sample && execution output](#running-the-sample--execution-output)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Architecture

- User provides a *PDF document* to serve as the knowledge base.
- The document is loaded using the [*Langchain PyPDFLoader*](https://python.langchain.com/docs/integrations/document_loaders/pypdfloader/) and split into smaller chunks using the [*Langchain Text Splitter*](https://python.langchain.com/docs/how_to/recursive_text_splitter/).
- These chunks are embedded using the [*Teapot LLM*](https://huggingface.co/teapotai/teapotllm) to form a searchable knowledge base.
- User submits an *audio query*, which is transcribed to text using the [*Whisper large-v3 model*](https://huggingface.co/openai/whisper-large-v3) (ASR).
- Transcribed query is passed to the RAG system, which retrieves relevant information and generates a response.
- The response text is then converted to *audio* using the [*SpeechT5 model*](https://huggingface.co/microsoft/speecht5_tts) (TTS), which can be played and saved.

![How it works](./assets/Pdf_to_audio_rag_workflow.png)

---

## Project Structure

    PDF-to-Audio-RAG/                                               # Project Sample folder
    ├── assets/                                                     # Assets folder which contains the images and diagrams
    │   ├── Audio_play.png                                          # Output screenshot image 2
    │   ├── Generating_audio_transcription_using_Pytorch_XPU.png    # Output screenshot image 1
    │   └── Pdf_to_audio_rag_workflow.png                           # Workflow image
    ├── PDF_to_Audio_RAG.ipynb                                      # Notebook file to excute the project sample
    ├── Readme.md                                                   # Readme file which contains all the details and instructions about the project sample
    ├── dpcpp_cpp_compiler_developer_guide_and_reference.pdf        # Document file for knowledge base
    ├── input_audio_query.wav                                       # Sample Input Audio Query
    ├── pyproject.toml                                              # Requirements for the project sample
    └── uv.lock                                                     # File which captures the packages installed for the project sample
   
---

## Prerequisites

|    Component   |   Recommended   |
|   ------   |   ------   |
|   Operating System(OS)   |   Windows 11 or later/ Ubuntu 20.04 or later   |
|   Random-access memory(RAM)   |   16 GB   |
|   Hardware   |   Intel® Core™ Ultra Processors, Intel Arc™ Graphics, Intel Graphics   |

---

## Installing Prerequisites && Setting Up the Environment

### For Windows:
To install any software using commands, Open the Command Prompt as an administrator by right-clicking the terminal icon and selecting `Run as administrator`.
1. **GPU Drivers installation**\
   Download and install the Intel® Graphics Driver for Intel® Arc™ B-Series, A-Series, Intel® Iris® Xe Graphics, and Intel® Core™ Ultra Processors with Intel® Arc™ Graphics from [here](https://www.intel.com/content/www/us/en/download/785597/intel-arc-iris-xe-graphics-windows.html)\
   **IMPORTANT:** Reboot the system after the installation.

2. **CMake for Windows**\
   Download and install the latest CMake for Windows from [here](https://cmake.org/download/)

3. **Git for Windows**\
   Download and install Git from [here](https://git-scm.com/downloads/win)

4. **uv for Windows**\
   Steps to install `uv` in the Command Prompt are as follows. Please refer to the [documentation](https://docs.astral.sh/uv/getting-started/installation/) for more information.
   ```
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```
   **NOTE:** Close and reopen the Command Prompt to recognize uv.
   
### For Linux:
To install any software using commands, Open a new terminal window by right-clicking the terminal and selecting `New Window`.
1. **GPU Drivers installation**\
   Download and install the GPU drivers from [here](https://dgpu-docs.intel.com/driver/client/overview.html)

2. **Dependencies on Linux**\
   Install CMake, Curl, Wget, Git using the following commands:
   - For Debian/Ubuntu-based systems:
   ```
   sudo apt update && sudo apt -y install cmake curl wget git
   ```
   - For RHEL/CentOS-based systems:
   ```
   sudo dnf update && sudo dnf -y install cmake curl wget git
   ```

3. **uv for Linux**\
   Steps to install uv are as follows. Please refer to the [documentation](https://docs.astral.sh/uv/getting-started/installation/) for more information.
   - If you want to use curl to download the script and execute it with sh:
   ```
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
   - If you want to use wget to download the script and execute it with sh:
   ```
   wget -qO- https://astral.sh/uv/install.sh | sh
   ```
   **NOTE:** Close and reopen the Terminal to recognize uv.

---

## Running the Sample && execution output
   
1. In the Command Prompt/terminal, navigate to `PDF-to-Audio-RAG` folder after cloning the sample:
   ```
   cd <path/to/PDF-to-Audio-RAG/folder>
   ```
   
2. Sync the UV environment:\
   On Windows:
   ```
   set CMAKE_POLICY_VERSION_MINIMUM=3.5
   uv sync
   ```
   On Linux:
   ```
   uv sync
   ```
3. Log in to Hugging Face, generate a token, and download the required models and datasets:\
   `huggingface-cli` lets you interact directly with the Hugging Face Hub from a terminal. Log in to [Huggingface](https://huggingface.co/) with your credentials. You need a [User Access Token](https://huggingface.co/docs/hub/security-tokens) from your [Settings page](https://huggingface.co/settings/tokens). The User Access Token is used to authenticate your identity to the Hub.\
   Once you have your token, run the following command in your terminal.
   ```
   uv run huggingface-cli login
   ```
   This command will prompt you for a token. Copy-paste yours and press Enter.
   ```
   uv run huggingface-cli download openai/whisper-large-v3
   uv run huggingface-cli download microsoft/speecht5_tts
   uv run huggingface-cli download teapotai/teapotllm model.safetensors
   uv run huggingface-cli download teapotai/teapotembedding
   uv run huggingface-cli download Matthijs/cmu-arctic-xvectors --repo-type dataset
   ```

4. Launch Jupyter Lab and Run the notebook:\
   Open the [PDF-to-Audio-RAG](./PDF_to_Audio_RAG.ipynb) notebook in the Jupyter Lab.
   - In the Jupyter Lab go to the kernel menu in the top-right corner of the notebook interface and choose default kernel i.e. `Python 3 (ipykernel)` from the available kernels list and run the code cells one by one in the notebook.
   ```
   uv run jupyter lab
   ```

5. GPU utilization can be seen in the Task Manager while generating audio transcription for input audio query which is processing on Intel XPUs.
   ![Generating_audio_transcription_using_Pytorch_XPU.png](./assets/Generating_audio_transcription_using_Pytorch_XPU.png)

6. The audio response can be played and saved.
   ![Audio_play](./assets/Audio_play.png)

---

## Troubleshooting

- **Dependency Issues:** Run `uv clean` and then `uv sync`.
- **CMake compatibility issues**: Run `set CMAKE_POLICY_VERSION_MINIMUM=3.5` to prevent building issues.

---

## License

This project is licensed under the MIT License. See [LICENSE](../LICENSE) for details.
