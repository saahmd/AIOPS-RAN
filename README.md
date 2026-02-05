
Create Project 
I. Navigate to: Automation Execution → Projects → Create Project.

II. Fill in the details

| Parameter             | Value                                               |
   |-----------------------|-----------------------------------------------------|
   | Name                  | MyProject                                           |
   | Organization          | Default                                             |
   | Source Control Type   | Git                                                 |
   | Source Control URL    | https://github.com/saahmd/LLamaonOpenshift.git      |


III. Save and verify that **MyProject** status is **Success**.  



Job Templates

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
   | Credentials     | AAP,Demo Credential                                            |
   | Extra variables | llama_stack_url: llamastack-server-llama-serve.apps.cluster-w8px7.w8px7.sandbox1697.opentlc.com
full_anomaly_report: '{{ full_anomaly_report }}'  |

