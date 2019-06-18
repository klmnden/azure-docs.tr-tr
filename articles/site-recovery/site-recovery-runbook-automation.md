---
title: Site Recovery kurtarma planlarına Azure Otomasyonu runbook'ları ekleme | Microsoft Docs
description: Azure Site Recovery ile olağanüstü durum kurtarma için kurtarma planlarına Azure Otomasyonu ile genişletmeyi öğrenin.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 26c3466080cb356ca3610d42eaaf5ee4975d3731
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61471944"
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Kurtarma planlarına Azure Otomasyonu runbook'ları ekleme
Bu makalede, Azure Site Recovery kurtarma planlarınızı genişletmek amacıyla Azure Otomasyonu ile nasıl tümleştirildiğini açıklar. Kurtarma planları, Site Recovery ile korunan Vm'leri kurtarma düzenleyebilirsiniz. Kurtarma planları, çoğaltma için ikincil bir Bulutu hem Azure'a çoğaltma için çalışır. Kurtarma planları da yardımcı olun kurtarma **tutarlı bir şekilde doğru**, **tekrarlanabilir**, ve **otomatik**. Azure Otomasyonu ile tümleştirme, sanal makinelerinizin azure'a yük devretme durumunda kurtarma planlarınızı genişletir. Güçlü otomasyon görevleri sunan runbook'ları çalıştırmak için kullanabilirsiniz.

Azure Otomasyonu yeni sağlayıcıysanız şunu yapabilirsiniz [kaydolun](https://azure.microsoft.com/services/automation/) ve [örnek betikler indirme](https://azure.microsoft.com/documentation/scripts/). Daha fazla bilgi ve kullanarak azure'a kurtarma düzenleyin öğrenmek için [kurtarma planları](./site-recovery-create-recovery-plans.md), bkz: [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

Bu makalede, Kurtarma planlarına Azure Otomasyonu runbook'ları nasıl tümleştirebilirsiniz açıklanmaktadır. Daha önce el ile müdahale gerekli temel görevleri otomatik hale getirmek için örneklerini kullanırız. Biz de çok adımlı kurtarma için tek tıklamayla çalışan kurtarma eylemi dönüştürme açıklanmaktadır.

## <a name="customize-the-recovery-plan"></a>Kurtarma planını özelleştirin
1. Git **Site Recovery** kurtarma planı kaynak dikey. Bu örnekte, kurtarma planı, kurtarma için eklenen iki VM var. Bir runbook eklemeye başlamak için tıklayın **Özelleştir** sekmesi.

    ![Özelleştirme düğmesine tıklayın](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Sağ **Grup 1: Başlangıç**ve ardından **sonraki eylem Ekle**.

    ![Sağ tıklama Grup 1: Başlat ve posta eylemi ekleme](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Tıklayın **betik seçme**.

4. Üzerinde **güncelleştirme eylemini** dikey penceresinde, adı komut **Hello World**.

    ![Güncelleştirme eylemi dikey penceresi](media/site-recovery-runbook-automation-new/update-rp.png)

5. Bir Otomasyon hesabı adı girin.
    >[!NOTE]
    > Otomasyon hesabı, dilediğiniz Azure bölgesinde olabilir. Otomasyon hesabı, Azure Site Recovery kasası ile aynı abonelikte olmalıdır.

6. Otomasyon hesabınızda, bir runbook seçin. Bu runbook ilk grubun kurtarma işleminden sonra kurtarma planı yürütülmesi sırasında çalışacak olan betiğe ' dir.

7. Komut dosyası kaydetmek için tıklatın **Tamam**. Betik eklenir **Grup 1: Sonraki adımlar**.

    ![Sonrası eylem grubu 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>Bir komut dosyası eklemek için dikkat edilmesi gerekenler

* İçin Seçenekler **bir adım silme** veya **betiği Güncelleştir**, komut dosyasını sağ tıklayın.
* Bir betiği Azure'da yük devretme sırasında bir şirket içi makine Azure'da çalıştırabilirsiniz. Bu da Azure üzerinde kapatın önce bir birincil siteyi komut olarak azure'dan şirket içi makineye sırasında çalıştırabilirsiniz.
* Bir komut dosyası çalıştığında, bir kurtarma planı bağlam ekler. Aşağıdaki örnek, bir bağlam değişkeni gösterir:

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    Aşağıdaki tabloda, adı ve içerik her bir değişken açıklaması listelenmektedir.

    | **Değişken adı** | **Açıklama** |
    | --- | --- |
    | RecoveryPlanName |Çalıştırılan planın adı. Bu değişken kurtarma planı adına göre farklı eylemlerde yardımcı olur. Ayrıca, komut dosyasını yeniden kullanabilirsiniz. |
    | FailoverType |Yük devretme testi olup olmadığını belirtir, planlı veya plansız. |
    | FailoverDirection |Kurtarma için birincil veya ikincil bir siteye olup olmadığını belirtir. |
    | GroupID |Plan çalışırken Grup numarası kurtarma planında tanımlar. |
    | VmMap |Gruptaki tüm VM'ler bir dizi. |
    | VMMap anahtarı |Her VM için benzersiz anahtar (GUID). Bu VM, Azure Virtual Machine Manager (VMM) Kimliğini uygunsa aynıdır. |
    | SubscriptionId |Sanal Makinenin oluşturulduğu Azure abonelik kimliği. |
    | Rol adı |Kurtarılmakta olan Azure VM adı. |
    | CloudServiceName |Sanal Makinenin oluşturulduğu Azure bulut hizmeti adı. |
    | ResourceGroupName|Sanal Makinenin oluşturulduğu Azure kaynak grubu adı. |
    | RecoveryPointId|VM, kurtarılır için zaman damgası. |

* Otomasyon hesabı aşağıdaki modüller olduğundan emin olun:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Tüm modüller, uyumlu sürümleri olmalıdır. Tüm modüller uyumlu olmasını sağlamak için kolay bir yol, tüm modülleri en son sürümlerine kullanmaktır.

### <a name="access-all-vms-of-the-vmmap-in-a-loop"></a>Tüm sanal makinelerin bir döngüye VMMap erişim
Microsoft VMMap tüm sanal makinelerde döngü gerçekleştirmek için aşağıdaki kodu kullanın:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is to ensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> Kaynak grubu adı ve rol adı değerleri, betiği bir ön eylem önyükleme grubuna olduğunda boştur. Yalnızca bu grubun VM yük devretme başarılı olursa, değerler doldurulur. Betik, önyükleme grubunun sonrası bir işlemdir.

## <a name="use-the-same-automation-runbook-in-multiple-recovery-plans"></a>Birden çok kurtarma planlarıyla, aynı Otomasyon runbook'u kullanın

Dış değişkenleri kullanarak birden çok kurtarma planlarıyla, tek bir komut dosyası kullanabilirsiniz. Kullanabileceğiniz [Azure Otomasyonu değişken](../automation/automation-variables.md) depolamak için bir kurtarma planı yürütme geçirdiğiniz parametreler için. Kurtarma planı adı ön eki olarak değişkenine eklediğinizde, her bir kurtarma planı için bağımsız değişkenler oluşturabilirsiniz. Ardından değişkenlerinin parametre olarak kullanın. Betik değiştirmeden bir parametreyi değiştirmek, ancak yine de betik çalışma biçimini değiştirmek.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Bir runbook betikte bir basit dize değişkeni kullanın

Bu örnekte, bir komut dosyası ' % s'Giriş bir ağ güvenlik grubu (NSG) alır ve bir kurtarma planı Vm'lere uygulanır.

Kurtarma algılamak komut dosyası için plan çalıştığından, kurtarma planı bağlamı kullanın:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

Mevcut bir nsg için NSG adı ve NSG kaynak grubu adını bilmeniz gerekir. Bu değişkenler, girdi olarak kurtarma planı betikler için kullanın. Bunu yapmak için Otomasyon hesabı varlıkları iki değişken oluşturun. Parametreler için bir değişken adının öneki olarak oluşturduğunuz kurtarma planının adı ekleyin.

1. NSG adı depolamak için bir değişken oluşturun. Bir önek, değişken adı kurtarma planının adını kullanarak ekleyin.

    ![Bir NSG adı değişkeni oluşturun](media/site-recovery-runbook-automation-new/var1.png)

2. NSG'ın kaynak grubu adı depolamak için bir değişken oluşturun. Bir önek, değişken adı kurtarma planının adını kullanarak ekleyin.

    ![Bir NSG kaynak grubu adı oluşturma](media/site-recovery-runbook-automation-new/var2.png)


3.  Betikte, değişken değerlerini almak için aşağıdaki başvuru kodu kullanın:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Runbook değişkenlerinde devredilen sanal makinenin ağ arabirimine NSG'yi uygulamak için kullanın:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

Komut dosyasını kullanabilirsiniz, böylece her bir kurtarma planı için bağımsız değişken oluşturun. Kurtarma planı adı kullanarak bir ön ek ekleyin. İçin tam, uçtan uca betik bu senaryo için bkz [bir genel IP ve NSG Vm'lere Site Recovery kurtarma planı test yük devretme sırasında ekleme](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-to-store-more-information"></a>Daha fazla bilgi depolamak için bir karmaşık değişken kullanın

Belirli sanal makineler üzerinde bir genel IP etkinleştirmek için tek bir komut dosyası istediğiniz bir senaryo düşünün. Başka bir senaryoda, farklı Vm'lere (değil, tüm sanal makineler) üzerinde farklı Nsg'ler uygulamak isteyebilirsiniz. Herhangi bir kurtarma planı için yeniden kullanılabilir bir betik yapabilirsiniz. Her bir kurtarma planı Vm'leri değişken bir sayı olabilir. Örneğin, bir SharePoint kurtarma iki ön ucu vardır. Bir temel çizgi iş kolu (LOB) uygulaması, yalnızca bir ön uç vardır. Her bir kurtarma planı için ayrı değişkenler oluşturamaz.

Aşağıdaki örnekte, size yeni bir teknik kullanın ve oluşturma bir [karmaşık değişkeni](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureautomationvariable) Azure Otomasyonu hesabı varlıkları içinde. Birden çok değer belirterek bunu yapabilirsiniz. Aşağıdaki adımları tamamlamak için Azure PowerShell kullanmanız gerekir:

1. PowerShell'de, Azure aboneliğinizde oturum açın:

    ```
    Connect-AzureRmAccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. Parametreleri depolamak için kurtarma planının adını kullanarak karmaşık değişkeni oluşturun:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. Bu karmaşık değişkeninde **VMDetails** korumalı VM için VM kimliğidir. Azure portalında VM Kimliği almak için VM özelliklerini görüntüleyin. Aşağıdaki ekran görüntüsünde iki VM ayrıntılarını depolayan bir değişkeni gösterir:

    ![VM kimliği bir GUID olarak kullanın](media/site-recovery-runbook-automation-new/vmguid.png)

4. Runbook'unuza bu değişkeni kullanın. Belirtilen VM GUID'sine kurtarma planı bağlamda bulunursa, NSG VM üzerinde geçerlidir:

    ```
    $VMDetailsObj = (Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName).ToObject([hashtable])
    ```

4. Runbook'unuzda, kurtarma planı bağlamı VM'ler arasında döngü. Sanal Makinenin var olup olmadığını denetleyin **$VMDetailsObj**. Varsa, erişim NSG'yi uygulamak için değişkenin özellikleri:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            $VMDetails = $VMDetailsObj[$VMID].ToObject([hashtable]);
            Write-output $VMDetails
            if ($VMDetails -ne $Null) { #If the VM exists in the context, this will not be Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetails.NSGName
                $NSGRGname = $VMDetails.NSGResourceGroupName

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

Farklı kurtarma planları için aynı betiği kullanabilirsiniz. Bir kurtarma planı farklı değişkenlerine karşılık gelen değer depolayarak farklı parametreler girin.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Otomasyon hesabınıza örnek betikler dağıtmak için **azure'a Dağıt** düğmesi.

[![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Başka bir örnek için aşağıdaki videoya bakın. Bu, iki katmanlı bir WordPress uygulaması azure'a kurtarma göstermektedir:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]


## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Otomasyonu hizmeti Run As hesabı](../automation/automation-create-runas-account.md)
* [Azure Otomasyonu'na genel bakış](https://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Otomasyonu'na genel bakış")
* [Azure Otomasyonu örnek betikler](https://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Otomasyonu örnek betikler")

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme işlemleri çalıştırma hakkında.
