## Introduction

The X-Cash Namespace allows users to register their own name in the namespace. This allows a user to associate a public address with an easy to remember name (NAME.xcash) This protocol also allows a user to setup a public only wallet that can only send and receive public transactions, and a private only wallet that can only send and receive private transactions (using NAME.pxcash for public and NAME.sxcash for private). Each registration cost an amount that is set by the delegates. Each delegate can set their own price, but users can only use the current block producer (or wait for the next one etc) to register. Each registration is valid for 1 year and renewals are the same cost as registering.

This protocol is built into xcash-dpops repo so it will be the same instructions to update for delegates.

## Get Started

<table>
  <thead>
    <tr>
      <th style="text-align:left">What are you looking for?</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>I want to register or renew a name in the xcash namespace</b>
      </td>
      <td style="text-align:left">Please review the <a href="https://docs.xcash.foundation/remotedata/register">register</a> or <a href="https://docs.xcash.foundation/remotedata/renew">renew</a> docmentation. Once your ready use the <a href="website">website</a> setup for checking if names are registered and for a step by step walkthrough on how to register or renew a name.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>I want to edit my namespace.</b>
      </td>
      <td style="text-align:left">Please review the <a href="https://docs.xcash.foundation/remotedata/manage">manage</a> documentation</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>I want to setup my price as a delegate.</b>
      </td>
      <td style="text-align:left">Please review the <a href="https://docs.xcash.foundation/remotedata/delegate-management">delegate management</a> documentation</td>
    </tr>
    <tr>
  </tbody>
</table>

## Key features <a id="key-features"></a>

#### Characteristics

* Have users be able to type NAME.xcash in the wallet instead of the long public address to send. 
* Be certain for a private transaction from a user by giving them NAME.sxcash
* Be certain for a public transaction from a user by giving them NAME.pxcash
* Delegates host the namespace and can set their registration/renewal price

## Useful Links <a id="key-features"></a>

* \*\*\*\*[**GitHub**](https://github.com/X-CASH-official/xcash-dpops) - Source code of the DPoPS and namespace program.
