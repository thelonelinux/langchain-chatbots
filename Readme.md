# LANGCHAIN CHATBOTS AND STUFFS PROJECTS
* following youtube video : https://www.youtube.com/watch?v=D74el9mvNak&t=1261s
* HERE WE WILL DO UV WAY RATHER THAN PIP
* To learning about MCP server - same way video explained
  * https://www.youtube.com/watch?v=tzrwxLNHtRY
* To get langchain api key - same way video explained
  * https://www.youtube.com/watch?v=A9Awgbgx7vY


## SETUP THE PROJECT
* https://docs.astral.sh/uv/getting-started/installation/#__tabbed_1_2
* first check python version in terminal visual studio
* USE BELOW COMMAND TO INSTALL UV
PS C:\Users\HP\Desktop\langchain-chatbots> powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
downloading uv 0.11.28 (x86_64-pc-windows-msvc)
installing to C:\Users\HP\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

To add C:\Users\HP\.local\bin to your PATH, either restart your shell or run:

    set Path=C:\Users\HP\.local\bin;%Path%   (cmd)
    $env:Path = "C:\Users\HP\.local\bin;$env:Path"   (powershell)

* USE BELOW COMMAND TO SET PATH FOR UV
PS C:\Users\HP\Desktop\langchain-chatbots>  $env:Path = "C:\Users\HP\.local\bin;$env:Path"                     
PS C:\Users\HP\Desktop\langchain-chatbots> uv --version
uv 0.11.28 (ebf0f43d7 2026-07-07 x86_64-pc-windows-msvc)
PS C:\Users\HP\Desktop\langchain-chatbots> 

### 1. FIRST USE COMMAND TO CREATE BOILER CODE PYTHON PROJECT
* $$ uv init
* UV is alternative to PIP. It makes python package installation very very easy.
* This command will setup the boilder code for setups and tools
* This will create file pyproject.toml and .python-version file

#### USE CASE OF PYPROJECT.TOML FILE
In this project, pyproject.toml is the project’s Python configuration file. It is used to define:

- the package/project metadata, such as name, version, and description
- the Python version requirement
- dependencies for the project

In your current file, it is very minimal, so it is mainly acting as a placeholder for the project setup. Once you add libraries like LangChain, OpenAI, or other packages, you would list them under the dependencies section so tools like uv or pip can install them correctly.

In short, it is the central file for managing how this Python project is built and what dependencies it needs.

## 2. Add this dependencies in pyproject.toml file
Dependencies
dependencies = [
    "langchain>=0.3.0",
    "langchain-core>=1.0.0",
    "langchain-google-genai>=4.2.2",
    "langchain-groq>=0.3.0",
    "python-dotenv>=1.2.2",
    "streamlit>=1.57.0",
    "ipykernel>=7.2.0",
    "notebook>=7.5.6",
]
* Save the pyproject.toml and run this below command
* PS C:\Users\HP\Desktop\langchain-chatbots> uv sync
* This will download all the dependencies mentioned in the pyproject.toml file and add the default .venv environment where all these libraries/dependencies are stored

## 3. Activate the virutal Environment
* PS C:\Users\HP\Desktop\langchain-chatbots> .\.venv\Scripts\activate

## 4. Create Jupyter Notebook to run the LLMs code
* Run this below command
* (langchain-chatbots) PS C:\Users\HP\Desktop\langchain-chatbots> python -m notebook
* This will open notebook web page with your projects : http://localhost:8888/tree

* See vidoes for how jupyter create notebook there

## 5. Coding part.
* 1. need api key to run the llms so we use dotenv.
* And using : from langchain_google_genai import ChatGoogleGenerativeAI
* This you can find in below page, and see the documentaiton on how it can be used and instantiated.
  * https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai

### Main steps
* 1. API Key
* 2. Create the model (Instantiate the model)
* 3. Invoke the model

### 1. For API Key
* Create the file named : .env
* Here you will specify api keys and other settings details etc. This is like appsettings file in .net
* for now we are using gemini model only, so only give gemini api key
* For gemini get google api key from this location : Head to Google AI Studio to generate an API key:
* https://aistudio.google.com/api-keys?project=gen-lang-client-0332160066
* ONCE YOU ADD KEY, FIRST RESTART JUPYTER AGAIN, SAVE IT AND RUN IN BROWSER JUPYTER

#### MAKE SURE YOU CHOSE THE SAME LANGCHAIN-ENVIRONMENT ONLY WHEN SELECTING FOR INNTER PROJECTS ALSO IN IPYNB FILES 
* These will ask for env. and select env which you have created, as there only all our dependencies are present.
* In other env if you chose then it wont recognise
* Just go to visual studio search : Type angular braces and search for interpretor or env (<)
* also inside pynb files also at top right side it will show you env name it is useing, just click on it and select the env which you project is using for that file as well. or it won't run.


## 6. PUSH THE CODE TO GITHUB
Checking project config to explain uv.lock accurately and confirm the recent Git history changes.

Read [](file:///c%3A/Users/HP/Desktop/langchain-chatbots/uv.lock#1-1), lines 1 to 120

Read [](file:///c%3A/Users/HP/Desktop/langchain-chatbots/pyproject.toml)

### What I exactly did
* Remember you will be  unable to push the secret content present in .env, that API keys, so git push won't allow
- Added .gitignore with .env so Git stops tracking that file in the future.
- Rewrote your branch history with:
  - `git filter-branch --force --index-filter "git rm --cached --ignore-unmatch .env" --prune-empty --tag-name-filter cat -- --all`
- That removed .env from every commit in the branch history, including the offending commit that GitHub secret scanning flagged.
- Then I force-pushed `main` to GitHub so the cleaned history replaced the old history and the push protection was resolved.

> In short: I removed the sensitive .env file from Git history and ensured Git ignores it going forward.

### What uv.lock is for

- uv.lock is a Python dependency lockfile.
- It records the exact resolved packages, versions, hashes, and platforms for the dependencies declared in pyproject.toml.
- That makes installs reproducible: everyone using the same lockfile gets the same dependency tree.
- In this project, uv.lock is the locked result of the `uv` package manager resolving `langchain`, `streamlit`, `python-dotenv`, etc.

So uv.lock is not a secret — it’s a dependency lockfile used for stable environment setup.


## 7. .env.example file usecase
Is there any other way that we can add .env file in github without any issue
### Short answer

No — if .env contains real secrets, you should not add it to GitHub.

### Why

- GitHub push protection and secret scanning will block commits that include API keys, passwords, or tokens.
- Even if you add .env to .gitignore, it only prevents future tracking, not already committed secrets.
- Committing secrets is a security risk because they can be leaked, reused, or abused.

### Safer alternatives

1. Use `.env.example`
   - Commit a template file with names only:
     - `OPENAI_API_KEY=`
     - `GCP_PROJECT=`
   - This documents required values without exposing secrets.

2. Use GitHub Secrets
   - Store actual values in:
     - GitHub Actions secrets
     - GitHub repository secrets
   - Use those at runtime instead of committing .env.

3. Use environment-specific secret storage
   - `aws secretsmanager`
   - `azure key vault`
   - `vault`
   - `doppler`, `onesh` etc.

### If you really need a tracked config file

Then do not put real secrets in it. Use:
- placeholders in the tracked file
- runtime environment variables
- an encrypted file solution like `git-crypt` / `sops` only if you need encrypted repo data

### Recommendation

- Keep .env out of GitHub.
- Commit `.env.example`.
- Put real secrets in GitHub Secrets or another secure secret manager.

> If you want, I can help you create `.env.example` and update your app to load secrets from environment variables safely.

