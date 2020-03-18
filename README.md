# swarm_on_vm
Testing installation on Swarm on public Cloud (AWS)

### Vars

in inventory/group_vars/all:
* vars.yml
``` yml

ssh_key_path: /tmp/id_rsa                           #path for local key creation
ansible_ssh_private_key_file: "{{ ssh_key_path }}"  #var for ansible connection using key
workers_count: 1                                    #numer of swarm workers
ec2_access_key: "{{ encrypted_ec2_access_key }}"    #ref for access key in vaul file
ec2_secret_key: "{{ encrypted_ec2_secret_key }}"    #ref for access key in vaul file
region: eu-west-1                                   #aws region for ec2 instances
key_name: temporary_key_ec2                         #name of key in AWS env
sec_group_master: swarm-master-sg                   #SG name for swarm Master (created in this playbook)
sec_group_workers: swarm-worker-sg                  #SG name for swarm worker (created in this playbook)
inbound_cidr_ip: "{{ encrypted_inbound_cidr_ip }}"  #used in SG swarm master to allow inbound connection on 22 need a cidr block ip
image: ami-0019f18ee3d4157d3                        #ami to provision instances
vpc_id: vpc-84d173e0                                #vpc-id used for SG configuration
vpc_subnet_id: subnet-a8a619de                      #subnet for swarm master
vpc_subnet_id_worker: subnet-a8a619de               #subnet for swarm worker NB: if you use private subnet you need to configure connetion in ansible.cfg to use tunneling

docker_service_override: |
  [Service]
  ExecStart=
  ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375

docker_service_state: "started"
docker_service_enabled: "yes"
docker_daemon_config:
  tls: true
  tlscert: "/etc/ssl/{{ ansible_ec2_public_hostname }}/{{ ansible_ec2_public_hostname }}.pem"
  tlskey: "/etc/ssl/{{ ansible_ec2_public_hostname }}/{{ ansible_ec2_public_hostname }}.key"
# }
```
