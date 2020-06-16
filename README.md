## Instalação do Passbolt, em um cluster Kubernetes, via manifestos YAML:

---------

### Antes de tudo:

- Configurar o acesso ao cluster em que será instalado o Passbolt:

```bash
export KUBECONFIG=/path/to/kubeconfig
```


- Verificar a conexão com o cluster:

```bash
kubectl get nodes
```

Você deve ser capaz de ver todos os nodes do seu cluster Kubernetes.


- Faça o clone do repositório:

```bash
git clone https://git.jac.bsb.br/JAC/infra/passbolt.git

cd passbolt
```

### Algumas alterações necessárias

Com o clone do repostiório já feito, será preciso alterar algumas variáveis no ConfigMap nos Secrets e no ```storageClassName```:


#### MariaDB:

- Alterar o valor da variável ```ROOT_PASSWORD``` no Secret do MariaDB:

```bash
vim passbolt-manifestos/mariadb-passbolt/mariadb-secret.yml
```
**NOTA:** Lembre-se que o valor passado no Secret precisa estar em Base64.

- Alterar o campo ```storageClassName```, no arquivo que cria o PVC para o MariaDB, para o nome do seu storage:

```bash
vim passbolt-manifestos/mariadb-passbolt/mariadb-pvc.yml
```

#### App Passbolt

- Alterar o valor das variáveis ```app-full-base-url```, ```email-default-from``` e ```email-transport-default-host``` no ConfigMap do Passbolt:

```bash
vim passbolt-manifestos/app-passbolt/passbolt-configmap.yml
```

- Alterar o valor da variável ```ADMIN_PASSWORD``` no Secret do Passbolt:

```bash
vim passbolt-manifestos/app-passbolt/passbolt-secret.yml
```

**NOTA:** Lembre-se que o valor passado no Secret precisa estar em Base64 e, no caso do Secret do Passbolt, a senha passada precisa ser a mesma senha do MariaDB.

### Instalando o Passbolt no cluster:

Com os valores das variáveis apontadas acima já definidos, podemos partir para a instalação do Passbolt.

- Fazer o Deploy do MariaDB:

```bash
kubectl apply -f passbolt-manifestos/mariadb-passbolt/
```

Esperados o MariaDB concluir a instalação e em seguida instalamos a aplicação do Passbolt:

```bash
kubectl apply -f passbolt-manifestos/app-passbolt/
```
