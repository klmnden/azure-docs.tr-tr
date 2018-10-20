---
title: Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma | Microsoft Docs
description: Depolama hesabınız için katmanlı ağ güvenliğini yapılandırın.
services: storage
author: cbrooksmsft
ms.service: storage
ms.topic: article
ms.date: 10/25/2017
ms.author: cbrooks
ms.component: common
ms.openlocfilehash: 7c01940c41067029bc3d47d19c2ded1d710cc2c6
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470073"
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks"></a>Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma
Azure depolama, belirli bir ağa izin kümesi, depolama hesaplarınıza güvenli olanak tanıyan bir katmanlı güvenlik modeli sağlar.  Ağ kuralları yapılandırıldığında, yalnızca izin verilen ağları uygulamalardan bir depolama hesabına erişebilir.  İzin verilen bir ağdan çağırırken uygulamalar (geçerli bir erişim anahtarı veya SAS belirteci) depolama hesabına erişmek için uygun yetkilendirme gerektirecek şekilde devam edin.

> [!IMPORTANT]
> Depolama hesabınız için güvenlik duvarı kurallarını etkinleştirmek, gelen istekleri için diğer Azure Hizmetleri dahil olmak üzere, veri erişimi engeller.  Bu günlükler, vb. yazmak, portalı kullanarak içerir.  Gelen bir Vnet'te çalışan azure Hizmetleri, alt ağ hizmeti örneğinin vererek erişim verilebilir.  Bir sanal ağ içinde gelen çalıştırmayın azure Hizmetleri Güvenlik Duvarı tarafından engellenir.  Sınırlı sayıda senaryo aracılığıyla etkinleştirilebilir [özel durumları](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) aşağıda açıklanan mekanizması.  Portala erişmek için bir makine ayarlamış olduğunuz güvenilen sınır (IP veya VNet) içinde bunu gerekecektir.
>

## <a name="scenarios"></a>Senaryolar
Depolama hesapları varsayılan olarak (internet trafiğini dahil) tüm ağlardan trafiği erişimini engellemek için yapılandırılabilir.  Uygulamalarınız için bir güvenli ağ sınırı oluşturmanıza izin vererek belirli Azure sanal ağlarından trafiği erişim verilebilir.  Erişim de genel internet'e verilecek IP adresi aralıkları, belirli bir internet veya şirket içi istemcilerinden gelen bağlantıları etkinleştirme.

Azure depolama REST ve SMB dahil olmak üzere, tüm ağ protokolleri üzerinde ağ kuralları uygulanır.  Azure portalında, Depolama Gezgini gibi araçlar verilerinizi erişim ve AZCopy açık ağ kuralları ağ kuralları yürürlükte olduğunda erişim gerektirir.

Ağ kurallarının var olan depolama hesapları için uygulanabilir veya yeni depolama hesabı oluşturma sırasında uygulanabilir.

Bir kez ağ kuralları uygulandığında, tüm istekler için zorunlu tutulmaz.  SAS belirteçleri, belirli bir IP adresi hizmete erişim sunmak için **sınırı** erişim belirteci sahibi olmayan yeni erişim verme yapılandırılmış ağ kuralları ötesinde ancak yapın. 

Sanal makine diski trafiği (bağlama dahil olmak üzere operations çıkarın ve disk g/ç) **değil** ağ kuralları tarafından etkilenen.  REST erişimini sayfa BLOB'ları, ağ kuralları tarafından korunur.

Klasik depolama hesapları **olmayan** güvenlik duvarları ve sanal ağlar'ı destekler.

Yedekleme ve geri yükleme, yönetilmeyen diskler depolama hesaplarında uygulanan ağ kurallarıyla kullanarak sanal makineleri desteklenir açıklandığı gibi bir özel durum oluşturma aracılığıyla [özel durumları](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) bu makalenin.  Güvenlik Duvarı özel durumları zaten Azure tarafından yönetildikleri yönetilen diskler ile ilgili değildir.

## <a name="change-the-default-network-access-rule"></a>Varsayılan ağ erişim kuralını değiştirme
Varsayılan olarak, depolama hesapları herhangi bir ağ üzerinde istemci bağlantılarını kabul edin.  Seçili ağlar erişimi sınırlamak için önce varsayılan eylem değiştirmeniz gerekir.

> [!WARNING]
> Ağ kuralları için değişiklik yapmadan uygulamalarınızın Azure depolamaya bağlanma yeteneğini etkileyebilir.  Varsayılan ağ kuralı ayarını **Reddet** sürece verilere erişim engellenir tüm belirli ağ kuralları *verme* erişim de uygulanır.  İzin verilen erişimi engellemek için varsayılan Kuralı değiştirmeden önce ağ kurallarını kullanarak herhangi bir ağa erişim vermek emin olun.
>

#### <a name="azure-portal"></a>Azure portal
1. Korumak istediğiniz depolama hesabına gidin.  

2. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.
3. Varsayılan olarak erişimi engellemek için 'Seçili ağlar' dan erişime izin vermek seçin.  Tüm ağlar gelen trafiğe izin vermek için 'Tüm ağlar' dan erişime izin vermek seçin.
4. Tıklayın *Kaydet* yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell
1. Son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).

2. Depolama hesabı için varsayılan kuralın durumunu görüntüler.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet  -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
``` 

3. Varsayılan olarak ağ erişimini engellemek için varsayılan kuralı ayarlayın.  
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
```    

4. Varsayılan olarak ağ erişim izni vermek için varsayılan kuralı ayarlayın.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
```    

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Depolama hesabı için varsayılan kuralın durumunu görüntüler.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
```

3. Varsayılan olarak ağ erişimini engellemek için varsayılan kuralı ayarlayın.  
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Deny
```

4. Varsayılan olarak ağ erişim izni vermek için varsayılan kuralı ayarlayın.
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Allow
```

## <a name="grant-access-from-a-virtual-network"></a>Bir sanal ağdan erişim izni ver
Depolama hesapları, yalnızca belirli Azure sanal ağlardan erişime izin verecek şekilde yapılandırılabilir. 

Sağlayarak bir [hizmet uç noktası](/azure/virtual-network/virtual-network-service-endpoints-overview) sanal ağ içinde Azure depolama için Azure depolama hizmeti için bir en uygun rotayı trafiği güvence altına. Sanal ağ ve alt ağ kimlikleri, ayrıca her bir istekle aktarılır.  Yöneticiler, daha sonra depolama hesabı için sanal ağdaki belirli alt ağlara alınan isteklerine izin ver ağ kuralları yapılandırabilirsiniz.  İstemcilerin bu ağ kuralları üzerinden erişim verilere erişmek için depolama hesabı yetkilendirme gereksinimlerini karşılamak devam etmelidir verildi.

Her Depolama hesabı ile birleştirilebilir 100'e kadar sanal ağ kuralları destekleyebilir [IP ağ kuralları](#grant-access-from-an-internet-ip-range).

### <a name="available-virtual-network-regions"></a>Kullanılabilir sanal ağ bölgeleri
Genel olarak, hizmet uç noktaları sanal ağları ve aynı Azure bölgesinde hizmet örnekleri arasında çalışır.  Hizmet uç noktaları Azure depolama ile kullanıldığında, bu kapsama dahil etmek için genişletilir [eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions).  Bu salt okunur coğrafi olarak yedekli depolama (RA-GRS) örnekleri sorunsuz erişim yanı sıra bir bölgesel yük devretme sırasında sürekliliği sağlar.  Sanal ağdan bir depolama hesabına erişim izni ağ kuralları da herhangi bir RA-GRS örneğine erişim.

Bölgesel bir kesinti sırasında olağanüstü durum kurtarma için planlama yaparken, önceden eşleştirilmiş bölge içindeki sanal ağları sağlamanız gerekir. Azure depolama için hizmet uç noktalarının etkinleştirilmesi gerekir ve bu diğer sanal ağlardan erişim verme ağ kuralları, coğrafi olarak yedekli depolama hesaplarınıza uygulanmalıdır.

> [!NOTE]
> Hizmet uç noktaları sanal ağı ve belirtilen bölge çiftine bölgesi dışında trafiği için geçerli değildir.  Depolama hesapları için sanal ağdan erişim verme ağ kuralları yalnızca birincil bölgede bir depolama hesabı veya belirlenen eşleştirilmiş bölge içindeki sanal ağlar için de uygulanabilir.
>

### <a name="required-permissions"></a>Gerekli izinler
Bir depolama hesabı için bir sanal ağ kuralı uygulamak için kullanıcının iznine sahip *bir alt ağa Hizmeti'ne Katıl* eklenen alt ağlarda için.  Bu izne eklenir *depolama hesabı Katılımcısı* yerleşik rolü ve özel rol tanımları için eklenebilir.

Depolama hesabı ve sanal ağlar, erişim izni **olabilir** farklı Aboneliklerde olabilir, ancak bu abonelikler aynı Azure Active Directory kiracısı bir parçası olmalıdır.

### <a name="managing-virtual-network-rules"></a>Sanal ağ kurallarını yönetme
Depolama hesapları için sanal ağ kuralları, Azure portalı üzerinden, PowerShell veya CLIv2 yönetilebilir.

#### <a name="azure-portal"></a>Azure portal
1. Korumak istediğiniz depolama hesabına gidin.  
2. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.
3. 'Seçili ağlar' dan erişime izin vermek seçtiniz emin olun.
4. "Sanal ağlar" altında yeni bir ağ kuralı ile bir sanal ağa erişimi vermek için "var olanı Ekle"'ye tıklayın. var olan bir sanal ağı ve alt ağları seçmek için ardından *Ekle*.  Yeni bir sanal ağ oluşturma ve erişim vermek için *yeni Ekle*, yeni bir sanal ağ oluşturmak ve ardından için gereken bilgileri sağlayan *Oluştur*.

> [!NOTE]
> Seçili sanal ağ ve alt ağlar için Azure depolama için hizmet uç noktası önceden yapılandırılmamış, bu işlemin bir parçası yapılandırılabilir.
>

5. Bir sanal ağ veya alt ağ kuralı kaldırmak için tıklayın sanal ağ veya alt ağ için bağlam menüsünü açmak için "..." ve "Kaldır" a tıklayın.
6. Tıklayın *Kaydet* yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell
1. Son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).
2. Sanal ağ kuralları listesi
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
```

3. Var olan bir sanal ağ ve alt ağ üzerinde Azure depolama için hizmet uç noktasını girin
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"  -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzureRmVirtualNetwork
```

4. Bir sanal ağ ve alt ağ için ağ kuralı ekleyin.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

5. Bir sanal ağ ve alt ağ için ağ kuralını kaldırın.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Remove-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için Deny veya ağ kuralları, herhangi bir etkisi olacaktır.
>

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Sanal ağ kuralları listesi
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
```

2. Var olan bir sanal ağ ve alt ağ üzerinde Azure depolama için hizmet uç noktasını girin
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
```

3. Bir sanal ağ ve alt ağ için ağ kuralı ekleyin.  
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

4. Bir sanal ağ ve alt ağ için ağ kuralını kaldırın. 
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için Deny veya ağ kuralları, herhangi bir etkisi olacaktır.
>

## <a name="grant-access-from-an-internet-ip-range"></a>İnternet'ten erişim ver IP aralığı
Depolama hesapları, belirli genel internet'ten erişime izin verecek şekilde yapılandırılabilir IP adresi aralıkları.  Bu yapılandırma belirli internet tabanlı hizmetler sağlar ve şirket içi ağlara genel Internet trafiğini bloke sırada erişim izni verilecek.

Internet adresi aralıkları kullanarak sağlanabilir izin [CIDR gösteriminde](https://tools.ietf.org/html/rfc4632) biçiminde *16.17.18.0/24* tek tek IP adresleri gibi veya *16.17.18.19* .

> [!NOTE]
> Küçük adres aralıkları kullanarak "/ 31" veya "/ 32" öneki boyutları desteklenmez.  Bu aralıklar, tek tek IP adresi kurallarını kullanarak yapılandırılmalıdır.
>

IP ağ kuralları için yalnızca verilir **genel internet** IP adresleri.  IP adres aralıkları özel ağlar için ayrılmış (sınıfında tanımlandığı gibi [RFC 1918](https://tools.ietf.org/html/rfc1918#section-3)) IP kurallarında izin verilmez.  Özel ağlar ile başlayan bir adres dahil *10.\** , *172.16.\**   -  *172.31.\**, ve *192.168.\** .

> [!NOTE]
> IP ağ kuralları için aynı Azure bölgesindeki depolama hesabı kaynaklanan taleplere üzerinde hiçbir etkisi olmaz.  Kullanım [sanal ağ kuralları](#grant-access-from-a-virtual-network) aynı bölgede isteklere izin vermek için.
>

Şu anda yalnızca IPv4 adresleri desteklenir.

Her Depolama hesabı ile birleştirilebilir 100'e kadar IP ağ kurallarını destekleyebilir [sanal ağ kuralları](#grant-access-from-a-virtual-network).

### <a name="configuring-access-from-on-premises-networks"></a>Şirket içi ağlardan erişimi yapılandırma
Depolama hesabınıza bir IP ağ kuralı ile şirket içi ağlarınızı erişim vermek için internet'e yönelik ağınız tarafından kullanılan IP adreslerini tanımlamanız gerekir.  Yardım için ağ yöneticinize başvurun.

Azure ağı kullanarak ağınıza bağlı olup olmadığını [ExpressRoute](/azure/expressroute/expressroute-introduction), her bağlantı hattı Microsoft kullanarakServices'eAzuredepolamagibibağlanmakiçinkullanılanikiortakIPadresi,MicrosoftEdgeileyapılandırılmış[Azure genel eşdüzey hizmet sağlama](/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains).  Azure depolama bağlantı hattınızın gelen iletişimlere izin vermesi için genel IP adresleri, bağlantı hatları için IP ağ kuralları oluşturmanız gerekir.  ExpressRoute bağlantı hattı'nın genel IP adreslerinizi bulmak için [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portal aracılığıyla.


### <a name="managing-ip-network-rules"></a>IP ağ kurallarını yönetme
Depolama hesapları için IP ağ kuralları, Azure portal, PowerShell veya CLIv2 yönetilebilir.

#### <a name="azure-portal"></a>Azure portal
1. Korumak istediğiniz depolama hesabına gidin.  
2. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.
3. 'Seçili ağlar' dan erişime izin vermek seçtiniz emin olun.
4. Vermek için internet'e bir IP aralığı erişmek için IP adresi veya adres aralığı (CIDR biçiminde) altında güvenlik duvarı, adres aralığı girin.
5. Bir IP ağ kuralı kaldırmak için ağ kuralı yanındaki çöp kutusu simgesine tıklayın.
6. Tıklayın *Kaydet* yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell
1. Son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).
2. IP ağ kuralları listesi.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
```

3. Tek bir IP adresi için bir ağ kuralı ekleyin.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19" 
``` 

4. Bir IP adresi aralığı için bir ağ kuralı ekleyin.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24" 
```    

5. Tek bir IP adresi için bir ağ kuralı kaldırın. 
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"  
```

6. Ağ kuralı için bir IP adresi aralığı kaldırın.  
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"  
```    

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için Deny veya ağ kuralları, herhangi bir etkisi olacaktır.
>

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Liste IP ağ kuralları
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
```

3. Tek bir IP adresi için bir ağ kuralı ekleyin.
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

4. Bir IP adresi aralığı için bir ağ kuralı ekleyin.  
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

5. Tek bir IP adresi için bir ağ kuralı kaldırın.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

6. Ağ kuralı için bir IP adresi aralığı kaldırın.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için Deny veya ağ kuralları, herhangi bir etkisi olacaktır.
>

## <a name="exceptions"></a>Özel durumlar
Ağ kurallarının çoğu senaryo için bir güvenli ağ yapılandırması etkinleştirebilirsiniz, ancak tam işlevselliğini etkinleştirmek için özel durumlar'ın nerede verilmelidir bazı durumlar vardır.  Depolama hesapları, güvenilen Microsoft Hizmetleri ve depolama analytics verilerine erişim için özel durumlar ile yapılandırılabilir.

### <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
Depolama hesapları ile etkileşimde bulunan bazı Microsoft Hizmetleri, ağ kuralları aracılığıyla erişim verilemez ağlardan çalışır. 

Bu tür bir hizmet beklendiği gibi çalışmaya izin vermek için ağ kuralları atlamak için güvenilen Microsoft hizmetlerinin kümesini izin verebilirsiniz. Bu hizmetler, daha sonra depolama hesabına erişmek için güçlü kimlik doğrulaması kullanır.

"Güvenilen Microsoft Hizmetleri" özel durum etkin olduğunda, aşağıdaki Hizmetleri (aboneliğinize kayıtlı olduğunda) depolama hesabına erişim izni verilir:

|Hizmet|Kaynak sağlayıcı adı|Amaç|
|:------|:---------------------|:------|
|Azure Backup|Microsoft.Backup|IAAS sanal makinelerini yedekleme ve geri yüklemeler yönetilmeyen disk gerçekleştirin. (yönetilen diskler için gerekli değildir). [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup).|
|Azure DevTest Labs|Microsoft.DevTestLab|Özel görüntü oluşturma ve yapıt yükleme.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|BLOB Depolama olayı yayımlamayı etkinleştirin.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/event-grid/overview).|
|Azure Event Hubs|Microsoft.EventHub|Event Hubs yakalama ile verileri arşivleme.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview).|
|Azure Ağı|Microsoft.Networking|Store ve ağ trafik günlüklerini analiz edin.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview).|
|Azure İzleyici|Microsoft.Insights| İzleme verilerinin güvenli storaage hesabına yazma sağlayan [daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security#monitoring-and-secured-Azure-storage-and-networks).|
|


### <a name="storage-analytics-data-access"></a>Depolama analizi veri erişimi
Bazı durumlarda, tanılama günlükleri ve ölçümleri okuma erişimi ağ sınırları dışından gelen gerekli değildir.  Ağ kuralların istisnaları, depolama hesabı günlük dosyaları, tabulky Metrik veya her ikisi de okuma erişimi izni verilebilir. [Depolama Analizi ile çalışma hakkında daha fazla bilgi edinin.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Özel durumları yönetme
Ağ kuralı özel durumlarını, Azure portal, PowerShell veya Azure CLI yönetilebilir v2.

#### <a name="azure-portal"></a>Azure portal
1. Korumak istediğiniz depolama hesabına gidin.  
2. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.
3. 'Seçili ağlar' dan erişime izin vermek seçtiniz emin olun.
4. Özel durumlar altında vermek istediğiniz özel durumlar'ı seçin.
5. Tıklayın *Kaydet* yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell
1. Son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).
2. Depolama hesabı ağ kuralları özel durumlarını görüntüler.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
```

3. Depolama hesabı ağ kuralları için özel durumlar yapılandırın.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass AzureServices,Metrics,Logging
```

4. Depolama hesabı ağ kurallarına özel durumları kaldırın.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass None
```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için Deny veya özel durumları kaldırılıyor herhangi bir etkisi olacaktır.
>

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Depolama hesabı ağ kuralları özel durumlarını görüntüler.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
```

3. Depolama hesabı ağ kuralları için özel durumlar yapılandırın.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
```

4. Depolama hesabı ağ kurallarına özel durumları kaldırın.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için Deny veya özel durumları kaldırılıyor herhangi bir etkisi olacaktır.
>

## <a name="next-steps"></a>Sonraki adımlar
Azure ağ hizmet uç noktalarına hakkında daha fazla bilgi [hizmet uç noktaları](/azure/virtual-network/virtual-network-service-endpoints-overview).

Azure depolama güvenlik ile daha derine inin [Azure depolama Güvenlik Kılavuzu](storage-security-guide.md).
