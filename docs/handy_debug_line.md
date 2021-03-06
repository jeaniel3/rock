
# Test a random command
ansible rockserver1.lan -i inventory/dev/inventory.yml -m debug -a "msg={{ '/dev/sdb' | regex_search('(sd.)') }}"

# Get all the variables for a host
ansible rockserver1.lan -i inventory/dev/inventory.yml -m debug -a "var=hostvars"

# View ansible built in variables
ansible rockserver1.lan -i inventory/dev/inventory.yml -m setup

# Standalone test file:
- hosts: localhost

  tasks:

  - name: Kube | Insert kube-dns into /etc/resolv.conf
    lineinfile:
      insertbefore: BOF
      line: "nameserver 192.168.1.5"
      path: /etc/resolv.conf

# Delete evicted pods in Kubernetes
kubectl get po -a --all-namespaces -o json | jq  '.items[] | select(.status.reason!=null) | select(.status.reason | contains("Evicted")) | "kubectl delete po \(.metadata.name) -n \(.metadata.namespace)"' | xargs -n 1 bash -c
