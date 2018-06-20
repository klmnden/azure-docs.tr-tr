---
title: Azure dosya eşitleme (Önizleme) dağıtma | Microsoft Docs
description: Başlangıçtan bitişe kadar Azure dosya eşitleme dağıtmayı öğrenin.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: wgries
ms.openlocfilehash: f1230cbc4d654bfb59bb328ed7d75c6fa76ff10c
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268251"
---
# <a name="deploy-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) dağıtma
Esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun tanırken kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmek için Azure dosya eşitleme (Önizleme) kullanın. Azure dosya eşitleme, Windows Server Hızlı Azure dosya paylaşımınıza önbelleğine dönüştürür. SMB ve NFS FTPS çeşitli verilerinize yerel olarak erişmek için Windows Server üzerinde kullanılabilir herhangi bir protokolünü kullanabilirsiniz. Dünya genelinde gerektiği kadar önbellekleri olabilir.

Okumanızı öneririz [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) ve [bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md) önce bu makalede açıklanan adımları tamamlayın.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure storage hesabını ve Azure dosya Azure dosya eşitleme dağıtmak istediğiniz aynı bölgede paylaşır. Daha fazla bilgi için bkz.
    - [Bölge kullanılabilirliği](storage-sync-files-planning.md#region-availability) Azure dosya eşitleme için.
    - [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) bir depolama hesabının nasıl oluşturulacağı hakkında adım adım açıklaması.
    - [Bir dosya paylaşımı oluşturmak](storage-how-to-create-file-share.md) nasıl bir dosya paylaşımı oluşturmak adım adım açıklaması.
* Windows Server veya Windows Server kümesi Azure dosya eşitleme ile eşitlemek için en az bir desteklenen örneği. Windows Server'ın desteklenen sürümleri hakkında daha fazla bilgi için bkz: [Windows Server ile birlikte çalışabilirlik](storage-sync-files-planning.md#azure-file-sync-interoperability).
* PowerShell 5.1, Windows Server'da yüklü emin olun. Windows Server 2012 R2 kullanıyorsanız, en az çalıştığından emin olun PowerShell 5.1. \*. PowerShell 5.1 varsayılan sürüm out-of-box olduğu gibi güvenli bir şekilde bu onay Windows Server 2016 atlayabilirsiniz. Windows Server 2012 R2'de PowerShell 5.1 çalıştığını doğrulayabilirsiniz. \* değeri, bakarak **PSVersion** özelliği **$PSVersionTable** nesnesi:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    PSVersion değeriniz değerinden 5.1 ise. \*olarak olacaktır Windows Server 2012 R2'in en yeni yüklemeler çalışmasıyla, indirme ve yükleme kolayca yükseltebilirsiniz [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). İndirmek ve Windows Server 2012 R2 için yüklemek için uygun paket **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

    > [!Note]  
    > Azure dosya eşitleme henüz PowerShell 6 + Windows Server 2012 R2 veya Windows Server 2016 desteklemiyor.
* Azure dosya eşitleme ile kullanmak istediğiniz sunucularda AzureRM PowerShell modülü. AzureRM PowerShell modülleri yükleme hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Her zaman Azure PowerShell modüllerinin en son sürümünü kullanmanızı öneririz. 

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Windows Server'ın Azure dosya eşitleme ile kullanmak için hazırlama
Her sunucu düğümünde bir yük devretme kümesinde de dahil olmak üzere Azure dosya eşitleme ile kullanmak istediğiniz her sunucu için devre dışı **Internet Explorer Artırılmış Güvenlik Yapılandırması**. Bu, yalnızca ilk sunucu kayıt için gereklidir. Sunucu kaydedildikten sonra yeniden etkinleştirebilirsiniz.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Sunucu Yöneticisi'ni açın.
2. Tıklatın **yerel sunucu**:  
    ![Sunucu Yöneticisi kullanıcı Arabirimi sol tarafındaki "yerel sunucu"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. Üzerinde **özellikleri** subpane, bağlantısını seçin **IE Artırılmış Güvenlik Yapılandırması**.  
    ![Sunucu Yöneticisi kullanıcı Arabirimi "IE Artırılmış Güvenlik Yapılandırması" bölmesinde](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. İçinde **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda **kapalı** için **Yöneticiler** ve **kullanıcılar**:  
    !["Kapalı" ile Internet Explorer Artırılmış Güvenlik Yapılandırması pop-penceresinde seçili](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Internet Explorer Artırılmış Güvenlik Yapılandırması devre dışı bırakmak için yükseltilmiş bir PowerShell oturumunda aşağıdakileri yürütün:

```PowerShell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="install-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı'nı yükleme
Azure dosya eşitleme Aracısı'nı Windows Server'ın bir Azure dosya paylaşımı ile senkronize sağlayan indirilebilir bir pakettir. 

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Aracısı'ndan yükleyebilirsiniz [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Yükleme tamamlandığında, Azure dosya eşitleme aracı yüklemesini başlatmak için MSI paketini çift tıklatın.

> [!Important]  
> Azure dosya eşitleme ile bir yük devretme kümesi kullanmayı planlıyorsanız, kümedeki her düğümde Azure dosya eşitleme Aracısı yüklenmesi gerekir.

Şunları yapmanız önerilir:
- Sorun giderme ve sunucu bakımının basitleştirmek için varsayılan yükleme yolunu (C:\Program Files\Azure\StorageSyncAgent) bırakın.
- Azure dosya eşitleme güncel tutmak için Microsoft Update'e etkinleştirin. Tüm güncelleştirmeleri özelliği güncelleştirmeler ve düzeltmeler de dahil olmak üzere Azure dosya eşitleme aracısı için Microsoft Update sitesinden oluşur. Azure dosya eşitleme için en son güncelleştirmeyi yüklemenizi öneririz. Daha fazla bilgi için bkz: [Azure dosya eşitleme güncelleştirme ilkesi](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Azure dosya eşitleme aracı yüklemesi tamamlandığında, sunucu kayıt UI otomatik olarak açılır. Bir depolama eşitleme hizmeti registrating önce olması gerekir; sonraki bölümde depolama eşitleme hizmeti oluşturma konusunda bakın.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Azure dosya eşitleme aracı, işletim sistemi için uygun sürümünü indirin ve sisteminize yüklemek için aşağıdaki PowerShell kodu yürütün.

```PowerShell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875004 `
        -OutFile "StorageSyncAgent.exe" 
}
elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875002 `
        -OutFile "StorageSyncAgent.exe" 
}
else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2 and Windows Server 2016")
}

# Extract the MSI from the install package
$tempFolder = New-Item -Path "afstemp" -ItemType Directory
Start-Process -FilePath ".\StorageSyncAgent.exe" -ArgumentList "/C /T:$tempFolder" -Wait

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "$($tempFolder.FullName)\StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.exe", ".\afstemp" -Recurse -Force
```

---

## <a name="deploy-the-storage-sync-service"></a>Depolama eşitleme hizmeti dağıtma 
Azure dosya eşitleme dağıtımını yerleştirme ile başlayan bir **depolama eşitleme hizmeti** kaynak bir kaynak grubuna seçili aboneliğinizin. Az sayıda bunların gerektiğinde sağlama öneririz. Sunucularınız ve bu kaynak arasında bir güven ilişkisi oluşturur ve bir sunucu yalnızca bir depolama eşitleme hizmeti için kaydedilebilir. Sonuç olarak, sunucu grupları ayırmak gerektiği kadar depolama Eşitleme Hizmetleri dağıtmak için önerilir. Farklı depolama Eşitleme Hizmetleri sunucularından birbirleri ile eşitleme yapamıyor aklınızda bulundurun.

> [!Note]
> Depolama eşitleme hizmeti erişim izinleri abonelik ve kaynak grubu içine dağıtıldı kaynağından devralındı. Kimlerin erişimi olduğunu dikkatle denetlemenizi öneririz. Yazma erişimine sahip varlıklar için kayıtlı sunucuları dosyalarından için yeni kümeler eşitleniyor başlayabileceğini bu depolama eşitleme hizmeti ve veri kendileri için erişilebilir Azure depolama akışını neden olabilir.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Bir depolama eşitleme hizmeti dağıtmak için şu adrese gidin [Azure portal](https://portal.azure.com/), tıklatın *yeni* ve Azure dosya eşitleme için arama yapın. Arama sonuçlarında seçin **Azure dosya eşitleme (Önizleme)** ve ardından **oluşturma** açmak için **dağıtmak depolama eşitleme** sekmesi.

Açılan bölmesinde, aşağıdaki bilgileri girin:

- **Ad**: depolama eşitleme hizmeti için benzersiz bir ad (her abonelik).
- **Abonelik**: depolama eşitleme hizmeti oluşturmak istediğiniz aboneliği. Kuruluşunuzun yapılandırma stratejisi bağlı olarak, bir veya daha fazla abonelik erişiminiz olması. Bir Azure aboneliği (örneğin, Azure dosyaları) her bir bulut hizmeti için fatura için en temel kapsayıcıdır.
- **Kaynak grubu**: bir kaynak grubu bir depolama hesabı veya bir depolama eşitleme hizmeti gibi Azure kaynakları mantıksal grubudur. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu Azure dosya eşitleme için kullanabilirsiniz. (Kaynak grupları kapsayıcılar olarak HR kaynakları veya belirli bir proje için kaynakları gruplandırma gibi mantıksal olarak, kuruluşunuz için kaynaklar yalıtmak için kullanmanızı öneririz.)
- **Konum**: Azure dosya eşitleme dağıtmak istediğiniz bölgesini. Desteklenen bölgeleri bu listede yalnızca kullanılabilir.

İşiniz bittiğinde, seçin **oluşturma** depolama eşitleme hizmeti dağıtmak için.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Azure dosya eşitleme yönetim Cmdlet'lerine etkileşim önce DLL içeri ve bir Azure dosya eşitleme yönetim bağlamı oluşturmak gerekir. Azure dosya eşitleme Yönetimi cmdlet'leri henüz AzureRM PowerShell modülünün bir parçası olduğundan, bu gereklidir.

> [!Note]  
> Azure dosya eşitleme yönetim cmdlet'leri içeren, StorageSync.Management.PowerShell.Cmdlets.dll paket (özellikle) onaylanmamış bir fiil bir cmdlet içerir (`Login`). Adı `Login-AzureRmStorageSync` eşleşecek şekilde seçilmiştir `Login-AzureRmAccount` AzureRM PowerShell modülü cmdlet diğer adı. Bu hata iletisi (ve cmdlet) kaldırılacak Azure dosya eşitleme Aracısı AzureRM PowerShell modülü eklenir.

```PowerShell
$acctInfo = Login-AzureRmAccount

# The location of the Azure File Sync Agent. If you have installed the Azure File Sync 
# agent to a non-standard location, please update this path.
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzureRmResourceGroup -Name $resourceGroup -Location $region
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region
```

Azure dosya eşitleme bağlamla oluşturduktan sonra `Login-AzureRmStorageSync` cmdlet, depolama eşitleme hizmeti oluşturabilirsiniz. Değiştirdiğinizden emin olun `<my-storage-sync-service>` depolama eşitleme hizmeti istenen adı.

```PowerShell
$storageSyncName = "<my-storage-sync-service>"
New-AzureRmStorageSyncService -StorageSyncServiceName $storageSyncName
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server depolama eşitleme hizmetine kaydetme
Windows sunucunuzu bir depolama eşitleme hizmeti ile kaydetme sunucunuz (veya küme) arasında bir güven ilişkisi oluşturur ve depolama eşitleme hizmeti. Bir sunucu yalnızca bir depolama eşitleme hizmetine kayıtlı ve diğer sunuculara ve Azure ile eşitleyebilir dosya paylaşımları aynı depolama eşitleme hizmeti ile ilişkilendirilmiş.

> [!Note]
> Sunucu kaydı depolama eşitleme hizmeti, Windows sunucu oluşturur ve sunucunun kayıtlı kalır sürece, geçerli olan kendi kimliğini kullanır ancak sonradan sunucusu arasında bir güven ilişkisi oluşturmak için Azure kimlik bilgilerinizi kullanır ve Geçerli paylaşılan erişim imzası (depolama SAS) belirteci geçerli değil. Sunucu, böylece tüm eşitleme durdurma, Azure dosya paylaşımlarına erişmek için sunucu özelliği kaldırma, kaydı olduğunda yeni bir SAS belirteci sunucuya yayınlanamıyor.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Sunucu kayıt UI Azure dosya eşitleme Aracısı yüklendikten sonra otomatik olarak açmanız gerekir. Seçili değilse, onu el ile dosya konumundan açabilirsiniz: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Sunucu kayıt UI açıldığında seçin **oturum açma** başlamak için.

Oturum açtıktan sonra aşağıdaki bilgileri girmeniz istenir:

![Sunucu kayıt kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure aboneliği**: depolama eşitleme hizmeti içeren abonelik (bkz [depolama eşitleme hizmeti dağıtmak](#deploy-the-storage-sync-service)). 
- **Kaynak grubu**: depolama eşitleme hizmeti içeren kaynak grubu.
- **Depolama eşitleme hizmeti**: depolama eşitleme istediğiniz kaydetmek hizmetin adı.

Uygun bilgileri seçtikten sonra Seç **kaydetmek** sunucu kayıt işlemini tamamlamak için. Kayıt işleminin bir parçası olarak, bir ek oturum açma için istenir.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$registeredServer = Register-AzureRmStorageSyncServer -StorageSyncServiceName $storageSyncName
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Eşitleme grubu ve bir bulut uç noktası oluşturma
Eşitleme grubu bir Grup dosyası için eşitleme topolojisi tanımlar. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş tutulur. Eşitleme grubu temsil eden bir Azure dosya paylaşımı ve bir en az bir bulut uç noktası veya sunucu uç nokta içermesi gerekir. Sunucusu uç noktası kayıtlı sunucudaki bir yolu temsil eder. Bir sunucunun birden çok eşitleme gruplarında sunucu uç noktaları olabilir. İçin uygun şekilde istenen eşitleme topolojinizi açıklamak gerektiği kadar eşitleme grubu oluşturabilirsiniz.

Bir bulut uç noktası, bir Azure dosya paylaşımı için bir işaretçidir. Tüm sunucu uç noktaları hub bulut uç noktası yapma bulut uç ile eşitler. Azure dosya paylaşımı için depolama hesabı depolama eşitleme hizmeti ile aynı bölgede bulunması gerekir. Bir özel durum ile Azure dosya paylaşımı tamamen eşitlenir:, bir NTFS biriminde bulunan gizli "Sistem birimi bilgileri" klasöre karşılaştırılabilir özel bir klasör sağlanacak. Bu dizin adında ". SystemShareInformation". Diğer uç noktalara eşitlenmez önemli eşitleme meta verileri içerir. Kullanmayın veya silmeden!

> [!Important]  
> Herhangi bir bulut uç noktası veya eşitleme grubundaki sunucusu uç noktası için değişiklik yapabilirsiniz ve dosyalarınızı eşitleme grubundaki diğer uç noktalarına eşitlendiğinden. (Azure dosya paylaşımı) bulut uç noktasına doğrudan bir değişiklik yaparsanız değişiklikleri ilk Azure dosya eşitleme değişiklik algılama işi tarafından bulunmaları gerekir. Bir değişiklik algılama işi yalnızca bir kez her 24 saatte bir bulut uç noktası için başlatılır. Daha fazla bilgi için bkz: [Azure sık sorulan sorular dosyaları](storage-files-faq.md#afs-change-detection).

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Bir eşitleme grubu oluşturmak için [Azure portal](https://portal.azure.com/)depolama eşitleme hizmetinize gidin ve ardından **+ eşitleme grubu**:

![Azure portalında yeni bir eşitleme grubu oluşturma](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Açılır bölmesinde, bir bulut uç noktası ile bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

- **Eşitleme grubu adı**: Oluşturulacak eşitleme grubunun adı. Bu ad depolama eşitleme hizmeti içinde benzersiz olmalıdır, ancak sizin için mantıklı olan herhangi bir ad olabilir.
- **Abonelik**: depolama eşitleme hizmetinin dağıtıldığı abonelik [depolama eşitleme hizmeti dağıtmak](#deploy-the-storage-sync-service).
- **Depolama hesabı**: seçerseniz **depolama hesabını seçin**, eşitlemek istediğiniz Azure dosya paylaşımı olan depolama hesabını seçebileceğiniz başka bir bölmesinde görünür.
- **Azure dosya paylaşımı**: istediğiniz eşitleme Azure dosya paylaşımı adı.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Eşitleme grubu oluşturmak için aşağıdaki PowerShell yürütün. Değiştirilecek unutmayın `<my-sync-group>` eşitleme grubunu istenen adı.

```PowerShell
$syncGroupName = "<my-sync-group>"
New-AzureRmStorageSyncGroup -SyncGroupName $syncGroupName -StorageSyncService $storageSyncName
```

Eşitleme grubunu başarıyla oluşturulduktan sonra bulut uç noktası oluşturabilirsiniz. Değiştirdiğinizden emin olun `<my-storage-account>` ve `<my-file-share>` beklenen değerlere sahip.

```PowerShell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzureRmStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzureStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzureRmStorageSyncCloudEndpoint `
    -StorageSyncServiceName $storageSyncName `
    -SyncGroupName $syncGroupName ` 
    -StorageAccountResourceId $storageAccount.Id
    -StorageAccountShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Bir sunucu uç noktası oluşturma
Sunucusu uç noktası kayıtlı bir sunucuda, bir sunucu birimdeki bir klasörü gibi belirli bir konuma temsil eder. Sunucusu uç noktası bir kayıtlı sunucu (bağlanılan Paylaşımı yerine) üzerinde bir yol olmalıdır ve bulut katmanlandırma kullanmak için yolun bir sistem dışı birimde olması gerekir. Ağa bağlı depolama (NAS) desteklenmiyor.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Sunucusu uç noktası eklemek için yeni oluşturulan eşitleme grubuna gidin ve ardından **sunucusu uç noktası ekleme**.

![Eşitleme grubu Bölmesi'nde yeni bir sunucu uç noktası ekleme](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

İçinde **sunucusu uç noktası ekleme** bölmesinde, bir sunucu uç noktası oluşturmak için aşağıdaki bilgileri girin:

- **Kayıtlı sunucu**: sunucu veya sunucu uç noktası oluşturmak istediğiniz kümenin adını.
- **Yol**: Windows Server yolu eşitleme grubunun bir parçası eşitlenir.
- **Bulut Katmanlandırma**: etkinleştirmek veya Bulut devre dışı bırakmak için bir anahtar katmanlama. Katmanlama, sık kullanılan veya erişilen dosyaları bulut ile Azure dosyaları katmanlı.
- **Birim boş alanı**: sunucusu uç noktası olduğu bulunduğu birimde ayrılan boş alan miktarı. Birim boş alanı, bir tek sunucu uç noktası bir birimi üzerinde % 50'si ayarlanırsa, örneğin, kabaca veri miktarının yarısına Azure dosyaları katmanlı. Bakılmaksızın olup bulut katmanlama etkinleştirildiğinde, Azure dosya paylaşımınıza verilerin tam bir kopyasını eşitleme grubunda her zaman vardır.

Sunucusu uç noktası eklemek için seçin **oluşturma**. Dosyalarınızı artık Azure dosya paylaşımı ve Windows Server arasında eşitleme tutulur. 

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Sunucusu uç noktası oluşturmak ve değiştirdiğinizden emin olun için aşağıdaki PowerShell komutları yürütün `<your-server-endpoint-path>` ve `<your-volume-free-space>` istenen değerleri ile.

```PowerShell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering $true `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
}
else {
    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Azure dosya eşitleme ile ekleme
Tam dosya uygunluğunu korurken kapalı kalma süresi için önerilen adımları giriş için Azure dosya eşitleme ile ilk kez sıfır ve erişim denetim listesi (ACL) aşağıdaki gibidir:
 
1. Bir depolama eşitleme hizmeti dağıtın.
2. Bir eşitleme grubu oluşturun.
3. Azure dosya eşitleme Aracısı tam veri kümesi ile sunucuya yükleyin.
4. Bu sunucuyu kaydetmek ve paylaşımı üzerinde bir sunucusu uç noktası oluşturun. 
5. Azure dosya paylaşımı (bulut uç noktası) için tam yükleme yapmak eşitleme sağlar.  
6. İlk yükleme tamamlandıktan sonra kalan sunucuların her birinde Azure dosya eşitleme aracısı yükleyin.
7. Yeni dosya paylaşımları kalan sunucuların her birinde oluşturun.
8. İstenirse, bulut katmanlama ilkesiyle yeni dosya paylaşımlarında sunucu uç noktaları oluşturun. (Bu adım için ilk kurulum kullanılabilir olması için ek depolama alanı gerektirir.)
9. Gerçek veri aktarımı olmadan tam ad alanının hızlı geri yükleme yapmak için Azure dosya eşitleme Aracısı olanak tanır. Tam ad alanı eşitlemeden sonra eşitleme altyapısı sunucusu uç noktası için bulut katmanlama ilkesini temel alarak yerel disk alanı doldurur. 
10. Eşitleme işlemi tamamlandıktan ve topolojinizi istediğiniz gibi test emin olun. 
11. Kullanıcılar ve uygulamalar bu yeni paylaşımına yönlendirin.
12. İsteğe bağlı olarak, tüm yinelenen paylaşımları sunucularda silebilirsiniz.
 
İlk eklenmesi için ek depolama alanı yok ve var olan paylaşımlar eklemek istiyorsanız, Azure dosya paylaşımlarının verileri önceden oluşturmak. Kapalı kalma süresi kabul edebilir ve kesinlikle ilk onboarding işlemi sırasında veri değişiklikleri sunucu paylaşımlarındaki garanti ve yalnızca, bu yaklaşım önerilir. 
 
1. Onboarding işlemi sırasında herhangi bir sunucu verileri değiştiremediğinden emin olmak.
2. Ön üretim Azure dosya herhangi bir veri aktarımı aracı üzerinden SMB örn kullanarak sunucu verileriyle Robocopy, doğrudan erişimli SMB kopya paylaşır. AzCopy SMB üzerinden verileri karşıya değil Bu nedenle, önceden dağıtım için kullanılamıyor.
3. Azure dosya eşitleme topolojisi için var olan paylaşımlar işaret eden istenen sunucu uç noktaları oluşturun.
4. Tüm uç noktaları uzlaştırma işlemi son eşitleme sağlar. 
5. Uzlaştırma işlemi tamamlandıktan sonra değişiklikler için paylaşımları açabilirsiniz.
 
Şu anda yaklaşım önceden dengeli birkaç kısıtlamaları olduğunu unutmayın- 
- Dosyalar üzerinde tam uygunluğunu korunmaz. Örneğin, dosyaları, ACL'ler ve zaman damgaları kaybedersiniz.
- Çalışan ve eşitleme topolojisi tam olarak çalışır durumda önce sunucudaki veri değişiklikleri sunucu uç noktalarda çakışmaları neden olabilir.  
- Bulut uç nokta oluşturulduktan sonra Azure dosya eşitleme ilk eşitlemeyi başlatmadan önce dosyaları bulutta algılamak için bir işlem yapar. Bu işlemi tamamlamak için geçen süre ağ hızı, kullanılabilir bant genişliğini ve dosya ve klasörleri sayısı gibi çeşitli etkenlere bağlı olarak değişir. Preview sürümünde kaba tahmin için yaklaşık 10 dosyaları saniye başına Algılama işlemi çalıştırır.  Veriler bulutta ön hazırlığı yapmış olduğunda bu nedenle, önceden çalışır hızlı dengeli olsa bile, tam olarak çalışan sistem almak için toplam süreyi önemli ölçüde uzun olabilir.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>DFS Çoğaltma (DFS-R) dağıtımı için Azure dosya eşitleme geçirme
Azure dosya eşitleme DFS-R dağıtım geçirmek için:

1. Değiştirdiğiniz DFS-R topoloji temsil etmek için bir eşitleme grubu oluşturun.
2. Tam veri geçirmek için DFS-R topolojinizi ayarlanmış sunucuda başlatın. Azure dosya eşitleme bu sunucuya yükleyin.
3. Bu sunucuyu kaydetmek ve yükseltilecek ilk sunucu için bir sunucu uç noktası oluşturur. Bulut etkinleştirmeyin katmanlama.
4. Tüm veri eşitleme, Azure dosya paylaşımına (bulut uç noktası) sağlar.
5. Yükleyin ve Azure dosya eşitleme Aracısı kalan DFS-R sunucuların her birinde kaydedin.
6. DFS-r devre dışı bırak 
7. Sunucusu uç noktası DFS-R sunucuların her birinde oluşturun. Bulut etkinleştirmeyin katmanlama.
8. Eşitleme işlemi tamamlandıktan ve topolojinizi istediğiniz gibi test emin olun.
9. DFS-r devre dışı bırakma
10. Katmanlama may bulut şimdi etkin olması istediğiniz gibi herhangi bir sunucu uç nokta üzerinde.

Daha fazla bilgi için bkz: [Azure dosya eşitleme birlikte çalışabilirliği dağıtılmış dosya sistemi (DFS) ile](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme sunucusu uç noktası Ekle Kaldır](storage-sync-files-server-endpoint.md)
- [Kaydedilemedi veya Azure dosya eşitleme sunucusuyla kaydı silinemedi](storage-sync-files-server-registration.md)