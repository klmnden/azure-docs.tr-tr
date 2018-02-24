---
title: "Bir sanal ağa Azure SSIS tümleştirmesi çalışma zamanı katılma | Microsoft Docs"
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
ms.openlocfilehash: 6f0038ea750d05b8d59bde07b54cb4aa22ab71b9
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Bir Azure SSIS tümleştirmesi çalışma zamanı sanal bir ağa katılmasını sağlayın
Azure sanal ağını aşağıdaki senaryolarda, Azure SSIS tümleştirmesi çalışma zamanı (IR) Katıl: 

- SQL Server Integration Services (SSIS) katalog veritabanında Azure SQL veritabanı örneğinde (özel olarak incelenmektedir) yönetilen bir sanal ağ barındırır.
- Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız.

 Azure Data Factory sürüm 2 (Önizleme), Klasik dağıtım modeli veya Azure Resource Manager dağıtım modeli oluşturulan sanal bir ağa, Azure SSIS tümleştirmesi çalışma zamanı katılma olanak tanır. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel kullanılabilirlik (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory sürüm 1 belgelerine](v1/data-factory-introduction.md).

## <a name="access-to-on-premises-data-stores"></a>Şirket içi veri depolarına erişim
SSIS paketleri yalnızca genel bulut veri depolarına erişirseniz, bir sanal ağa Azure SSIS IR katılma gerek yoktur. SSIS paketleri şirket içi veri depolarına erişirse, şirket içi ağı'na bağlı bir sanal ağ Azure SSIS IR katılması gerekir. 

SSIS katalog sanal ağda yer almayan bir Azure SQL veritabanı örneğinde barındırılıyorsa, uygun bağlantı noktalarını açmanız gerekir. 

SSIS katalog sanal bir ağa SQL veritabanı yönetilen örneğinde barındırılıyorsa bir Azure SSIS IR birleştirebilirsiniz:

- Aynı sanal ağı.
- SQL veritabanı yönetilen örneğinin tek bir ağ ağ bağlantısına sahip farklı bir sanal ağ. 

Sanal ağı Klasik dağıtım modeli veya Azure Resource Manager dağıtım modeli dağıtılabilir. Azure SSIS IR katılın varsa planlıyorsanız *aynı sanal ağ* SQL veritabanı yönetilen örneği olan, Azure SSIS IR olduğundan emin olun bir *farklı alt* SQL veritabanının bulunduğu bir Yönetilen örneği.   

Aşağıdaki bölümlerde daha ayrıntılı bilgi sağlar.

Dikkat edilecek bazı önemli noktalar şunlardır: 

- Varsa var olan bir sanal ağı şirket içi ağınıza bağlı, ilk Oluştur bir [Azure Resource Manager sanal ağ](../virtual-network/quick-create-portal.md#create-a-virtual-network) veya [Klasik sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) Azure SSIS tümleştirme katılmak için çalışma zamanı. Ardından, bir site siteye yapılandırın [VPN ağ geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) veya [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bu sanal ağ bağlantısından şirket içi ağınıza.
- Mevcut Azure Resource Manager veya Klasik sanal ağa, Azure SSIS IR ile aynı konumda şirket içi ağınıza bağlı ise, bu sanal ağa IR birleştirebilirsiniz.
- Mevcut Klasik sanal ağda, Azure SSIS IR farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Klasik sanal ağı](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS IR için. Ardından, yapılandırma bir [Klasik Klasik sanal ağda](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) bağlantı. Veya oluşturabileceğiniz bir [Azure Resource Manager sanal ağ](../virtual-network/quick-create-portal.md#create-a-virtual-network) katılmak, Azure SSIS tümleştirmesi çalışma zamanı için. Daha sonra yapılandırma bir [Klasik Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.
- Mevcut Azure Resource Manager sanal ağ, Azure SSIS IR farklı bir konumda şirket içi ağınıza bağlı ise, ilk oluşturabileceğiniz bir [Azure Resource Manager sanal ağ](../virtual-network/quick-create-portal.md##create-a-virtual-network) Azure-SSIS için Katılmak için IR. Ardından, bir Azure Kaynak Yöneticisi Azure Resource Manager sanal ağ bağlantısını yapılandırın. Veya oluşturabileceğiniz bir [Klasik sanal ağı](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) katılmak, Azure SSIS IR için. Ardından, yapılandırma bir [Klasik Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantı.

## <a name="domain-name-services-server"></a>Etki alanı adı Hizmetleri sunucusu 
Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir sanal ağ içinde kendi etki alanı adı Hizmetleri (DNS) sunucusu kullanmanız gerekiyorsa, için yönergeleri izleyin [Azure sanal ağında Azure SSIS Integration zamanının düğümleri çözümleyebildiğinden emin olun uç noktaları](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Ağ güvenlik grubu
Gelen/giden trafik bir ağ güvenlik grubu (NSG) uygulamak, Azure SSIS tümleştirmesi çalışma zamanı tarafından birleştirilmiş bir sanal ağdaki gerekiyorsa, aşağıdaki bağlantı noktaları izin ver:

| Bağlantı Noktaları | Yön | Aktarım Protokolü | Amaç | Gelen kaynağı/giden hedef |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100, 20100, (bir Klasik sanal ağı'de IR katılırsanız) 30100<br/><br/>29876, (bir Azure Resource Manager sanal ağı'de IR katılırsanız) 29877 | Gelen | TCP | Azure Hizmetleri, sanal ağda Azure SSIS Integration zamanının düğümleriyle iletişim kurmak için bu bağlantı noktalarını kullanır. | Internet | 
| 443 | Giden | TCP | Sanal ağda Azure SSIS Integration zamanının düğümleri, Azure Storage ve Azure Event Hubs gibi Azure hizmetlerine erişmek için bu bağlantı noktasını kullanır. | Internet | 
| 1433<br/>11000-11999<br/>14000-14999  | Giden | TCP | (Bu amaçla yönetilen SQL veritabanı örneği tarafından barındırılan SSISDB için geçerli değildir.), Azure SQL veritabanı sunucusu tarafından barındırılan SSISDB erişmek için bu bağlantı noktaları sanal ağda Azure SSIS Integration zamanının düğümleri kullanın | Internet | 

## <a name="azure-portal-data-factory-ui"></a>Azure portal (Data Factory UI)
Bu bölümde Azure portalı ve veri fabrikası UI kullanarak bir sanal ağ (Klasik veya Azure Resource Manager) var olan bir Azure SSIS çalışma zamanı katılma gösterilmiştir. İlk olarak, Azure SSIS IR katılmadan önce sanal ağ uygun şekilde yapılandırmanız gerekir. Sanal ağ (Klasik veya Azure Resource Manager) türüne göre sonraki iki bölümde biri aracılığıyla gidin. Ardından, sanal ağa, Azure SSIS IR katılmaya üçüncü bölümüyle devam edin. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Klasik sanal ağı yapılandırmak üzere portalı kullanın
Bir sanal ağ için bir Azure SSIS IR katılabilmesi için önce yapılandırmanız gerekir.

1. Microsoft Edge veya Google Chrome başlatın. Şu anda, veri fabrikası UI yalnızca bu web tarayıcılarda desteklenir.
2. [Azure Portal](https://portal.azure.com) oturum açın.
3. Seçin **daha fazla hizmet**. Filtreleyip seçmek **sanal ağları (Klasik)**.
4. Filtre ve sanal ağınız listeden seçin. 
5. Üzerinde **sanal ağ (Klasik)** sayfasında, **özellikleri**. 

    ![Klasik sanal ağ kaynak kimliği](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Kopyala düğmesini seçin **kaynak kimliği** Klasik ağ için kaynak kimliği panoya kopyalamak için. OneNote veya dosyasında panodan kimliği kaydedin.
6. Seçin **alt ağlar** sol menüde. Sayısı emin **kullanılabilir adresler** Azure SSIS tümleştirmesi Çalışma Zamanı Modülü düğümlerin değerinden daha büyük.

    ![Sanal ağda kullanılabilir adreslerin sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Katılma **MicrosoftAzureBatch** için **Klasik sanal makine Katılımcısı** rolü sanal ağ için.

    a. Seçin **erişim denetimi (IAM)** soldaki menüden ve seçim **Ekle** araç çubuğunda. 
       !["Erişim denetimi" ve "düğme ekleme"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Üzerinde **izinleri eklemek** sayfasında, **Klasik sanal makine Katılımcısı** için **rol**. Yapıştır **ddbf3205-c6bd-46ae-8127-60eb93363864** içinde **seçin** kutusuna ve ardından **Microsoft Azure toplu işlem** arama sonuçları listesinden.   
       !["İzinleri Ekle" sayfasında arama sonuçları](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Seçin **kaydetmek** ayarlarını kaydetmek için ve sayfasını kapatın.  
       ![Erişim ayarlarını Kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Gördüğünüzü onaylayın **Microsoft Azure toplu işlem** katkıda bulunanlar listesinde.  
       ![Azure Batch erişimi onaylayın](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

5. Azure Batch sağlayıcısı sanal ağ olan Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya, Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil.

   a. Azure portalında seçin **abonelikleri** sol menüde.

   b. Aboneliğinizi seçin.

   c. Seçin **kaynak sağlayıcıları** sol taraftaki onaylayın **Microsoft.Batch** kayıtlı bir sağlayıcı.     
      !["Kaydedildi" durumunu onaylama](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Görmüyorsanız, **Microsoft.Batch** , kaydedilecek listesinde [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Bir Azure Resource Manager sanal ağı yapılandırmak üzere portalı kullanın
Bir sanal ağ için bir Azure SSIS IR katılabilmesi için önce yapılandırmanız gerekir.

1. Microsoft Edge veya Google Chrome başlatın. Şu anda, veri fabrikası UI yalnızca bu web tarayıcılarda desteklenir.
2. [Azure Portal](https://portal.azure.com) oturum açın.
3. Seçin **daha fazla hizmet**. Filtreleyip seçmek **sanal ağlar**.
4. Filtre ve sanal ağınız listeden seçin. 
5. Üzerinde **sanal ağ** sayfasında, **özellikleri**. 
6. Kopyala düğmesini seçin **kaynak kimliği** sanal ağ için kaynak kimliği panoya kopyalamak için. OneNote veya dosyasında panodan kimliği kaydedin.
7. Seçin **alt ağlar** sol menüde. Sayısı emin **kullanılabilir adresler** Azure SSIS tümleştirmesi Çalışma Zamanı Modülü düğümlerin değerinden daha büyük.
8. Azure Batch sağlayıcısı sanal ağ olan Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya, Azure Batch sağlayıcıyı kaydettirin. Bir Azure Batch hesabı aboneliğinizde zaten varsa, aboneliğiniz için Azure Batch kayıtlı değil.

   a. Azure portalında seçin **abonelikleri** sol menüde.

   b. Aboneliğinizi seçin. 
   
   c. Seçin **kaynak sağlayıcıları** sol taraftaki onaylayın **Microsoft.Batch** kayıtlı bir sağlayıcı.     
      !["Kaydedildi" durumunu onaylama](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Görmüyorsanız, **Microsoft.Batch** , kaydedilecek listesinde [boş bir Azure Batch hesabı oluşturma](../batch/batch-account-create-portal.md) aboneliğinizde. Daha sonra silebilirsiniz.

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Bir sanal ağa Azure SSIS IR katılma


1. Microsoft Edge veya Google Chrome başlatın. Şu anda, veri fabrikası UI yalnızca bu web tarayıcılarda desteklenir.
2. İçinde [Azure portal](https://portal.azure.com)seçin **veri fabrikaları** sol menüde. Görmüyorsanız, **veri fabrikaları** menüsünde seçin **daha fazla hizmet**seçip **veri fabrikaları** içinde **INTELLİGENCE + ANALİZ**bölümü. 
    
   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)
2. Data factory'nizi Azure SSIS tümleştirmesi çalışma zamanı listeden seçin. Veri fabrikanızın giriş sayfasına bakın. Seçin **Yazar & Dağıt** döşeme. Veri Fabrikası UI ayrı bir sekmede bakın. 

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)
3. Veri Fabrikası UI geçmek **Düzenle** sekmesine **bağlantıları**ve geçiş **tümleştirme çalışma zamanları** sekmesi. 

   !["Tümleştirme çalışma zamanları" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)
4. Azure SSIS IR çalışıyorsa tümleştirmesi çalışma zamanı listesinde seçin **durdurmak** düğmesini **Eylemler** Azure SSIS IR sütun Siz durduruncaya kadar bir IR düzenleyemezsiniz. 

   ![IR Durdur](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)
1. Tümleştirme çalışma zamanı listeden seçin **Düzenle** düğmesini **Eylemler** Azure SSIS IR sütun

   ![Tümleştirme çalışma zamanı Düzenle](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)
5. Üzerinde **genel ayarları** sayfasında **tümleştirmesi çalışma zamanı Kurulum** penceresinde, seçin **sonraki**. 

   ![IR kurulumu için genel ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)
6. Üzerinde **SQL ayarlarını** sayfasında, yönetici parolasını girin ve seçin **sonraki**.

   ![IR kurulumu için SQL Server ayarları](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)
7. Üzerinde **Gelişmiş ayarları** sayfasında, aşağıdaki eylemleri gerçekleştirebilirsiniz: 

   a. Onay kutusunu seçin **VNet izinleri/ayarlarını yapılandırmak için bir VNet için Azure SSIS tümleştirme katılmak ve Azure Hizmetleri izin vermek çalışma zamanı modülü seçin**.

   b. İçin **türü**, sanal ağı Klasik sanal ağ veya bir Azure Resource Manager sanal ağ olup olmadığını belirtin. 

   c. İçin **VNet adı**, sanal ağınızı seçin.

   d. İçin **alt ağ adı**, sanal ağ, alt ağ seçin.

   e. Seçin **güncelleştirme**. 

   ![IR kurulumu için Gelişmiş ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)
8. Kullanarak IR şimdi başlatabilirsiniz **Başlat** düğmesini **Eylemler** Azure SSIS IR sütun Bir Azure SSIS IR başlatmak için yaklaşık 20 dakika sürer 


## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma
Bir sanal ağ için bir Azure SSIS IR katılabilmesi için önce yapılandırmanız gerekir. Otomatik olarak sanal ağına bağlanmak, Azure SSIS tümleştirmesi çalışma zamanı için sanal ağ izinlerini/ayarlarını yapılandırmak için aşağıdaki betiği ekleyin:

```powershell
# Register to the Azure Batch resource provider
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
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Bir Azure SSIS IR oluşturmak ve bir sanal ağa katılma
Bir Azure SSIS IR oluşturabilir ve aynı anda bir sanal ağa katılın. Tam komut dosyası ve yönergeler için bkz: [Azure SSIS tümleştirmesi çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Var olan bir Azure SSIS IR sanal bir ağa katılmasını sağlayın
Komut dosyasında [Azure SSIS tümleştirmesi çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md) makale bir Azure SSIS IR oluşturmak ve bir sanal ağ aynı komut eklemek nasıl gösterir. Var olan bir Azure SSIS IR varsa, sanal ağa eklemek için aşağıdaki adımları gerçekleştirin: 

1. Azure SSIS IR Durdur
2. Azure SSIS IR sanal ağına bağlanmak için yapılandırın. 
3. Azure SSIS IR Başlat 

### <a name="define-the-variables"></a>Değişkenleri tanımlayın

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a virtual network and Azure SQL Database Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network.
$VnetId = "<Name of your Azure virtual network>"
$SubnetName = "<Name of the subnet in the virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Azure SSIS IR Durdur
Bir sanal ağa katılabilmesi için önce Azure SSIS tümleştirmesi çalışma zamanı durdurun. Bu komut tüm düğümlerinin serbest bırakır ve faturalama durdurur:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Katılmak Azure SSIS IR için sanal ağ ayarlarını yapılandırma
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
Sanal ağına bağlanmak için Azure SSIS tümleştirmesi çalışma zamanı yapılandırmak için çalıştırın `Set-AzureRmDataFactoryV2IntegrationRuntime` komutu: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Azure SSIS IR Başlat
Azure SSIS tümleştirmesi çalışma zamanı başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Bu komutun tamamlanması için 20-30 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
Azure SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR dahil olmak üzere tümleştirme çalışma zamanları hakkında kavramsal bilgiler genel olarak, sağlar. 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure SSIS IR oluşturmak için adım adım yönergeler sağlar SSIS katalog barındırmak için Azure SQL veritabanını kullanır. 
- [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makalede öğreticiyi genişletir ve Azure SQL veritabanı yönetilen örneği (özel olarak incelenmektedir) kullanarak ve bir sanal ağa IR birleştirme yönergeler sağlar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, düğüm ekleyerek, Azure SSIS IR ölçeklendirmek nasıl gösterir. 
