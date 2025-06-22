# ===========================
#  BASIC VARIABLES
# ===========================
$resourceGroup = "CapsResourceGroup"
$location = "CentralIndia"
$vnetName = "capsVNet"
$subnetName = "capsSubnet"
$subnetPrefix = "10.0.0.0/24"
$publicIpName = "capsPublicIP"
$nicName = "capsNic"
$vmName = "capsVM"
$vmSize = "Standard_B1s"

# ===========================
#  CREATE RESOURCE GROUP
# ===========================
New-AzResourceGroup -Name $resourceGroup -Location $location

# ===========================
#  CREATE VNET & SUBNET
# ===========================
$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix $subnetPrefix
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $vnetName `
    -AddressPrefix "10.0.0.0/16" `
    -Subnet $subnetConfig

# ===========================
#  CREATE PUBLIC IP
# ===========================
$publicIp = New-AzPublicIpAddress -Name $publicIpName `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -Sku Standard

# ===========================
#  CREATE NIC
# ===========================
$nic = New-AzNetworkInterface -Name $nicName `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id

# ===========================
#  GET VM CREDENTIALS
# ===========================
$cred = Get-Credential -Message "Enter username and password for VM"

# ===========================
#  CONFIGURE VM
# ===========================
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize $vmSize |
    Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate |
    Set-AzVMSourceImage -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2022-Datacenter" -Version "latest" |
    Add-AzVMNetworkInterface -Id $nic.Id

# ===========================
#  DEPLOY VM
# ===========================
New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
