Pihole docker container to use without a raspberry pi.
use environment file for variables specific to personal setup

Disable systemd-resolved port 53
    Modern releases of Ubuntu (17.10+) and Fedora (33+) include systemd-resolved which is configured by default to implement a caching DNS stub resolver. This will prevent pi-hole from listening on port 53. The stub resolver should be disabled with:

    sudo sh -c 'mkdir -p /etc/systemd/resolved.conf.d && printf "[Resolve]\nDNSStubListener=no\n" | tee /etc/systemd/resolved.conf.d/no-stub.conf'

    This will not change the nameserver settings, which point to the stub resolver thus preventing DNS resolution. Change the /etc/resolv.conf symlink to point to /run/systemd/resolve/resolv.conf, which is automatically updated to follow the ubuntu system's netplan or fedora system's sysconfig:

    sudo sh -c 'rm -f /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'

    After making these changes, you should restart systemd-resolved using:

    systemctl restart systemd-resolved

Nginx Proxy Manager conflict with port 80
    Nginx proxy manager also uses port 80 and 443 which creates a conflict with pihole.

    Edit docker-compose.yaml to:

    "8080:80/tcp"
    "8443:443/tcp"

