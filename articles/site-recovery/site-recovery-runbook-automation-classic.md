---
title: "Kurtarma planları Klasik portalda Azure Otomasyonu runbook'ları ekleyin | Microsoft Docs"
description: "Bu makalede, nasıl Azure Site Recovery artık Azure Kurtarma sırasında karmaşık görevleri tamamlamak için Azure otomasyonu kullanarak kurtarma planlarını genişletmenizi sağlar açıklanmaktadır."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0a248e7c3f39a35ac10dc6ac64e5cef7d152e033
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans-in-the-classic-portal"></a>Kurtarma planları Klasik portalda Azure Otomasyonu runbook'ları ekleyin
Bu öğretici, Azure Site Recovery kurtarma planlarına genişletilebilirlik sağlamak için Azure Automation ile nasıl tümleşik çalıştığını açıklar. Kurtarma planları ikincil bulut için çoğaltma ve Azure senaryoları için çoğaltma için Azure Site Recovery tarafından korunan sanal makinelerinizin kurtarma düzenleyebilirsiniz. Ayrıca kurtarma yaparken yardımcı **tutarlı bir şekilde doğru**, **yinelenebilir**, ve **otomatik**. Azure sanal makineleriniz başarısız oluyorsa, Azure Automation tümleştirme kurtarma planları genişletir ve runbook'ları, böylece güçlü bir otomatikleştirme görevleri sağlar yürütmek için yeteneği verir.

Azure Automation hakkında henüz duymuş değil, kaydolma [burada](https://azure.microsoft.com/services/automation/) ve bunların örnek komut dosyası yükleme [burada](https://azure.microsoft.com/documentation/scripts/). Daha fazla bilgi edinin [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) ve kurtarma planlarınızı kullanarak Azure kurtarma düzenlemek nasıl [burada](https://azure.microsoft.com/blog/?p=166264).

Kısa Bu öğreticide, nasıl Azure Otomasyon çalışma kitabı kurtarma planları tümleştirebilirsiniz arar. Biz, daha önce el ile müdahale gerekli basit görevleri otomatik hale getirmek ve birden çok adım kurtarma tek tıklamayla kurtarma eyleme dönüştürmek bkz. Biz de sorun yaşanırsa nasıl basit bir komut dosyası giderebileceğinizi adresindeki arar.

## <a name="protect-the-application-to-azure"></a>Uygulamayı Azure'a koruma
Bize iki sanal makine içeren basit bir uygulama ile başlar. Burada, Fabrikam HRweb uygulamasının sahip. Fabrikam HRweb frontend ve Fabrikam Hrweb arka Azure için korunan iki sanal makine Azure Site Recovery kullanıldığında'dır. Azure Site Recovery kullanarak sanal makineleri korumak için aşağıdaki adımları izleyin.

1. Sanal makineler için korumayı etkinleştirin.
2. Sanal makinelerin başlangıç çoğaltmasını tamamlamış olmanız ve çoğaltma olduğundan emin olun.
3. İlk çoğaltma işlemi tamamlandıktan ve korumalı çoğaltma durumunu bildiren kadar bekleyin.

## ![](media/site-recovery-runbook-automation/01.png)
Bu öğreticide, bir kurtarma planı yük devretme uygulamayı Azure'a Fabrikam HRweb uygulamasının oluşturacağız. Biz bir uç nokta başarısız üzerinde oluşturacak bir runbook ile tümleştirecek sonra bağlantı noktası 80 web sayfaları için Azure sanal makinesi üzerinde.

Öncelikle, uygulamamız için bir kurtarma planı oluşturalım.

## <a name="create-the-recovery-plan"></a>Kurtarma planı oluşturun
Uygulamayı Azure'a kurtarmak için bir kurtarma planı oluşturmanız gerekir.
Kurtarma sanal makinelerin sırasını belirttiğiniz bir kurtarma planı kullanıyor. 1 grupta sanal makineyi kurtarmak ve ilk başlatın ve sonra 2 grubundaki sanal makine izleyin.

Aşağıdaki gibi görünen bir kurtarma planı oluşturun.

![](media/site-recovery-runbook-automation/12.png)

Daha fazla bilgi için kurtarma planları hakkında belgelerini okuyun [burada](https://msdn.microsoft.com/library/azure/dn788799.aspx "burada").

Ardından, gerekli yapıları Azure Otomasyonu'nda oluşturalım.

## <a name="create-the-automation-account-and-its-assets"></a>Otomasyon hesabı ve varlıklarını oluşturma
Runbook'ları oluşturmak için bir Azure Otomasyonu hesabı gerekir. Bir hesap zaten yoksa tarafından belirtilen Azure Otomasyonu sekmesine gidin ![](media/site-recovery-runbook-automation/02.png)ve yeni bir hesap oluşturun.

1. Hesabı ile tanımlamak için bir ad verin.
2. Coğrafi bölge hesap eklemek istediğiniz yeri belirtin.

ASR kasasıyla aynı bölgede hesap yerleştirmek için önerilir.

![](media/site-recovery-runbook-automation/03.png)

Ardından, aşağıdaki varlıklar hesabı oluşturun.

### <a name="add-a-subscription-name-as-asset"></a>Abonelik adı varlık ekleme
1. Yeni bir ayar Ekle ![](media/site-recovery-runbook-automation/04.png) Azure Otomasyon varlıkları ve seçmek için![](media/site-recovery-runbook-automation/05.png)
2. Değişken türü olarak seçin **dize**
3. Değişken adı olarak belirtmeniz **AzureSubscriptionName**

   ![](media/site-recovery-runbook-automation/06.png)
4. Değişken değeri olarak, gerçek Azure abonelik adını belirtin.

   ![](media/site-recovery-runbook-automation/07_1.png)

Aboneliğiniz Azure portalındaki hesabınızın Ayarları sayfasından adını belirleyebilirsiniz.

### <a name="add-an-azure-login-credential-as-asset"></a>Bir Azure oturum açma kimlik bilgileri varlık Ekle
Azure Otomasyonu aboneliğine bağlanmak için Azure PowerShell kullanır ve var. yapıları üzerinde çalışır. Bunun için Microsoft hesabınızla veya bir iş veya Okul hesabı kullanarak kimlik doğrulaması gerekir.
Hesap kimlik bilgilerini güvenli bir şekilde runbook tarafından kullanılacak bir varlık depolayabilirsiniz.

1. Yeni bir ayar Ekle ![](media/site-recovery-runbook-automation/04.png) Azure Otomasyon varlıkları ve seçin![](media/site-recovery-runbook-automation/09.png)
2. Kimlik bilgisi türü seçin **Windows PowerShell kimlik bilgisi**
3. Adı olarak belirtmeniz **AzureCredential**

   ![](media/site-recovery-runbook-automation/10.png)
4. Kullanıcı adı ve oturum ile açmak için parola belirtin.

Şimdi iki bu ayarlar, varlıkları kullanılabilir.

![](media/site-recovery-runbook-automation/11.png)

Aboneliğinizi PowerShell aracılığıyla bağlanma hakkında daha fazla bilgi verilen [burada](/powershell/azure/overview).

Ardından, ön uç sanal makine için bir uç nokta yük devretme sonrasında ekleyebilirsiniz Azure Automation runbook oluşturur.

## <a name="azure-automation-context"></a>Azure Otomasyonu bağlamı
ASR runbook'a belirleyici komut dosyaları yazmanıza yardımcı olacak bir bağlamının geçirir. Bir bulut hizmeti ve sanal makine adları tahmin edilebilir, ancak, her zaman bir gibi belirli senaryolar owing to durumda olduğu sanal makinenin adı desteklenmeyen karakterler nedeniyle değiştirilmiş olabilir olmadığını olur karşıdır Azure. Bu nedenle bu bilgileri ASR kurtarma planına parçası olarak geçirilen *bağlamı*.

Aşağıda bağlamının nasıl göründüğünü bir örnektir.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Aşağıdaki tabloda, her bir değişken bağlamında için ad ve açıklama içerir.

| **Değişken adı** | **Açıklama** |
| --- | --- |
| RecoveryPlanName |Çalıştırılan planının adı. Aynı komut dosyasını kullanarak adına göre eyleme yardımcı olur |
| FailoverType |Yük devretme sınamasını olup olmadığını, planlı veya plansız belirtir. |
| FailoverDirection |Kurtarma birincil veya ikincil olup olmadığını belirtin |
| GroupID |Kurtarma planı içinde Grup numarası plan çalıştırırken tanımlayın |
| VmMap |Dizi, gruptaki tüm sanal makineler |
| VMMap anahtarı |Her VM için benzersiz anahtar (GUID). Bu sanal makinenin VMM kimliği uygunsa aynıdır. |
| Rol adı |Kurtarılacak Azure VM adı |
| CloudServiceName |Azure bulut hizmeti adı altında sanal makine oluşturulur. |

Bağlam VmMap anahtarında tanımlamak için ayrıca ASR VM özellikler sayfasına gidin ve VM GUID özelliğine bakın.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Bir Otomasyon runbook'u Yaz
Ön uç sanal makine üzerinde bağlantı noktası 80 açmak için runbook'u şimdi oluşturun.

1. Azure Otomasyonu hesabı adıyla yeni bir runbook oluşturmak **OpenPort80**

   ![](media/site-recovery-runbook-automation/14.png)
2. Runbook yazarı görünümüne gidin ve taslak modunda girin.
3. Kurtarma planı bağlamı olarak kullanmak için değişken ilk belirtin

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. Sonraki kimlik bilgisi ve abonelik adını kullanarak aboneliğine bağlanma

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect to Azure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   Azure varlıklar – kullandığını unutmayın **AzureCredential** ve **AzureSubscriptionName** burada.
5. Artık uç nokta ayrıntılarını ve uç noktayı kullanıma sunmak istediğiniz sanal makine GUID belirtin. Bu, ön uç sanal makine durumda.

   ```
       # Specify the parameters to be used by the script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   Bu, Azure uç noktası protokolü, VM'de yerel bağlantı noktası ve eşlenen ortak bağlantı noktasını belirtir. Bu değişkenler VM'ler için uç noktaları ekleme Azure komutları gerekli parametrelerdir. VMGUID üzerinde çalışması için gereken sanal makine GUID tutar.
6. Komut dosyası şimdi bağlam için belirtilen VM GUID ayıklayın ve tarafından başvurulan sanal makineye bir uç nokta oluşturun.

   ```
       #Read the VM GUID from the context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. Tamamlandığında, Yayımla isabet ![](media/site-recovery-runbook-automation/20.png) , komut yürütme için kullanılabilir olmasını sağlamak için.

Tam komut dosyası başvuru için aşağıda verilmiştir

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Komut dosyası kurtarma planına ekleyin
Komut dosyası hazır olduktan sonra daha önce oluşturduğunuz kurtarma planına ekleyin.

1. Oluşturduğunuz kurtarma planında, Grup 2 sonra bir komut dosyası eklemek seçin. ![](media/site-recovery-runbook-automation/15.png)
2. Bir komut dosyası adı belirtin. Yalnızca kurtarma planı içinde gösteren için bu komut için bir kolay ad budur.
3. Azure'a yük devretme betiği – Azure Automation hesabı adını seçin.
4. Azure Runbook'larda yazdığınız runbook'u seçin.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Birincil tarafı betikler
Bir yük devretme Azure yürütülürken birincil tarafı komut yürütmek seçebilirsiniz. Bu komut dosyaları, yük devretme sırasında VMM sunucusunda çalıştırılır.
Birincil tarafı komut dosyaları yalnızca yalnızca Kapatma öncesi için kullanılabilir ve kapatma aşamaları gönderin. Birincil sitenin bir olağanüstü durum sağlar, normal olarak kullanılamaz hale gelmesine bekliyoruz olmasıdır.
Yalnızca birincil site işlemleri için kabul ederseniz planlanmamış bir yük devretme sırasında birincil tarafı komut dosyalarını çalıştırmak deneyecek. Bunlar erişilebilir değil veya zaman aşımı, sanal makineler kurtarmak yük devretme devam edecek varsa.
Birincil tarafı komut dosyaları, yük devretme Azure sırasında Azure için - korumalı VMM olmadan VMware/fiziksel/Hyper-v siteleri için beklemediğiniz kullanılabilir.
Ancak, ne zaman, azure'dan şirket içi, birincil tarafı komut dosyaları (Runbook'lar) VMware dışındaki tüm hedefler için kullanılabilir.

## <a name="test-the-recovery-plan"></a>Kurtarma planı test
Runbook bir test yük devretme başlatın ve istemciyi kullanımda görme plana ekledikten sonra. Ayrıca, uygulamanızı ve herhangi bir hata olduğundan emin olmak için kurtarma planını test etmek için bir yük devretme testi çalıştırmak için her zaman önerilir.

1. Kurtarma planı seçin ve bir test yük devretme başlatın.
2. Plan yürütme sırasında runbook yürütüldü olup olmadığını veya durumunu aracılığıyla değil görebilirsiniz.

   ![](media/site-recovery-runbook-automation/17.png)
3. Azure Otomasyonu işleri sayfasında runbook için ayrıntılı runbook yürütme durumu de görebilirsiniz.

   ![](media/site-recovery-runbook-automation/18.png)
4. Yük devretme tamamlandığında runbook yürütme sonucu dışında yürütme başarılı olup olmadığını veya Azure sanal makinesi sayfasını ziyaret ve uç noktada arayarak tarafından görebilirsiniz.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Örnek komut dosyaları
Vazgeçtik sırada Bu öğreticide bir Azure sanal makineye bir uç nokta ekleme görev kullanılan bir genellikle otomatikleştirme, bir dizi Azure otomasyonu kullanarak diğer güçlü Otomasyon görevi yapabilirsiniz. Microsoft ve Azure Automation topluluk yardımcı olabilecek örnek runbook'lar kendi çözümleri ve daha büyük bir otomatikleştirme görevleri için yapı taşı olarak kullanabileceğiniz yardımcı programı runbook'lar oluşturmaya başlamak sağlar. Galeriden kullanmaya başlamak ve Azure Site Recovery kullanarak uygulamalarınızı için güçlü tek tıklatmayla kurtarma planları oluşturun.

## <a name="additional-resources"></a>Ek Kaynaklar
[Azure Otomasyonu genel bakış](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Otomasyonu genel bakış")

[Örnek Azure Otomasyon betikleri](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "örnek Azure Otomasyonu komutlar")
