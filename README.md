# Get DigitalOcean Droplet

A composition action that retrieves information about an existing DigitalOcean droplet using the DigitalOcean API v2.

## Overview

This action performs a single step:
1. **Get Droplet**: Fetches droplet information from the DigitalOcean API using `http-get`

## Inputs

The action expects the following inputs:

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `api_token` | string | Yes | Digital Ocean API token with read permissions |
| `droplet_id` | number | Yes | ID of the droplet to retrieve |

## Usage

### Basic Example

```json
{
  "api_token": "dop_v1_abc123...",
  "droplet_id": 123456789
}
```

### In a Workflow

```json
{
  "steps": {
    "get_droplet_info": {
      "uses": "tgirotto/do-get-droplet:0.0.1",
      "inputs": [
        "{{inputs[0].api_token}}",
        "{{inputs[0].droplet_id}}"
      ]
    }
  }
}
```

## Outputs

The action provides one output:

1. **`droplet`**: A `DigitalOceanDroplet` object containing complete droplet information

### DigitalOceanDroplet Structure

The output includes the following fields:

| Field | Type | Description |
|-------|------|-------------|
| `id` | number | Droplet ID |
| `name` | string | Droplet name |
| `memory` | number | Memory in MB |
| `vcpus` | number | Number of virtual CPUs |
| `disk` | number | Disk size in GB |
| `locked` | boolean | Whether the droplet is locked |
| `status` | string | Droplet status (e.g., "active", "off", "archive") |
| `kernel` | object | Kernel information |
| `created_at` | string | ISO 8601 timestamp of creation |
| `features` | array | Enabled features |
| `backup_ids` | array | List of backup IDs |
| `snapshot_ids` | array | List of snapshot IDs |
| `image` | object | Image information |
| `size` | object | Size information |
| `size_slug` | string | Size slug (e.g., "s-1vcpu-1gb") |
| `networks` | object | Network configuration (v4, v6, private, public) |
| `region` | object | Region information |
| `tags` | array | List of tags |
| `volume_ids` | array | List of attached volume IDs |

### Example Output

```json
{
  "droplet": {
    "id": 123456789,
    "name": "my-droplet",
    "memory": 1024,
    "vcpus": 1,
    "disk": 25,
    "locked": false,
    "status": "active",
    "kernel": { ... },
    "created_at": "2024-01-15T10:30:00Z",
    "features": [],
    "backup_ids": [],
    "snapshot_ids": [],
    "image": { ... },
    "size": { ... },
    "size_slug": "s-1vcpu-1gb",
    "networks": {
      "v4": [...],
      "v6": [...]
    },
    "region": { ... },
    "tags": ["production"],
    "volume_ids": []
  }
}
```

## API Endpoint

This action uses the DigitalOcean API v2 endpoint:
```
GET https://api.digitalocean.com/v2/droplets/{droplet_id}
```

## Requirements

- **API Token**: A valid DigitalOcean API token with read permissions
- **Droplet ID**: The numeric ID of an existing droplet
- **Network Access**: HTTPS access to `api.digitalocean.com`

## Error Handling

The action will fail if:
- The API token is invalid or lacks permissions
- The droplet ID doesn't exist
- Network connectivity issues occur
- The API returns an error response

## Composition Structure

This action is a composition that orchestrates one sub-action:

1. **`get_droplet`**: Uses `tgirotto/http-get:0.0.18` to fetch droplet information from the DigitalOcean API

## Permissions

The action requires network permissions for:
- `http` - To communicate with the DigitalOcean API
- `https` - To communicate with the DigitalOcean API (primary)

## Notes

- The action returns the complete droplet object as returned by the DigitalOcean API
- All network information (IPv4, IPv6, public, private) is included in the `networks` field
- The `status` field indicates the current state of the droplet (e.g., "active", "off", "archive")
- The action is read-only and does not modify the droplet in any way

## Related Actions

- `tgirotto/do-create-droplet-tf` - Create a new droplet using Terraform
- `tgirotto/do-create-ssh-key` - Create SSH keys for droplet access
- `tgirotto/do-create-vpc` - Create VPCs for droplet networking

