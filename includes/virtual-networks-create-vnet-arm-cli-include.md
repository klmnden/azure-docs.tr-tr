## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Azure CLI kullanarak VNet oluşturma
Windows, Linux veya OSX çalıştıran herhangi bir bilgisayarın komut isteminden Azure kaynaklarınızı yönetmek için Azure CLI’yi kullanabilirsiniz. Azure CLI’yi kullanarak VNet oluşturmak için aşağıdaki adımları uygulayın.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../articles/xplat-cli-install.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Resource Manager moduna geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.
   
        azure config mode arm
   
    Yukarıdaki komut için beklenen çıkış buradaki gibidir:
   
        info:    New mode is arm
3. Gerekirse, yeni bir kaynak grubu oluşturmak için **azure group create** komutunu aşağıda gösterildiği gibi çalıştırın. Komutun çıktısına dikkat edin. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) sayfasını ziyaret edin.
   
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
4. Aşağıda gösterildiği gibi, VNet ve alt ağ oluşturmak için **azure network vnet create** komutunu çalıştırın. 
   
        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus
   
    Yukarıdaki komut için beklenen çıktı şu şekildedir:
   
        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK
   
   * **-g (veya --resource-group)**. VNet kaynak grubunun oluşturulacağı kaynak grubunun adı. Bizim senaryomuz için bu *TestRG* ’dir.
   * **-n (veya --name)**. Oluşturulacak VNet’in adı. Bizim senaryomuz için bu *TestVNet* ’tir.
   * **-a (veya--adres-önekler)**. VNet adres alanı için kullanılan CIDR blokları listesi. Bizim senaryomuz için bu *192.168.0.0/16* ’dır.
   * **-l (veya --location)**. VNet’in oluşturulacağı Azure bölgesi. Bizim senaryomuz için bu *centralus* ’tur.
5. Aşağıda gösterildiği gibi, alt ağ oluşturmak için **azure network vnet subnet create** komutunu çalıştırın. Komutun çıktısına dikkat edin. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır.
   
        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
   
    Yukarıdaki komut için beklenen çıktı şu şekildedir:
   
        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK
   
   * **-e (veya--vnet-ad**. Alt ağın oluşturulacağı VNet’in adı. Bizim senaryomuz için bu *TestVNet* ’tir.
   * **-n (veya --name)**. Yeni alt ağın adı. Bizim senaryomuz için bu *FrontEnd* ’dir.
   * **-a (veya--adres-önek)**. Alt ağ CIDR bloğu. Bizim senaryomuz için bu *192.168.1.0/24* ’tür.
6. Gerekiyorsa, başka alt ağlar oluşturmak için yukarıdaki 5. adımı yineleyin. Senaryomuz için, *BackEnd* alt ağını oluşturacak aşağıdaki komutu çalıştırın.
   
        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24
7. Aşağıda gösterildiği gibi, yeni vnet’in özelliklerini görüntülemek için **azure network vnet show** komutunu çalıştırın.
   
        azure network vnet show -g TestRG -n TestVNet
   
    Yukarıdaki komut için beklenen çıktı şu şekildedir:
   
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



<!--HONumber=Nov16_HO2-->


