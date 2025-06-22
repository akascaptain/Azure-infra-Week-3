# Task 3
Create Virtual maching and Vnet from Azure CLI

# Azure VM & VNet Deployment Using Azure CLI

---

# Task Objective:
Perform the following steps to create and configure a Virtual Network and Virtual Machine using Azure CLI:
1. Create a new Resource Group
2. Create a Virtual Network (VNet) with Subnet
3. Create a Network Security Group (NSG) and configure SSH rule
4. Create a Public IP Address
5. Create a Network Interface (NIC)
6. Deploy a Virtual Machine with Ubuntu OS
7. Connect to the VM via SSH
8. Verify all created resources and configuration

---

# Task Steps Performed

## 1. Create a Resource Group
- Resource Group: MyResourceGroupWest
- Region: westus

  az group create --name MyResourceGroupWest --location westus

## 2. Create Virtual Network and Subnet
VNet: MyVNetWest | Address Space: 10.1.0.0/16
Subnet: MySubnetWest | Prefix: 10.1.1.0/24

Command Used:
az network vnet create \
  --resource-group MyResourceGroupWest \
  --name MyVNetWest \
  --address-prefix 10.1.0.0/16 \
  --subnet-name MySubnetWest \
  --subnet-prefix 10.1.1.0/24


## 3. Create NSG and Allow SSH
NSG Name: MyNSGWest
SSH Rule: Port 22 open for inbound access

az network nsg create --resource-group MyResourceGroupWest --name MyNSGWest

az network nsg rule create \
  --resource-group MyResourceGroupWest \
  --nsg-name MyNSGWest \
  --name AllowSSH \
  --protocol Tcp \
  --direction Inbound \
  --priority 1000 \
  --source-address-prefixes '*' \
  --source-port-ranges '*' \
  --destination-address-prefixes '*' \
  --destination-port-ranges 22 \
  --access Allow
  

## 4. Create Public IP Address
Public IP Name: MyPublicIPWest

az network public-ip create --resource-group MyResourceGroupWest --name MyPublicIPWest


## 5. Create Network Interface (NIC)
NIC Name: MyNICWest

az network nic create \
  --resource-group MyResourceGroupWest \
  --name MyNICWest \
  --vnet-name MyVNetWest \
  --subnet MySubnetWest \
  --network-security-group MyNSGWest \
  --public-ip-address MyPublicIPWest


## 6. Create Virtual Machine
VM Name: MyVMWest
Image: Ubuntu2204
Size: Standard_B1s
Username: azureuser
SSH keys auto-generated

az vm create \
  --resource-group MyResourceGroupWest \
  --name MyVMWest \
  --nics MyNICWest \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --size Standard_B1s


## 7. Get Public IP and Connect via SSH

az vm show \
  --resource-group MyResourceGroupWest \
  --name MyVMWest \
  --show-details \
  --query publicIps \
  --output tsv
Output IP: 172.184.130.117

ssh azureuser@172.184.130.117


## 8. Verify Resource Creation
All components created successfully:

az resource list --resource-group MyResourceGroupWest --output table
Outcome:
All task objectives were completed successfully:

--------

References:
Azure CLI Documentation: https://learn.microsoft.com/en-us/cli/azure

VM Create: https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest

VNet: https://learn.microsoft.com/en-us/azure/virtual-network/

SSH to Linux VM: https://learn.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows
