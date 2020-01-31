---
description: >-
  This section comprises every information to properly and securely set up an
  X-Cash DPoPS node.
---

# Set up your node

## Parameters <a id="parameters"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>--parameters</code>
      </td>
      <td style="text-align:left">Show a list of all parameters</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--test</code>
      </td>
      <td style="text-align:left">Run the test to make sure the program is compatible with your system</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--total_threads &lt;thread_param&gt;</code>
      </td>
      <td style="text-align:left">
        <p>Total number of threads to use</p>
        <p>If &lt;param&gt; is not defined, &quot;default&quot; is the max number
          of threads</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--delegates_website</code>
      </td>
      <td style="text-align:left">Run the delegates website</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--shared-delegates-website --fee &lt;fee_param&gt; --minimum_amount &lt;amount_param&gt;</code>
      </td>
      <td style="text-align:left">
        <p>Run the shared delegates website, with a fee of <code>&lt;fee_param&gt;</code> and
          a minimum amount of <code>&lt;amount_param&gt;.</code>
        </p>
        <p><code>&lt;fee_param&gt;</code> : expressed in percentage (up to 6 decimal
          places). <code>Ex: 6.020000 equals to 6.02%</code>
        </p>
        <p><code>&lt;amount_param&gt;</code> : Minimum amount for a public_address
          to receive a payment (in normal units).</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--synchronize_database</code>
      </td>
      <td style="text-align:left">Synchronize the database from a network data node.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--disable_synchronizing_databases_and_starting_timers</code>
      </td>
      <td style="text-align:left">(Used for testing) Disables synchronizing of the databases and starts
        the timers.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--test_data_add</code>
      </td>
      <td style="text-align:left">Add test data to the databases</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>--test_data_remove</code>
      </td>
      <td style="text-align:left">Remove test data from the databases</td>
    </tr>
  </tbody>
</table>\#\# Set up the Firewall The goal of setting up a firewall in the DPoPS node is to block any DDoS attacks. We are using IPtables for the firewall. \#\#\# Solo node setup The \`firewall\_script.sh\` is already configured for a solo node setup, no change is required. \#\#\# Delegates or shared delegates setup To configure the firewall for a shared delegates website or delegates website, a few changes need to be made to the firewall script. First, open the firewall script: \`\`\`bash nano ~/Installed-Programs/XCASH\_DPOPS/scripts/firewall/firewall\_script.sh \`\`\` Then, un-comment these 3 lines \\(by removing the \`\#\`\\) \`\`\`bash \(line 66\) \# iptables -t filter -I INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 --connlimit-mask 32 -j DROP \(line 82\) \# iptables -A INPUT -p tcp --dport 80 -j ACCEPT \(line 96\) \# iptables -A PREROUTING -t nat -p tcp --dport 80 -j REDIRECT --to-ports 1828 \`\`\` {% hint style="info" %} If you want to run the \_\*\*shared delegates\*\*\_ website or \_\*\*delegates\*\*\_ website using HTTPS, you will need to install a webserver \\(e.g. \_nginx\_\\) and configure it. {% endhint %} \#\#\# Run the firewall script Now we need to run the firewall script and activate it \`\`\`bash chmod +x ~/Installed-Programs/XCASH\_DPOPS/scripts/firewall/firewall\_script.sh ~/Installed-Programs/XCASH\_DPOPS/scripts/firewall/firewall\_script.sh iptables-save &gt; /etc/network/iptables.up.rules iptables-apply -t 60 \`\`\` You should then open another connection to the server to make sure it worked as planned and did not lock you out. Then press \`y\` to confirm the changes for the firewall. \#\# Set up and install the \`systemd\` files Edit the below \`systemd\` files to your paths. Copy all of the service files in the \`systemd\` folder to \`/lib/systemd/system/\` \`\`\`bash cp -a ~/Installed-Programs/XCASH\_DPOPS/scripts/systemd/\* /lib/systemd/system/ \`\`\` Reload systemd \`\`\`bash systemctl daemon-reload \`\`\` Create a systemd PID folder \`\`\`bash mkdir ~/Installed-Programs/systemdpid/ \`\`\` Create a mongod pid file and a xcashd pid file \`\`\`bash touch ~/Installed-Programs/systemdpid/mongod.pid touch ~/Installed-Programs/systemdpid/xcash\_daemon.pid \`\`\` \#\#\# MongoDB {% code title="MongoDB systemd file" %} \`\`\`bash \[Unit\] Description=MongoDB Database Server After=network.target \[Service\] Type=forking User=root PIDFile=/root/Installed-Programs/systemdpid/mongod.pid ExecStart=/root/Installed-Programs/mongodb-linux-x86\_64-ubuntu1804-4.2.0/bin/mongod Restart=always LimitFSIZE=infinity LimitCPU=infinity LimitAS=infinity LimitNOFILE=64000 LimitNPROC=64000 LimitMEMLOCK=infinity TasksMax=infinity TasksAccounting=false \[Install\] WantedBy=multi-user.target \`\`\` {% endcode %} To properly set up the systemd file, you will need to change: \* \*\*\`User\`\*\*to the user of the system \* \*\*\`PIDFile\`\*\*to the full path of the \`mongod.pid\` file \* \*\*\`ExecStart\`\*\* to the path of the \`mongod\` file Reload \`systemd\` after you have made any changes to the \`systemd\` service files \`\`\`bash systemctl daemon-reload \`\`\` \#\#\# XCASH Daemon {% code title="XCASH Daemon systemd file" %} \`\`\`text \[Unit\] Description=XCASH Daemon systemd file \[Service\] Type=forking User=root PIDFile=/root/Installed-Programs/systemdpid/xcash\_daemon.pid ExecStart=/root/Installed-Programs/X-CASH/build/release/bin/xcashd --rpc-bind-ip 0.0.0.0 --rpc-bind-port 18281 --restricted-rpc --confirm-external-bind --detach --pidfile /root/Installed-Programs/systemdpid/xcash\_daemon.pid RuntimeMaxSec=15d Restart=always \[Install\] WantedBy=multi-user.target \`\`\` {% endcode %} {% hint style="warning" %} Make sure to leave the \`RuntimeMaxSec=15d\` in the \`systemd\` service file, as the XCASH Daemon usually needs to restart after a while to prevent it from not synchronizing. {% endhint %} To properly set up the systemd file, you will need to change: \* \*\*\`User\`\*\*to the user of the system \* \*\*\`PIDFile\`\*\*to the full path of the \`xcash\_daemon.pid\` file \* \*\*\`ExecStart\`\*\* to the path of the \`xcashd\` file Reload \`systemd\` after you have made any changes to the \`systemd\` service files \`\`\`bash systemctl daemon-reload \`\`\` \#\#\# XCASH Daemon \\(Block Verifier\\) {% hint style="info" %} Only run the XCASH Daemon service file, as the XCASH DPOPS program will determine if you are a block verifier and start the correct systemd service file {% endhint %} {% code title="XCASH Daemon \\(block verifier\\) systemd file" %} \`\`\`text \[Unit\] Description=XCASH Daemon Block Verifier systemd file \[Service\] Type=forking User=root PIDFile=/root/Installed-Programs/systemdpid/xcash\_daemon.pid ExecStart=/root/Installed-Programs/X-CASH/build/release/bin/xcashd --block-verifier --rpc-bind-ip 0.0.0.0 --rpc-bind-port 18281 --restricted-rpc --confirm-external-bind --detach --pidfile /root/Installed-Programs/systemdpid/xcash\_daemon.pid RuntimeMaxSec=15d Restart=always \[Install\] WantedBy=multi-user.target \`\`\` {% endcode %} {% hint style="warning" %} Make sure to leave the \`RuntimeMaxSec=15d\` in the \`systemd\` service file, as the XCASH Daemon usually needs to restart after a while to prevent it from not synchronizing. {% endhint %} To properly set up the systemd file, you will need to change: \* \*\*\`User\`\*\*to the user of the system \* \*\*\`PIDFile\`\*\*to the full path of the \`xcash\_daemon.pid\` file \* \*\*\`ExecStart\`\*\* to the path of the \`xcashd\` file Reload \`systemd\` after you have made any changes to the \`systemd\` service files \`\`\`bash systemctl daemon-reload \`\`\` \#\#\# XCASH Wallet This is the systemd file for XCASH Wallet {% code title="XCASH Wallet systemd file" %} \`\`\`bash \[Unit\] Description=XCASH Wallet \[Service\] Type=simple User=root ExecStart=/root/Installed-Programs/X-CASH/build/release/bin/xcash-wallet-rpc --wallet-file /root/Installed-Programs/xcash\_wallets/WALLET\_FILE\_NAME --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon Restart=always \[Install\] WantedBy=multi-user.target \`\`\` {% endcode %} To properly set up the \`systemd\`file, you will need to change: \* \*\*\`User\`\*\*to the user of the system \* \*\*\`WALLET\_FILE\_NAME\`\*\* with the name of your wallet file \* \*\*\`PASSWORD\`\*\* with the password of your wallet file Reload \`systemd\` after you have made any changes to the \`systemd\` service files \`\`\`bash systemctl daemon-reload \`\`\` \#\#\# XCASH\\_DPOPS This is the systemd file for XCASH DPOPS {% code title="XCASH DPOPS systemd file" %} \`\`\`text \[Unit\] Description=XCASH DPOPS \[Service\] Type=simple LimitNOFILE=64000 User=root WorkingDirectory=/root/Installed-Programs/XCASH\_DPOPS/ ExecStart=/root/Installed-Programs/XCASH\_DPOPS/XCASH\_DPOPS Restart=always \[Install\] WantedBy=multi-user.target \`\`\` {% endcode %} {% hint style="info" %} The \`LimitNOFILE=64000\` will allow the XCASH\\_DPOPS program to utilize up to 64000 concurrent file descriptors instead of the default 4096 for a Linux process. The actual XCASH\\_DPOPS program is limited to only accept up to 1000 concurrent connections due to that DPOPS program usage will not be extensive at the start. This will help with DDOS at this time and the limits in the XCASH\\_DPOPS program will be updated. {% endhint %} To properly set up the \`systemd\`file, you will need to change: \* \*\*\`User\`\*\*to the user of the system \* \*\*\`ExecStart\`\*\* to the full path of the\`XCASH\_DPOPS\`file \* Add the startup flags : \* \`--shared\_delegates\_website\` to run the XCASH\\_DPoPS node and the shared delegates website \* \`--delegates\_website\` to run the XCASH\\_DPoPS node and the delegates website Reload \`systemd\` after you have made any changes to the \`systemd\` service files \`\`\`bash systemctl daemon-reload \`\`\` \#\#\# Firewall {% code title="Firewall systemd file" %} \`\`\`text \[Unit\] Description=firewall \[Service\] Type=oneshot RemainAfterExit=yes User=root ExecStart=/root/Installed-Programs/XCASH\_DPOPS/scripts/firewall/firewall\_script.sh \[Install\] WantedBy=multi-user.target \`\`\` {% endcode %} To properly set up the \`systemd\`file, you will need to change: \* \*\*\`User\`\*\*to the user of the system \* \*\*\`ExecStart\`\*\* to the full path of the\`firewall\_script.sh\`file Reload \`systemd\` after you have made any changes to the \`systemd\` service files \`\`\`bash systemctl daemon-reload \`\`\` \#\# Set up a domain name instead of an IP Address The XCASH DPOPS system needs a IP Address when registering a delegate to be able to let other delegates know where to send messages to. One can instead setup a domain name \\(without the \`www.\`\\) and register this instead of an IP address. The possible benefits of using a domain name over an IP address are: \* One can change IP's from their domain page if they change servers instead of having to update that info in the DPoPS database. \* It could be more recognizable if there is an issue, since in the XCASH\\_DPOPS logs and the XCASH\\_Daemon logs will print the source and destination of messages. To set up a domain instead of an IP address, go to the domain registrar you have purchased the domain name from and add an \`A record\` to the domain. {% hint style="info" %} Each domain registrar is going to be a little different, so you will want to check if they have an official article on how to add an A record. {% endhint %} Now you need to setup the reverse DNS as well. Go to the hosting dashboard of the place where you are renting the server. {% hint style="info" %} Not all hosting companies let you change the reverse DNS, so you might not be able to complete the process. {% endhint %} Navigate to the DPoPS server your are renting. Depending on your service provider, you might have the choice to change the \`reverse DNS\`. Change it to the domain name you used in the first step. At this point you can now register the domain name \\(without the \`www.\`\\) to the XCASH DPOPS system. \#\# Start/stop/logs for component

<table>
  <thead>
    <tr>
      <th style="text-align:left">Action on systemd service</th>
      <th style="text-align:left">Command</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>start</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl start name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl start XCASH_DPOPS</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>stop</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl stop name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl stop XCASH_DPOPS</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>restart</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl stop name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl restart XCASH_DPOPS</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check status</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl status name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl status XCASH_DPOPS</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check the logs</code>
      </td>
      <td style="text-align:left">
        <p><b><code>journalctl --unit=name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. journalctl --unit=XCASH_DPOPS</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check the logs<br />(last 100 lines)</code>
      </td>
      <td style="text-align:left">
        <p><b><code>journalctl --unit=name_of_service_file_without.service -n 100 --output cat</code></b>
        </p>
        <p><code>e.g. journalctl --unit=XCASH_DPOPS -n 100 --output cat</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check the logs<br />(live logging)</code>
      </td>
      <td style="text-align:left">
        <p><b><code>journalctl --unit=name_of_service_file_without.service --follow -n 100 --output cat</code></b>
        </p>
        <p><code>e.g. journalctl --unit=XCASH_DPOPS --follow -n 100 --output cat</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>