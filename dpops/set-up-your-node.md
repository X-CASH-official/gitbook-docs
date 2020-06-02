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

## 

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

