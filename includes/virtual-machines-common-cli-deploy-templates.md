
* [Azure’da hızlı bir şekilde sanal makine oluşturma](#quick-create-a-vm-in-azure)
* [Azure’da bir sanal makineyi şablondan dağıtma](#deploy-a-vm-in-azure-from-a-template)
* [Özel bir görüntüden sanal makine oluşturma](#create-a-custom-vm-image)
* [Sanal ağ ve yük dengeleyicisi kullanan bir sanal makine dağıtma](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [Kaynak grubunu kaldırma](#remove-a-resource-group)
* [Bir kaynak grubu dağıtımının günlüğünü görüntüleme](#show-the-log-for-a-resource-group-deployment)
* [Bir sanal makine hakkında bilgi görüntüleme](#display-information-about-a-virtual-machine)
* [Linux tabanlı sanal makineye bağlanma](#log-on-to-a-linux-based-virtual-machine)
* [Sanal makineyi durdurma](#stop-a-virtual-machine)
* [Sanal makineyi başlatma](#start-a-virtual-machine)
* [Veri diski ekleme](#attach-a-data-disk)

## <a name="getting-ready"></a>Hazırlanma
Azure CLI’yi Azure kaynak gruplarıyla kullanabilmeniz için doğru Azure CLI sürümüne ve bir Azure hesabına sahip olmanız gerekir. Azure CLI yoksa [yükleyin](../articles/xplat-cli-install.md).

### <a name="update-your-azure-cli-version-to-090-or-later"></a>Azure CLI sürümünüzü 0.9.0 veya sonraki bir sürüme güncelleştirin
0.9.0 veya sonraki sürümün yüklü olup olmadığını görmek için `azure --version` yazın.

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

Sürümünüz 0.9.0 veya üzeri değilse, yerel yükleyicilerden biriyle ya da `npm update -g azure-cli` yazarak **npm** aracılığıyla güncelleştirmeniz gerekir.

Aşağıdaki [Docker görüntüsünü](https://registry.hub.docker.com/u/microsoft/azure-cli/) kullanarak Azure CLI’yi bir Docker kapsayıcısı olarak da çalıştırabilirsiniz. Docker ana bilgisayarından aşağıdaki komutu çalıştırın:

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a>Azure hesabınızı ve aboneliğinizi ayarlama
Henüz bir Azure aboneliğiniz olmamasına rağmen bir MSDN aboneliğiniz varsa [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilirsiniz. Veya [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

`azure login` yazarak ve Azure hesabınızda etkileşimli oturum açma deneyimine yönelik istemleri izleyerek [Azure hesabınızda etkileşimli bir oturum açın](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). 

> [!NOTE]
> İş veya okul kimliğiniz varsa ve iki faktörlü kimlik doğrulamasının etkin olmadığını biliyorsanız, iş veya okul kimlik numaranıza **ek olarak** `azure login -u` kullanabilir ve etkileşimli bir oturum *olmadan* oturum açabilirsiniz. İş veya okul kimliğiniz yoksa, aynı şekilde oturum açmak için [kişisel Microsoft hesabınızdan iş veya okul kimliği oluşturabilirsiniz](../articles/virtual-machines/virtual-machines-windows-create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Hesabınızın birden fazla aboneliği olabilir. `azure account list` yazarak aboneliklerinizin aşağıdakine benzer bir listesini görüntüleyebilirsiniz:

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

Aşağıdaki komutu yazarak geçerli Azure aboneliğinizi ayarlayabilirsiniz. Yönetmek istediğiniz kaynaklara sahip abonelik adını veya kimliği kullanın.

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-to-the-azure-cli-resource-group-mode"></a>Azure CLI kaynak grubu moduna geçme
Varsayılan olarak, Azure CLI hizmet yönetimi modunda başlatılır (**asm** modu). Kaynak grubu moduna geçmek için aşağıdaki komutu yazın.

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Azure kaynak şablonlarını ve kaynak gruplarını anlama
Çoğu uygulama farklı kaynak türlerinin bir birleşiminden oluşturulur (bir veya daha fazla VM ve depolama hesabı, bir SQL veritabanı, bir sanal ağ ya da içerik teslim ağı gibi). Varsayılan Azure hizmet yönetim API’si ve klasik Azure portalı, hizmete göre yaklaşım kullanarak bu öğeleri temsil eder. Bu yaklaşım, belirli hizmetleri tek bir dağıtım mantıksal birim olarak değil ayrı ayrı dağıtıp yönetmenizi (veya bunu yapan başka araçlar bulmanızı) gerektirir.

Ancak *Azure Resource Manager şablonları*, bu farklı kaynakları bildirim temelli olarak tek bir mantıksal dağıtım birimi halinde dağıtıp yönetmenizi mümkün hale getirir. Her komuttan sonra Azure’a neyi dağıtacağını zorunlu olarak belirtmek yerine, tüm dağıtımınızı (tüm kaynaklar ve ilişkili yapılandırma ile dağıtım parametreleri) bir JSON dosyasında açıklar ve Azure’a bu kaynakları tek grup haline dağıtmasını söylersiniz.

Bundan sonra Azure CLI kaynak yönetimi komutlarını kullanarak grup kaynaklarının genel yaşam döngüsünü aşağıdaki işlemlerle yönetebilirsiniz:

* Gruptaki tüm kaynakları tek seferde durdurma, başlatma veya silme.
* Kaynaklar üzerindeki izinleri kilitlemek için Rol Tabanlı Access Control (RBAC) kuralları uygulama.
* İşlemleri denetleme.
* Daha iyi izleme için ek meta verilerle kaynakları etiketleme.

Azure kaynak grupları ve size sunabilecekleri hakkında daha fazla bilgiyi [Azure Resource Manager’a genel bakış](../articles/azure-resource-manager/resource-group-overview.md) bölümünde bulabilirsiniz. Şablon yazmak istiyorsanız bkz. [Azure Resource Manager şablonları yazma](../articles/resource-group-authoring-templates.md).

## <a name="a-idquick-create-a-vm-in-azureatask-quick-create-a-vm-in-azure"></a><a id="quick-create-a-vm-in-azure"></a>Görev: Azure’da hızlı bir şekilde VM oluşturma
Bazı durumlarda gerekli olan görüntüyü bilirsiniz ve bu görüntüden hemen bir VM oluşturmanız gerekirken altyapıyı fazla önemsemezsiniz; belki de temiz bir VM üzerinde bir test yapmanız gerekiyordur. Bu durumda `azure vm quick-create` komutunu kullanmanız ve bir VM ile altyapısını oluşturmak için gerekli bağımsız değişkenleri geçirmeniz gerekir.

İlk olarak kaynak grubunuzu oluşturun.

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

İkinci olarak, bir görüntü gereklidir. Azure CLI ile bir görüntü bulmak için bkz. [PowerShell ve Azure CLI ile Azure sanal makine görüntülerine gitme ve görüntüyü seçme](../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ancak bu makalede popüler görüntülerin kısa bir listesi verilmiştir. Bu hızlı oluşturma işlemi için CoreOS Kararlı görüntüsü kullanılmaktadır.

> [!NOTE]
> ComputeImageVersion için ayrıca hem şablon dilinde hem de Azure CLI’da yalnızca 'latest' parametresini belirtebilirsiniz. Bunun yapılması, betikleri veya şablonları değiştirmek zorunda olmadan görüntünün her zaman en son ve düzeltme eki uygulanmış sürümünü kullanmanıza olanak tanır. Bu örnek aşağıda gösterilmiştir.
>
>

| PublisherName | Sunduğu | Sku | Sürüm |
|:--- |:--- |:--- |:--- |
| OpenLogic |CentOS |7 |7.0.201503 |
| OpenLogic |CentOS |7.1 |7.1.201504 |
| CoreOS |CoreOS |Beta |647.0.0 |
| CoreOS |CoreOS |Dengeli |633.1.0 |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |8.0.40459 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Standart |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Enterprise |1.0.0 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |12.0.2430 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |12.0.2430 |
| Canonical |UbuntuServer |12.04.5-LTS |12.04.201504230 |
| Canonical |UbuntuServer |14.04.2-LTS |14.04.201503090 |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |3.0.201503 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |4.0.201503 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |5.0.201504 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |1.0.141204 |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |4.3.4665 |

Yalnızca `azure vm quick-create` komutunu girip istemlere hazır olarak VM’nizi oluşturun. Şuna benzer şekilde görünecektir:

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up the VM "coreos"
info:    Using the VM Size "Standard_A1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in the region "westus", trying to create new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up the storage account cli9fd3fce49e9a9b3d14302
+ Looking up the NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up the virtual network "coreo-westu-1430261891570-vnet"
+ Looking up the subnet "coreo-westu-1430261891570-snet" under the virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up the VM "coreos"
+ Looking up the NIC "coreo-westu-1430261891570-nic"
+ Looking up the public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

Yeni VM’niz hazırdır.

## <a name="a-iddeploy-a-vm-in-azure-from-a-templateatask-deploy-a-vm-in-azure-from-a-template"></a><a id="deploy-a-vm-in-azure-from-a-template"></a>Görev: Azure’da şablondan VM dağıtma
Azure CLI ile bir şablon kullanarak yeni bir Azure VM dağıtmak için bu bölümlerdeki yönergeleri kullanın. Bu şablon tek bir alt ağa sahip yeni bir sanal ağda tek bir sanal makine oluşturur ve `azure vm quick-create` seçeneğinin aksine, tam olarak ne istediğinizi açıklamanıza ve hata olmadan tekrarlamanıza olanak tanır. Bu şablon aşağıdaki gibidir:

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-the-json-file-for-the-template-parameters"></a>Adım 1: Şablon parametreleri için JSON dosyasını inceleme
Şablon için JSON dosyasının içeriği aşağıda verilmiştir. (Şablon [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json) içinde de bulunur.)

Şablonlar esnektir, bu nedenle tasarımcı size çok sayıda parametre sunmayı veya daha sabit bir şablon oluşturarak az sayıda parametre sunmayı seçmiş olabilir. Bilgileri toplamak için şablonu parametre olarak geçirmeniz, şablon dosyasını açmanız (bu konunun sonraki kısımlarında bir şablon satır içi mevcuttur) ve **parameters** değerlerini incelemeniz gerekir.

Bu durumda aşağıdaki şablon şunları ister:

* Benzersiz bir depolama hesabı adı.
* VM için bir yönetici kullanıcı adı.
* Parola.
* Dış dünyada kullanmak bir etki alanı adı.
* Ubuntu Server sürüm numarası -- ancak yalnızca bir listesini kabul eder.

[Kullanıcı adı ve parola gereksinimleri](../articles/virtual-machines/virtual-machines-linux-faq.md#what-are-the-username-requirements-when-creating-a-vm) hakkında daha fazla bilgi edinin.

Bu değerlere karar verdikten sonra bir grup oluşturmaya ve bu şablonu Azure aboneliğinize dağıtmaya hazır olursunuz.

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the storage account where the virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for the virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for the virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for the public IP used to access the virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-the-virtual-machine-by-using-the-template"></a>Adım 2: Şablon kullanarak sanal makine oluşturma
Parametre değerleriniz hazır olduktan sonra şablon dağıtımınıza yönelik bir kaynak grubu oluşturmanız ve ardından şablonu dağıtmanız gerekir.

Kaynak grubu oluşturmak için istediğiniz grubun adı ve dağıtımı yapmak istediğiniz veri merkezinin konumu ile birlikte `azure group create <group name> <location>` yazın. Bu işlem hızlı bir şekilde yapılır:

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Şimdi dağıtımı oluşturmak için `azure group deployment create` öğesini çağırıp şu komutu geçirin:

* Şablon dosyası (yukarıdaki JSON şablonunu bir yerel dosyaya kaydettiyseniz).
* Bir şablon URI’si (GitHub veya diğer bazı web adreslerinde dosyayı işaret etmek istiyorsanız).
* Dağıtımı yapmak istediğiniz kaynak grubu.
* İsteğe bağlı dağıtım adı.

JSON dosyasının "parameters" bölümünde parametrelerin değerlerini belirtmeniz istenir. Tüm parametre değerlerini belirttiğinizde dağıtımınız başlar.

Örnek aşağıda verilmiştir:

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

Aşağıdaki türde bilgiler alırsınız:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <a name="a-idcreate-a-custom-vm-imageatask-create-a-custom-vm-image"></a><a id="create-a-custom-vm-image"></a>Görev: Özel bir VM görüntüsü oluşturma
Şablonların temel kullanımını yukarıda gördünüz; bu nedenle, Azure CLI üzerinden bir şablon kullanarak Azure’daki belirli bir .vhd dosyasından özel bir VM oluşturmak için benzer yönergeler kullanılabilir. Buradaki farklılık, bu şablonun belirli bir sanal sabit diskten (VHD) tek bir sanal makine oluşturmasıdır.

### <a name="step-1-examine-the-json-file-for-the-template"></a>Adım 1: Şablon için JSON dosyasını inceleme
Bu bölümde örnek olarak kullanılan şablon için JSON dosyasının içeriği aşağıda verilmiştir. (Şablon [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json) içinde de bulunur.)

Burada da, varsayılan değerlere sahip olmayan parametreler için girmek istediğiniz değerleri bulmanız gerekir. `azure group deployment create` komutunu çalıştırdığınızda Azure CLI bu değerleri girmenizi ister.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-the-vhd"></a>Adım 2: VHD alma
Bunun için bir .vhd dosyasının gerekli olduğu açıktır. Azure’da zaten sahip olduğunuz dosyayı kullanabilir veya bir tane yükleyebilirsiniz.

Windows tabanlı bir sanal makine için bkz. [Windows Server VHD’si oluşturup Azure’a yükleme](../articles/virtual-machines/virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Linux tabanlı sanal makine için bkz. [Linux işletim sistemi içeren bir sanal sabit disk oluşturma ve karşıya yükleme](../articles/virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="step-3-create-the-virtual-machine-by-using-the-template"></a>Adım 3: Şablon kullanarak sanal makine oluşturma
Şu anda .vhd’yi temel alan yeni bir sanal makine oluşturmaya hazırsınız. `azure group create <location>` komutunu kullanarak dağıtımın yapılacağı grubu oluşturun:

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Ardından `--template-uri` seçeneğini kullanarak şablonu doğrudan çağırmak için dağıtımı oluşturun (veya `--template-file` seçeneğini kullanarak yerel olarak kaydettiğiniz bir dosyayı kullanabilirsiniz). Şablonun varsayılan değerleri belirtildiği için sizden yalnızca birkaç şey istenir. Şablonu farklı yerlere dağıtırsanız, varsayılan değerlerle bazı adlandırma çakışmalarının oluştuğunu görebilirsiniz (özellikle oluşturduğunuz DNS adı).

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Çıktı aşağıdakine benzer olacaktır:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment to complete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <a name="a-iddeploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balanceratask-deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Görev: Sanal ağ ve dış yük dengeleyici kullanan çok sanal makineli uygulama dağıtma
Bu şablon bir yük dengeleyici altında iki sanal makine oluşturmanıza ve Bağlantı Noktası 80 üzerinde bir yük dengeleme kuralı yapılandırmanıza olanak tanır. Bu şablon ayrıca bir depolama hesabı, sanal ağ, genel IP adresi, kullanılabilirlik kümesi ve ağ arabirimleri dağıtır.

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

GitHub şablon deposunda Azure PowerShell komutları aracılığıyla Resource Manager şablonu kullanarak, sanal ağ ve yük dengeleyicisi kullanan çok sanal makineli bir uygulama dağıtmak için bu adımları izleyin.

### <a name="step-1-examine-the-json-file-for-the-template"></a>Adım 1: Şablon için JSON dosyasını inceleme
Şablon için JSON dosyasının içeriği aşağıda verilmiştir. En son sürümü istiyorsanız [şablonlar için Github deposunda bulabilirsiniz](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json). Bu konu başlığında şablonu çağırmak için `--template-uri` anahtarı kullanılır, ancak yerel bir sürüm geçirmek için `--template-file` anahtarını da kullanabilirsiniz.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix to use for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of the VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-the-deployment-by-using-the-template"></a>Adım 2: Şablon kullanarak dağıtım oluşturma
`azure group create <location>` kullanarak şablon için bir kaynak grubu oluşturun. Ardından, `azure group deployment create` komutunu kullanıp kaynak grubunu geçirerek, bir dağıtım adı geçirerek ve varsayılan değerlere sahip olmayan şablondaki parametrelere yönelik istemleri yanıtlayarak bu kaynak grubunda bir dağıtım oluşturun.

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Şimdi `azure group deployment create` komutu ile `--template-uri` seçeneğini kullanarak şablonu dağıtın. Aşağıda gösterildiği gibi sizden istediğinde parametre değerlerini belirtmeye hazır olun.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for the following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment to complete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

Bu şablon bir Windows Server görüntüsünü dağıtır; ancak herhangi bir Linux görüntüsü ile kolayca değiştirilebilir. Birden fazla swarm yöneticisine sahip bir Docker kümesi mi oluşturmak istiyorsunuz? [Bunu yapabilirsiniz](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

## <a name="a-idremove-a-resource-groupatask-remove-a-resource-group"></a><a id="remove-a-resource-group"></a>Görev: Kaynak grubunu kaldırma
Bir kaynak grubuna yeniden dağıtım yapabilirsiniz, ancak bir kaynak grubuyla işiniz bittiyse `azure group delete <group name>` komutunu kullanarak kaynak grubunu silebilirsiniz.

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <a name="a-idshow-the-log-for-a-resource-group-deploymentatask-show-the-log-for-a-resource-group-deployment"></a><a id="show-the-log-for-a-resource-group-deployment"></a>Görev: Bir kaynak grubu dağıtımının günlüğünü görüntüleme
Şablon oluştururken veya kullanırken bu işlem yaygın olarak yapılır. Bir grubun dağıtım günlüklerini görüntüleme çağrısı, bir olayın neden gerçekleştiğini (veya gerçekleşmediğini) anlamak için yararlı olabilecek bilgileri `azure group log show <groupname>` komutudur. (Dağıtımlarınızla ilgili daha fazla sorun giderme bilgisi ve sorunlar hakkında diğer bilgiler için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)

Örneğin, belirli hataları hedeflemek için **jq** gibi araçlar kullanarak, düzeltmeniz gereken belirli hatalar gibi sorunları daha kesin bir şekilde sorgulayabilirsiniz. Aşağıdaki örnekte hatalar aranırken **lbgroup** dağıtım günlüğünü ayrıştırmak için **jq** kullanılmaktadır.

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
Neyin yanlış gittiğini hızlıca bulabilir, düzeltebilir ve yeniden deneyebilirsiniz. Aşağıdaki örnekte, şablon aynı anda iki VM oluşturmuş ve .vhd üzerinde bir kilit oluşturmuştur. (Şablon değiştirildikten sonra dağıtım hızlıca başarılı olmuştur.)

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed to acquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <a name="a-iddisplay-information-about-a-virtual-machineatask-display-information-about-a-virtual-machine"></a><a id="display-information-about-a-virtual-machine"></a>Görev: Bir sanal makine hakkında bilgi görüntüleme
`azure vm show <groupname> <vmname>` komutunu kullanarak kaynak grubunuzdaki belirli VM’ler hakkındaki bilgileri görebilirsiniz. Grubunuzda birden fazla VM varsa, ilk olarak `azure vm list <groupname>` ile gruptaki VM’leri listelemeniz gerekebilir.

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

Ardından, myVM1 öğesini arayın:

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up the VM "myVM1"
+ Looking up the NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> Konsol komutlarınızın çıktısını programlı olarak depolamak ve yönlendirmek istiyorsanız **[jq](https://github.com/stedolan/jq)** veya **[jsawk](https://github.com/micha/jsawk)** gibi bir JSON ayrıştırma aracı ya da görev için yararlı dil kitaplıkları kullanmak isteyebilirsiniz.
>
>

## <a name="a-idlog-on-to-a-linux-based-virtual-machineatask-log-on-to-a-linux-based-virtual-machine"></a><a id="log-on-to-a-linux-based-virtual-machine"></a>Görev: Linux tabanlı sanal makinede oturum açma
Linux makinelerine genellikle SSH aracılığıyla bağlanılır. Daha fazla bilgi için bkz. [Azure’da Linux ile SSH kullanma](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="a-idstop-a-virtual-machineatask-stop-a-vm"></a><a id="stop-a-virtual-machine"></a>Görev: VM durdurma
Şu komutu çalıştırın:

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> Bir sanal ağ içindeki son VM olması durumunda sanal ağın sanal IP’sini (VIP) saklamak için bu parametreyi kullanın. <br><br> `StayProvisioned` parametresini kullansanız bile VM için fatura almaya devam edersiniz.
>
>

## <a name="a-idstart-a-virtual-machineatask-start-a-vm"></a><a id="start-a-virtual-machine"></a>Görev: VM başlatma
Şu komutu çalıştırın:

```azurecli
azure vm start <group name> <virtual machine name>
```

## <a name="a-idattach-a-data-diskatask-attach-a-data-disk"></a><a id="attach-a-data-disk"></a>Görev: Veri diski ekleme
Ayrıca yeni bir disk veya veri içeren bir disk eklemeye karar vermeniz gerekir. Yeni bir disk için komut .vhd dosyasını oluşturup aynı komuta ekler.

Yeni bir disk eklemek için şu komutu çalıştırın:

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

Var olan bir veri diski eklemek için şu komutu çalıştırın:

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

Ardından Linux’ta yaptığınız gibi diski bağlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
**Arm** modu ile Azure CLI kullanımı hakkında çok daha fazla örnek için bkz. [Azure Resource Manager ile Mac, Linux ve Windows için Azure CLI kullanma](../articles/xplat-cli-azure-resource-manager.md). Azure kaynakları ile kavramları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/resource-group-overview.md).

Kullanabileceğiniz diğer şablonlar için bkz. [Azure Hızlı Başlangıç şablonları](https://azure.microsoft.com/documentation/templates/) ve [Şablon kullanan uygulama çerçeveleri](../articles/virtual-machines/virtual-machines-linux-app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


<!--HONumber=Jan17_HO4-->


