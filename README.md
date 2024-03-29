# Reference
Example playbooks / tools

# shell.yml - run ad hoc command(s) on ad hoc target(s)
```
ansible-playbook shell.yml --extra-vars "command='whoami && uptime' target=127.0.0.1,vm2.example.com,target3"
```

# crypt.yml - encrypted string(s) in playbooks
```
# generate password file (~/password.txt)
cat /dev/urandom | tr -dc 'a-zA-Z0-9=;:`"<>,./?!@#$%^&(){}[]' | fold -w 512 | head -n 1 > ~/password.txt

# encrypt string with password file (type/paste string, then CTRL+D)
ansible-vault encrypt_string --vault-password-file=~/password.txt > ~/encryptstring.txt

# replace PLACEHOLDER with content of ~/encryptstring.txt in crypt.yml
perl -pe 's#PLACEHOLDER#`cat ~/encryptstring.txt`#ge' -i crypt.yml

# execute crypt.yml
ansible-playbook crypt.yml --vault-password-file=~/password.txt --extra-vars "target=127.0.0.1 commands='echo {{ encryptstringvar }}'"
```
