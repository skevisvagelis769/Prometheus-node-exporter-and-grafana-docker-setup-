# To run: 
docker compose up -d (in the same directory as the docker-compose.yml)

# grafana_restart.service
Is a systemd service that auto-restarts the grafana containers via running the above run command

to make changes to it / replicate it:

- sudo nano /etc/systemd/system/grafana_restart.service 

To reload systemd
- sudo systemctl daemon-reexec
- sudo systemctl daemon-reload

To enable the service at boot
- sudo systemctl enable grafana-stack.service

Start it immediately
- sudo systemctl start grafana-stack.service

Verify status 
- sudo systemctl status grafana-stack.service


Explanation of code: 

The [Unit] section describes what the service is and when it should start

Requires=docker.service , the service runs only if the docker service is running

After=docker.service network-online.target, makes sure the service starts after docker service is running and the network is online

Wants=network-online.target, systemd tries to bring the networking online

The [Service] section describes what the service must do 
Type=oneshot , runs only once and then exits

ExecStart=/usr/bin/docker compose up -d , command that executes at system boot and runs docker compose up

RemainAfterExit=yes , even though the command exited, consider the service active so that systemd doesnt think the service stopped immediately

TimeoutStartSec=0, it disables timeout at startup, we want this because docker might need to pull images

The [Install]

WantedBy=multi-user.target , starts the service during system boot