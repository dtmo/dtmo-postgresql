# dtmo.postgresql.postgresql

A role to install PostgreSQL.

## Requirements

No requirements.

## Role Variables

<!-- ANSIBLE DOCSMITH MAIN START -->
<!-- ANSIBLE DOCSMITH MAIN END -->

## Dependencies

No dependencies.

## Example Playbook

```yaml
---
- name: Configure PostgreSQL
  hosts: postgresql_hosts
  vars:
    postgresql_listen_addresses: ['*']
    postgresql_port: 5432
  tasks:
    - name: Install PostgreSQL
      ansible.builtin.include_role:
        name: dtmo.postgresql.postgresql

    - name: Configure firewalld
      become: true
      ansible.posix.firewalld:
        port: "{{ postgresql_port }}/tcp"
        state: enabled
        permanent: true
        immediate: true
      when: ansible_facts.os_family == 'RedHat'

    - name: Configure ufw
      become: true
      community.general.ufw:
        rule: allow
        port: "{{ postgresql_port }}"
        proto: tcp
      when: ansible_facts.os_family == 'Debian'
```

## License

PostgreSQL
