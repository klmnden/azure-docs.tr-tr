---
title: "Azure Site Recovery kurtarma planlarında Azure Otomasyon çalışma kitabı ekleme | Microsoft Docs"
description: "Azure Site Recovery kurtarma planları Azure otomasyonu kullanarak genişletme nasıl yardımcı olabileceğini öğrenin. Azure Kurtarma sırasında karmaşık görevleri tamamlamak öğrenin."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/09/2018
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 4802215f903eb196afbf05637ad5e38dbbbc09a3
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure Otomasyon çalışma kitabı kurtarma planlara Ekle
Bu makalede, Azure Site Recovery kurtarma planlarınızı genişletmek amacıyla Azure Automation ile nasıl tümleşik çalıştığını açıklar. Kurtarma planları korunan sanal makineleri kurtarma Site Recovery ile düzenleyebilirsiniz. Kurtarma planları hem ikincil bulutu için çoğaltma ve çoğaltma Azure için çalışır. Kurtarma planları de yardımcı kurtarma yapmak **tutarlı bir şekilde doğru**, **yinelenebilir**, ve **otomatik**. Vm'lerinizi Azure üzerinden başarısız olursa, Azure Automation tümleştirme kurtarma planlarınızı genişletir. Runbook'ları, ama güçlü bir otomatikleştirme görevleri teklif çalıştırmak için kullanabilirsiniz.

Azure Otomasyonu yeniyseniz, yapabilecekleriniz [kaydolun](https://azure.microsoft.com/services/automation/) ve [örnek komut dosyası yükleme](https://azure.microsoft.com/documentation/scripts/). Daha fazla bilgi için ve Azure Kurtarma kullanarak düzenlemek öğrenmek için [kurtarma planlarına](./site-recovery-create-recovery-plans.md), bkz: [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

Bu makalede, nasıl Azure Otomasyon çalışma kitabı kurtarma planlarınızı tümleştirebilir açıklanmaktadır. Daha önce el ile müdahale gerekli temel görevleri otomatikleştirmek için örnekler kullanırız. Biz de çok adımlı kurtarma için bir tek tıklamalı kurtarma eylemi dönüştürmek açıklar.

## <a name="customize-the-recovery-plan"></a>Kurtarma planı özelleştirme
1. Git **Site Recovery** kurtarma planı kaynak dikey. Bu örnekte, iki VM, kurtarma için eklenene kurtarma planına sahip. Bir runbook eklemeye başlamak için tıklatın **Özelleştir** sekmesi.

    ![Özelleştirme düğmesini tıklatın](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Sağ **Grup 1: Başlangıç**ve ardından **post eylem eklemek**.

    ![Sağ Grup 1: Başlatın ve sonrası eylemi ekleyin](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Tıklatın **bir komut dosyası seçin**.

4. Üzerinde **güncelleştirme eylem** dikey penceresinde, adı betik **Hello World**.

    ![Güncelleştirme eylem dikey penceresi](media/site-recovery-runbook-automation-new/update-rp.png)

5. Bir Otomasyon hesabı adı girin.
    >[!NOTE]
    > Otomasyon hesabının Azure herhangi bir bölgede olabilir. Otomasyon hesabının Azure Site kurtarma kasası ile aynı abonelikte olması gerekir.

6. Otomasyon hesabınızda bir runbook seçin. Bu runbook ilk grup kurtarma işleminden sonra kurtarma planı yürütülmesi sırasında çalışan komut dosyasıdır.

7. Komut dosyasını kaydetmek için tıklatın **Tamam**. Komut dosyası eklenen **Grup 1: sonrası adımları**.

    ![Eylem sonrası Grup 1:Start](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>Bir komut dosyası eklemek için dikkat edilecek noktalar

* Seçenekler için **bir adım silme** veya **betik güncelleştirme**, komut dosyasını sağ tıklatın.
* Bir komut dosyası Azure üzerinde yük devretme sırasında bir şirket içi makineden Azure'a çalıştırabilirsiniz. Ayrıca Azure üzerinde bir birincil site komut dosyası kapatma önce olarak azure'dan şirket içi makineye sırasında çalıştırabilirsiniz.
* Bir komut dosyası çalıştığında, bir kurtarma planı içeriği yerleştirir. Aşağıdaki örnek, bir bağlam değişkeni gösterir:

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

    Aşağıdaki tabloda, ad ve açıklama bağlamında her değişkenin listeler.

    | **Değişken adı** | **Açıklama** |
    | --- | --- |
    | RecoveryPlanName |Çalıştırılan planın adı. Bu değişken, kurtarma planı adına göre farklı eylemleri yardımcı olur. Ayrıca komut dosyasını yeniden kullanabilirsiniz. |
    | FailoverType |Yük devretme testi olup olmadığını belirtir, planlı veya plansız. |
    | FailoverDirection |Kurtarma için birincil veya ikincil bir siteye olup olmadığını belirtir. |
    | GroupID |Plan çalıştırırken Grup numarası kurtarma planında tanımlar. |
    | VmMap |Gruptaki tüm sanal makineleri bir dizi. |
    | VMMap anahtarı |Her VM için benzersiz anahtar (GUID). Bu VM, Azure Virtual Machine Manager (VMM) Kimliğini uygunsa aynıdır. |
    | SubscriptionId |VM oluşturulduğu Azure abonelik kimliği. |
    | RoleName |Kurtarılacak Azure VM adı. |
    | CloudServiceName |VM oluşturulduğu Azure bulut hizmeti adı. |
    | ResourceGroupName|VM oluşturulduğu Azure kaynak grubu adı. |
    | RecoveryPointId|VM zaman kurtarılan için zaman damgası. |

* Otomasyon hesabı aşağıdaki modüller sahip olduğundan emin olun:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Tüm modüllerin uyumlu sürümlerini olmalıdır. Tüm modüllerin uyumlu olduğundan emin olmak için kolay bir yol tüm modülleri en son sürümlerini kullanmaktır.

### <a name="access-all-vms-of-the-vmmap-in-a-loop"></a>Döngü VMMap tüm Vm'leri erişim
Microsoft VMMap tüm VM'ler arasında döngü için aşağıdaki kodu kullanın:

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
> Komut dosyasını bir ön eylem önyükleme grubuna olduğunda kaynak grubu adı ve rol adı değeri boş. Yalnızca bu grubun VM yük devretme kümesinde başarılı olursa değerleri doldurulur. Komut dosyası, önyükleme grubunun sonrası bir işlemdir.

## <a name="use-the-same-automation-runbook-in-multiple-recovery-plans"></a>Birden çok kurtarma planları aynı Otomasyon runbook'u kullanın

Dış değişkenler kullanarak birden çok kurtarma planları tek bir komut dosyası kullanabilirsiniz. Kullanabileceğiniz [Azure Otomasyon değişkenleri](../automation/automation-variables.md) depolamak için bir kurtarma planı yürütme geçirdiğiniz parametreler için. Kurtarma planı adı önek olarak değişkenine için ekleyerek, her kurtarma planı için bağımsız değişkenleri oluşturabilirsiniz. Ardından, değişkenleri olarak parametreler kullanın. Komut dosyası olmadan bir parametre değiştirmek, ancak hala komut dosyası çalışma biçimini değiştirme.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Basit bir dize değişkeni bir runbook komut dosyası kullanın

Bu örnekte, bir komut dosyası bir ağ güvenlik grubu (NSG) girdi alır ve bir kurtarma planı VM'ler için geçerlidir.

Hangi kurtarma algılamak için komut dosyası için plan çalıştığından, kurtarma planı bağlam kullanın:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

Varolan bir NSG'yi uygulamak için NSG adını ve NSG kaynak grubu adını bilmeniz gerekir. Bu değişkenler kurtarma planı komut için giriş olarak kullanın. Bunu yapmak için Otomasyon hesabı varlıkları iki değişken oluşturun. Değişken adı öneki olarak parametrelerini oluşturmakta olduğunuz kurtarma planının adı ekleyin.

1. NSG adı depolamak için bir değişken oluşturun. Bir önek kurtarma planının adı kullanarak değişken adına ekleyin.

    ![Bir NSG adı değişkeni oluşturun](media/site-recovery-runbook-automation-new/var1.png)

2. NSG'ın kaynak grubu adı depolamak için bir değişken oluşturun. Bir önek kurtarma planının adı kullanarak değişken adına ekleyin.

    ![Bir NSG kaynak grubu adı oluşturma](media/site-recovery-runbook-automation-new/var2.png)


3.  Komut dosyasında aşağıdaki başvuru kodu değişken değerleri almak için kullanın:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  NSG başarısız üzerinden VM ağ arabirimine uygulamak için runbook değişkenlerde kullanın:

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

Her kurtarma planı için bağımsız değişkenleri oluşturun, böylece komut dosyasını yeniden kullanabilirsiniz. Kurtarma planı adı kullanarak bir önek ekleyin. Bir tam, uçtan uca komut dosyası için bu senaryo için bkz: [bir ortak IP NSG VM'ler için bir Site Recovery kurtarma planı test yük devretmesi sırasında ekleyip](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-to-store-more-information"></a>Daha fazla bilgi depolamak için bir karmaşık değişkenini kullanın

Genel IP belirli vm'lerde etkinleştirmek için tek bir komut dosyası istediğiniz bir senaryo düşünün. Başka bir senaryoda, farklı sanal makineleri (değil, tüm VM'ler) üzerinde farklı Nsg'leri uygulamak isteyebilirsiniz. Herhangi bir kurtarma planı için yeniden kullanılabilir bir komut dosyası yapabilirsiniz. Her kurtarma planı VM'ler değişken sayıda olabilir. Örneğin, bir SharePoint kurtarma iki ön ucu vardır. Bir temel çizgi iş kolu (LOB) uygulaması yalnızca bir ön uç vardır. Her kurtarma planı için ayrı değişkenler oluşturamaz.

Aşağıdaki örnekte, yeni bir teknik kullanır ve oluşturma bir [karmaşık değişkeni](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) Azure Otomasyon hesabı varlıkları içinde. Bunun için birden çok değer belirterek. Aşağıdaki adımları tamamlamak için Azure PowerShell kullanmanız gerekir:

1. PowerShell'de, Azure aboneliğinizde oturum açın:

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. Parametreleri depolamak için kurtarma planının adı kullanarak karmaşık değişkeni oluşturun:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. Bu karmaşık değişkeninde **VMDetails** korumalı VM için VM kimliğidir. Azure portalında VM Kimliği almak için VM özelliklerini görüntüleyin. Aşağıdaki ekran görüntüsünde, iki VM ayrıntılarını depolayan bir değişken gösterir:

    ![VM kimliği GUID olarak kullanın](media/site-recovery-runbook-automation-new/vmguid.png)

4. Runbook'unuzda bu değişkeni kullanın. Belirtilen VM GUID kurtarma planı bağlamda bulunursa, NSG VM Uygula:

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. Runbook'unuzda, kurtarma planı bağlam VM'ler döngü. VM var olup olmadığını denetleyin **$VMDetailsObj**. Varsa, erişim NSG'yi uygulamak için değişken özellikleri:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If the VM exists in the context, this will not b Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

Farklı bir kurtarma planları için aynı komut dosyasını kullanabilirsiniz. Bir kurtarma planı farklı değişkenlerine karşılık gelen değer depolayarak farklı parametreler girin.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Automation hesabınız için örnek komut dosyaları dağıtmak için **Azure'a Dağıt** düğmesi.

[![Azure’a dağıtma](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Başka bir örnek için aşağıdaki videoya bakın. İki katmanlı WordPress uygulamayı azure'a kurtarmak nasıl gösterir:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]


## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Otomasyon hizmeti farklı çalıştır hesabı](../automation/automation-create-runas-account.md)
* [Azure Otomasyonu genel bakış](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Otomasyonu genel bakış")
* [Azure Automation örnek betikler](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation örnek betikler")

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme işlemleri çalıştırma hakkında.
