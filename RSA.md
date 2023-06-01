# Importing RSA (.pem) Key into WSL

This guide outlines the steps to import a pre-existing RSA key in `.pem` format into Windows Subsystem for Linux (WSL).

## Prerequisites

- Windows Subsystem for Linux (WSL) installed and configured.
- RSA key in `.pem` format available on your Windows system.

## Steps

1. **Copy the RSA key file:**

   - Locate the RSA key file in `.pem` format on your Windows system.
   - Copy the RSA key file to a directory within your WSL instance. For example, you can copy it to the `~/.ssh` directory in your WSL home directory.

2. **Set appropriate permissions:**

   - In your WSL terminal, navigate to the directory where you copied the RSA key file. For example:
     ```
     cd ~/.ssh
     ```
   - Set the correct permissions on the key file to ensure it is secure. Run the following command:
     ```
     chmod 600 your_key.pem
     ```

3. **Convert the `.pem` key to OpenSSH format:**

   - WSL uses OpenSSH format keys, so you'll need to convert the `.pem` key to OpenSSH format using the `ssh-keygen` command.
   - In your WSL terminal, run the following command:
     ```
     ssh-keygen -p -f your_key.pem -m pem -i -P ''
     ```
     Replace `your_key.pem` with the actual name of your `.pem` key file.

4. **Enter a new file name for the converted key:**

   - When prompted, enter a new file name for the converted key. For example, you can use `id_rsa_converted`.
   - The converted key file will be saved in the same directory (`~/.ssh`) with the new name.

5. **Set up SSH configuration:**

   - Open the `~/.ssh/config` file (create it if it doesn't exist) using a text editor within your WSL instance. For example:
     ```
     nano ~/.ssh/config
     ```
   - Ensure that the following lines are present and correctly configured in the `config` file:
     ```
     Host *
         AddKeysToAgent yes
         IdentityFile ~/.ssh/id_rsa_converted
     ```
   - Save and exit the file.

6. **Add the RSA key to the SSH agent:**

   - In your WSL terminal, run the following command to add the RSA private key to the SSH agent:
     ```
     eval $(ssh-agent)
     ssh-add ~/.ssh/id_rsa_converted
     ```
   - You may be prompted to enter the passphrase for the private key if it was generated with one.

That's it! You have successfully imported the RSA key in `.pem` format into your WSL instance. You should now be able to use the converted key for SSH connections within WSL.
