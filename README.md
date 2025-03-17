<!-- Sentient Banner -->
<p align="center">
  <img src="banner.png"/>
</p>

<!-- Socials -->
<p align="center">
      <a href="https://sentient.xyz/" target="_blank" style="margin: 2px;">
    <img alt="Homepage" src="
    <!-- License -->
    <a href="https://github.com/sentient-agi/Sentient-Agent-Framework/tree/main?tab=Apache-2.0-1-ov-file">
        <img alt="License" src="https://img.shields.io/badge/License-Apache_2.0-green">
    </a>
</p>


<h1 align="center">Sentient Agent Framework</h1>

> [!NOTE]
> **This framework is currently in beta.**

The Sentient Agent Framework is a lightweight framework, with minimal dependencies, for building autonomous AI agents for social platforms. Aligned with Sentient's mission, the framework is designed to be easily extensible and it is open to community contributions. Create an issue to ask a question or open a PR to add a feature!



# Features 🦾
For now the only platforms that are supported by the framework are X (Twitter) and Discord. We're going to continuously work on this framework. If there is a tool or platform that you would like to see supported, please create an issue or, better yet, get coding and open a PR! Telegram support is the next feature in the pipeline. We also plan to add tools to support more sophisticated features, such as data sources and on-chain functionality.
- [x] Supports any OpenAI API compatible LLM endpoint
- [x] Supports X (Twitter)
- [x] Supports Discord
- [ ] Telegram is coming soon...
- [ ] Data is coming soon...
- [ ] Web3 is coming soon...



# Quickstart 🚀
### [1/4]&nbsp;&nbsp;Set Up Agent Credentials

> [!NOTE]
> **We suggest creating a new X account for your agent.**

#### 1.1. Create secrets file
Create the `.env` file by copying the contents of `.env.example`. This is where you will store all of your agent's credentials.
```
cp .env.example .env
```

#### 1.2. Add model credentials
Add your Fireworks API key to the `.env` file (you can also use any other OpenAI compatible inference provider).

#### 1.3. Add X (Twitter) credentials
In order to interact with the X (Twitter) API, your agent needs developer credentials from the X (Twitter) developer portal [here](https://developer.x.com/en/portal/dashboard).

From the *Dashboard* page, click on the gear icon to access the *Settings* page for your default project.

Set the user authentication settings for your app as follows:
- App permissions: "Read and write"
- Type of App: "Web App, Automated App or Bot"
- Callback URI / Redirect URL: http://localhost:8000
- Website URL: http://example.com

Generate all of the required credentials on the *Keys and tokens* page. Add them to the `.env` file.

#### 1.4. Add Discord credentials
Before you can build a Discord bot you need to create it in the Discord Developer Portal. Follow the steps in the *Creating an App* part of [this](https://discord.com/developers/docs/quick-start/getting-started#step-1-creating-an-app) guide and add your token to the `.env` file.


### [2/4]&nbsp;&nbsp;Set Up Python Virtual Environment
> [!NOTE]
> **These instructions are for unix-based systems (i.e. MacOS, Linux). Before you proceed, make sure that you have installed `python` and `pip`. If you have not, follow [these](https://packaging.python.org/en/latest/tutorials/installing-packages/) instructions to do so.**

#### 2.1. Create Python virtual environment
```
python3 -m venv .venv
```

#### 2.2. Activate Python virtual environment
```
source .venv/bin/activate
```

#### 2.3. Install dependencies
```
pip install -r requirements.txt
```


### [3/4]&nbsp;Test Agent Tools
#### 3.1. Test Connection to Model
```
python3 -m src.agent.agent_tools.model
```
Expected output:
```
Connecting to model...
To exit just type 'exit' and press enter.
Query model: 
```

#### 3.2. Test Connection to Twitter
```
python3 -m src.agent.agent_tools.twitter
```
Expected output:
```
Connecting to twitter...
Connected to twitter user <USERNAME> with id <USER_ID>.
```

#### 3.3. Test Connection to Discord
```
python3 -m src.agent.agent_tools.discord
```
Expected output:
```
Connecting to discord...
Connected to discrod bot <USERNAME> with id <USER_ID>.
```


### [4/4]&nbsp;Run Agent Locally
```
python3 -m src.agent
```
Expected output (if you have added a key user in `twitter_config` and you have enabled Twitter and Discord in `agent_config`):
```
INFO: Agent starting up...
INFO: Twitter client starting up...
INFO: Connected to twitter user <USERNAME> with id <USER_ID>.
INFO: Discord client starting up...
WARNING: PyNaCl is not installed, voice will NOT be supported
INFO: Connected to discrod bot <USERNAME> with id <USER_ID>.
```



# Configuration ⚙️
### Configurating Exisiting Tools
You can enable and disable tools in the `agent_config` module in the `agent` package. Each tool can be configured using its configuration module that is located in the tool's directory in the `agent_tools` directory. Each tool also has its own README file that describes its configuration options.

### Adding New Tools
The `agent` class will automatically discover and initialize tools that are in the `agent_tools` directory. However, you need to follow these conventions when adding a new tool:
1. Each tool must have a corresponding `<TOOL_NAME>_ENABLED` boolean flag in the `agent_config` module.
2. Each tool must have its own directory within `agent_tools`.
3. Each tool must have a main module that is named `<tool_name>.py`.
    - Within the main module, the main class name must be the capitalized version of the directory name.
    - The main class must have an `__init__` method that will be called when the tool is initialized.
        - The `__init__` method should take the secrets and the model as arguments.
        - Each secret should be stored in the `.env` file and named `<TOOL_NAME>_<SECRET_NAME>` where `<SECRET_NAME>` is the name of the corresponding argument in the `__init__` method.
    - The main class must have a `run` method that will be called when the tool is run.
4. Each tool must have a configuration module that is named `<tool_name>_config.py`.
    - The configuration module must have a `<Tool_name>Config` class.
5. Each tool must have a README file that describes the configuration options.
6. If you install a new package, you must update the `requirements.txt` file:
    ```
    pip freeze > requirements.txt
    ``` 

For example, consider the twitter tool:
1. There is a `TWITTER_ENABLED` boolean flag in the `agent_config` module.
2. There is a twitter directory in `agent_tools`.
3. There is a main module `twitter.py`.
    - There is a `Twitter` class in `twitter.py`.
    - The `Twitter` class has an `__init__` method that takes the secrets and the model as arguments. The secrets are stored in the `.env` file and are prefixed with `TWITTER_`.
        - `consumer_key` corresponds to the `TWITTER_CONSUMER_KEY` environment variable.
        - `consumer_secret` corresponds to the `TWITTER_CONSUMER_SECRET` environment variable.
        - `access_token` corresponds to the `TWITTER_ACCESS_TOKEN` environment variable.
        - `access_token_secret` corresponds to the `TWITTER_ACCESS_TOKEN_SECRET` environment variable.
        - `bearer_token` corresponds to the `TWITTER_BEARER_TOKEN` environment variable.
    - The `Twitter` class has a `run` method that is called to run the tool.
4. There is a configuration module `twitter_config.py`.
    - There is a `TwitterConfig` class in `twitter_config.py`.
5. There is a README file `twitter/README.md` that describes the configuration options.
