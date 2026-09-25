# Ansible Workspace 

Welcome to my central repository for **Infrastructure as Code (IaC)**. This repository houses all of my automated configurations, system provisioning playbooks, and server orchestration management scripts using **Ansible**.

## 📁 Repository Structure

The directory is organized into logical functional areas to keep the playbooks modular and reusable:

```text
ansible-workspace/
├── inventory/           # Encrypted server inventories (Production, Staging, etc.)
├── playbooks/           # Reusable configuration management playbooks
│   └── docker-setup.yml # Automated system cleaning & Docker engine setup
├── .gitignore           # Prevents accidental commits of local vault passwords
└── README.md            # Repository documentation
```

## 🛠️ Included Playbooks

### 1. Docker Installation & Environment Setup (`playbooks/docker-setup.yml`)
* **Purpose:** Cleans up broken or conflicting system binaries, provisions standard container engines, ensures execution states, and safely grants execution permissions to deployment accounts.
* **Target OS:** Ubuntu / Debian family.
* **Features:** Safe dependency mapping resolution (`containerd.io` vs standard system structures) and native idempotent execution mapping.

---

## 🔒 Security & Vault Architecture

To protect server credentials, SSH pathways, and privilege escalating tokens, all inventories are encrypted locally using **Ansible Vault (AES256)**. 

### Running Playbooks with Encrypted Variables
To execute playbooks in this repository, you must supply the explicit vault secret key.

**Option A: Manual Prompt (Recommended for manual execution)**
```bash
ansible-playbook -i inventory playbooks/docker-setup.yml --ask-vault-pass
```

**Option B: Automation Engine Pathway (Recommended for continuous pipelines)**
Ensure you point to a restricted local plain-text wrapper file (`chmod 600 ~/.ansible_vault_pass`) containing the decryption secret via an operational `ansible.cfg` mapping or inline command switches:
```bash
ansible-playbook -i inventory playbooks/docker-setup.yml --vault-password-file ~/.ansible_vault_pass
```

> ⚠️ **Important Security Note:** The `.ansible_vault_pass` tracking target is explicitly listed in the `.gitignore` parameter tree. Never check raw environment secrets or local authentication key bindings into the upstream GitHub repository branches.
