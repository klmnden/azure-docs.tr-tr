## Azure CLI kullanarak ARM şablonu dağıtma
Azure CLI kullanarak yüklediğiniz ARM şablonunu dağıtmak için aşağıdaki adımları izleyin.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../articles/xplat-cli-install.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Resource Manager moduna geçmek için **`azure config mode`** komutunu aşağıda gösterildiği gibi çalıştırın.
   
        azure config mode arm
   
    Yukarıdaki komut için beklenen çıkış buradaki gibidir:
   
        info:    New mode is arm
3. Gerekirse, yeni bir kaynak grubu oluşturmak için **`azure group create`** komutunu aşağıda gösterildiği gibi çalıştırın. Komutun çıktısına dikkat edin. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/resource-group-overview.md) sayfasını ziyaret edin.
   
        azure group create -n TestRG -l centralus
   
    Yukarıdaki komut için beklenen çıktı şu şekildedir:
   
        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
   
   * **-n (veya--ad)**. Yeni kaynak grubunun adı. Bizim senaryomuz için bu *TestRG* ’dir.
   * **-l (veya --konum)**. Yeni kaynak grubunun oluşturulacağı Azure bölgesi. Bizim senaryomuz için bu *centralus* ’tur.
4. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni VNet’i dağıtmak için **`azure group deployment create`** cmdlet’ini çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.
   
        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json
   
    Yukarıdaki komut için beklenen çıktı şu şekildedir:
   
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK
   
   * **-g (veya--kaynak-grubu)**. Yeni VNet kaynak grubunun oluşturulduğu kaynak grubunun adı.
   * **-f (veya --şablon-dosyası)**. ARM şablon dosyanızın yolu.
   * **-e (veya--parametreler-dosyası)**. ARM parametreleri dosyanızın yolu.
5. Aşağıda gösterildiği gibi, yeni vnet’in özelliklerini görüntülemek için **`azure network vnet show`** komutunu çalıştırın.
   
        azure network vnet show -g TestRG -n TestVNet
   
    Yukarıdaki komut için beklenen çıkış buradaki gibidir:
   
        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK

<!--HONumber=Sep16_HO3-->


