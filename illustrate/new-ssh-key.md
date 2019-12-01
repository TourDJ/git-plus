
## 生成新的 SSH key

1. Open Git Bash
2. Paste the text below, substituting in your GitHub email address

    $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
This creates a new ssh key, using the provided email as a label.

    > Generating public/private rsa key pair.
3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location

    > Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]

4. At the prompt, type a secure passphrase

    Enter passphrase (empty for no passphrase): [Type a passphrase]
    Enter same passphrase again: [Type passphrase again]

## Adding your SSH key to the ssh-agent
1. Start the ssh-agent in the background

    # start the ssh-agent in the background
    $ eval $(ssh-agent -s)
    > Agent pid 59566
2. Add your SSH private key to the ssh-agent. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_rsa in the command with the name of your private key file.

    $ ssh-add ~/.ssh/id_rsa

## Add the SSH key to your GitHub account
1. Copy the SSH key to your clipboard.

    $ clip < ~/.ssh/id_rsa.pub
    # Copies the contents of the id_rsa.pub file to your clipboard
2. In the upper-right corner of any page, click your profile photo, then click Settings.
3. In the user settings sidebar, click SSH and GPG keys.
4. Click New SSH key or Add SSH key.
5. In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".
6. Paste your key into the "Key" field.
7. Click Add SSH key.
8. If prompted, confirm your GitHub password.


