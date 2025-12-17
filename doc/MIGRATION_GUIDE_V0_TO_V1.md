# Migration guide from Chart V0.x to V1.x

# V0.x characteristics
- Less secure
- Direct API access from NeoLoad Web to the Kubernetes cluster
- Requires opening network access from NeoLoad Web to the cluster

# V1.x characteristics
- More secure
- Agent-based communication between NeoLoad Web and the Kubernetes cluster
- The connection is initiated by the agent from the cluster to NeoLoad Web using a long-term token (no inbound connection)
- The agent polls orders from NeoLoad Web and execute them in the cluster

# Migration steps
1. Helm install V1 as a new release (see [installation instructions in Readme.md](../Readme.md#installation))
2. In NeoLoad Web
   1. Configure an Agent-based Infrastructure provider to use the newly deployed agent
   2. Create a zone with this provider and verify that tests can run successfully
   3. Update the provider of existing zones
   4. Remove the old Infrastructure provider
3. Helm uninstall V0 release (see [uninstall instructions in Readme.md](../Readme.md#uninstall))
