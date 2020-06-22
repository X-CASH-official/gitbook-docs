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
</table>

## How To Setup and Install the Systemd Files

Edit the below systemd files to your paths

Copy all of the service files in the systemd folder to `/lib/systemd/system/`  
`cp -a ~/xcash-official/xcash-dpops/scripts/systemd/* /lib/systemd/system/`

Reload systemd  
`systemctl daemon-reload`

Create a systemd PID folder  
`mkdir ~/xcash-official/systemdpid/`

Create a logs folder  
`mkdir ~/xcash-official/logs/`

Create a mongod pid file and a xcashd pid file

```text
touch ~/xcash-official/systemdpid/mongod.pid
touch ~/xcash-official/systemdpid/xcash_daemon.pid
```

### MongoDB

This is the systemd file for MongoDB

```text
[Unit]
Description=MongoDB Database Server
After=network.target

[Service]
Type=forking
User=root
Type=oneshot
RemainAfterExit=yes
PIDFile=/root/xcash-official/systemdpid/mongod.pid
ExecStart=/root/xcash-official/mongodb-linux-x86_64-ubuntu1804-4.2.0/bin/mongod --fork --syslog

LimitFSIZE=infinity
LimitCPU=infinity
LimitAS=infinity
LimitNOFILE=64000
LimitNPROC=64000
LimitMEMLOCK=infinity
TasksMax=infinity
TasksAccounting=false

[Install]
WantedBy=multi-user.target
```

You will need to change the **User** to the user of the system

You will need to change the **PIDFile** to the full path of the `mongod.pid` file

You will need to change the **ExecStart** to the full path of the `mongod` file

Reload systemd after you have made any changes to the systemd service files  
`systemctl daemon-reload`

### XCASH Daemon

This is the systemd file for XCASH Daemon

```text
[Unit]
Description=XCASH Daemon systemd file

[Service]
Type=forking
User=root
PIDFile=/root/xcash-official/systemdpid/xcash_daemon.pid
ExecStart=/root/xcash-official/xcash-core/build/release/bin/xcashd --rpc-bind-ip 0.0.0.0 --rpc-bind-port 18281 --restricted-rpc --confirm-external-bind --log-file /root/xcash-official/logs/xcash-daemon_log.txt --max-log-file-size 0 --detach --pidfile /root/xcash-official/systemdpid/xcash_daemon.pid
RuntimeMaxSec=15d
Restart=always

[Install]
WantedBy=multi-user.target
```

Make sure to leave the RuntimeMaxSec in the systemd service file, as the XCASH Daemon usually needs to restart after a while to prevent it from not synchronizing

You will need to change the **User** to the user of the system

You will need to change the **PIDFile** to the full path of the `xcash_daemon.pid` file

You will need to change the **ExecStart** to the full path of the `xcashd` file

Reload systemd after you have made any changes to the systemd service files  
`systemctl daemon-reload`

### XCASH Wallet

This is the systemd file for XCASH Wallet

```text
[Unit]
Description=XCASH Wallet

[Service]
Type=simple
User=root
ExecStart=/root/xcash-official/xcash-core/build/release/bin/xcash-wallet-rpc --wallet-file /root/xcash-official/xcash_wallets/WALLET_FILE_NAME --password PASSWORD --rpc-bind-port 18285 --confirm-external-bind --daemon-port 18281 --disable-rpc-login --trusted-daemon
Restart=always

[Install]
WantedBy=multi-user.target
```

You will need to change the **User** to the user of the system

You will need to change the **WALLET\_FILE\_NAME** with the name of your wallet file, and the **PASSWORD** with the password of your wallet file.

Reload systemd after you have made any changes to the systemd service files  
`systemctl daemon-reload`

### XCASH DPOPS

This is the systemd file for XCASH DPOPS

```text
[Unit]
Description=XCASH DPOPS

[Service]
Type=simple
LimitNOFILE=64000
User=root
WorkingDirectory=/root/xcash-official/xcash-dpops/build/
ExecStart=/root/xcash-official/xcash-dpops/build/xcash-dpops --block_verifiers_secret_key BLOCK_VERIFIERS_SECRET_KEY
Restart=always

[Install]
WantedBy=multi-user.target
```

The LimitNOFILE will allow the XCASH DPOPS program to utilize up to 64000 concurrent file descriptors instead of the default 4096 for a linux process. The actual XCASH DPOPS program is limited to only accept up to 1000 concurrent connections due to that DPOPS usage and shared delegates website or delegates website usage will not be that much at the start. This will help with DDOS at this time and the limits in the XCASH DPOPS program will be updated.

You will need to change the **User** to the user of the system

You will need to change the **ExecStart** to the full path of the `xcash-dpops` file and add any startup flags if running a shared delegates website or a delegates website

You will need to change the **BLOCK\_VERIFIERS\_SECRET\_KEY** to your block verifiers secret key. Make sure this is the first parameter

For a full list of XCASH\_DPOPS parameters please read the [XCASH\_DPOPS Parameters]() section. This section will explain how to change any of the provided parameters in detail. Since the shared delegate option is probably the most used parameter, this parameter will be explained below.

```text
--shared_delegates_website --fee "fee" --minimum_amount "minimum_amount" - Run the shared delegates website, with a fee of "fee" and a minimum amount of "minimum_amount"
The fee in a percentage (1 would equal 1 percent. You can use up to 6 decimal places.)
The minimum for a public_address to receive a payment (10000 etc. The minimum amount should be in regular units, not atomic units.)
```

Reload systemd after you have made any changes to the systemd service files  
`systemctl daemon-reload`

### Firewall

This is the systemd file for firewall

```text
[Unit]
Description=firewall

[Service]
Type=oneshot
RemainAfterExit=yes
User=root
ExecStart=/root/xcash-official/xcash-dpops/scripts/firewall/firewall_script.sh

[Install]
WantedBy=multi-user.target
```

You will need to change the **User** to the user of the system

You will need to change the **ExecStart** to the full path of the `firewall_script.sh` file

Reload systemd after you have made any changes to the systemd service files  
`systemctl daemon-reload`

## How To Setup the Firewall

We will need to setup a firewall for our DPOPS node. The goal of settings up the firewall is to block any DDOS attacks. We will use IPtables for the firewall

Note this step is only necessary if using LXC containers, as the autoinstaller script will install the firewall for you if not using LXC containers

Run the bash script and choose either Firewall or Shared Delegates Firewall to install the firewall

```text
bash -c "$(curl -sSL https://raw.githubusercontent.com/X-CASH-official/xcash-dpops/master/scripts/autoinstaller/autoinstaller.sh)"
```

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
        <p><code>e.g. systemctl start xcash-dpops</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>stop</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl stop name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl stop xcash-dpops</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>restart</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl stop name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl restart xcash-dpops</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check status</code>
      </td>
      <td style="text-align:left">
        <p><b><code>systemctl status name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. systemctl status xcash-dpops</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check the logs</code>
      </td>
      <td style="text-align:left">
        <p><b><code>journalctl --unit=name_of_service_file_without.service</code></b>
        </p>
        <p><code>e.g. journalctl --unit=xcash-dpops</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check the logs<br />(last 100 lines)</code>
      </td>
      <td style="text-align:left">
        <p><b><code>journalctl --unit=name_of_service_file_without.service -n 100 --output cat</code></b>
        </p>
        <p><code>e.g. journalctl --unit=xcash-dpops -n 100 --output cat</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>check the logs<br />(live logging)</code>
      </td>
      <td style="text-align:left">
        <p><b><code>journalctl --unit=name_of_service_file_without.service --follow -n 100 --output cat</code></b>
        </p>
        <p><code>e.g. journalctl --unit=xcash-dpops --follow -n 100 --output cat</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

