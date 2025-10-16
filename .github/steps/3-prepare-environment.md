## Step 3: Preparing Copilot's environment

Let's add some information about the school, roles to assume, and typical tasks the teachers request and a pre-configured development environment to make it faster and more reliable (so Jessica in IT doesn't ask about increased Actions minutes usage).

- **copilot instructions** - Provide project specific context for copilot before considering the issue.
  - Provide business considerations for developing the project.
  - Provide roles to guide Copilot.
  - Provide useful commands for common tasks.
- **copilot setup steps** - Customize the development environment in advance to make sessions faster.
  - Pre-install useful tools for Copilot.
  - Reducing errors from Copilot installing incorrect versions.
- **environment** - Use repository environments for configurations.
  - Provide variables to adjust deployments for different environments.
  - Provide secrets to access additional resources.

> [!TIP]
> You can also [enable a Model Context Protocal (MCP) server](https://docs.github.com/en/enterprise-cloud@latest/early-access/copilot/coding-agent/extending-copilot-coding-agent-with-model-context-protocol) for Copilot to provide even more functionality!

### ⌨️ Activity: Create instructions to guide Copilot

1. In the top navigation, select the **Code** tab.

1. Create a new branch with the name `prepare-environment`.

   <img width="250" alt="image" src="https://github.com/user-attachments/assets/c48deded-4214-4edd-9a50-d1368bfb12e8" />

1. Navigate to and open the `.github/copilot-instructions.md` file for editing.

1. Replace the placeholder text with a link to the development guide.

   ```md
   ## Development Environment

   For detailed setup and development instructions, please refer to our [Development Guide](../docs/how-to-develop.md).
   ```

1. Add some additional information about the school and teachers to help Copilot interact more naturally.

   ```md
   ### User Interaction

   Consider the following when communicating with the staff.

   - The staff is not technical. Explain in simple terms as much as possible and avoid software jargon.
   - Any new code must be easy to maintain and understand, without significant coding experience.

   ## Program architecture

   - The website users are the students and teachers. Make sure the user experience is simple.
   - Do not make additional apps or services.
   - Do not make command line tools.
   - Do not create a long single file application. Always use an easy-to-understand directory structure.
   - Only use HTML, CSS, Javascript, and Python. No other languages.
   ```

   > 💡 Tip: You can add more details. Check out the `copilot-instructions-ext.md` file for ideas.

1. When finished, **commit your changes** to the `prepare-environment` branch.

### ⌨️ Activity: Prepare the coding environment for copilot

Customizing Copilot's development environment and adjusting [permissions](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/controlling-permissions-for-github_token) is done with a unique [GitHub Actions](https://github.com/features/actions) workflow. For all configuration options, see the [pre-installing dependencies for Copilot](https://docs.github.com/en/enterprise-cloud@latest/early-access/copilot/coding-agent/customizing-copilot-coding-agents-development-environment#pre-installing-tools-or-dependencies-in-copilots-environment) documentation.

1. Ensure you are still on the `prepare-environment` branch.

1. Navigate to the `.github/workflows/` directory.

1. In the top right, click the **Add file** button and select **Create new file**.

   <img width="250" alt="image" src="https://github.com/user-attachments/assets/c135dd3f-72bd-4d2b-b21f-9c4968a06f5f" />

1. Set the file name to `copilot-setup-steps.yml`.

   <img width="650" alt="image" src="https://github.com/user-attachments/assets/ac615290-1045-45a5-8201-637721ef6fd2" />

1. Paste the following workflow configuration, which will pre-install the dependencies for the website's Python backend.

   ```yml
   name: "Copilot Setup Steps"

   on: workflow_dispatch
   jobs:
     # This is the required job name. If different, Copilot will ignore it.
     copilot-setup-steps:
       runs-on: ubuntu-latest

       # Grant Copilot early access to read the repository content.
       permissions:
         contents: read

       steps:
         - name: Checkout code
           uses: actions/checkout@v4

         - name: Set up Python
           uses: actions/setup-python@v4
           with:
             python-version: "3.x"
             cache: "pip"

         - name: Install Python dependencies
           run: |
             python -m pip install --upgrade pip
             pip install -r src/requirements.txt
   ```

   > 🪧 **Note:** Copilot will automatically retrieve the repository contents later. This workflow provides early access during setup to install the dependencies.

   > 🪧 **Note:** Copilot will automatically identify and install missing dependencies. Doing it now saves Copilot time and ensures proper environment setup

1. In the top right, click the **Commit changes...** button and commit your changes to the `prepare-environment` branch.

1. Create a **pull request**, but do **NOT** merge it yet. Mona will check your files to confirm they are correct.

1. After Mona shares the next steps, you can merge the pull request.

> 🙋 **Question:** How did the manual process feel compared to letting Copilot do most of the work? 🤔


<details>
<summary>🤷 Having trouble?</summary><br/>

If you accidentally merged the pull request before Mona shared feedback about mistakes, that is ok. Just recreate the branch and try again with a new pull request.

</details>