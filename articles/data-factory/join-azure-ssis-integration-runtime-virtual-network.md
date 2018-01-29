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
ms.date: 01/22/2018
ms.author: spelluru
ms.openlocfilehash: 917c3e23fed468a04783456e7dc74a42bea60ae7
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Bir Azure SSIS tümleştirmesi çalışma zamanı sanal bir ağa katılmasını sağlayın
Aşağıdaki koşullardan biri doğruysa bir Azure sanal ağı (VNet) için Azure SSIS tümleştirmesi çalışma zamanı (IR) eklemeniz gerekir: 

- SSIS Katalog veritabanını bir sanal ağın parçası olan SQL Server Yönetilen Örneği (özel önizleme) üzerinde barındırıyorsanız.
- Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız.

 Azure Data Factory sürüm 2 (Önizleme), Azure SSIS tümleştirmesi çalışma zamanı Klasik veya bir Azure Resource Manager Vnet'i katılma olanak sağlar. 

 > [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md).

## <a name="access-on-premises-data-stores"></a>Erişim içi veri depoları
SSIS paketleri yalnızca genel bulut veri depolarına erişirseniz, bir sanal ağa Azure SSIS IR katılma gerek yoktur. SSIS paketleri şirket içi veri depolarına erişirse, şirket içi ağı'na bağlı bir sanal ağa Azure SSIS IR katılması gerekir. SSIS katalog Azure SQL veritabanında, VNet içinde değil barındırılıyorsa, uygun bağlantı noktalarını açmanız gerekir. SSIS katalog Azure SQL yönetilen bir Azure Resource Manager Vnet'i ya da Klasik VNet örneğinde barındırılıyorsa, aynı Vnet'i (veya) yönetilen Azure SQL örneğinin tek bir VNet-VNet bağlantısı olan farklı bir VNet Azure SSIS IR birleştirebilirsiniz. Aşağıdaki bölümlerde daha ayrıntılı bilgi sağlar.

Dikkat edilecek bazı önemli noktalar şunlardır: 

- Varsa mevcut bir VNet şirket içi ağınıza bağlı, ilk oluşturun bir [Azure Resource Manager Vnet'i](../virtual-network/virtual-network-get-started-vnet-subnet.md#create-vnet) veya [Klasik VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Ardından, bir site siteye yapılandırın [VPN ağ geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)/[ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bu sanal ağ bağlantısından şirket içi ağınıza.
- Var olan bir Azure Resource Manager Vnet'i ya da Klasik VNet Azure SSIS tümleştirmesi çalışma zamanı modülü ile aynı konumda şirket içi ağınıza bağlı ise, Azure SSIS tümleştirmesi çalışma zamanı birleştirebilirsiniz.
- Varolan bir Klasik VNet Azure SSIS tümleştirme çalışma zamanını şuradan farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Klasik VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) , katılmak Azure SSIS tümleştirmesi çalışma zamanı için. Ardından, yapılandırma bir [Klasik Klasik VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) bağlantı. Veya oluşturabileceğiniz bir [Azure Resource Manager Vnet'i](../virtual-network/virtual-network-get-started-vnet-subnet.md#create-vnet) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Daha sonra yapılandırma bir [Klasik Azure Resource Manager Vnet'i](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.
- Varsa mevcut bir Azure Resource Manager Vnet'i Azure SSIS tümleştirme çalışma zamanını şuradan farklı bir konumda şirket içi ağınıza bağlı, ilk oluşturabileceğiniz bir [Azure Resource Manager Vnet'i](../virtual-network/virtual-network-get-started-vnet-subnet.md#create-vnet) Azure-SSIS için katılmak için tümleştirme çalışma zamanı. Ardından, bir Azure Kaynak Yöneticisi Azure Resource Manager Vnet'i bağlantısını yapılandırın. Veya oluşturabileceğiniz bir [Klasik VNet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Ardından, yapılandırma bir [Klasik Azure Resource Manager Vnet'i](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.

## <a name="domain-name-services-server"></a>Etki alanı adı Hizmetleri sunucusu 
Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir VNet içinde kendi etki alanı adı Hizmetleri (DNS) sunucusu kullanmanız gerekiyorsa, için yönergeleri izleyin [VNet içindeki Azure SSIS Integration zamanının düğümleri Azure uç noktaları çözümleyebileceğinden emin](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Ağ Güvenlik Grubu
Ağ güvenlik grubu (NSG), Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir VNet içindeki uygulamanız gerekiyorsa, aşağıdaki bağlantı noktaları gelen/giden traffics izin ver:

| Bağlantı Noktaları | Yön | Aktarım Protokolü | Amaç | Gelen kaynağı/giden hedef |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100<br/>20100<br/>30100  | Gelen | TCP | Azure Hizmetleri VNet içindeki Azure SSIS Integration zamanının düğümleriyle iletişim kurmak için bu bağlantı noktalarını kullanır. | Internet | 
| 443 | Giden | TCP | VNet içindeki Azure SSIS Integration zamanının düğümleri, örneğin, Azure Storage, olay hub'ı, vb. Azure hizmetlerine erişmek için bu bağlantı noktası kullanır. | INTERNET | 
| 1433<br/>11000-11999<br/>14000-14999  | Giden | TCP | VNet içinde Azure SSIS Integration zamanının düğümleri (yönetilen Azure SQL örneği tarafından barındırılan SSISDB için geçerli değildir), Azure SQL veritabanı sunucusu tarafından barındırılan SSISDB erişmek için bu bağlantı noktalarını kullanır. | Internet | 

## <a name="configure-vnet"></a>Sanal ağ yapılandırma
İlk (komut dosyası vs. aşağıdaki yöntemlerden birini kullanarak VNet yapılandırmanız gerekiyor Azure portalı) Vnet'e Azure SSIS IR katılabilmesi için önce. 

### <a name="script-to-configure-vnet"></a>VNet yapılandırmak için komut dosyası 
Otomatik olarak VNet katılmak, Azure SSIS tümleştirmesi çalışma zamanı VNet izinleri/ayarlarını yapılandırmak için aşağıdaki betiği ekleyin.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="use-portal-to-configure-a-classic-vnet"></a>Klasik bir VNet yapılandırmak için portalı kullanın
Komut dosyası çalıştırarak, VNet yapılandırmak için kolay bir yoludur. Sahip değilse, VNet / otomatik yapılandırma yapılandırmak için erişim başarısız oluyor, bu sanal ağ sahibi / aşağıdaki adımlarda bunları el ile yapılandırmak deneyebilirsiniz:

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
    2. İçinde **izinleri eklemek** sayfasında, **Klasik sanal makine Katılımcısı** için **rol**. Kopyala/Yapıştır **ddbf3205-c6bd-46ae-8127-60eb93363864** içinde **seçin** metin kutusuna ve ardından **Microsoft Azure toplu işlem** arama sonuçları listesinden. 
    
        ![İzinleri add - arama](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
    3. Ayarları kaydetmek ve sayfayı kapatmak için Kaydet'i tıklatın.
    
        ![Erişim ayarlarını Kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
    4. Gördüğünüzü onaylayın **MicrosoftAzureBatch** katkıda bulunanlar listesinde.
    
        ![AzureBatch erişimi onaylayın](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
5. Bu Azure Batch sağlayıcısı VNet sahip Azure aboneliğinde kayıtlı doğrulamak veya Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil.
    1. Azure portalında tıklatın **abonelikleri** sol menüde. 
    2. Seçin, **abonelik**. 
    3. Tıklatın **kaynak sağlayıcıları** sol taraftaki onaylayın `Microsoft.Batch` kayıtlı bir sağlayıcı. 
    
        ![confirmation-batch-registered](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Görmüyorsanız, `Microsoft.Batch` , kaydetmek için listede [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz. 

### <a name="use-portal-to-configure-an-azure-resource-manager-vnet"></a>Bir Azure Resource Manager Vnet'i yapılandırmak için portalı kullanın
Komut dosyası çalıştırarak, VNet yapılandırmak için kolay bir yoludur. Sahip değilse, VNet / otomatik yapılandırma yapılandırmak için erişim başarısız oluyor, bu sanal ağ sahibi / aşağıdaki adımlarda bunları el ile yapılandırmak deneyebilirsiniz:

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
2. Tıklatın **daha fazla hizmet**. Filtreleyip seçmek **sanal ağlar**.
3. Filtre ve seçin, **sanal ağ** listesinde. 
4. Sanal ağ sayfasında seçin **özellikleri**. 
5. Kopyala düğmesini tıklatın **kaynak kimliği** sanal ağ için kaynak kimliği panoya kopyalamak için. OneNote veya dosyasında panodan kimliği kaydedin.
6. Tıklatın **alt ağlar** sol menüsünde ve sayısı emin olun **kullanılabilir adresler** Azure SSIS tümleştirmesi Çalışma Zamanı Modülü düğümlerin değerinden daha büyük.
5. Bu Azure Batch sağlayıcısı VNet sahip Azure aboneliğinde kayıtlı doğrulamak veya Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil.
    1. Azure portalında tıklatın **abonelikleri** sol menüde. 
    2. Seçin, **abonelik**. 
    3. Tıklatın **kaynak sağlayıcıları** sol taraftaki onaylayın `Microsoft.Batch` kayıtlı bir sağlayıcı. 
    
        ![confirmation-batch-registered](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Görmüyorsanız, `Microsoft.Batch` , kaydetmek için listede [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz.

## <a name="create-an-azure-ssis-ir-and-join-it-to-a-vnet"></a>Bir Azure SSIS IR oluşturmak ve bir sanal ağa katılma
Bir Azure SSIS IR oluşturabilir ve aynı anda Vnet'e katılın. Tam komut dosyası ve bir Azure SSIS IR oluşturmak ve aynı anda bir sanal ağa eklemek için yönergeler için bkz: [oluşturma Azure SSIS IR](create-azure-ssis-integration-runtime.md).

## <a name="join-an-existing-azure-ssis-ir-to-a-vnet"></a>Bir sanal ağa var olan bir Azure SSIS IR katılma
Komut dosyasında [oluşturma Azure SSIS tümleştirmesi çalışma zamanı](create-azure-ssis-integration-runtime.md) makale bir Azure SSIS IR oluşturmak ve sanal ağ aynı komut içinde alanına nasıl gösterir. Var olan bir Azure SSIS varsa, Vnet'e eklemek için aşağıdaki adımları gerçekleştirin. 

1. Azure SSIS IR Durdur
2. VNet katılmak için Azure SSIS IR yapılandırın. 
3. Azure SSIS IR Başlat 

## <a name="define-the-variables"></a>Değişkenleri tanımlayın

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a VNet and an Azure SQL Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network (VNet).
$VnetId = "<Name of your Azure virtual netowrk>"
$SubnetName = "<Name of the subnet in VNet>"
```

### <a name="stop-the-azure-ssis-ir"></a>Azure SSIS IR Durdur
Bir sanal ağa katılabilmesi için önce Azure SSIS tümleştirmesi çalışma zamanı durdurun. Bu komut tüm düğümlerinin serbest bırakır ve faturalama durdurur.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-vnet-settings-for-the-azure-ssis-ir-to-join"></a>Katılmak Azure SSIS IR VNet ayarlarını yapılandırın
Azure Batch kaynak sağlayıcısı için kaydolun:

```powershell
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Azure SSIS IR yapılandırın
VNet katılmak için Azure SSIS tümleştirmesi çalışma zamanı yapılandırmak için Set-AzureRmDataFactoryV2IntegrationRuntime komutu çalıştırın: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

## <a name="start-the-azure-ssis-ir"></a>Azure SSIS IR Başlat
Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Bu komutun tamamlanması **20-30 dakika** sürer.

## <a name="next-steps"></a>Sonraki adımlar
Azure SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-deploy-ssis-packages-azure.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale, öğreticiyi genişletip Azure SQL Yönetilen Örneğini (özel önizleme) kullanma ve IR’yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
