# ssh-agent

> **Warning**: The default inputs are designed to be convenient, but they are **not security best practices**. For production environments, I recommend restricting the `host` pattern and setting `insecure: false`.

## Inputs

| Input | Description | Required | Default |
| --- | --- | --- | --- |
| `key` | Private key to load into the agent. | **Yes** | |
| `host` | Host pattern for SSH config (e.g. `*` or `prod-*`). | No | `*` |
| `user` | Default user for SSH config. | No | `admin` |
| `port` | Default port for SSH config. | No | `22` |
| `insecure` | Disable `StrictHostKeyChecking` and `UserKnownHostsFile` for the host pattern. | No | `true` |

## Example

```yaml
name: Deploy

on: [push]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup SSH
        uses: BitHostDev/ssh-agent@main
        with:
          key: ${{ secrets.SSH_KEY }}
          user: ubuntu
          
      - name: Deploy
        run: |
          ssh ec2.aws "echo 'deploying...'"

      - name: Copy files
        run: |
          scp -r ./dist/* ec2.aws:/var/www/html/
```
