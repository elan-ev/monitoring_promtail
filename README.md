# Ansible Role for Loki and Promtail

![molecule](https://github.com/elan-ev/monitoring_promtail/actions/workflows/molecule.yml/badge.svg)

Install the latest [promtail](https://grafana.com/docs/loki/latest/clients/promtail/) version with [ansible](https://docs.ansible.com/).

## Role Variables

Have a look at the [defaults](defaults/main.yml) to see what variables you can set.
If your [loki](https://github.com/grafana/loki) instance does not run on the same host, you should set `loki_host` to the corresponding ip.

For promtail, as an example config, you can turn on two jobs as an example config by setting either one of `promtail_job_journal` or `promtail_job_nginx` to `true`.
In most cases, however, you would probably want to provide your own config-templates for loki and promtail.
You can achieve this by changing the path to the template file in `promtail_config_template`.

Make sure that promtail has reading rights on the corresponding log files.
Per default the promtail user will be added to the `adm` group that can read logs.
If you want to change this behavior set `make_varlog_accessible` to `false`.
You can also add the user to arbitrary other groups by specifying `additional_groups` as a list.

## Example Playbook

Your playbook might look like this:

```yaml
- hosts: all
  become: true
  roles:
    - elan.monitoring_promtail
```

## Development

For development and testing you can use [molecule](https://molecule.readthedocs.io/en/latest/).
With podman as driver you can install it like this – preferably in a virtual environment (if you use docker, substitute `podman` with `docker`):

```bash
pip install -r .dev_requirements.txt
```

Then you can *create* the test instances, apply the ansible config (*converge*) and *destroy* the test instances with these commands:

```bash
molecule create
molecule converge
molecule destroy
```

If you want to inspect a running test instance use `molecule login --host <instance_name>`, where you replace `<instance_name>` with the desired value.

The [prepare.yml](molecule/default/prepare.yml)-file will also install loki in the same container, so that promtail can be tested against a local installation of loki. 

## License

[BSD-3-Clause](LICENSE)

## Author Information

[ELAN e.V](https://elan-ev.de/)
