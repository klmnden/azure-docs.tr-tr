---
title: "Azure depolama güvenlik duvarları ve sanal ağlar (Önizleme) yapılandırma | Microsoft Docs"
description: "Depolama hesabınız için katmanlı ağ güvenliği yapılandırın."
services: storage
documentationcenter: 
author: cbrooksmsft
manager: cbrooks
editor: cbrooks
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/25/2017
ms.author: cbrooks
ms.openlocfilehash: 9b00faa06684be353cfcf5f67f182a56511210c5
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks-preview"></a>Azure depolama güvenlik duvarları ve sanal ağlar (Önizleme) yapılandırma
Azure depolama, izin verilen ağlar belirli bir dizi depolama hesaplarınıza güvenli imkan tanıyan katmanlı bir güvenlik modeli sağlar.  Ağ kuralları yapılandırıldığında, yalnızca izin verilen ağlar uygulamalardan bir depolama hesabına erişebilir.  İzin verilen bir ağdan çağrılırken uygulamalar (geçerli erişim tuşu veya SAS belirteci) depolama hesabına erişmek için uygun yetkilendirme gerektirecek şekilde devam edin.

## <a name="preview-availability-and-support"></a>Önizleme kullanılabilirliği ve Destek
Depolama güvenlik duvarları ve sanal ağlar önizlemede.  Bu özellik şu anda tüm Azure genel bulut bölgeleri yeni veya var olan depolama hesapları için kullanılabilir.

> [!NOTE]
> Önizleme sırasında üretim iş yükleri desteklenmez.
>

## <a name="scenarios"></a>Senaryolar
Depolama hesapları (Internet trafiğini dahil) tüm ağlardan trafiği erişimini engellemek için varsayılan olarak yapılandırılabilir.  Erişim trafiği için uygulamalarınız için güvenli bir ağ sınırında oluşturmanıza olanak sağlayan belirli Azure sanal ağlardaki verilebilir.  Erişim de genel internet'e verilmesi IP adres aralıkları, belirli Internet veya şirket içi istemcilerinden gelen bağlantıları etkinleştirme.

Azure depolama, REST ve SMB dahil olmak üzere tüm ağ protokolleri üzerinde ağ kurallar zorlanır.  Verilerinize erişim Araçları'ndan gibi Azure portal, Depolama Gezgini ve AZCopy açık ağ kuralları ağ kurallar yürürlükte olduğunda erişim verilmesi gerekir.

Ağ kurallarının var olan depolama hesaplarına uygulanabilir veya yeni depolama hesapları oluşturma sırasında uygulanabilir.

Ağ kurallarının uygulandıktan sonra tüm istekler için zorunlu tutulmaz.  Belirli bir IP adresi hizmete erişim SAS belirteci hizmet için **sınırı** erişim belirteci sahibinin ancak değil yeni erişime yapılandırılmış ağ kuralları ötesinde yapın. 

(Bağlama dahil olmak üzere operations çıkarın ve disk g/ç) sanal makine diski trafiği **değil** ağ kurallarından etkilenen.  Sayfa bloblarını REST erişimi ağ kuralları tarafından korunur.

> [!NOTE]
> Yedekleme ve geri yükleme, yönetilmeyen diskleri depolama hesaplarında uygulanan ağ kurallarıyla kullanarak sanal makineleri şu anda desteklenmiyor.  Daha fazla bilgi için bkz: [yedekleme ve geri yükleme VM sınırlamaları](/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm)
>

Klasik depolama hesaplarını **sağlamadığı** güvenlik duvarları ve sanal ağları destekler.

## <a name="change-the-default-network-access-rule"></a>Varsayılan ağ erişim kuralını değiştirme
Varsayılan olarak, depolama hesapları herhangi bir ağ üzerindeki istemcilerden gelen bağlantıları kabul.  Seçili ağlara erişimi sınırlamak için önce varsayılan eylem değiştirmeniz gerekir.

> [!WARNING]
> Ağ kuralları için değişiklik yapmadan, uygulamalarınızın Azure depolamaya bağlanma yeteneğini etkileyebilir.  Varsayılan ağ kuralı ayarını **Reddet** tüm erişimi verileri sürece belirli ağ kuralları *verme* erişim de uygulanır.  İzin verilen erişimini engellemek için varsayılan Kuralı değiştirmeden önce ağ kurallarını kullanarak herhangi bir ağa erişim emin olun.
>

#### <a name="azure-portal"></a>Azure portalına
1. Korumak istediğiniz depolama hesabınıza gidin.  
> [!NOTE]
> Genel Önizleme için desteklenen bölgeler birinde depolama hesabınız olduğundan emin olun.
>

2. Adlı ayarlar menüsünde **güvenlik duvarları ve sanal ağlar**.
3. Varsayılan olarak erişimi reddetmek için 'Seçili ağlardan' erişime izin vermek seçin.  'Tüm ağlar' erişime izin verecek şekilde tüm ağlardan trafiğine izin vermek için seçin.
4. Tıklatın *kaydetmek* değişikliklerinizi uygulamak için.

#### <a name="powershell"></a>PowerShell
1. En son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).

2. Depolama hesabı için varsayılan kuralı durumunu görüntüleyin.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet  -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
``` 

3. Varsayılan olarak ağ erişimi reddetmek için varsayılan kural ayarlayın.  
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
```    

4. Varsayılan olarak ağ erişimi izin veren varsayılan kural ayarlayın.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
```    

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Depolama hesabı için varsayılan kuralı durumunu görüntüleyin.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
```

3. Varsayılan olarak ağ erişimi reddetmek için varsayılan kural ayarlayın.  
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Deny
```

4. Varsayılan olarak ağ erişimi izin veren varsayılan kural ayarlayın.
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Allow
```

## <a name="grant-access-from-a-virtual-network"></a>Bir sanal ağdan erişim izni ver
Depolama hesapları, yalnızca belirli Azure sanal ağlardan erişime izin verecek şekilde yapılandırılabilir. 

Etkinleştirerek bir [Hizmeti uç noktası](/azure/virtual-network/virtual-network-service-endpoints-overview) sanal ağ içinde Azure Storage için Azure Storage hizmetine uygun bir rotayı trafiği sağlamış. Sanal ağ ve alt ağ kimlikleri, ayrıca her istek ile aktarılır.  Yöneticiler, daha sonra depolama hesabı için belirli alt ağları sanal ağda alınan isteklerine izin ver ağ kuralları yapılandırabilirsiniz.  İstemcilerin bu ağ kuralları üzerinden erişim verilere erişmek için depolama hesabı yetkilendirme gereksinimlerini karşılamak devam etmeniz gerekir verildi.

Her Depolama hesabı ile birleştirilebilir 100'e kadar sanal ağ kuralları destekleyebilir [IP ağ kuralları](#grant-access-from-an-internet-ip-range).

### <a name="available-virtual-network-regions"></a>Kullanılabilir sanal ağ bölgeleri
Genel olarak, sanal ağlar ve aynı Azure bölgesinde hizmet örnekleri arasında hizmet uç noktaları çalışır.  Hizmet uç noktaları Azure Storage ile birlikte kullanıldığında, bu kapsamı içerecek şekilde genişletilir [eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions).  Bu salt okunur coğrafi reduntant depolama (RA-GRS) örneklerine seemless erişimin yanı sıra bir bölgesel yük devretme sırasında devamlılığı sağlar.  Sanal ağdan bir depolama hesabına erişim izni ağ kuralları da herhangi bir RA-GRS örneğine erişim.

Sırasında bölgesel bir kesintinin olağanüstü durum kurtarma için planlama yaparken, eşleştirilmiş bölgede bulunan sanal ağlar önceden hazırlamanız. Hizmet uç noktaları Azure Storage için etkinleştirilmiş olmalıdır ve bu diğer sanal ağlardan erişim verilmesi ağ kuralları, coğrafi olarak yedekli depolama hesaplarınıza uygulanmalıdır.

> [!NOTE]
> Hizmet uç noktaları sanal ağ ve belirtilen bölge çifti bölgesi dışında trafiği için geçerli değildir.  Sanal ağlardan depolama hesapları için erişim verme ağ kuralları yalnızca bir depolama hesabının birincil bölgesinde veya belirlenen eşleştirilmiş bölgede bulunan sanal ağlar için uygulanabilir.
>

### <a name="required-permissions"></a>Gerekli izinler
Bir depolama hesabı için bir sanal ağ kuralı uygulamak için kullanıcı izni olmalıdır *bir alt ağa Hizmeti'ne katılın* eklenmekte olan alt ağlar için.  Bu izni dahil *depolama hesabı katkıda bulunan* yerleşik rol ve özel rol tanımları için eklenebilir.

Depolama hesabı ve sanal ağlar, erişim izni **olabilir** farklı Aboneliklerde olabilir, ancak bu abonelikleri aynı Azure Active Directory Kiracı parçası olması gerekir.

### <a name="managing-virtual-network-rules"></a>Sanal ağ kuralları yönetme
Depolama hesapları için sanal ağ kuralları Azure portalı üzerinden, PowerShell veya CLIv2 yönetilebilir.

#### <a name="azure-portal"></a>Azure portalına
1. Korumak istediğiniz depolama hesabınıza gidin.  
2. Adlı ayarlar menüsünde **güvenlik duvarları ve sanal ağlar**.
3. 'Seçili ağlar' erişime izin verecek şekilde seçtiniz emin olun.
4. "Sanal ağlar" altında yeni bir ağ kuralla sanal ağa erişim vermek için "var olan Ekle"'ye tıklayın. var olan bir sanal ağ ve alt ağları seçmek için ardından *Ekle*.  Yeni bir sanal ağ oluşturmak ve erişim vermek için tıklatın *yeni Ekle*, yeni bir sanal ağ oluşturun ve ardından gerekli bilgileri sağlayın *oluşturma*.

> [!NOTE]
> Seçilen sanal ağ ve alt ağlar için Azure Depolama Hizmeti uç noktası önceden yapılandırılmamış, bu işlemin bir parçası olarak yapılandırılabilir.
>

5. Bir sanal ağ veya alt ağ kuralı kaldırmak için tıklayın sanal ağ veya alt bağlam menüsünü açmak için "..." ve "Kaldır" ı tıklatın.
6. Tıklatın *kaydetmek* değişikliklerinizi uygulamak için.

#### <a name="powershell"></a>PowerShell
1. En son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).
2. Sanal ağ kuralları listesinde
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
```

3. Var olan bir sanal ağ ve alt ağ üzerinde Azure Depolama Hizmeti uç noktası etkinleştirme
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"  -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzureRmVirtualNetwork
```

4. Bir sanal ağ ve alt ağ için ağ kuralı ekleyin.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

5. Bir sanal ağ ve alt ağ için ağ kuralı kaldırın.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Remove-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

> [!IMPORTANT]
> Şunları yaptığınızdan emin olun [varsayılan kural kümesi](#change-the-default-network-access-rule) engelle, ya da ağ kuralları, herhangi bir etkisi olacaktır.
>

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Sanal ağ kuralları listesinde
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
```

2. Var olan bir sanal ağ ve alt ağ üzerinde Azure Depolama Hizmeti uç noktası etkinleştirme
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
```

3. Bir sanal ağ ve alt ağ için ağ kuralı ekleyin.  
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

4. Bir sanal ağ ve alt ağ için ağ kuralı kaldırın. 
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

> [!IMPORTANT]
> Şunları yaptığınızdan emin olun [varsayılan kural kümesi](#change-the-default-network-access-rule) engelle, ya da ağ kuralları, herhangi bir etkisi olacaktır.
>

## <a name="grant-access-from-an-internet-ip-range"></a>Bir internet'ten erişim izni verme IP aralığı
Depolama hesapları, belirli genel internet'ten erişime izin verecek şekilde yapılandırılabilir IP adresi aralıkları.  Bu yapılandırma belirli Internet tabanlı hizmetler sağlar ve şirket içi ağlar genel Internet trafiğini engellenen sırada erişim verilmesi.

Internet adresi aralıklarını kullanılarak sağlanabilir izin [CIDR gösteriminde](https://tools.ietf.org/html/rfc4632) biçiminde *16.17.18.0/24* veya tek tek IP adresleri *16.17.18.19* .

> [!NOTE]
> Küçük bir adres aralıkları kullanarak "/ 31" veya "/ 32" öneki boyutları desteklenmez.  Bu aralıklar, tek tek IP adresi kuralları kullanılarak yapılandırılmalıdır.
>

IP ağ kuralları için izin verilmez **genel internet** IP adresleri.  IP adres aralıklarını (RFC 1918 tanımlandığı gibi) özel ağlar için ayrılmış IP kurallarında izin verilmez.  Özel ağlardaki dahil başlayın adresleri *10.\** , *172.16.\** , ve *192.168.\** .

Şu anda yalnızca IPv4 adresleri desteklenir.

Her Depolama hesabı ile birleştirilebilir en fazla 100 IP ağ kurallarını destekleyebilir [sanal ağ kuralları](#grant-access-from-a-virtual-network)

### <a name="configuring-access-from-on-premises-networks"></a>Şirket içi ağlardan erişimi yapılandırma
Şirket içi ağlarınız ile bir IP ağ kuralı depolama hesabınıza erişim için ağınızı tarafından kullanılan IP adresleri Internet'e tanımlamanız gerekir.  Yardım için ağ yöneticinize başvurun.

Azure ağı kullanarak ağınıza bağlı olup olmadığını [ExpressRoute](/azure/expressroute/expressroute-introduction), her bağlantı hattı Microsoft kullanarakServicesAzuredepolamagibibağlanmakiçinkullanılanikiortakIPadresiadresindekiMicrosoftEdgeileyapılandırılmış[Azure ortak eşleme](/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains).  Azure Storage bağlantı hattınız arasında iletişimi izin vermek için ortak IP adresleri, bağlantı hatları için IP ağ kuralları oluşturmanız gerekir.  Expressroute bağlantı hattı'nın genel IP adresleri bulmak için [ExpressRoute ile bir destek bileti açmanız](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure Portalı aracılığıyla.


### <a name="managing-ip-network-rules"></a>IP ağ kurallarını yönetme
IP ağ kuralları depolama hesapları için Azure portal, PowerShell veya CLIv2 yönetilebilir.

#### <a name="azure-portal"></a>Azure portalına
1. Korumak istediğiniz depolama hesabınıza gidin.  
2. Adlı ayarlar menüsünde **güvenlik duvarları ve sanal ağlar**.
3. 'Seçili ağlar' erişime izin verecek şekilde seçtiniz emin olun.
4. Vermek için IP aralığı bir internet'e erişmek için IP adresi veya adres aralığı (CIDR biçiminde) güvenlik duvarı, adres aralıklarını altında girin.
5. Bir IP ağ kuralı kaldırmak için tıklatın kural bağlam menüsünü açmak için "..." ve "Kaldır" ı tıklatın.
6. Tıklatın *kaydetmek* değişikliklerinizi uygulamak için.

#### <a name="powershell"></a>PowerShell
1. En son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).
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

6. Bir IP adresi aralığı için bir ağ kuralı kaldırın.  
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"  
```    

> [!IMPORTANT]
> Şunları yaptığınızdan emin olun [varsayılan kural kümesi](#change-the-default-network-access-rule) engelle, ya da ağ kuralları, herhangi bir etkisi olacaktır.
>

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
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

6. Bir IP adresi aralığı için bir ağ kuralı kaldırın.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

> [!IMPORTANT]
> Şunları yaptığınızdan emin olun [varsayılan kural kümesi](#change-the-default-network-access-rule) engelle, ya da ağ kuralları, herhangi bir etkisi olacaktır.
>

## <a name="exceptions"></a>Özel durumlar
Çoğu senaryo için güvenli ağ yapılandırması ağ kuralları etkinleştirebilirsiniz, ancak tam işlevselliğini etkinleştirmek için özel durumlar'ın nerede verilmelidir bazı durumlar vardır.  Depolama hesapları özel durumlar depolama analytics veri erişimi ve güvenilen Microsoft Hizmetleri için yapılandırılabilir.

### <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri
Ağ kurallarının üzerinden erişim verilemez ağlardan depolama hesapları ile etkileşim bazı Microsoft Hizmetleri çalışır. 

Bu tür hizmet beklendiği gibi çalışmaya izin vermek için ağ kurallarını atlamak için güvenilen Microsoft Hizmetleri kümesi izin verebilir. Bu hizmetleri sonra depolama hesabına erişim için güçlü kimlik doğrulaması'nı kullanır.

"Güvenilen Microsoft Hizmetleri" özel durumu etkinleştirildiğinde, aşağıdaki Hizmetleri (aboneliğinizde kaydolurken) depolama hesabına erişim izni verilir:

|Hizmet|Kaynak sağlayıcı adı|Amaç|
|:------|:---------------------|:------|
|Azure DevTest Labs|Microsoft.DevTestLab|Özel görüntü oluşturma ve yapı yükleme.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|BLOB Storage olay yayımlamayı etkinleştirin.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/event-grid/overview).|
|Azure Event Hubs|Microsoft.EventHub|Olay hub'ları yakalama ile arşiv verileri.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview).|
|Azure Hdınsight|Microsoft.HDInsight|Küme hazırlama ve yükleme.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-blob-storage).|
|Azure Ağı|Microsoft.Networking|Depolama ve ağ trafiği günlüklerini analiz edin.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview).|
||||

### <a name="storage-analytics-data-access"></a>Storage analytics veri erişimi
Bazı durumlarda, tanılama günlüklerini ve ölçümleri okuma erişimi gelen ağ sınırının dışında gereklidir.  Ağ kurallarında özel durumların, depolama hesabı günlük dosyaları, ölçümleri tablolar veya her ikisine de okuma erişimi izni verilebilir. [Storage analytics ile çalışma hakkında daha fazla bilgi edinin.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Özel durumlar yönetme
Ağ kuralı özel durumlarını Azure portal, PowerShell veya Azure CLI yönetilebilir v2.

#### <a name="azure-portal"></a>Azure portalına
1. Korumak istediğiniz depolama hesabınıza gidin.  
2. Adlı ayarlar menüsünde **güvenlik duvarları ve sanal ağlar**.
3. 'Seçili ağlar' erişime izin verecek şekilde seçtiniz emin olun.
4. Özel durumlar altında vermek istediğiniz özel durumları seçin.
5. Tıklatın *kaydetmek* değişikliklerinizi uygulamak için.

#### <a name="powershell"></a>PowerShell
1. En son yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) ve [oturum açma](/powershell/azure/authenticate-azureps).
2. Depolama hesabı ağ kuralları için özel durumlarını görüntüler.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
```

3. Depolama hesabı ağ kurallar için özel durumlar yapılandırın.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass AzureServices,Metrics,Logging
```

4. Depolama hesabı ağ kurallar için özel durumlar kaldırın.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass None
```

> [!IMPORTANT]
> Şunları yaptığınızdan emin olun [varsayılan kural kümesi](#change-the-default-network-access-rule) engelle, veya özel durumları kaldırmayı herhangi bir etkisi olacaktır.
>

#### <a name="cliv2"></a>CLIv2
1. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli) ve [oturum açma](/cli/azure/authenticate-azure-cli).
2. Depolama hesabı ağ kuralları için özel durumlarını görüntüler.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
```

3. Depolama hesabı ağ kurallar için özel durumlar yapılandırın.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
```

4. Depolama hesabı ağ kurallar için özel durumlar kaldırın.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
```

> [!IMPORTANT]
> Şunları yaptığınızdan emin olun [varsayılan kural kümesi](#change-the-default-network-access-rule) engelle, veya özel durumları kaldırmayı herhangi bir etkisi olacaktır.
>

## <a name="next-steps"></a>Sonraki adımlar
Azure Ağ Hizmeti uç noktalarını hakkında daha fazla bilgi [hizmet uç noktaları](/azure/virtual-network/virtual-network-service-endpoints-overview).

Azure Storage güvenlik içine derin dıg [Azure depolama Güvenlik Kılavuzu](storage-security-guide.md).
