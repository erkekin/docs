---
title: Generating a new SSH key and adding it to the ssh-agent
intro: 'After you''ve checked for existing SSH keys, you can generate a new SSH key to use for authentication, then add it to the ssh-agent.'
redirect_from:
  - /articles/adding-a-new-ssh-key-to-the-ssh-agent/
  - /articles/generating-a-new-ssh-key/
  - /articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
versions:
  free-pro-team: '*'
  enterprise-server: '*'
  github-ae: '*'
topics:
  - SSH
---

If you don't already have an SSH key, you must [generate a new SSH key](#generating-a-new-ssh-key). If you're unsure whether you already have an SSH key, check for [existing keys](/articles/checking-for-existing-ssh-keys).

If you don't want to reenter your passphrase every time you use your SSH key, you can [add your key to the SSH agent](#adding-your-ssh-key-to-the-ssh-agent), which manages your SSH keys and remembers your passphrase.

### Generating a new SSH key

{% data reusables.command_line.open_the_multi_os_terminal %}
2. Paste the text below, substituting in your {% data variables.product.product_name %} email address.
  ```shell
  $ ssh-keygen -t ed25519 -C "<em>your_email@example.com</em>"
  ```
  {% note %}

  **Note:** If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
  ```shell
   $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
  ```

  {% endnote %}  
  This creates a new ssh key, using the provided email as a label.
  ```shell
  > Generating public/private ed25519 key pair.
  ```
3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

  {% mac %}

  ```shell
  > Enter a file in which to save the key (/Users/<em>you</em>/.ssh/id_ed25519): <em>[Press enter]</em>
  ```

  {% endmac %}

  {% windows %}

  ```shell
  > Enter a file in which to save the key (/c/Users/<em>you</em>/.ssh/id_ed25519):<em>[Press enter]</em>
  ```

  {% endwindows %}

  {% linux %}

  ```shell
  > Enter a file in which to save the key (/home/<em>you</em>/.ssh/id_ed25519): <em>[Press enter]</em>
  ```

  {% endlinux %}

4. At the prompt, type a secure passphrase. For more information, see ["Working with SSH key passphrases"](/articles/working-with-ssh-key-passphrases).
  ```shell
  > Enter passphrase (empty for no passphrase): <em>[Type a passphrase]</em>
  > Enter same passphrase again: <em>[Type passphrase again]</em>
  ```

### Adding your SSH key to the ssh-agent

Before adding a new SSH key to the ssh-agent to manage your keys, you should have [checked for existing SSH keys](/articles/checking-for-existing-ssh-keys) and [generated a new SSH key](/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key). <span class="platform-mac">When adding your SSH key to the agent, use the default macOS `ssh-add` command, and not an application installed by [macports](https://www.macports.org/), [homebrew](http://brew.sh/), or some other external source.</span>

{% mac %}

1. {% data reusables.command_line.start_ssh_agent %}

2. If you're using macOS Sierra 10.12.2 or later, you will need to modify your `~/.ssh/config` file to automatically load keys into the ssh-agent and store passphrases in your keychain.

    * First, check to see if your `~/.ssh/config` file exists in the default location.

      ```shell
      $ open ~/.ssh/config
      > The file /Users/<em>you</em>/.ssh/config does not exist.
      ```

    * If the file doesn't exist, create the file.

      ```shell
      $ touch ~/.ssh/config
      ```

    * Open your `~/.ssh/config` file, then modify the file to contain the following lines. If your SSH key file has a different name or path than the example code, modify the filename or path to match your current setup. 

      ```
      Host *
        AddKeysToAgent yes
        UseKeychain yes
        IdentityFile ~/.ssh/id_ed25519
      ```

     {% note %}

     **Note:** If you chose not to add a passphrase to your key, you should omit the `UseKeychain` line.
  
     {% endnote %}
     
      {% mac %}
      {% note %}

      **Note:** If you see an error like this

      ```
      /Users/USER/.ssh/config: line 16: Bad configuration option: usekeychain
      ```

      add an additional config line to your `Host *` section:

      ```
      Host *
        IgnoreUnknown UseKeychain
      ```

      {% endnote %}
      {% endmac %}
  
3. Add your SSH private key to the ssh-agent and store your passphrase in the keychain. {% data reusables.ssh.add-ssh-key-to-ssh-agent %}
   ```shell
   $ ssh-add -K ~/.ssh/id_ed25519
  ```
  {% note %}

  **Note:** The `-K` option is Apple's standard version of `ssh-add`, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent. If you chose not to add a passphrase to your key, run the command without the `-K` option. 

  If you don't have Apple's standard version installed, you may receive an error. For more information on resolving this error, see "[Error: ssh-add: illegal option -- K](/articles/error-ssh-add-illegal-option-k)."

  {% endnote %}

4. [Add the SSH key to your GitHub account](/articles/adding-a-new-ssh-key-to-your-github-account).

{% endmac %}

{% windows %}

{% data reusables.desktop.windows_git_bash %}

1. Ensure the ssh-agent is running. You can use the "Auto-launching the ssh-agent" instructions in "[Working with SSH key passphrases](/articles/working-with-ssh-key-passphrases)", or start it manually:
  ```shell
  # start the ssh-agent in the background
  $ eval `ssh-agent -s`
  > Agent pid 59566
  ```

2. Add your SSH private key to the ssh-agent. {% data reusables.ssh.add-ssh-key-to-ssh-agent %}
   {% data reusables.ssh.add-ssh-key-to-ssh-agent-commandline %}

3. [Add the SSH key to your GitHub account](/articles/adding-a-new-ssh-key-to-your-github-account).

{% endwindows %}

{% linux %}

1. {% data reusables.command_line.start_ssh_agent %}

2. Add your SSH private key to the ssh-agent. {% data reusables.ssh.add-ssh-key-to-ssh-agent %}
   {% data reusables.ssh.add-ssh-key-to-ssh-agent-commandline %}

3. [Add the SSH key to your GitHub account](/articles/adding-a-new-ssh-key-to-your-github-account).

{% endlinux %}

### Further reading

- "[About SSH](/articles/about-ssh)"
- "[Working with SSH key passphrases](/articles/working-with-ssh-key-passphrases)"
{%- if currentVersion == "free-pro-team@latest" %}
- "[Authorizing an SSH key for use with SAML single sign-on](/articles/authorizing-an-ssh-key-for-use-with-saml-single-sign-on)"
{%- endif %}
