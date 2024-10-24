# Setup Guide for Signing Git Commits with GPG on Windows

In this guide, you will learn how to configure your Windows machine to sign your Git commits using GPG keys. Signed commits are marked as verified on GitHub, ensuring that your changes come from a trusted source.

## Table of Contents

1. [Introduction to Signed Commits](#introduction-to-signed-commits)
2. [Verified Commits vs Unverified Commits on GitHub](#verified-commits-vs-unverified-commits-on-github)
3. [Download and Install GPG4WIN](#download-and-install-gpg4win)
4. [Generate GPG Key and Configure GitHub](#generate-gpg-key-and-configure-github)
5. [Configure Git to Sign Commits](#configure-git-to-sign-commits)
6. [Create Signed Commits](#create-signed-commits)
7. [Thank You](#thank-you)

## Introduction to Signed Commits

Configuring your Windows machine to sign your Git commits using GPG keys enhances the security of your commits. Signed commits provide an additional layer of verification, ensuring that the changes are indeed made by you.

## Verified Commits vs Unverified Commits on GitHub

- **Verified Commits:** Marked with a "Verified" badge on GitHub, indicating that the commits are from a trusted source.
- **Unverified Commits:** Do not have this badge, making it unclear if they were indeed made by the claimed author.

## Download and Install GPG4WIN

1. **Download GPG4WIN:**

   - Visit the [GPG4WIN website](https://www.gpg4win.org/) and download the installer.

2. **Install GPG4WIN:**
   - Run the installer and follow the on-screen instructions.
   - Ensure that you install all the default components, especially GnuPG and Kleopatra.

## Generate GPG Key and Configure GitHub

1. **Generate a GPG Key:**

   - Open a terminal (Command Prompt or Git Bash) and run:
     ```sh
     gpg --full-generate-key
     ```
   - Follow the prompts to create your key. Choose RSA and RSA, key size of 4096, and set it to never expire (or choose an expiration date if preferred). Provide your name and email when prompted.

2. **List Your GPG Keys:**

   - List your secret keys to find your key ID:
     ```sh
     gpg --list-secret-keys --keyid-format LONG
     ```
   - The output will look something like this:

     ```
     /path/to/secring.gpg
     --------------------
     sec   rsa4096/ABC123DEF456 2020-06-18 [SC] [expires: 2022-06-18]
           ABC123DEF456ABC123DEF456ABC123DEF456ABC1
     uid                          Your Name <your.email@example.com>
     ssb   rsa4096/DEF123ABC456 2020-06-18 [E] [expires: 2022-06-18]
     ```

   - Identify your key ID in the output. The key ID is the portion after `rsa4096/`, in this case, `ABC123DEF456`.

3. **Export Your Public Key:**

   - Export your public key in ASCII format:
     ```sh
     gpg --armor --export ABC123DEF456
     ```
   - Copy the entire output.

4. **Add GPG Key to GitHub:**
   - Go to GitHub, navigate to **Settings > SSH and GPG keys**.
   - Click **New GPG key**, paste the exported key, and save it.

## Configure Git to Sign Commits

1. **Set Your Git Username and Email:**

   - Configure Git with your name and email:
     ```sh
     git config --global user.name "YOUR-NAME"
     git config --global user.email "YOUR-EMAIL"
     ```

2. **Configure Git to Use Your GPG Key:**

   - Set your signing key:
     ```sh
     git config --global user.signingkey YOUR-KEY-ID
     ```
   - Enable commit signing:
     ```sh
     git config --global commit.gpgsign true
     ```
   - Enable tag signing:
     ```sh
     git config --global tag.gpgsign true
     ```

3. **Find the GPG Program Location:**

   - By default, GPG is installed in:
     ```
     C:\Program Files (x86)\GnuPG\bin\gpg.exe
     ```
   - If you installed GPG in a different location or want to confirm its location, follow these steps:

     1. **Open File Explorer**: Press `Win + E` to open File Explorer.
     2. **Navigate to the Default Install Path**: Go to `C:\Program Files (x86)\GnuPG\bin\`.
     3. **Locate gpg.exe**: You should see `gpg.exe` in this directory. This confirms the installation path.

   - Alternatively, you can find the GPG executable path using the Command Prompt:
     ```sh
     where gpg
     ```
   - This command will return the path where GPG is installed.

4. **Configure Git to Use the GPG Program:**

   - Set the GPG program location in Git:
     ```sh
     git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"
     ```

5. **Verify Your Git Configuration:**
   - List your global Git configuration to ensure everything is set up correctly:
     ```sh
     git config --global --list
     ```

## Create Signed Commits

1. **Make Changes and Commit:**

   - Create or modify files in your repository.
   - Stage the changes:
     ```sh
     git add .
     ```
   - Commit with a signed commit:
     ```sh
     git commit -S -m "Your commit message"
     ```

2. **Push the Commit to GitHub:**
   - Push your changes to the remote repository:
     ```sh
     git push origin main
     ```
   - Your commit should now appear as verified on GitHub.

## Thank You

Thank you for following this guide. Signed commits help ensure the integrity and authenticity of your contributions.
