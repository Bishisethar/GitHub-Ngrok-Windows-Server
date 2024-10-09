# Free Windows Server 2022 with RDP for Lifetime
This repository provides a step-by-step guide on how to set up a free Windows Server 2022 with Remote Desktop Protocol (RDP) access for lifetime use, leveraging GitHub Actions and an Ngrok tunnel.

By following the instructions below, you can create a free VPS running Windows Server 2022, which can be accessed via Remote Desktop Connection using Ngrok.

## Steps to Create Windows Server 2022 with RDP
### 1. Sign Up for a GitHub Account

If you don’t have a GitHub account yet, sign up here: GitHub.

### 2. Create a Private Repository
1.Log in to your GitHub account and create a new private repository.

2.Go to Settings > Secrets and Variables > Actions > New Repository Secret.

This is where you'll store your Ngrok authentication token securely.

### 3. Sign Up for an Ngrok Account
Visit Ngrok and sign up for an account. https://ngrok.com/

Copy your Ngrok Authentication Token from your Ngrok dashboard.

### 4. Add Ngrok Authentication Token to GitHub Secrets
Navigate to your repository's Settings > Secrets and Variables > Actions.

Click New Repository Secret and add the Ngrok authentication token with the name  - NGROK_AUTH_TOKEN.

## 5. Set Up GitHub Actions Workflow
You will now set up a GitHub Actions workflow to run the server. Follow these steps:

In your repository, go to the Actions tab.

Create a new workflow and name it CI.

Add the following code to the main.yml file and commit the changes.

    name: CI

    on: [push, workflow_dispatch]
    jobs:
      build:
        runs-on: windows-latest 

      steps:
     - name: Download Ngrok
      run: Invoke-WebRequest https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-windows-amd64.zip -OutFile ngrok.zip
     - name: Extract Ngrok
      run: Expand-Archive ngrok.zip
      - name: Authenticate Ngrok
      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
     - name: Enable Remote Desktop
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
    - name: Enable Firewall Rule
      run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
    - name: Configure User Authentication
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 1
     - name: Set User Password
      run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)
    - name: Start Ngrok Tunnel
      run: .\ngrok\ngrok.exe tcp 3389

### 6. Run the Workflow
Go to the Actions tab in your repository and manually run the workflow.

Wait for the workflow to complete.

Take note of the credentials:

Username:    - runneradmin

Password:    - P@ssw0rd!

### 7. Connect via Remote Desktop
After the workflow runs, Ngrok will provide an endpoint URL.

Use this Ngrok endpoint as the IP or address when connecting through Remote Desktop Connection.

Enter the credentials (username: runneradmin, password: P@ssw0rd!) to log in.

Thanks!

Now you have access to a free Windows Server 2022 with RDP access for lifetime! Feel free to contribute to this repository or share your feedback.

