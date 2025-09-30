
# Projeto AWS CloudFormation

Este projeto contém **templates CloudFormation em YAML** utilizados para criar e configurar recursos na AWS de forma automatizada. A ideia é mostrar como é possível provisionar serviços de infraestrutura como EC2, S3 e IAM usando apenas scripts declarativos.

---

## 📂 Arquivos

### 1. `01-EC2.yaml`

**Objetivo:** Criar uma instância EC2 simples.

* Cria uma instância EC2 do tipo `t2.micro` na zona `us-east-1a`.
* Utiliza a AMI `ami-0ed9277fb7eb570c9` (Amazon Linux 2).
* Aplica a tag `Name=EC2`.

👉 **Resultado:** Instância EC2 básica, sem software adicional.

---

### 2. `02-Apache.yaml`

**Objetivo:** Criar uma instância EC2 com **Apache** instalado automaticamente.

* Cria uma instância `t2.micro` com a mesma AMI anterior.
* Aplica a tag `Name=Webserver-Apache`.
* Usa **UserData** para:

  * Instalar Apache (`httpd`).
  * Iniciar e habilitar o serviço.
  * Criar página HTML em `/var/www/html/index.html` com hostname da máquina.

👉 **Resultado:** Instância EC2 já configurada como servidor web Apache.

---

### 3. `03-Firewall.yaml`

**Objetivo:** Criar instância EC2 com Apache e configurar **Security Group**.

* Repete configuração do `02-Apache.yaml`.
* Cria um **Security Group** chamado `GrupoSeguranca`.
* Regras do Security Group:

  * Permite tráfego **TCP porta 80** de qualquer IP (`0.0.0.0/0`).

👉 **Resultado:** Instância EC2 rodando Apache, acessível via HTTP público.

---

### 4. `04-EC2_S3_UserGroup.yaml`

**Objetivo:** Exemplo mais completo: criar EC2, bucket S3, grupo e usuário IAM.

* **Parâmetros**

  * `InstanceType`: define tipo da instância (padrão `t2.micro`).

* **Recursos criados:**

  1. **S3Bucket** → Cria bucket `S3-FOUNDATION`.
  2. **IAMGroup** → Cria grupo `GPO-ADMIN-LAB`.
  3. **IAMUser** → Cria usuário `admin01` e associa ao grupo.
  4. **EC2Instance** → Instância Ubuntu:

     * AMI escolhida via `Mappings` dependendo da região.
     * Necessário definir `KeyName` válido (par de chaves existente na conta).
     * Executa script de inicialização instalando `python3-pip`.
  5. **EC2SecurityGroup** → Grupo de segurança liberando porta 22 (SSH).

* **Mappings**

  * Define AMIs Ubuntu específicas para `us-east-1` e `us-west-2`.

* **Outputs**

  * `InstanceId` → ID da instância criada.
  * `S3BucketName` → Nome do bucket.
  * `IAMUserName` → Nome do usuário IAM.

👉 **Resultado:** Estrutura completa com instância EC2, bucket S3, grupo e usuário IAM.

---

## 🚀 Conclusão

Esses templates mostram a evolução no uso do **AWS CloudFormation**:

* Do simples (instância EC2) até a criação de ambientes mais completos (EC2 + S3 + IAM).
* Permitem automatizar e versionar infraestrutura.
* Facilitam reuso e documentação de ambientes em nuvem.

📌 Esse aprendizado demonstra como a **Infraestrutura como Código (IaC)** ajuda a ganhar agilidade e padronização em ambientes AWS.
