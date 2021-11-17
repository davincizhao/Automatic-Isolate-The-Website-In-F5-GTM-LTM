# Automatic Isolate The Website In F5 GTM LTM




## Propose and Scope the Project
 - Good structure XML file as Direct input, using ansible module to parsed xml
 - According to the keywords in xml,
 - Auto select data or action into ansible module. 
 - Fully autmate finish the isolation tasks, eliminate the manual's step of transform the XML to YAML formatting.

## Tool-box server(AWX)
- CI/CD tool platform: AWX (Ansible tower)
- tools: Ansible. net_spec.yaml(template for xml data structure)
- Data format: XML, Yaml
- Repo: Github
- API: F5 REST API

## Repo Structure
- main.yml: main entry of Ansible playbook, parse the input XML file, using parse_xml module with "**net_spec.yml**" template. loading group vars,extract the nework operations from XML, as input argument to task playbook "**take_action.yml**"
- take_action.yml: take network operations action as input, define a variable: ltm_vip_n(**LTM vip name**) and ltm_p_m(**LTM pool memeber**), extract host_ip as input to task "**run_device.yml**". according to the host ip to take action in different specify network devices.
- run_device.yml: take host_ip as input, according to isolution procedures and the keywords(**DisableGTM,DisableLTM,Disableltm_cross**) from XML, like (GTM start up or shutdown, pasue 4 min,) call F5 bigip modules (**bigip_gtm_pool_member,bigip_virtual_server**) to extract "partition and pool", and use "partition and pool" as input arguments to call deeper task "**run_member.yml**" which need to take action in pool member level.
- run_member.yml: call F5 bigip modules (bigip_pool_member) to offline LTM pool members.
- net_spec.yml: this is very important template file to extract Good structure XML file. 1. Analyze the XML structure, 2. define the variables "**vars**" according the keywords in XML, 3. define the "**keys**", according to the keywords that you want to use. this file is used for extract the data from XML file.
