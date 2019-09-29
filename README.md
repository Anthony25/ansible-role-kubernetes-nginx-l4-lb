Ansible Role: nginx L4 loadbalancer for Kubernetes
==================================================

Ansible role to install a simple nginx as a L4 loadbalancer in Kubernetes.

Role Variables
--------------

```yaml
# Image used
kubernetes_nginx_l4_lb_image: "linuxserver/sickchill:latest"

# Namespace
kubernetes_nginx_l4_lb_namespace: "default"
# App name (used as selector)
kubernetes_nginx_l4_lb_app: "nginx-l4-lb"
# Daemonset name
kubernetes_nginx_l4_lb_daemonset: "nginx-l4-lb-daemonset"
# Service name
kubernetes_nginx_l4_lb_service: "nginx-l4-lb"

# Healthcheck port. If none, no healthcheck will be setup
kubernetes_nginx_l4_lb_healthcheck_port:

# Node selector
kubernetes_nginx_l4_lb_node_selector: {}

# Add custom labels in the daemonset metadata section
kubernetes_nginx_l4_lb_daemonset_labels: {}
# Add custom annotations in the daemonset metadata section
kubernetes_nginx_l4_lb_daemonset_annotations: {}

kubernetes_nginx_l4_lb_resources:
  limits:
    memory: "756Mi"
  requests:
    memory: "256Mi"

kubernetes_nginx_l4_lb_clients:
  - listen:  # listening address, with port
    proxy_pass:  # destination address, with port

```

Dependencies
------------

Kubectl needs to be installed on the host targeted by the role.


Example Playbook
----------------

```yaml

- hosts: kube-master
  run_once: true
  vars:
    kubernetes_nginx_l4_lb_clients:
      - listen: 0.0.0.0:25
        proxy_pass: mail.tld:25
  roles:
    - role: Anthony25.kubernetes_nginx_l4_lb
```

Use `run_once` to run the role on only one available master in the cluster.

An autoreload is set in the nginx image to detect any configuration change
and apply it without having to restart your pods.

License
-------

Tool under the BSD license. Do not hesitate to report bugs, ask me some
questions or do some pull request if you want to!
