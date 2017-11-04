---
title: "Bir sanal AĞA Azure SSIS tümleştirmesi çalışma zamanı katılma | Microsoft Docs"
description: "Bir Azure sanal ağı Azure SSIS tümleştirmesi çalışma zamanı katılma öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: spelluru
ms.openlocfilehash: 8a58f55bd627594145661e1c8d5c1da360cd1e30
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Bir Azure SSIS tümleştirmesi çalışma zamanı sanal bir ağa katılmasını sağlayın
Aşağıdaki koşullardan biri doğruysa bir Azure sanal ağı (VNet) için Azure SSIS tümleştirmesi çalışma zamanı (IR) eklemeniz gerekir: 

- SSIS Katalog veritabanını bir sanal ağın parçası olan SQL Server Yönetilen Örneği (özel önizleme) üzerinde barındırıyorsanız.
- Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız.

 Azure Data Factory sürüm 2 (Önizleme), Azure SSIS tümleştirme çalışma zamanını klasik bir sanal ağa eklemenize olanak tanır. Şu anda, Azure Resource Manager Vnet'i henüz desteklenmiyor. Ancak, bunun çevresine gösterildiği gibi aşağıdaki bölümde çalışabilirsiniz. 

 > [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

SSIS paketleri yalnızca genel bulut veri depolarına erişirseniz, bir sanal ağa Azure SSIS IR katılma gerek yoktur. SSIS paketleri şirket içi veri depolarına erişirse, şirket içi ağı'na bağlı bir sanal ağa Azure SSIS IR katılması gerekir. SSIS katalog Azure SQL veritabanında, VNet içinde değil barındırılıyorsa, uygun bağlantı noktalarını açmanız gerekir. SSIS katalog Azure SQL yönetilen Klasik VNet içinde olan örneğinde barındırılıyorsa, aynı Klasik VNet (veya) yönetilen Azure SQL örneğinin tek bir sınıf Klasik VNet bağlantısı olan farklı bir Klasik VNet Azure SSIS IR birleştirebilirsiniz. Aşağıdaki bölümlerde daha ayrıntılı bilgi sağlar.  

## <a name="access-on-premises-data-stores"></a>Erişim içi veri depoları
SSIS paketleri şirket içi veri depolarına erişirse, şirket içi ağınıza bağlı bir sanal ağa, Azure SSIS tümleştirmesi çalışma zamanı katılın. Dikkat edilecek bazı önemli noktalar şunlardır: 

- Varsa mevcut bir VNet şirket içi ağınıza bağlı, ilk oluşturun bir [Klasik VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Ardından, bir site siteye yapılandırın [VPN ağ geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)/[ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bu sanal ağ bağlantısından şirket içi ağınıza.
- Azure SSIS tümleştirmesi çalışma zamanı modülü ile aynı konumda şirket içi ağınıza bağlı olan bir Klasik VNet ise, Azure SSIS tümleştirmesi çalışma zamanı birleştirebilirsiniz.
- Varolan bir Klasik VNet Azure SSIS tümleştirme çalışma zamanını şuradan farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Klasik VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) , katılmak Azure SSIS tümleştirmesi çalışma zamanı için. Ardından, yapılandırma bir [Klasik Klasik VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) bağlantı.
- Varsa mevcut bir Azure Resource Manager Vnet'i şirket içi ağınıza bağlı, ilk oluşturun bir [Klasik VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Ardından, yapılandırma bir [Klasik Azure Resource Manager Vnet'i](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.

## <a name="domain-name-services-server"></a>Etki alanı adı Hizmetleri sunucusu 
Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir VNet içinde kendi etki alanı adı Hizmetleri (DNS) sunucusu kullanmanız gerekiyorsa, için yönergeleri izleyin [VNet içindeki Azure SSIS Integration zamanının düğümleri Azure uç noktaları çözümleyebileceğinden emin](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Ağ güvenlik grubu
Ağ güvenlik grubu (NSG), Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir VNet içindeki uygulamanız gerekiyorsa, aşağıdaki bağlantı noktaları gelen/giden traffics izin ver:

| Bağlantı Noktaları | Yön | Aktarım Protokolü | Amaç | Gelen kaynağı/giden hedef |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100<br/>20100<br/>30100  | Gelen | TCP | Azure Hizmetleri VNet içindeki Azure SSIS Integration zamanının düğümleriyle iletişim kurmak için bu bağlantı noktalarını kullanır. | Internet | 
| 443 | Giden | TCP | VNet içindeki Azure SSIS Integration zamanının düğümleri, örneğin, Azure Storage, olay hub'ı, vb. Azure hizmetlerine erişmek için bu bağlantı noktası kullanır. | INTERNET | 
| 1433<br/>11000-11999<br/>14000-14999  | Giden | TCP | VNet içinde Azure SSIS Integration zamanının düğümleri (yönetilen Azure SQL örneği tarafından barındırılan SSISDB için geçerli değildir), Azure SQL veritabanı sunucusu tarafından barındırılan SSISDB erişmek için bu bağlantı noktalarını kullanır. | Internet | 

## <a name="script-to-configure-vnet"></a>VNet yapılandırmak için komut dosyası 
Destekli PowerShell komut dosyasını kullanabilirsiniz [bu makalede](create-azure-ssis-integration-runtime.md) VNet bir Azure SSIS tümleştirmesi çalışma zamanı sağlamak için. Böylece, Azure SSIS tümleştirmesi çalışma zamanı Vnet'e katılabilirsiniz betik VNet izinler ve ayarlar otomatik olarak yapılandırır.  


## <a name="use-portal-to-configure-vnet"></a>VNet yapılandırmak için portalı kullanın
Komut dosyası çalıştırarak, VNet yapılandırmak için kolay bir yoludur. Sahip değilse, VNet / otomatik yapılandırma yapılandırmak için erişim başarısız oluyor, bu sanal ağ sahibi / aşağıdaki adımlarda bunları el ile yapılandırmak deneyebilirsiniz:

### <a name="find-the-resource-id-for-your-azure-vnet"></a>Kaynak kimliği için Azure sanal bulun.
 
1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
2. Tıklatın **daha fazla hizmet**. Filtreleyip seçmek **sanal ağları (Klasik)**.
3. Filtre ve seçin, **sanal ağ** listesinde. 
4. Sanal ağ (Klasik) sayfasında seçin **özellikleri**. 

    ![Klasik VNet kaynak kimliği](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Kopyala düğmesini tıklatın **kaynak kimliği** Klasik ağ için kaynak kimliği panoya kopyalamak için. OneNote veya dosyasında panodan kimliği kaydedin.
6. Tıklatın **alt ağlar** sol menüsünde ve sayısı emin olun **kullanılabilir adresler** Azure SSIS tümleştirmesi Çalışma Zamanı Modülü düğümlerin değerinden daha büyük.

    ![VNet içinde kullanılabilir adreslerin sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Katılma **MicrosoftAzureBatch** için **Klasik sanal makine Katılımcısı** vnet rol. 
    1. Sol menüde erişim denetimi (IAM) tıklatın ve tıklatın **Ekle** araç.
    
        ![Erişim denetimi -> Ekle](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png) 
    2. İçinde **izinleri eklemek** sayfasında, **Klasik sanal makine Katılımcısı** için **rol**. Tür **MicrosoftAzureBatch** içinde **seçin** metin kutusuna ve ardından **MicrosoftAzureBatch** arama sonuçları listesinden. 
    
        ![İzinleri add - arama](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
    3. Ayarları kaydetmek ve sayfayı kapatmak için Kaydet'i tıklatın.
    
        ![Erişim ayarlarını Kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
    4. Gördüğünüzü onaylayın **MicrosoftAzureBatch** katkıda bulunanlar listesinde.
    
        ![AzureBatch erişimi onaylayın](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
5. Bu Azure Batch sağlayıcısı VNet sahip Azure aboneliğinde kayıtlı doğrulamak veya Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil.
    1. Azure portalında tıklatın **abonelikleri** sol menüde. 
    2. Seçin, **abonelik**. 
    3. Tıklatın **kaynak sağlayıcıları** sol taraftaki onaylayın `Microsoft.Batch` kayıtlı bir sağlayıcı. 
    
        ![onay toplu kayıtlı](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Görmüyorsanız, `Microsoft.Batch` , kaydetmek için listede [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz. 
         


## <a name="next-steps"></a>Sonraki adımlar
Azure SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-deploy-ssis-packages-azure.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletip Azure SQL Yönetilen Örneğini (özel önizleme) kullanma ve IR’yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
