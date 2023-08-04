# AdGuard Home on Kubernetes

This repository contains the necessary resources and documentation to deploy AdGuard Home on a Kubernetes cluster using Helm.

## Overview

AdGuard Home is a network-wide software for blocking ads & tracking. After you set it up, it'll cover ALL your home devices, and you don't need any client-side software for that. 

In this setup, we use Helm, a package manager for Kubernetes, to deploy AdGuard Home. We also use MetalLB, a load-balancer implementation for bare metal Kubernetes clusters, to expose the AdGuard Home service outside of the cluster.

## Prerequisites

- A running Kubernetes cluster.
- Helm installed on your local machine and your cluster.
- MetalLB installed on your cluster.

## Deployment

The deployment consists of the following steps:

1. **Install MetalLB**: MetalLB is used to assign external IPs to services of type LoadBalancer in bare metal clusters.

2. **Configure MetalLB**: Set the IP range that MetalLB can use to assign external IPs. This range should be a part of your local network subnet that is reserved for the cluster.

3. **Install AdGuard Home using Helm**: Use the Helm chart provided in this repository to install AdGuard Home. The values in the chart have been modified to use a service of type LoadBalancer, so that MetalLB can assign an external IP to it.

4. **Configure DNS**: Configure your router or your devices to use the external IP assigned to the AdGuard Home service as their DNS server.

## Known Issues

AdGuard Home does not display the correct IP address for each device in its query log. Instead, all queries appear to come from the same IP. This is due to the way DNS queries are routed in the network and is currently a known limitation.

## Contributions

Contributions are welcome! If you have a fix or an improvement, feel free to submit a pull request.
