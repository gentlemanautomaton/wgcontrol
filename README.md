wgcontrol [![GoDoc](https://godoc.org/github.com/gentlemanautomaton/wgcontrol?status.svg)](https://godoc.org/github.com/gentlemanautomaton/wgcontrol)
====

This is a work-in-progress and is not yet ready for use.

The `wgcontrol` project manages a virtual private network based on
[WireGuard](https://www.wireguard.com/). Its intended use case is a large
number of computers connecting to one or more remote networks such as
corporate offices or cloud-hosted environments.

WireGuard is a building block that provides a secure channel between peers on
the internet. It does not distinguish between the kind of peers that it
connects. The role of `wgcontrol` is to synchronize the WireGuard
configuration for these peers so that they can be connected together with a
desired topology.

In `wgcontrol`, any peer _without_ an addressable endpoint is called a
`Device` and any peer _with_ an addressable endpoint is called a `Node`.
Corporate gateways and cloud ingress services are nodes; laptops and remote
workstations are devices.

Devices do not share their addresses via `wgcontrol`. This is an intentional
design decision enforced at the protocol level in order to protect the privacy
of employees working at home. As a result of this decision, devices are unable
to communicate with each other directly. Devices only communicate with nodes.

A central authority, known as the `wgcontroller`, is responsible for collecting
and distributing configuration changes. It collects public keys from devices
and distributes them to all nodes. Likewise, public keys and addresses are
collected from nodes and distributed to all devices.

Private keys never leave the hardware to which they are assigned.

Initial device registration can be a single-step or dual-step process. In
single-step registration a device authenticates itself during registration
through an external authenticator like kerberos or a pre-shared or
time-limited secret. In dual-step registration a device adds itself to a
pending registration list in the controller and awaits approval at a later
time.