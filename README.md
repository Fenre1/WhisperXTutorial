# WhisperX Tutorial

This tutorial will guide you through installing and using WhisperX, an enhanced version of OpenAI's Whisper. With WhisperX, you can automatically transcribe audio files, such as interviews and CVR/ATC recordings (although we have conducted only limited testing with CVRs). 

The tutorial is written for a PC running Windows 11, but WhisperX should also work on Linux and macOS. You will need administrator access to install the required software. Additionally, you will need to create a Hugging Face account (see Step 3). The tutorial covers both the GPU and CPU versions. If everything goes smoothly, completing the tutorial should take about an hour.

## Key Notes
- The **GPU version** is approximately 10 times faster than the CPU version. It requires an NVIDIA GPU with at least **4 GB of VRAM**. To check if you have a compatible GPU, go to **Task Manager > Performance > GPU**, and confirm it is an NVIDIA GPU with at least **4 GB of Dedicated GPU memory**.
- For the initial installation and first run of the script, an internet connection is required to download the software and the Whisper model. After downloading the model, WhisperX can be run entirely offline if needed.

If you encounter issues, start by asking ChatGPT for help. Copy this entire tutorial into your ChatGPT prompt, explain the steps you took, and describe the issue. If you're still unable to resolve it, feel free to email me.

If you notice any problems with this tutorial or find anything incomplete, please let me know!

---

## Tutorial Overview

This tutorial consists of 4 steps:
1. Install Python
2. Install required Python packages
3. Create a Hugging Face account
4. Run the script

---

## Step 1: Install Python

While there are various ways to install and manage Python, this tutorial uses Anaconda for simplicity and ease of use.

1. **Download and Install Anaconda:**
   - Go to [Anaconda](https://www.anaconda.com/download/success) and select the Windows installer.
   - Use the default installation options. Ensure the option **"Register Anaconda3 as the system Python"** is selected. *(Note: If another Python version is already installed, this may cause conflicts, so proceed with caution.)*

2. **Launch Anaconda Navigator.**

3. **Create a Python Environment:**
   - In Anaconda Navigator, go to the **Environments** tab on the left.
   - Click **[+] Create** (bottom-left corner). Name the environment (e.g., `whisper_env`) and select Python version **3.12.5** from the dropdown menu.
   - Click **Create** to finish. The new environment should now appear in the list, below **base (root)**.

---

## Step 2: Install Required Python Packages

Python packages are collections of pre-written code that add specific features or functionality to your projects. For this tutorial, we will install the required packages in the environment you just created.

### Steps

1. **Download WhisperX:**
   - Go to [WhisperX GitHub Repository](https://github.com/cvl01/whisperX), click the green **<> Code** button, and select **Download ZIP**.
   - Unzip the file to a folder of your choice (e.g., `C:\Data\whisperX-main`).

2. **Activate the Environment:**
   - In Anaconda Navigator, select your environment (`whisper_env`) and click the green play button.
   - Choose **Open Terminal** from the menu to open a command window.

3. **Navigate to the WhisperX Folder:**
   ```bash
   cd /d C:\Data\whisperX-main


*(Replace the folder path if you unzipped WhisperX elsewhere.)*

4. **Install PyTorch (Torch):**
   - If you **do not have a compatible GPU**, run:
     ```bash
     pip3 install torch torchvision torchaudio
     ```
   - If you **have a compatible GPU**, run:
     ```bash
     pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
     ```
   - *(Note: The tutorial uses PyTorch version `2.5.1`, which may differ from the latest version.)*

5. **Install Additional Required Packages:**
   Run the following commands one by one:
   ```bash
   pip install pandas==2.2.3
   pip install transformers==4.47.1
   pip install nltk==3.9.1
   pip install pyannote.audio==3.3.2
   pip install ctranslate2==3.24.0
   pip install faster-whisper==1.0.3
   pip install python-docx==1.1.2
   pip install python-dotenv==1.0.1

6. **Add the Base Script:**
   - Download the `base_transcription.py` file and place it in the `whisperX-main` folder.
   - Alternatively, copy the script text into a `.txt` file, rename the file extension to `.py`, and move it to the `whisperX-main` folder.

---

## Step 3: Create a Hugging Face Account

WhisperX uses a diarization model from Hugging Face, which requires an account.

1. Go to [Hugging Face](https://huggingface.co) and create an account.
2. In your account, navigate to **Access Tokens** (located in the top-right menu under your profile) and click **+ Create new token**.
   - Choose "Read" as the token type and give it a name (e.g., `whisper_token`).
   - Copy the token and create a `.env` file in the `whisperX-main` folder:
     ```env
     HF_TOKEN=your_token_here
     ```
   - Save the file as `.env`. *(If Windows gives an error, use "Save As", select "All file types", and name the file `.env`.)*

---

## Step 4: Run the Script

1. Use the following command format in the terminal:
   ```bash
   python base_transcription.py <audio_folder_path> --language=<language_code> --max_speakers=<number_of_speakers>
Example:
  ```bash
   python base_transcription.py C:\Data\audio --language=en --max_speakers=3
  ```
2. **Optional Arguments:**
   - To force CPU usage (even if a GPU is installed), add the following flag to the command:
     ```bash
     --device=cpu
     ```

3. **Output Files:**
   - The outputs are saved in the `output` subfolder within the specified audio folder. These files include:
     - `.docx` and `.txt` files containing the diarized text with timestamps.
     - A subfolder named `large-v3` with:
       - `.json` containing the transcription, timestamps, word scores, and other details (e.g., for use with other tools).
       - `.srt` for subtitles (useful if the audio is part of a video).
       - `.txt` containing the line-by-line transcription.

---

## Advanced Topics (Not Covered in this Tutorial)
Some additional features of WhisperX that were discussed but are beyond the scope of this tutorial include:
- Transcribing audio files containing multiple languages.
- Fine-tuning (training) your own Whisper model.
- Using a large language model to proofread and refine transcripts.
- Denoising audio files for better transcription quality.

---

