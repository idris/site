---
title: Racks
permalink: /guide/racks/
phase: deploy
---

A Rack is secure cloud environment. It is a collection of a VPC, VM and Container cloud services that are configured to run your app Images and Services.

A Rack enables you to deploy and run your app in the cloud in a way that is secure and scalable.

Provisioning a Rack requires AWS credentials with sufficient IAM permissions. You may optionally specify a region other than the default of `us-east-1`:

<pre class="terminal">
<span class="command">convox install --region=us-east-1</span>
...

Success. Try `convox apps`.
</pre>

This will take approximately 10 minutes while AWS configures all the cloud services in your Rack.

Now run `convox doctor` to validate your Rack:

<pre class="terminal">
<span class="command">convox doctor</span>

### Deploy: Rack
[<span class="pass">✓</span>] Rack set up
[<span class="pass">✓</span>] Rack up to date
</pre>

Now that you have a Rack, you can [release your first App](/guide/releases/).
