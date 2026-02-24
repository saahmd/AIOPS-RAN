# Optimizing RAN operations with OpenShift AI and Remediation using Ansible Automation Platform

## Steps to configure Ansible Automation Platform

This demo is based out on a catalog item available on RHDP

🚀 Prerequisites

- Access to RHDP Catalog

- Red Hat account with valid credentials

- Slack token (for sending notifications via Slack)

---

### 📦 Step 1: Order RHDP Catalog Item

- Order order this catalog item [RHDP](https://catalog.demo.redhat.com/catalog?search=ai+driven&item=babylon-catalog-prod%2Fsandboxes-gpte.ai-driven-ansible-automation.prod)
with default values

- Once the catalog item is ready, head over to the lab environment.

---

### 📂 Step 3: Create Project

I. Navigate to: Automation Execution → Projects → Create Project.

II. Fill in the details

| Parameter             | Value                                               |
   |-----------------------|-----------------------------------------------------|
   | Name                  | MyProject                                           |
   | Organization          | Default                                             |
   | Source Control Type   | Git                                                 |
   | Source Control URL    | https://github.com/saahmd/LLamaonOpenshift.git      |


III. Save and verify that **MyProject** status is **Success**.  



### 📝 Step 4: Create Job Templates

I. Create "Print Anomaly" Template

   | Parameter       | Value                                |
   |-----------------|--------------------------------------|
   | Name            | Print Anomaly                 |
   | Inventory       | Demo Inventory                       |
   | Project         | MyProject                            |
   | Playbook        | playbooks/ai-ran-template.yml      |
   | Credentials     | AAP,lab-credential                                            |
   | Extra variables | full_anomaly_report: '{{ full_anomaly_report }}'  |

II. Create "Agent for RAN" Template

   | Parameter       | Value                                |
   |-----------------|--------------------------------------|
   | Name            | Agent for RAN                 |
   | Inventory       | Demo Inventory                       |
   | Project         | MyProject                            |
   | Playbook        | playbooks/ran_agent.yml      |
   | Credentials     | AAP,Demo Credential      |
   | Extra variables |  llama_stack_url: "YOUR LLAMA-STACK URL" <br> <br> full_anomaly_report: '{{ full_anomaly_report }}'  |
   | Prompt on Launch | ✅ |

   

III. Create "Get Lightspeed Prompt" Template

   | Parameter       | Value                                |
   |-----------------|--------------------------------------|
   | Name            | Get Lightspeed Prompt         |
   | Inventory       | Demo Inventory                       |
   | Project         | MyProject                            |
   | Playbook        | playbooks/aap_create_job_template.yml      |
   | Credentials     | AAP                                          |

IV. Create "Send Report to Slack" Template
 
 | Parameter       | Value                                |
   |-----------------|--------------------------------------|
   | Name            | Send Report to Slack       |
   | Inventory       | Demo Inventory                       |
   | Project         | MyProject                            |
   | Playbook        | playbooks/slack_ran.yml      |
   | Credentials     | AAP                                          |
   

V. Create "AI Insights and Lightspeed prompt generation" Workflow Template

   1. Navigate to **Automation Execution → Templates → Create → Workflow Job Template**.  
   2. Fill in:  
      | Parameter    | Value                                        |
      |--------------|----------------------------------------------|
      | Name         | AI Insights and Lightspeed prompt generation |
      | Organization | Default        

   3. Click **Create workflow job template**.  
   
      ![Visual](images/ai-ran-workflow.png)


### 📝 Step 5: Configure Apache Kafka Container

I. Create "Setup Kafka" Template

   | Parameter       | Value                                |
   |-----------------|--------------------------------------|
   | Name            | Setup Kafka                |
   | Inventory       | service-inventory                      |
   | Project         | MyProject                            |
   | Playbook        | playbooks/kafka.yml      |
   | Credentials     | AAP,lab-credential                                            |
   | Extra variables | kafka_external_hostname: YOUR-HOSTNAME `(ex: service1.pchzg.sandbox2169.opentlc.com)`  |

   `The kafka_external_hostname is similar to the Mattermost External Hostname except the https and port`

II. Run the Setup Kafka template

III. log on to bastion or mac and test endpoint using:
    
       nc -vz YOUR-HOSTNAME 9092

IV. log on the service1 host and exec into cp-kafka pod
    
    podman exec -it cp-kafka /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server service1:9092 --topic ai-ran-logs --from-beginning

V. On mac enter test messages using below command
    
    podman run --rm -it \
        --platform linux/arm64 \
        confluentinc/cp-kafka:7.6.0 \
        /usr/bin/kafka-console-producer \
        --broker-list YOUR-HOSTNAME:9092 \
        --topic ai-ran-logs

VI. check entry on mac
   
      podman run --rm -it \
        --platform linux/arm64 \
        confluentinc/cp-kafka:7.6.0 \
        /usr/bin/kafka-console-consumer \
        --bootstrap-server YOUR-HOSTNAME:9092 \
        --topic ai-ran-logs \
        --from-beginning

     
### 📝 Step 6: Configure Event Driven Ansible

I. Create Project for Event Driven Ansible
   Navigate to: Automation Decision → Projects → Create Project.

ii. Fill in the details:

| Parameter             | Value                                               |
   |-----------------------|-----------------------------------------------------|
   | Name                  | mwc-project                                  |
   | Organization          | Default                                             |
   | Source Control Type   | Git                                                 |
   | Source Control URL    | https://github.com/saahmd/LLamaonOpenshift.git      |


### Create Rulebook Activation

I. Navigate to: Automation Decision → Rulebook Activations.
II. Click Rulebook Activation
II. Fill in these details

   | Parameter       | Value                                |
   |-----------------|--------------------------------------|
   | Name            | ai-ran                 |
   | Inventory       | Demo Inventory                       |
   | Project         | mwc-project                            |
   | Playbook        | ai_ran.yml      |
   | Credentials     | AAP                                           |


