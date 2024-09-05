# Day-4-Elasticsearch-Security-Configuration

In this tutorial, we’ll continue from where we left off with Elasticsearch, and now install and configure Kibana on your Vultr Cloud server. Kibana is a powerful data visualization and exploration tool used with Elasticsearch, allowing you to create dashboards and analyze your data.

## Step 1: Download and Install Kibana

1. **Download Kibana:**

   - Head over to the [official Kibana download page](https://www.elastic.co/downloads/kibana).
   - Choose the `DEB x86_64` option and right-click on the download link, selecting **Copy Link Address**.

2. **Download Kibana on Your Server:**

   - On your Ubuntu server (connected via SSH), download Kibana using the `wget` command:
     ```bash
     wget <paste_the_copied_link_here>
     ```
   - Once the download is complete, type `ls` to list the downloaded files and ensure Kibana is there.
  
   - ![Right Click and Copy Link Address](https://raw.githubusercontent.com/Virus192/Day-4-Elasticsearch-Security-Configuration/main/images/photo_5965442483469009313_w.jpg)


3. **Install Kibana:**

   - Install Kibana using the `dpkg` command:
     ```bash
     dpkg -i kibana-8.15.0-amd64.deb
     ```
   - ![Download Kibana in your Instance](https://raw.githubusercontent.com/Virus192/Day-4-Elasticsearch-Security-Configuration/main/images/photo_5965442483469009319_w.jpg)

## Step 2: Configure Kibana

1. **Edit the Kibana Configuration File:**

   - Open the `kibana.yml` configuration file using the `nano` text editor:
     ```bash
     nano /etc/kibana/kibana.yml
     ```
   - Find the lines for `server.port` and `server.host`, and uncomment these lines (remove the `#` symbol at the beginning).
   - Set `server.host` to your server’s public IP address.

2. **Save and Exit:**

   - Press `CTRL + S` to save your changes, and then `CTRL + X` to exit the editor.
    - ![Make Changes to your Kibana.yml file](https://raw.githubusercontent.com/Virus192/Day-4-Elasticsearch-Security-Configuration/main/images/photo_5965442483469009317_w.jpg)


## Step 3: Enable and Start Kibana Service

1. **Reload the Daemon:**

   - Reload the systemd configuration to recognize the Kibana service:
     ```bash
     systemctl daemon-reload
     ```

2. **Enable Kibana Service:**

   - Enable Kibana to automatically start on boot:
     ```bash
     sudo systemctl enable kibana.service
     ```

3. **Start Kibana Service:**

   - Start the Kibana service:
     ```bash
     systemctl start kibana.service
     ```

4. **Check Kibana Service Status:**

   - Verify that Kibana is running with the following command:
     ```bash
     sudo systemctl status kibana.service
     ```
   - The status should show as `active (running)`.

## Step 4: Generate Elasticsearch Enrollment Token

1. **Generate an Enrollment Token:**

   - Navigate to the Elasticsearch bin directory:
     ```bash
     cd /usr/share/elasticsearch/bin
     ```
   - Run the following command to generate an enrollment token:
     ```bash
     ./elasticsearch-create-enrollment-token --scope kibana
     ```
   - Copy the generated token and store it somewhere safe, as you’ll need it later.

## Step 5: Update Firewall Rules for Kibana

1. **Allow Traffic on Port 5601:**

   - Kibana runs on port 5601, so you’ll need to update your firewall rules to allow traffic on this port:
     ```bash
     ufw allow 5601
     ```

## Step 6: Access Kibana from Your Browser

1. **Access Kibana:**

   - Open your web browser and go to the following URL:
     ```plaintext
     http://<your_server_public_ip>:5601
     ```
   - Replace `<your_server_public_ip>` with the actual public IP of your Ubuntu server.

2. **Enter the Enrollment Token:**

   - When prompted, paste the enrollment token that you generated in Step 4.

## Step 7: Enter the Verification Code

1. **Generate a Verification Code:**

   - In your SSH terminal, navigate to the Kibana bin directory:
     ```bash
     cd /usr/share/kibana/bin
     ```
   - Run the following command to generate a verification code:
     ```bash
     ./kibana-verification-code
     ```
   - Copy the code and paste it in your browser when prompted.

## Step 8: Configure Kibana Security and Enable API Integration

1. **Generate Encryption Keys:**

   - In your SSH terminal, navigate back to the Kibana bin directory:
     ```bash
     cd /usr/share/kibana/bin
     ```
   - Run the following command to generate encryption keys:
     ```bash
     ./kibana-encryption-keys generate
     ```
   - This will generate three encryption keys. Copy and save them, as they will be needed in the next step.

2. **Add Encryption Keys to Kibana Keystore:**

   - Run the following command to add each of the three keys to the Kibana keystore:
     ```bash
     ./kibana-keystore add <key_name>
     ```
   - For each key, you will be prompted to paste the corresponding encryption key.

## Step 9: Restart Kibana

1. **Restart the Kibana Service:**

   - After adding the encryption keys, restart the Kibana service for the changes to take effect:
     ```bash
     systemctl restart kibana.service
     ```

2. **Refresh Kibana Web Interface:**

   - Go back to your web browser and reload the Kibana page. The security warnings should now be resolved.

## Conclusion

Congratulations! You have successfully installed and configured Kibana on your Vultr Cloud server. You can now start using Kibana to visualize your Elasticsearch data and explore its powerful features.

With this setup, you now have a fully functional Kibana instance running on your Vultr Cloud server, securely connected to your Elasticsearch cluster. If you have any questions or run into any issues, feel free to leave a comment below!
