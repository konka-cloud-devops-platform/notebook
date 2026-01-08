# 01-01-2026
- I was used ubuntu server
- Jenkins server setup on vm
  - Install java 21 on both Jenkins Master and build agents
- Jenkins Build Agent & Github Runner Setup on VM
  

- Install Java in both master build agents
- configure master slave configuration
- install github runner
create service file

```
#sudo vi /etc/systemd/system/github-runner.service

[Unit]
Description=GitHub Actions Runner
After=network.target

[Service]
Type=simple
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/actions-runner
ExecStart=/home/ubuntu/actions-runner/run.sh
Restart=always
RestartSec=10
KillMode=process
TimeoutStopSec=30

[Install]
WantedBy=multi-user.target
```

sudo systemctl daemon-reload
sudo systemctl enable github-runner
sudo systemctl start github-runner
sudo systemctl status github-runner

- Define the reusable workflow in central repo and tested successfully
