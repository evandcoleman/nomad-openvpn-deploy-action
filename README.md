# Nomad Deploy Action

This GitHub Action sets up an OpenVPN connection and deploys a job to a Nomad cluster using a specified job file.

## Inputs

- `openvpn_config`: The OpenVPN configuration file content.
- `openvpn_username`: The OpenVPN username.
- `openvpn_password`: The OpenVPN password.
- `job_file_path`: Path to the Nomad job file.
- `tag`: The Docker image tag to deploy.
- `nomad_address`: The address of the Nomad server.

## Example Usage

```yaml
name: Deploy to Nomad with OpenVPN

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v3

      - name: Deploy to Nomad
        uses: evandcoleman/nomad-openvpb-deploy-action@main
        with:
          openvpn_config: |
            client
            dev tun
            proto udp
            remote your-vpn-server 1194
            resolv-retry infinite
            nobind
            persist-key
            persist-tun
            ca /path/to/ca.crt
            cert /path/to/client.crt
            key /path/to/client.key
          openvpn_username: "your-username"
          openvpn_password: "your-password"
          job_file_path: "./path/to/your-nomad-job.hcl"
