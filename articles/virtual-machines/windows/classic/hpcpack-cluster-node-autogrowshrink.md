---
title: "Otomatik ölçeklendirme HPC paketi küme düğümleri | Microsoft Docs"
description: "Otomatik olarak Büyüt ve azure'da HPC paketi küme işlem düğümü sayısını Daralt"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0dc0d15c64d8951c3c457df73588c37418a3c8a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Otomatik olarak Büyüt ve Azure HPC paketi küme kaynaklarında göre küme iş yükü küçültme
HPC Pack kümenizdeki Azure "veri bloğu" düğümlerini dağıtmak ya da Azure Vm'lerde bir HPC paketi küme oluşturmak, bir şekilde otomatik olarak büyütür veya küme kaynaklarını düğümleri veya çekirdek küme iş yüküne göre gibi küçültmek isteyebilirsiniz. Bu şekilde küme kaynaklarını ölçeklendirme, Azure kaynaklarınızı daha verimli şekilde kullanabilir ve bunların maliyetlerini denetlemenize olanak tanır.

Bu makalede, HPC Pack sağlayan iki otomatik ölçeklendirme yolları işlem kaynaklarını gösterir:

* HPC Pack küme özelliği **AutoGrowShrink**

* **AzureAutoGrowShrink.ps1** HPC PowerShell Betiği

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Şu anda, yalnızca otomatik olarak Büyüt ve Windows Server işletim sistemi çalıştırılan HPC Pack işlem düğümleri daraltma.


## <a name="set-the-autogrowshrink-cluster-property"></a>AutoGrowShrink küme özelliğini ayarlayın
### <a name="prerequisites"></a>Ön koşullar

* **HPC Pack 2012 R2 güncelleştirme 2 veya sonraki bir küme** -küme baş düğümüne olabilir ya da şirket içi dağıtılan veya bir Azure VM. Bkz: [HPC paketi ile karma küme ayarlama](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) bir şirket içi baş düğüm ve Azure "veri bloğu" düğümleri ile çalışmaya başlamak için. Bkz: [HPC Pack Iaas dağıtım betiği](hpcpack-cluster-powershell-script.md) hızlı bir şekilde Azure VM'de HPC paketi küme dağıtmak için.

* **Bir küme baş düğümü Azure (Resource Manager dağıtım modeli) bulunan** - HPC Pack 2016'dan itibaren sertifika kimlik doğrulaması Azure Active Directory uygulamada otomatik olarak artan için kullanılır ve küçültme küme VM'ler dağıtılabilir kullanma Azure Resource Manager. Bir sertifika aşağıdaki gibi yapılandırın:

  1. Küme dağıtıldıktan sonra Uzak Masaüstü tarafından bir baş düğümüne bağlanmak.

  2. Her baş düğümüne (özel anahtarı olan PFX biçimi) sertifikasını yükleyin ve Cert: \LocalMachine\My ve Cert: \LocalMachine\Root yükleyin.

  3. Azure PowerShell'i yönetici olarak başlatın ve bir baş düğüm üzerinde aşağıdaki komutları çalıştırın:

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    Hesabınızın birden fazla Azure Active Directory kiracısı veya Azure aboneliği varsa, doğru Kiracı ve aboneliği seçmek için aşağıdaki komutu çalıştırabilirsiniz:
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    Şu anda seçili Kiracı ve abonelik görüntülemek için aşağıdaki komutu çalıştırın:
    
    ```powershell
        Get-AzureRMContext
    ```

  4. Aşağıdaki betiği çalıştırın

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    Burada

    **DisplayName** -Azure Active uygulama görünen adı. Uygulama yok, Azure Active Directory içinde oluşturulur.

    **Giriş sayfası** -uygulama giriş sayfası. Önceki örnekte olduğu gibi bir kukla URL yapılandırabilirsiniz.

    **IdentifierUri** -uygulama tanıtıcısı. Önceki örnekte olduğu gibi bir kukla URL yapılandırabilirsiniz.

    **CertificateThumbprint** -baş düğüm 1. adımda yüklediğiniz sertifikanın parmak izi.

    **Tenantıd** -Kiracı kimliği, Azure Active Directory. Kiracı kimliği Azure Active Directory Portalı'ndan elde edebilirsiniz **özellikleri** sayfası.

    Hakkında daha fazla ayrıntı için **ConfigARMAutoGrowShrinkCert.ps1**, çalışma `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.


* **Bir küme baş düğümü Azure (Klasik dağıtım modeli) bulunan** - HPC Pack Iaas dağıtım komut dosyası kullanırsanız Klasik dağıtım modelinde küme oluşturma, etkinleştirme için **AutoGrowShrink** küme özelliği tarafından AutoGrowShrink seçeneği küme yapılandırma dosyasında ayarlama. Belge eşlik Ayrıntılar için bkz [komut dosyası indirme](https://www.microsoft.com/download/details.aspx?id=44949).

    Alternatif olarak, etkinleştirme **AutoGrowShrink** aşağıdaki bölümde açıklanan HPC PowerShell komutlarını kullanarak küme dağıttıktan sonra özellik kümesi. Bunun için hazırlamak için önce aşağıdaki adımları tamamlayın:

  1. Azure yönetim sertifikası baş düğüm ve Azure aboneliğini yapılandırın. Bir sınama dağıtımı için HPC Pack baş düğümüne yükleyen varsayılan Microsoft HPC Azure otomatik olarak imzalanan sertifika kullan ve Azure aboneliğinize bu sertifikayı karşıya yükleyin. Seçenekler ve adımları için bkz: [TechNet Kitaplığı Kılavuzu](https://technet.microsoft.com/library/gg481759.aspx).

  2. Çalıştırma **regedit** baş düğüm için HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo gidin ve bir dize değeri ekleyin. Değer adı "Parmak izi" olarak ayarlayın ve sertifikanın parmak izi 1. adımda veri değer.

### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell komutlarını AutoGrowShrink özelliğini ayarlamak için
Şunlardır ayarlamak için örnek HPC PowerShell komutları **AutoGrowShrink** ve ek parametrelerle davranışını ayarlamak için. Bkz: [AutoGrowShrink parametreleri](#AutoGrowShrink-parameters) ayarlarının tam listesi için bu makalede daha sonra.

Bu komutları çalıştırmak için yönetici olarak küme baş düğümünde HPC PowerShell'i başlatın.

**AutoGrowShrink özelliğini etkinleştirmek için**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

**AutoGrowShrink özelliği devre dışı bırakmak için**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

**Dakika cinsinden Büyüt aralığını değiştirmek için**

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

**Dakika cinsinden küçültme aralığını değiştirmek için**

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

**AutoGrowShrink geçerli yapılandırmasını görüntülemek için**

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

**Düğüm grupları AutoGrowShrink dışlamak için**

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> Bu parametre HPC Pack 2016'dan itibaren desteklenir
>

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parametreleri
Kullanarak değiştirebilirsiniz AutoGrowShrink Parametreler şunlardır **kümesi HpcClusterProperty** komutu.

* **EnableGrowShrink** -etkinleştirmek veya devre dışı bırakmak için anahtar **AutoGrowShrink** özelliği.
* **ParamSweepTasksPerCore** -bir çekirdek büyümeye parametrik tarama görev sayısı. Görev başına bir çekirdek büyümeye varsayılandır.

  > [!NOTE]
  > HPC Pack QFE KB3134307 değişiklikleri **ParamSweepTasksPerCore** için **TasksPerResourceUnit**. İş kaynak türüne göre ve düğüm, yuva veya çekirdek olabilir.
  >
  >
* **GrowThreshold** -otomatik büyüme tetiklemek için kuyruğa alınmış görevlerin eşiği. Sıraya alınmış durumda 1 veya daha fazla görev varsa, otomatik olarak düğümleri Büyüt anlamına gelir 1 varsayılandır.
* **GrowInterval** -otomatik büyüme tetiklemek için zaman aralığını dakika cinsinden. Varsayılan aralığı 5 dakikadır.
* **ShrinkInterval** -otomatik küçültme tetiklemek için zaman aralığını dakika cinsinden. Varsayılan aralığı 5 dakikadır. |
* **ShrinkIdleTimes** -düğümleri göstermek için daraltmak için sürekli denetimlerinin sayısının boş. 3 kez varsayılandır. Örneğin, varsa **ShrinkInterval** 5 dakika, HPC Pack her 5 dakikada bir düğüm boş olup olmadığını denetler. 3 sürekli (15 dakika) denetledikten sonra düğümler boşta durumunda ise, HPC Pack bu düğüme küçültür.
* **ExtraNodesGrowRatio** -ileti geçirme arabirimi (MPI) işleri büyümeye düğümleri ek yüzdesi. HPC Pack %1 MPI işlerini için düğümleri büyür anlamına gelir 1 varsayılan değerdir.
* **GrowByMin** -otomatik büyüme İlkesi iş için gereken en düşük kaynaklardaki dayalı olup olmadığını belirtmek için anahtar. HPC Pack işleri için gereken en fazla kaynakları göre işleri için düğümleri büyür başka bir deyişle, false, varsayılandır.
* **SoaJobGrowThreshold** -otomatik tetiklemek için gelen SOA istekleri eşiğinin büyümesine işlemi. Varsayılan değer 50000 ' dir.

  > [!NOTE]
  > Bu parametre, HPC Pack 2012 R2 güncelleştirme 3'te itibaren desteklenmektedir.
  >
  >
* **SoaRequestsPerCore** -bir çekirdek büyümeye istekleri Gelen SOA sayısı. 20000 varsayılan değerdir.

  > [!NOTE]
  > Bu parametre, HPC Pack 2012 R2 güncelleştirme 3'te itibaren desteklenmektedir.
  >
* **ExcludeNodeGroups** – belirtilen düğümün gruplardaki düğümler otomatik olarak Büyüt ve daraltma.
  
  > [!NOTE]
  > Bu parametre, HPC Pack 2016'dan itibaren desteklenir.
  >

### <a name="mpi-example"></a>MPI örneği
Varsayılan olarak, %1 HPC Pack büyür MPI işlerini için ek düğümleri (**ExtraNodesGrowRatio** 1 olarak ayarlayın). , Birden çok düğüm MPI gerektirebilir ve tüm düğümler hazır olduğunuzda, iş yalnızca çalıştırabilirsiniz nedenidir. Zaman zaman Azure düğümleri başladığında, tek bir düğüm diğerlerinden, diğer düğümler hazır hale getirmek bu düğümü için beklenirken boşta olması neden başlatmak için daha fazla süre gerekebilir. Ek düğümler büyüyen HPC Pack bu kaynak bekleme süresini azaltır ve maliyetleri potansiyel olarak kaydeder. MPI işlerini (örneğin, % 10) için ek düğümleri artırmak için benzer bir komutu çalıştırın

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA örneği
Varsayılan olarak, **SoaJobGrowThreshold** 50000 için ayarlanır ve **SoaRequestsPerCore** 200000 için ayarlanır. Bir SOA işi 70000 isteği göndermek, bir Sıraya alınan görev olduğunu ve gelen istekleri 70000. Bu durumda HPC Pack 1 çekirdek kuyruğa alınmış görev için ve gelen istekleri büyüdükçe, (70000-50000) büyür / 20000 = 1 çekirdek, bu nedenle, toplam bu SOA işi için 2 Çekirdek büyür.

## <a name="run-the-azureautogrowshrinkps1-script"></a>AzureAutoGrowShrink.ps1 komut dosyasını çalıştır
### <a name="prerequisites"></a>Ön koşullar

* **HPC Pack 2012 R2 güncelleştirme 1 veya sonraki bir küme** - **AzureAutoGrowShrink.ps1** komut dosyası, % CCP_HOME % bin klasörüne yüklenir. Küme baş düğümüne olabilir ya da şirket içi dağıtılan veya bir Azure VM. Bkz: [HPC paketi ile karma küme ayarlama](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) bir şirket içi baş düğüm ve Azure "veri bloğu" düğümleri ile çalışmaya başlamak için. Bkz: [HPC Pack Iaas dağıtım betiği](hpcpack-cluster-powershell-script.md) hızlı bir şekilde Azure VM'de HPC paketi Küme dağıtımı veya kullanmak için bir [Azure Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).
* **Azure PowerShell 1.4.0** -komut dosyası şu anda bu belirli Azure PowerShell sürümüne bağlıdır.
* **Azure ile bir küme düğümleri veri bloğu** -HPC Pack yüklü olduğu bir istemci bilgisayarı veya baş düğüm komut dosyasını çalıştırın. Bir istemci bilgisayarda çalışıyorsa $env değişken Ayarla olun: baş düğümüne işaret edecek şekilde CCP_SCHEDULER. Azure "veri bloğu" düğümler kümeye eklenmesi gerekir, ancak değil dağıtılan bir durumda olabilir.
* **Azure VM'ler (Resource Manager dağıtım modeli) dağıtılan bir küme için** -Azure Resource Manager dağıtım modelinde dağıtılan VM'ler küme için komut dosyası Azure kimlik doğrulaması için iki yöntemi destekler: Azure hesabınızda oturum çalıştırmak için Her komut dosyası (çalıştırarak `Login-AzureRmAccount`, veya bir sertifika ile kimlik doğrulaması için bir hizmet sorumlusu yapılandırın. HPC Pack komut dosyası sağlar **ConfigARMAutoGrowShrinkCert.ps** sertifikayla bir hizmet sorumlusu oluşturmak için. Komut dosyasını bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu oluşturur ve hizmet sorumlusu katılımcı rolü atar. Komut dosyasını çalıştırmak için Azure PowerShell'i yönetici olarak başlatın ve aşağıdaki komutları çalıştırın:

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    Hakkında daha fazla ayrıntı için **ConfigARMAutoGrowShrinkCert.ps1**, çalışma `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,

* **Azure VM'ler (Klasik dağıtım modeli) dağıtılan bir küme için** -bağımlı olduğundan dolayı VM baş düğümünde komut dosyasını çalıştırmak **başlangıç HpcIaaSNode.ps1** ve **Stop-HpcIaaSNode.ps1** yüklü komutlar. Bu komut ayrıca bir Azure yönetim sertifikası gerektirir veya yayımlama ayarları dosyası (bkz [bir HPC Pack Yönet işlem düğümleri küme Azure'da](hpcpack-cluster-node-manage.md)). Tüm işlem düğümü ihtiyacınız VM'ler zaten kümeye eklenmiş olduğundan emin olun. Bunlar durdurulmuş durumda olabilir.



### <a name="syntax"></a>Sözdizimi
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a>Parametreler
* **NodeTemplates** -düğüm şablonlarının adlarını büyür ve daraltmak için düğümlerini kapsamını tanımlar. Belirtilmezse, (varsayılan değer: @()), tüm düğümlerde **AzureNodes** düğüm grubu içinde olduğunda kapsam olan **NodeType** AzureNodes ve tüm düğümleri değerine sahip **ComputeNodes**düğüm grubu içinde olduğunda kapsam olan **NodeType** ComputeNodes değerine sahip.
* **JobTemplates** -büyümeye düğümlerin kapsamını tanımlamak için proje şablonları adları.
* **NodeType** -büyür ve küçültme düğümünün türü. Desteklenen değerler şunlardır:

  * **AzureNodes** – bir şirket içi Azure PaaS (veri bloğu) düğümler için ya da Azure Iaas küme.
  * **ComputeNodes** - işlem düğümünde VM'ler yalnızca bir Azure Iaas kümesi.

* **NumOfQueuedJobsPerNodeToGrow** -tek bir düğüm büyümeye gerekli sıraya alınmış işlerin sayısı.
* **NumOfQueuedJobsToGrowThreshold** -Büyüt işlemini başlatmak üzere sıraya alınan iş eşik sayısı.
* **NumOfActiveQueuedTasksPerNodeToGrow** - sayı etkin bir düğümün ulaşması için gereken görevler sıraya. Varsa **NumOfQueuedJobsPerNodeToGrow** belirtilen 0'dan büyük bir değer ile bu parametre yoksayılır.
* **NumOfActiveQueuedTasksToGrowThreshold** -Büyüt işlemini başlatmak için etkin kuyruğa alınmış görevlerin eşik sayısı.
* **NumOfInitialNodesToGrow** -ilk düğüm sayısı alt sınırı kapsamdaki tüm düğümler varsa büyümeye **değil dağıtılan** veya **durduruldu (Deallocated)**.
* **GrowCheckIntervalMins** -büyümeye denetimleri arasındaki dakika aralığında.
* **ShrinkCheckIntervalMins** -daraltma için denetimler arasındaki dakika aralığında.
* **ShrinkCheckIdleTimes** -sürekli sayısını Küçült denetimleri (ayırarak **ShrinkCheckIntervalMins**) düğümler boşta olduğunu belirtmek için.
* **UseLastConfigurations** -bağımsız değişken dosyasında kaydedilen önceki yapılandırmaları.
* **ArgFile**-kaydetmek ve komut dosyasını çalıştırmak üzere yapılandırmaları güncelleştirmek için kullanılan bağımsız değişken dosya adı.
* **LogFilePrefix** -günlük dosyası ön ek adı. Bir yolu belirtebilirsiniz. Varsayılan olarak, geçerli çalışma dizini günlüğüne yazılır.

### <a name="example-1"></a>Örnek 1
Aşağıdaki örnek varsayılan AzureNode büyür ve otomatik olarak küçültmek için şablonla dağıtılan Azure veri bloğu düğümleri yapılandırır. Tüm düğüm başlangıçta giriş gerekirse **değil dağıtılan** durumunda, en az 3 düğümleri başlatılacağı. Sıraya alınmış işlerin sayısı 8 aşarsa, kuyruğa alınmış işleri oranını kendi sayıyı aşıyor kadar komut dosyasını düğümleri başlatır **NumOfQueuedJobsPerNodeToGrow**. Bir düğüm ardışık 3 boşta kalma sürelerini boşta bulunursa durdurulur.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Örnek 2
Aşağıdaki örnek, Azure işlem düğümü varsayılan ComputeNode büyür ve otomatik olarak küçültmek için şablonu ile dağıtılan VM'ler yapılandırır.
Varsayılan proje şablonu tarafından yapılandırılan işleri, küme üzerinde iş yükü kapsamını tanımlayın. Tüm düğümleri başlangıçta durdurduysanız, en az 5 düğümleri başlatılır. 15 etkin kuyruğa alınmış görevlerin sayısını aşarsa, kendi sayıyı aşıyor active sıraya alınan görevlere oranını kadar komut dosyasını düğümleri başlatır **NumOfActiveQueuedTasksPerNodeToGrow**. Bir düğüm 10 ardışık boşta kalma sürelerini boşta bulunursa durdurulur.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
