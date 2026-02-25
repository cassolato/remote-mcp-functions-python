You are an ai assistant tasked assisting the developer to build and deploy MCP Servers successfully using Azure Functions.  Our ultimate goal is for the user to be able to complete this quickstart guide, where the app is deployed and healthy in azure, and the MCP client tools are calling this MCP server.  

Here are some specific requirements and pieces of context:

- This must build a working Azure Function project complete with code, Readme changes, and AZD bicep.  The original repo is already in this state, so your job is to preserve it.  
- if i ask you ever to say hello, save a snippet, or get a snippet, please do not prompt me to run the function first or run vs code tasks.  instead simply run the tools provided as mcp servers
- AZD and the func (aka Azure Functions Core Tools) commandline tools are the main tools to be used for deployment, provisioning and running locally.  As soon as the user has done the `azd up` or `azd provision` step at least once, you can learn all values of their azure application like resource group and function app name using the environment variables stored in the .azure folder.  Please proactively use these and be helpful to suggest running commands for the developer, replacing placeholder values when possible with these environment variables.
- This particular project is Python Azure Function using the v2 programming model
- We prefer using Azure Functions bindings if they can work versus the Azure SDKs, but Azure SDKs are ok if there is no substitute.  

## Running the project locally

When asked to run the project locally, follow these steps in order:

1. **Start Azurite** (storage emulator) — required for blob storage bindings. Run in detached mode so it doesn't block the terminal:
   ```
   docker run -d -p 10000:10000 -p 10001:10001 -p 10002:10002 mcr.microsoft.com/azure-storage/azurite
   ```

2. **Install dependencies** — from the `src` directory:
   ```
   cd src
   pip install -r requirements.txt --target=".python_packages/lib/site-packages"
   ```

3. **Start the Functions host** — this is a long-running process that must run from the `src/` directory with `PYTHONPATH` set. Because background terminals always start in the workspace root, combine `cd`, environment setup, and `func start` into a **single command**:
   ```powershell
   # Windows (PowerShell) — run as a background terminal
   Push-Location src; $env:PYTHONPATH = ".python_packages/lib/site-packages"; func start
   ```
   ```bash
   # macOS / Linux — run as a background terminal
   cd src && PYTHONPATH=".python_packages/lib/site-packages" func start
   ```

4. **Verify** the output shows all 5 functions indexed (`get_snippet`, `get_weather`, `get_weather_widget`, `hello_mcp`, `save_snippet`) and the MCP endpoint at `http://localhost:7071/runtime/webhooks/mcp`.
