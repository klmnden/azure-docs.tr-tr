---
title: Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma | Microsoft Docs
description: Depolama hesabınız için katmanlı ağ güvenliğini yapılandırın.
services: storage
author: cbrooksmsft
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: cbrooks
ms.subservice: common
ms.openlocfilehash: 6d6ca1fe1256f1571079027ebd299492bfa62f41
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280749"
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks"></a>Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma

Azure depolama, katmanlı güvenlik modeli sağlar. Bu model, desteklenen ağları belirli bir kümesi, depolama hesaplarınıza güvenli olanak tanır. Ağ kuralları yapılandırıldığında, yalnızca belirtilen ağlar kümesini projelerimizin veri isteyen uygulamalar bir depolama hesabına erişebilir.

Bir depolama hesabı ağ kuralları geçerli olduğunda erişen bir uygulama istek üzerine uygun yetkilendirme gerektirir. Yetkilendirme, BLOB'lar ve Kuyruklar için Azure Active Directory (Azure AD) kimlik bilgilerini, geçerli hesap erişim anahtarı ile veya bir SAS belirteci ile desteklenir.

> [!IMPORTANT]
> Bir Azure sanal ağı (VNet) içinde çalışan bir hizmet isteği gelen sürece depolama hesabınız için güvenlik duvarı kurallarını etkinleştirmek varsayılan olarak, veri gelen istekleri engeller. Engellenen istekleri, Azure portalından, günlük ve ölçüm hizmetlerden, diğer Azure hizmetlerinden gelen içerir ve benzeri.
>
> Gelen alt ağ hizmeti örneğinin vererek bir Vnet'te çalışan Azure hizmetlerine erişime izin verebilirsiniz. Etkinleştirme senaryoları ile sınırlı sayıda [özel durumları](#exceptions) aşağıdaki bölümde anlatılan mekanizması. Azure portalına erişmek için ayarladığınız güvenilen sınırları (IP veya VNet) içinde bir makinede olması gerekir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="scenarios"></a>Senaryolar

(İnternet trafiğini dahil) tüm ağlardan erişime trafiği reddetmeye yönelik depolama hesapları varsayılan olarak yapılandırın. Daha sonra erişim akışına ait özel sanal ağlar verin. Bu yapılandırma, uygulamalarınız için bir güvenli ağ sınırı oluşturmanıza olanak sağlar. Ayrıca genel internet'e erişim izni verebilirsiniz IP adresi aralıkları, belirli bir internet veya şirket içi istemcilerinden gelen bağlantıları etkinleştirme.

Azure depolama REST ve SMB dahil olmak üzere, tüm ağ protokolleri üzerinde ağ kuralları uygulanır. Azure portalında, Depolama Gezgini ve AZCopy gibi araçlarla verilere erişmek için açık ağ kuralları gerekli değildir.

Var olan depolama hesaplarını veya yeni bir depolama hesabı oluşturduğunuzda ağ kuralları uygulayabilirsiniz.

Bir kez ağ kuralları uygulandığında, tüm istekler için zorunlu. Erişim belirli bir IP adresi için SAS belirteçleri, belirteç sahibinin erişimi sınırlamak için hizmet, ancak yapılandırılmış ağ kuralları ötesinde yeni erişim izni yok.

Sanal makine diski trafiği (ve işlemleri ve disk g/ç çıkarın bağlama dahil), ağ kuralları tarafından etkilenmez. REST erişimini sayfa BLOB'ları, ağ kuralları tarafından korunur.

Klasik depolama hesapları, güvenlik duvarları ve sanal ağlar'ı desteklemez.

Bir özel durum oluşturarak yedekleme ve geri yükleme Vm'lere uygulanan ağ kuralları ile depolama hesaplarında yönetilmeyen diskler kullanabilirsiniz. Bu işlem belgelenen [özel durumları](#exceptions) bu makalenin. Zaten Azure tarafından yönetildikleri gibi güvenlik duvarı özel durumlarını yönetilen disklerle ilgili değildir.

## <a name="change-the-default-network-access-rule"></a>Varsayılan ağ erişim kuralını değiştirme

Varsayılan olarak, depolama hesapları herhangi bir ağ üzerinde istemci bağlantılarını kabul edin. Seçili ağlar erişimi sınırlamak için önce varsayılan eylem değiştirmeniz gerekir.

> [!WARNING]
> Ağ kuralları için değişiklik yapmadan uygulamalarınızın Azure depolamaya bağlanma yeteneğini etkileyebilir. Varsayılan ağ kuralı ayarını **Reddet** belirli ağ kurallar sürece verilere erişim engellenir tüm **vermek** erişim de uygulanır. İzin verilen erişimi engellemek için varsayılan Kuralı değiştirmeden önce ağ kurallarını kullanarak herhangi bir ağa erişim vermek emin olun.

### <a name="managing-default-network-access-rules"></a>Varsayılan ağ erişim kurallarını yönetme

Azure portalı, PowerShell veya CLIv2 aracılığıyla depolama hesapları için varsayılan ağ erişim kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Korumak istediğiniz depolama hesabına gidin.

1. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.

1. Varsayılan olarak erişimi reddetmek için erişime izin verecek şekilde seçin: **seçili ağlar**. Tüm ağlar gelen trafiğe izin vermek için erişime izin verecek şekilde seçin: **tüm ağlar**.

1. Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell

1. Yükleme [Azure PowerShell](/powershell/azure/install-Az-ps) ve [oturum](/powershell/azure/authenticate-azureps).

1. Depolama hesabı için varsayılan kuralın durumunu görüntüler.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
    ```

1. Varsayılan olarak ağ erişimini engellemek için varsayılan kuralı ayarlayın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
    ```

1. Varsayılan olarak ağ erişim izni vermek için varsayılan kuralı ayarlayın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
    ```

#### <a name="cliv2"></a>CLIv2

1. Yükleme [Azure CLI](/cli/azure/install-azure-cli) ve [oturum](/cli/azure/authenticate-azure-cli).

1. Depolama hesabı için varsayılan kuralın durumunu görüntüler.

    ```azurecli
    az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
    ```

1. Varsayılan olarak ağ erişimini engellemek için varsayılan kuralı ayarlayın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --default-action Deny
    ```

1. Varsayılan olarak ağ erişim izni vermek için varsayılan kuralı ayarlayın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --default-action Allow
    ```

## <a name="grant-access-from-a-virtual-network"></a>Bir sanal ağdan erişim izni ver

Depolama hesapları yalnızca belirli sanal ağlar erişime izin verecek şekilde yapılandırabilirsiniz.

Etkinleştirme bir [hizmet uç noktası](/azure/virtual-network/virtual-network-service-endpoints-overview) Azure depolama sanal ağ içinden. Bu uç nokta trafiği için Azure depolama hizmetine bir en uygun rotayı sunar. Sanal ağ ve alt ağ kimlikleri, ayrıca her bir istekle aktarılır. Yöneticiler, daha sonra depolama hesabı için sanal ağ içindeki belirli alt ağlara alınan isteklerine izin ver ağ kuralları yapılandırabilirsiniz. İstemcilerin bu ağ kuralları üzerinden erişim verilere erişmek için depolama hesabı yetkilendirme gereksinimlerini karşılamak devam etmelidir verildi.

Her Depolama hesabı birleştirilebilir 100'e kadar sanal ağ kuralları desteklediği [IP ağ kuralları](#grant-access-from-an-internet-ip-range).

### <a name="available-virtual-network-regions"></a>Kullanılabilir sanal ağ bölgeleri

Genel olarak, hizmet uç noktaları sanal ağları ve aynı Azure bölgesinde hizmet örnekleri arasında çalışır. Hizmet uç noktaları Azure Depolama'yı kullanırken, bu kapsamı içerecek şekilde büyür [eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions). Hizmet uç noktaları sürekliliği sırasında bölgesel yük devretme ve salt okunur coğrafi olarak yedekli depolama (RA-GRS) örnekleri için erişim verin. Sanal ağdan bir depolama hesabına erişim izni ağ kuralları da herhangi bir RA-GRS örneğine erişim.

Bölgesel bir kesinti sırasında olağanüstü durum kurtarma için planlama yaparken, sanal ağlar eşlenen bölgesi içinde önceden oluşturmanız gerekir. Hizmet uç noktaları Azure depolama için bu diğer sanal ağlardan erişim verme ağ kurallarıyla etkinleştirin. Ardından bu kurallar, coğrafi olarak yedekli depolama hesaplarınıza uygulayın.

> [!NOTE]
> Hizmet uç noktaları için trafiği sanal ağ ve belirtilen bölge çiftine bölgesi dışında geçerli değildir. Yalnızca depolama hesaplarına birincil bölgede bir depolama hesabı veya belirlenen eşleştirilmiş bölge sanal ağdan erişim verme ağ kuralları uygulayabilirsiniz.

### <a name="required-permissions"></a>Gerekli izinler

Bir depolama hesabı için bir sanal ağ kuralı uygulamak için kullanıcının eklenen alt ağlarda için uygun izinleri olmalıdır. Gerekli izni *bir alt ağa Hizmeti'ne Katıl* ve dahildir *depolama hesabı Katılımcısı* yerleşik rolü. Özel rol tanımları da eklenebilir.

Depolama hesabı ve sanal ağlar, erişim farklı Aboneliklerde olabilir, ancak bu abonelikler aynı Azure AD kiracısına bir parçası olmalıdır verildi.

### <a name="managing-virtual-network-rules"></a>Sanal ağ kurallarını yönetme

Azure portalı, PowerShell veya CLIv2 aracılığıyla depolama hesapları için sanal ağ kuralları yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Korumak istediğiniz depolama hesabına gidin.

1. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.

1. Erişime izin verecek şekilde seçtiğiniz denetleyin **seçili ağlar**.

1. Altında yeni bir ağ kuralı ile bir sanal ağa erişim vermek için **sanal ağlar**, tıklayın **var olan sanal ağı Ekle**seçin **sanal ağlar** ve **Alt ağlar** seçenekleri ve ardından **Ekle**. Yeni bir sanal ağ oluşturma ve erişim vermek için **Ekle yeni sanal ağ**. Yeni bir sanal ağ oluşturun ve ardından için gereken bilgileri sağlayan **Oluştur**.

    > [!NOTE]
    > Azure depolama için hizmet uç noktası seçilen sanal ağ ve alt ağlar için önceden yapılandırılmış değildi, bu işlemin bir parçası yapılandırabilirsiniz.

1. Bir sanal ağ veya alt ağ kuralı kaldırmak için tıklayın **...**  sanal ağ veya alt ağ için bağlam menüsünü açın ve **Kaldır**.

1. Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell

1. Yükleme [Azure PowerShell](/powershell/azure/install-Az-ps) ve [oturum](/powershell/azure/authenticate-azureps).

1. Sanal ağ kuralları listesi.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
    ```

1. Hizmet uç noktası, bir var olan sanal ağı ve alt ağ üzerinde Azure depolama için etkinleştirin.

    ```powershell
    Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.0.0.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzVirtualNetwork
    ```

1. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.

    ```powershell
    $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id
    ```

1. Bir sanal ağ ve alt ağ için bir ağ kuralı kaldırın.

    ```powershell
    $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id
    ```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için **Reddet**, veya ağ kuralları etkilemez.

#### <a name="cliv2"></a>CLIv2

1. Yükleme [Azure CLI](/cli/azure/install-azure-cli) ve [oturum](/cli/azure/authenticate-azure-cli).

1. Sanal ağ kuralları listesi.

    ```azurecli
    az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
    ```

1. Hizmet uç noktası, bir var olan sanal ağı ve alt ağ üzerinde Azure depolama için etkinleştirin.

    ```azurecli
    az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
    ```

1. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.

    ```azurecli
    $subnetid=(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
    ```

1. Bir sanal ağ ve alt ağ için bir ağ kuralı kaldırın.

    ```azurecli
    $subnetid=(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
    ```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için **Reddet**, veya ağ kuralları etkilemez.

## <a name="grant-access-from-an-internet-ip-range"></a>İnternet'ten erişim ver IP aralığı

Depolama hesapları, belirli genel internet'ten erişim izni vermek için yapılandırdığınız IP adresi aralıkları. Bu yapılandırma belirli internet tabanlı hizmetlere erişim verir ve şirket içi ağlara ve genel internet trafiği engeller.

Adres aralıkları kullanarak izin verilen internet sağlamak [CIDR gösteriminde](https://tools.ietf.org/html/rfc4632) biçiminde *16.17.18.0/24* tek tek IP adresleri gibi veya *16.17.18.19*.

   > [!NOTE]
   > Küçük adres aralıkları kullanarak "/ 31" veya "/ 32" öneki boyutları desteklenmez. Bu aralıklar, tek tek IP adresi kurallarını kullanarak yapılandırılmalıdır.

IP ağ kuralları için yalnızca verilir **genel internet** IP adresleri. IP adres aralıkları özel ağlar için ayrılmış (sınıfında tanımlandığı gibi [RFC 1918](https://tools.ietf.org/html/rfc1918#section-3)) IP kurallarında izin verilmez. Özel ağlar ile başlayan bir adres dahil _10.*_, _172.16. *_ - _172.31. *_, ve _192.168. *_.

   > [!NOTE]
   > IP ağ kuralları için aynı Azure bölgesindeki depolama hesabı kaynaklanan taleplere üzerinde etkisi yoktur. Kullanım [sanal ağ kuralları](#grant-access-from-a-virtual-network) aynı bölgede isteklere izin vermek için.

Şu anda yalnızca IPv4 adresleri desteklenir.

Her Depolama hesabı ile birleştirilebilir, 100'e kadar IP ağ kurallarını destekler [sanal ağ kuralları](#grant-access-from-a-virtual-network).

### <a name="configuring-access-from-on-premises-networks"></a>Şirket içi ağlardan erişimi yapılandırma

Depolama hesabınıza bir IP ağ kuralı ile şirket içi ağlarınızı erişim vermek için internet'e yönelik ağınız tarafından kullanılan IP adreslerini tanımlamanız gerekir. Yardım için ağ yöneticinize başvurun.

Kullanıyorsanız [ExpressRoute](/azure/expressroute/expressroute-introduction) şirket içinden ortak eşleme veya Microsoft eşlemesi için kullanılan NAT IP adreslerini tanımlamanız gerekecek. Ortak eşleme için, her bir ExpressRoute varsayılan olarak bağlantı hattında trafik Microsoft Azure omurga ağına girdiğinde Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanılır. Microsoft eşlemesi için, kullanılan NAT IP adresleri müşteri tarafından sağlanır veya hizmet sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](/azure/expressroute/expressroute-nat#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.

### <a name="managing-ip-network-rules"></a>IP ağ kurallarını yönetme

Azure portalı, PowerShell veya CLIv2 aracılığıyla depolama hesapları için IP ağ kuralları yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Korumak istediğiniz depolama hesabına gidin.

1. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.

1. Erişime izin verecek şekilde seçtiğiniz denetleyin **seçili ağlar**.

1. Vermek için IP aralığı bir internet'e erişmek için IP adresi veya adres aralığı (CIDR biçiminde) altında girin **Güvenlik Duvarı** > **adres aralığı**.

1. Bir IP ağ kuralı kaldırmak için adres aralığı yanındaki çöp kutusu simgesine tıklayın.

1. Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell

1. Yükleme [Azure PowerShell](/powershell/azure/install-Az-ps) ve [oturum](/powershell/azure/authenticate-azureps).

1. IP ağ kuralları listesi.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
    ```

1. Tek bir IP adresi için bir ağ kuralı ekleyin.

    ```powershell
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"
    ```

1. Bir IP adresi aralığı için bir ağ kuralı ekleyin.

    ```powershell
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"
    ```

1. Tek bir IP adresi için bir ağ kuralı kaldırın.

    ```powershell
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"
    ```

1. Ağ kuralı için bir IP adresi aralığı kaldırın.

    ```powershell
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"
    ```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için **Reddet**, veya ağ kuralları etkilemez.

#### <a name="cliv2"></a>CLIv2

1. Yükleme [Azure CLI](/cli/azure/install-azure-cli) ve [oturum](/cli/azure/authenticate-azure-cli).

1. IP ağ kuralları listesi.

    ```azurecli
    az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
    ```

1. Tek bir IP adresi için bir ağ kuralı ekleyin.

    ```azurecli
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
    ```

1. Bir IP adresi aralığı için bir ağ kuralı ekleyin.

    ```azurecli
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
    ```

1. Tek bir IP adresi için bir ağ kuralı kaldırın.

    ```azurecli
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
    ```

1. Ağ kuralı için bir IP adresi aralığı kaldırın.

    ```azurecli
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
    ```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için **Reddet**, veya ağ kuralları etkilemez.

## <a name="exceptions"></a>Özel durumlar

Ağ kurallarının çoğu senaryo için bir güvenli ağ yapılandırması etkinleştirebilirsiniz. Ancak, tam işlevselliğini etkinleştirmek için özel durumlar'ın nerede verilmelidir bazı durumlar vardır. Depolama hesapları, güvenilen Microsoft Hizmetleri ve depolama analytics verilerine erişim için özel durumlar ile yapılandırabilirsiniz.

### <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri

Depolama hesapları ile etkileşimde bulunan bazı Microsoft Hizmetleri, ağ kuralları aracılığıyla erişim verilemez ağlardan çalışır.

Bu tür bir hizmet iş beklendiği gibi yardımcı olmak için ağ kuralları atlamak için güvenilen Microsoft Hizmetleri kümesi sağlar. Bu hizmetler, daha sonra depolama hesabına erişmek için güçlü kimlik doğrulaması kullanır.

Etkinleştirirseniz **izin güvenilen Microsoft Hizmetleri...**  özel durum, aşağıdaki Hizmetleri (aboneliğinize kayıtlı olduğunda), depolama hesabına erişim izni verilir:

|Hizmet|Kaynak sağlayıcı adı|Amaç|
|:------|:---------------------|:------|
|Azure Backup|Microsoft.Backup|Yedekleme ve geri yüklemeler yönetilmeyen diskler, IAAS sanal makinelerde çalıştırır. (yönetilen diskler için gerekli değildir). [Daha fazla bilgi edinin](/azure/backup/backup-introduction-to-azure-backup).|
|Azure Data Box|Microsoft.DataBox|Data Box'ı kullanarak azure'a veri aktarımını sağlar. [Daha fazla bilgi edinin](/azure/databox/data-box-overview).|
|Azure DevTest Labs|Microsoft.DevTestLab|Özel görüntü oluşturma ve yapıt yükleme. [Daha fazla bilgi edinin](/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|BLOB Depolama olayı yayımlamayı etkinleştirme ve depolama kuyrukları yayımlamak Event Grid sağlar. Hakkında bilgi edinin [blob depolama olayları](/azure/event-grid/event-sources) ve [sıralara yayımlama](/azure/event-grid/event-handlers).|
|Azure Event Hubs|Microsoft.EventHub|Event Hubs yakalama ile verileri arşivleme. [Daha fazla bilgi edinin](/azure/event-hubs/event-hubs-capture-overview).|
|Azure HDInsight|Microsoft.HDInsight|Yeni bir HDInsight kümesinin varsayılan dosya sistemi ilk içeriğini sağlayın. [Daha fazla bilgi edinin](https://azure.microsoft.com/en-us/blog/enhance-hdinsight-security-with-service-endpoints/).|
|Azure İzleyici|Microsoft.Insights|İzleme verilerinin bir güvenli depolama hesabına yazma sağlayan [daha fazla bilgi edinin](/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security).|
|Azure Ağı|Microsoft.Networking|Store ve ağ trafik günlüklerini analiz edin. [Daha fazla bilgi edinin](/azure/network-watcher/network-watcher-packet-capture-overview).|
|Azure Site Recovery|Microsoft.SiteRecovery |Olağanüstü durum kurtarma, Azure Iaas sanal makineler için çoğaltma etkinleştirerek yapılandırın. Güvenlik Duvarı etkin önbellek depolama hesabı veya kaynak depolama hesabı veya hedef depolama hesabı kullanıyorsanız, bu gereklidir.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication).|
|Azure SQL Veri Ambarı|Microsoft.Sql|İçeri aktarma sağlar ve PolyBase kullanarak senaryolar dışarı aktarın. [Daha fazla bilgi edinin](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview).|

### <a name="storage-analytics-data-access"></a>Depolama analizi veri erişimi

Bazı durumlarda, tanılama günlükleri ve ölçümleri okuma erişimi ağ sınırları dışından gelen gerekli değildir. Depolama hesabı günlük dosyaları, tabulky Metrik veya her ikisini okuma-erişime izin vermek için ağ kuralların istisnaları verebilirsiniz. [Depolama Analizi ile çalışma hakkında daha fazla bilgi edinin.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Özel durumları yönetme

Ağ kuralı özel durumlarını Azure portal, PowerShell veya Azure CLI aracılığıyla yönetebileceğiniz v2.

#### <a name="azure-portal"></a>Azure portal

1. Korumak istediğiniz depolama hesabına gidin.

1. Adlı ayarları menüsünde **güvenlik duvarları ve sanal ağlar**.

1. Erişime izin verecek şekilde seçtiğiniz denetleyin **seçili ağlar**.

1. Altında **özel durumları**, istediğiniz vermek için özel durumlar'ı seçin.

1. Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için.

#### <a name="powershell"></a>PowerShell

1. Yükleme [Azure PowerShell](/powershell/azure/install-Az-ps) ve [oturum](/powershell/azure/authenticate-azureps).

1. Depolama hesabı ağ kuralları özel durumlarını görüntüler.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
    ```

1. Depolama hesabı ağ kuralları için özel durumlar yapılandırın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -Bypass AzureServices,Metrics,Logging
    ```

1. Depolama hesabı ağ kurallarına özel durumları kaldırın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -Bypass None
    ```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için **Reddet**, veya kaldırılırken özel durum etkilemez.

#### <a name="cliv2"></a>CLIv2

1. Yükleme [Azure CLI](/cli/azure/install-azure-cli) ve [oturum](/cli/azure/authenticate-azure-cli).

1. Depolama hesabı ağ kuralları özel durumlarını görüntüler.

    ```azurecli
    az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
    ```

1. Depolama hesabı ağ kuralları için özel durumlar yapılandırın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
    ```

1. Depolama hesabı ağ kurallarına özel durumları kaldırın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
    ```

> [!IMPORTANT]
> Mutlaka [varsayılan kural kümesi](#change-the-default-network-access-rule) için **Reddet**, veya kaldırılırken özel durum etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

Azure ağ hizmet uç noktalarına hakkında daha fazla bilgi [hizmet uç noktalarını](/azure/virtual-network/virtual-network-service-endpoints-overview).

Azure depolama güvenlik ile daha derine inin [Azure depolama Güvenlik Kılavuzu](storage-security-guide.md).
