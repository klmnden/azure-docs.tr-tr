---
title: Azure Data Lake Storage 1. Nesil'de ağ güvenliği | Microsoft Docs
description: Azure Data Lake Storage 1. Nesil'deki IP güvenlik duvarı ve sanal ağ tümleştirme kavramlarını anlama
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/09/2018
ms.author: elsung
ms.openlocfilehash: 0da5962bc0b48a387ee82a1db36099682e14bca3
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49168195"
---
# <a name="virtual-network-integration-for-azure-data-lake-storage-gen1---preview"></a>Azure Data Lake Storage 1. Nesil için sanal ağ tümleştirmesi - Önizleme

Azure Data Lake Storage 1. Nesil için sanal ağ tümleştirmesi (önizleme) tanıtımı. Sanal ağ tümleştirmesi, Azure Data Lake Storage 1. Nesil hesaplarınızı belirli sanal ağlarınız ve alt ağlarınızla sınırlandırarak yetkisiz erişimi önlemenizi sağlar. Artık ADLS 1. Nesil hesabınızı yalnızca belirli sanal ağ ve alt ağlardan gelen trafiği kabul edip diğer tüm erişimi engelleyecek şekilde yapılandırabilirsiniz. Bu durum ADLS hesabınızı dışarıdan gelen tehditlere karşı korumanıza yardımcı olur.

ADLS 1. Nesil için sanal ağ tümleştirmesi, erişim belirtecinde ek güvenlik talepleri oluşturmak için sanal ağınızla Azure Active Directory hizmeti arasında sanal ağ hizmet uç noktası güvenliğini kullanır. Ardından bu talepler sanal ağınız için ADLS 1. Nesil hesabınızda kimlik doğrulaması gerçekleştirme ve erişim izni verme amacıyla kullanılır.

> [!NOTE]
> Bu teknoloji önizleme aşamasındadır ve üretim ortamlarında kullanılması önerilmez.
>
> Bu özellikler ek ücrete tabi değildir. ADLS 1. Nesil ([fiyatlandırma](https://azure.microsoft.com/pricing/details/data-lake-store/?cdn=disable)) ve kullandığınız tüm Azure hizmetleri ([fiyatlandırma](https://azure.microsoft.com/pricing/#product-picker)) için standart ücretler tahsil edilir.

## <a name="scenarios-for-vnet-integration-for-adls-gen1"></a>ADLS 1. Nesil için Sanal Ağ Tümleştirmesi senaryoları

ADLS Gen1 Sanal Ağ Tümleştirmesi, ADLS 1. Nesil hesabınıza erişimi belirlenen sanal ağlar ve alt ağlarla sınırlamanızı sağlar.  Erişim belirli bir Sanal Ağ Alt Ağı ile kilitlendikten sonra Azure'daki diğer sanal ağlara/VM'lere hesabınız için erişim izni verilmez.  ADLS 1. Nesil Sanal Ağ Tümleştirmesi, işlev açısından [sanal ağ hizmeti uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) ile aynı senaryoyu etkinleştirir.  İki senaryo arasında aşağıdaki bölümlerde belirtilen önemli farklar vardır. 

![ADLS 1. Nesil Sanal Ağ Tümleştirmesi senaryo diyagramı](media/data-lake-store-network-security/scenario-diagram.png)

> [!NOTE]
> Sanal ağ kurallarına ek olarak var olan IP güvenlik duvarı kurallarını kullanarak şirket içi ağlardan erişime de izin verebilirsiniz. 

## <a name="optimal-routing-with-adls-gen1-vnet-integration"></a>ADLS 1. Nesil Sanal Ağ Tümleştirmesi ile en iyi rota

Sanal ağ hizmet uç noktalarının temel avantajlarından biri, sanal ağınızdan [en iyi rotayı](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview#key-benefits) kullanma seçeneğidir.  ADLS 1. Nesil hesaplarında aynı rota iyileştirmesini gerçekleştirmek için sanal ağınızdan ADLS 1. Nesil hesabınıza aşağıdaki [kullanıcı tanımlı rotaları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) kullanabilirsiniz:

- **ADLS Genel IP Adresi**: Hedef ADLS 1. Nesil hesaplarınız için genel IP adresini kullanın.  Hesaplarınızın [DNS adlarını çözümleyerek](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-connectivity-from-vnets#enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity) ADLS 1. Nesil hesabınızın IP adresini tanımlayabilirsiniz.  Her adres için ayrı bir giriş oluşturun.

```azurecli
# Create a Route table for your resource group
az network route-table create --resource-group $RgName --name $RouteTableName

# Create Route Table Rules for ADLS Public IP Addresses
# There will be one rule per ADLS Public IP Addresses 
az network route-table route create --name toADLSregion1 --resource-group $RgName --route-table-name $RouteTableName --address-prefix <ADLS Public IP Address> --next-hop-type Internet

# Update the VNet and apply the newly created Route Table to it
az network vnet subnet update --vnet-name $VnetName --name $SubnetName --resource-group $RgName --route-table $RouteTableName
```

## <a name="data-exfiltration-from-the-customer-vnet"></a>Müşteri sanal ağından veri sızdırma

ADLS hesaplarını sanal ağ erişimiyle sınırlandırmaya ek olarak yetkisiz hesaba sızma olmamasını da sağlamak isteyebilirsiniz.

Önerimiz sanal ağınızda kullanacağınız bir güvenlik duvarı çözümü ile giden trafiği hedef hesap URL'sine göre filtrelemek ve yalnızca yetkili ADLS 1. Nesil hesaplarına izin vermektir.

Kullanılabilen seçeneklerin bazıları şunlardır:
- [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/overview): Sanal ağınız için bir [Azure Güvenlik Duvarı dağıtıp yapılandırabilir](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) ve giden ADLS trafiğinin güvenliğini sağlayıp bilinen ve yetkili hesap URL'leriyle sınırlandırabilirsiniz.
- [Ağ Sanal Gereci](https://azure.microsoft.com/solutions/network-appliances/) Güvenlik Duvarı: Yöneticiniz yalnızca belirli ticari güvenlik duvarı satıcılarının kullanımını yetkilendiriyorsa Azure Market'te bulunan bir NVA güvenlik duvarı çözümünü kullanarak aynı işleve erişebilirsiniz.

> [!NOTE]
> Veri yolunda güvenlik duvarı kullanımı, veri yoluna bir atlama ekler ve kullanılabilir aktarım hızı ile bağlantı gecikme süresi dahil olmak üzere uçtan uca veri alışverişinde ağ performansını olumsuz etkileyebilir. 

## <a name="limitations"></a>Sınırlamalar
1.  HDInsight kümeleri önizleme sürümüne eklendikten sonra oluşturulmalıdır.  ADLS 1. Nesil sanal ağ tümleştirmesi desteğinden önce oluşturulan kümelerin bu yeni özelliği desteklemesi için yeniden oluşturulması gerekir. 
2.  Yeni bir HDInsight kümesi oluştururken sanal ağ tümleştirmesi etkin bir ADLS 1. Nesil hesabının seçilmesi durumunda işlem başarısız olacaktır. Öncelikle sanal ağ kuralını devre dışı bırakmanız gerekir. Alternatif olarak ADLS hesabının **Güvenlik duvarı ve sanal ağlar** dikey penceresinden **Tüm ağlardan ve hizmetlerden erişime izin ver** seçeneğini belirleyebilirsiniz.  Daha fazla bilgi için [Özel durumlar](##Exceptions) bölümüne bakın.
3.  ADLS 1. Nesil sanal ağ tümleştirmesi önizleme sürümü [Azure kaynakları için yönetilen kimliklerle](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) çalışmaz.  
4.  Sanal ağın etkinleştirilmiş olduğu ADLS 1. Nesil hesabınızdaki dosya/klasör verilerine portaldan erişim sağlanamaz.  Buna sanal ağ içindeki VM'lerden erişim ve Veri Gezgini kullanımı gibi etkinlikler dahildir.  Hesap yönetimi etkinlikleri çalışmaya devam eder.  Sanal ağın etkinleştirilmiş olduğu ADLS hesabınızdaki dosya/klasör verilerine portal dışı tüm kaynaklardan erişim sağlanabilir: SDK erişimi, PowerShell betikleri, diğer Azure hizmetleri (kaynağı portal olmayan) vb. 

## <a name="configuration"></a>Yapılandırma

### <a name="step1-configure-your-vnet-to-use-aad-service-endpoint"></a>1. adım: Sanal ağınızı AAD Hizmet Uç Noktasını kullanacak şekilde yapılandırma
1.  Azure portala gidin ve hesabınızda oturum açın. 
2.  Aboneliğinizde [yeni bir sanal ağ oluşturun](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) veya var olan sanal ağlardan birine gidin.  Sanal ağın ADLS 1. Nesil hesabıyla aynı bölgede olması gerekir. 
3.  Sanal ağ dikey penceresinden **Hizmet uç noktaları**'nı seçin. 
4.  **Ekle**'ye tıklayarak yeni bir hizmet uç noktası ekleyin.
![Sanal ağ hizmet uç noktası ekleme](media/data-lake-store-network-security/config-vnet-1.png)
5.  Uç noktası hizmeti olarak **Microsoft.AzureActiveDirectory** seçeneğini belirleyin.
![Microsoft.AzureActiveDirectory hizmet uç noktasını seçme](media/data-lake-store-network-security/config-vnet-2.png)
6.  Bağlantı izni vermek istediğiniz alt ağları seçin ve **Ekle**'ye tıklayın.
![Alt ağı seçme](media/data-lake-store-network-security/config-vnet-3.png)
7.  Hizmet uç noktasının eklenmesi 15 dakika sürebilir. Uç nokta eklendikten sonra listede görünür. Göründüğünden ve tüm ayrıntıların yapılandırmaya uygun olduğundan emin olun. 
![Hizmet uç noktası ekleme başarılı](media/data-lake-store-network-security/config-vnet-4.png)

### <a name="step-2-set-up-the-allowed-vnetsubnet-for-your-adls-gen1-account"></a>2. Adım: ADLS 1. Nesil hesabınız için izin verilen sanal ağı/alt ağı ayarlama
1.  Sanal ağınızı yapılandırdıktan sonra aboneliğinizde [yeni bir Azure Data Lake Storage 1. Nesil hesabı](data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account) oluşturun veya var olan bir ADLS 1. Nesil hesabına gidin. ADLS 1. Nesil hesabının sanal ağ ile aynı bölgede bulunması gerekir. 
2.  **Güvenlik duvarları ve sanal ağlar**'ı seçin.

  > [!NOTE]
  > Ayarlarda **Güvenlik duvarları ve sanal ağlar** seçeneği görünmüyorsa lütfen portal oturumunu kapatın. Tarayıcıyı kapatın. Tarayıcı önbelleğini temizleyin. Makineyi yeniden başlatın ve yeniden deneyin.

  ![ADLS hesabınıza sanal ağ kuralı ekleme](media/data-lake-store-network-security/config-adls-1.png)
3.  **Seçili ağlar**'ı seçin. 
4.  **Var olan sanal ağı ekle**’ye tıklayın.
  ![Var olan sanal ağı ekle](media/data-lake-store-network-security/config-adls-2.png)
5.  Bağlantı izni vermek istediğiniz sanal ağları ve alt ağları seçip **Ekle**'ye tıklayın.
  ![Sanal ağları ve alt ağları seçme](media/data-lake-store-network-security/config-adls-3.png)
6.  Sanal ağların ve alt ağların listede düzgün şekilde gösterildiğinden emin olun ve **Kaydet**'e tıklayın.
  ![Yeni kuralı kaydetme](media/data-lake-store-network-security/config-adls-4.png)

  > [!NOTE]
  > Kaydettikten sonra ayarların geçerli olması 5 dakika sürebilir.

7.  [İsteğe bağlı] Sanal ağlara ve alt ağlara ek olarak belirli IP adreslerine erişim izni vermek isterseniz bu işlemi aynı sayfadaki **Güvenlik duvarı** bölümünden gerçekleştirebilirsiniz. 

## <a name="exceptions"></a>Özel durumlar
**Güvenlik duvarı ve sanal ağlar** dikey penceresinin Özel durumlar bölümünde, belirli hizmetlerden ve sanal makinelerden Azure'a erişilmesini sağlayan iki onay kutusu bulunur.
![Güvenlik duvarı ve sanal ağ özel durumları](media/data-lake-store-network-security/firewall-exceptions.png)
- **Tüm Azure hizmetlerinin bu Data Lake Storage 1. Nesil hesabına erişmesine izin ver** seçeneği Azure Data Factory, Event Hubs, Azure VM'leri gibi tüm Azure hizmetlerine erişim izni verir. Bu hizmetler ADLS hesabınızla iletişim kurabilir.

- **Azure Data Lake Analytics'in bu Data Lake Storage 1. Nesil hesabına erişmesine izin ver** seçeneği, Azure Data Lake Analytics hizmetinin bu ADLS hesabıyla bağlantı kurmasını sağlar. 

Bu özel durumları kapalı tutmanızı ve yalnızca sanal ağınızın dışındaki bu hizmetlerden bağlantı kurulmasını istediğiniz durumlarda açmanızı öneririz.
