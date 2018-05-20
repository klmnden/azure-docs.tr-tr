---
title: Azure yığın 1804 güncelleştirme | Microsoft Docs
description: Azure yığın 1804 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: dde2783db08ec00696a70c0cad08ca211194a470
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="azure-stack-1804-update"></a>Azure yığın 1804 güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye 1804 güncelleştirme paketindeki giderir. Bilinen sorunlar için güncelleştirme işlemini doğrudan ilgili sorunlar ve yapı (yükleme sonrası) ile ilgili sorunları ayrılır.

> [!IMPORTANT]        
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemleri içindir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="build-reference"></a>Derleme başvurusu    
Azure yığın 1804 güncelleştirme yapı numarası **20180513.1**.   

### <a name="new-features"></a>Yeni Özellikler
Bu güncelleştirme Azure yığını için aşağıdaki geliştirmeleri içerir.

- <!-- 15028744 - IS -->  **Visual Studio support for disconnected Azure Stack deployments using AD FS**. Within Visual Studio you now can add subscriptions and authenticate using AD FS federated User credentials. 
 
- <!-- 1779474, 1779458 - IS --> **Use Av2 and F series virtual machines**. Azure Stack can now use virtual machines based on the Av2-series and F-series virtual machine sizes. For more information see [Virtual machine sizes supported in Azure Stack](/user/azure-stack-vm-sizes.md). 

- <!-- 1759172 - IS, ASDK --> **More granular administrative subscriptions**. With version 1804 and later, the Default Provider subscription is now complemented with two additional subscriptions. The additions facilitate separating the management of core infrastructure, additional resource providers, and workloads. The following three subscriptions are available after the update installs:
  - *Sağlayıcı abonelik varsayılan*. Bu abonelik, yalnızca çekirdek altyapıyı kullanın. Kaynaklar veya kaynak sağlayıcıları bu abonelikte dağıtmayın.
  - *Abonelik ölçümü*. Bu abonelik, kaynak sağlayıcısı dağıtım için kullanın. Bu abonelikte dağıtılan kaynak sizden ücret istenmese.
  - *Tüketim abonelik*. Bu abonelik, dağıtmak istediğiniz tüm diğer iş yükü için kullanın. Burada dağıtılan kaynak normal kullanım fiyatlar sizden ücret kesilir.


## <a name="fixed-issues"></a>Giderilen sorunlar

- <!-- IS, ASDK -->  In the admin portal, you no longer have to refresh the Update tile before it displays information.
 
- <!-- 2050709  -->  You can now use the admin portal to edit storage metrics for Blob service, Table service, and Queue service.
 
- <!-- IS, ASDK --> Under **Networking**, when you click **Connection** to set up a VPN connection, **Site-to-site (IPsec)** is now the only available option.

- **Çeşitli düzeltmeleri** performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için.

## <a name="additional-releases-timed-with-this-update"></a>Bu güncelleştirmeyle zaman aşımına ek sürümler  
Aşağıdaki artık kullanılabilir, ancak Azure yığın güncelleştirme 1804 gerektirmez.
- **Güncelleştirme izleme paketi Microsoft Azure yığın System Center Operations Manager**. Yeni bir sürümünü (1.0.3.0) Microsoft System Center Operations Manager izleme paketi Azure yığın kullanılabilir [karşıdan](https://www.microsoft.com/download/details.aspx?id=55184). Bağlı bir Azure yığın dağıtımına eklediğinizde bu sürümle birlikte, hizmet asıl adı kullanabilirsiniz. Bu sürüm ayrıca Operations Manager içinde düzeltme eyleme doğrudan izin veren bir güncelleştirme yönetim deneyimi sunar. Kaynak sağlayıcıları görüntülemek, birimleri ölçeklendirme ve birim düğümleri ölçeklendirme yeni panolar vardır.

- **Yeni Azure yığın yönetici PowerShell sürümü 1.2.12**.  Azure yığın PowerShell 1.2.12 yükleme için kullanıma sunulmuştur. Bu sürüm Azure yığın yönetmek tüm yönetim kaynak sağlayıcıları için komutları sağlar.  Bu sürümle birlikte, bazı içerikler Azure yığın araçları Github'dan kullanım dışı kalacaktır [depo](https://github.com/Azure/AzureStack-Tools). 

   Yükleme ayrıntılarını izleyin [yönergeleri](azure-stack-powershell-install.md) veya [yardımcı](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.2.12) Azure yığın modülü 1.2.12 için içerik. 

- **İlk Azure yığın API Rest başvurusu sürümü**. Tüm Azure yığın yönetici kaynak sağlayıcısı için API Başvurusu şimdi yayımlanır



## <a name="before-you-begin"></a>Başlamadan önce    

### <a name="prerequisites"></a>Önkoşullar
- Azure yığın yükleme [1803 güncelleştirme](azure-stack-update-1803.md) Azure yığın 1804 güncelleştirmeyi uygulamadan önce.    

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar   
- <!-- TBD - IS --> Do not attempt to create virtual machines during the installation of this update. For more information about managing updates, see [Manage updates in Azure Stack overview](azure-stack-updates.md#plan-for-updates).
 
- <!-- 2328416 - IS --> During installation of the 1804 update, there can be downtime of the blob service and internal services that use blob service. This includes some virtual machine operations. This down time can cause failures of tenant operations or alerts from services that can’t access data. This issue resolves itself when the update completes installation. 

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar
*Güncelleştirme sonrası adımı 1804 güncelleştirmesi yoktur.*



### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)
Derleme için yükleme sonrası bilinen sorunlar verilmiştir **20180513.1**.

#### <a name="portal"></a>Portal
- <!-- TBD -  IS ASDK -->The ability [to open a new support request from the dropdown](azure-stack-manage-portals.md#quick-access-to-help-and-support) from within the administrator portal isn’t available. Instead, use the following link:     
    - Azure yığını için tümleşik sistemleri kullanan https://aka.ms/newsupportrequest.

- <!-- 2403291 - IS ASDK --> You might not have use of the horizontal scroll bar along the bottom of the admin and user portals. If you can’t access the horizontal scroll bar, use the breadcrumbs to navigate to a previous blade in the portal by selecting the name of the blade you want to view from the breadcrumb list found at the top left of the portal.
  ![İçerik haritası](media/azure-stack-update-1804/breadcrumb.png) 

- <!-- TBD - IS --> It might not be possible to view compute or storage resources in the administrator portal. The cause of this issue is an error during the installation of the update that causes the update to be incorrectly reported as successful. If this issue occurs, contact Microsoft Customer Support Services for assistance.

- <!-- TBD - IS --> You might see a blank dashboard in the portal. To recover the dashboard, select the gear icon in the upper right corner of the portal, and then select **Restore default settings**.

- <!-- TBD - IS ASDK --> Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete user subscriptions.

- <!-- TBD - IS ASDK --> You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use PowerShell to verify permissions.

- <!-- TBD - IS ASDK --> In the admin portal, you might see a critical alert for the *Microsoft.Update.Admin* component. The Alert name, description, and remediation all display as:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

  Bu uyarı güvenle yoksayılabilir. 


#### <a name="compute"></a>İşlem
- <!-- TBD - IS ASDK --> Scaling settings for virtual machine scale sets are not available in the portal. As a workaround, you can use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Because of PowerShell version differences, you must use the `-Name` parameter instead of `-VMScaleSetName`.

- <!-- TBD - IS --> When you create an availability set in the portal by going to **New** > **Compute** > **Availability set**, you can only create an availability set with a fault domain and update domain of 1. As a workaround, when creating a new virtual machine, create the availability set by using PowerShell, CLI, or from within the portal.

- <!-- TBD - IS ASDK --> When you create virtual machines on the Azure Stack user portal, the portal displays an incorrect number of data disks that can attach to a DS series VM. DS series VMs can accommodate as many data disks as the Azure configuration.

- <!-- TBD - IS ASDK --> When a VM image fails to be created, a failed item that you cannot delete might be added to the VM images compute blade.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

- <!-- TBD - IS ASDK --> If provisioning an extension on a VM deployment takes too long, users should let the provisioning time-out instead of trying to stop the process to deallocate or delete the VM.  

- <!-- 1662991 IS ASDK --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  


#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS ASDK --> Under **Networking**, if you click **Create VPN Gateway** to set up a VPN connection, **Policy Based** is listed as a VPN type. Do not select this option. Only the **Route Based** option is supported in Azure Stack.

- <!-- 2388980 - IS ASDK --> After a VM is created and associated with a public IP address, you can't disassociate that VM from that IP address. Disassociation appears to work, but the previously assigned public IP address remains associated with the original VM.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> If you raise a Quota limit for a Network resource that is part of an Offer and Plan that is associated with a tenant subscription, the new limit is not applied to that subscription. However, the new limit does apply to new subscriptions that are created after the quota is increased. 

  Bu sorunu geçici olarak çözmek için plan zaten bir aboneliği ile ilişkili olduğunda ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz: nasıl yapılır [bir eklenti planı kullanılabilir hale](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> You cannot delete a subscription that has DNS Zone resources or Route Table resources associated with it. To successfully delete the subscription, you must first delete DNS Zone and Route Table resources from the tenant subscription. 
  

- <!-- 1902460 - IS ASDK --> Azure Stack supports a single *local network gateway* per IP address. This is true across all tenant subscriptions. After the creation of the first local network gateway connection, subsequent attempts to create a local network gateway resource with the same IP address are blocked.

- <!-- 16309153 - IS ASDK --> On a Virtual Network that was created with a DNS Server setting of *Automatic*, changing to a custom DNS Server fails. The updated settings are not pushed to VMs in that Vnet.

- <!-- TBD - IS ASDK --> Azure Stack does not support adding additional network interfaces to a VM instance after the VM is deployed. If the VM requires more than one network interface, they must be defined at deployment time.

- <!-- 2096388 IS --> You cannot use the admin portal to update rules for a network security group. 

    Uygulama hizmeti için geçici çözüm: Denetleyici örnekleri için Uzak Masaüstü'nü gerekiyorsa, PowerShell ile ağ güvenlik grupları içindeki güvenlik kuralları Değiştir.  Nasıl yapılır örnekleri şunlardır *izin*ve yapılandırmasını geri yüklemek *Reddet*:  
    
    - *İzin ver:*
 
      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
      
      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
      
      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
      
      ##This doesn’t work. Need to set properties again even in case of edit
      
      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
      
      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
      
      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Reddet:*

        ```powershell
        
        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
        
        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
        
        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
        
        ##This doesn’t work. Need to set properties again even in case of edit
    
        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
    
        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
          
        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg 
        ```

#### <a name="sql-and-mysql"></a>SQL ve MySQL

- <!-- TBD - IS --> Only the resource provider is supported to create items on servers that host SQL or MySQL. Items created on a host server that are not created by the resource provider might result in a mismatched state.  

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** or **Tier** names when you create a SKU for the SQL and MySQL resource providers.


> [!NOTE]  
> <!-- TBD - IS --> After you update to Azure Stack 1804, you can continue to use the SQL and MySQL resource providers that you previously deployed.  We recommend you update SQL and MySQL when a new release becomes available. Like Azure Stack, apply updates to SQL and MySQL resource providers sequentially.  For example, if you use version 1802, first apply version 1803, and then update to 1804.      
>   
> Güncelleştirme 1804 yüklemesini SQL veya MySQL kaynak sağlayıcıları kullanıcılarınız tarafından geçerli kullanımını etkilemez.
> Kullandığınız kaynak sağlayıcıları sürümü, bağımsız olarak kendi veritabanlarını kullanıcılar verilerinizi touched değil ve erişilebilir kalır.    



#### <a name="app-service"></a>App Service
- <!-- TBD - IS ASDK --> Users must register the storage resource provider before they create their first Azure Function in the subscription.

- <!-- TBD - IS ASDK --> In order to scale out infrastructure (workers, management, front-end roles), you must use PowerShell as described in the release notes for Compute.


#### <a name="usage"></a>Kullanım  
- <!-- TBD - IS ASDK --> Usage Public IP address usage meter data shows the same *EventDateTime* value for each record instead of the *TimeDate* stamp that shows when the record was created. Currently, you can’t use this data to perform accurate accounting of public IP address usage.


<!-- #### Identity -->
<!-- #### Health and monitoring --> 
<!-- #### Marketplace --> 


## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle
Azure yığın 1804 güncelleştirme paketinden indirebilirsiniz [burada](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Ayrıca bkz.
- İzleme ve güncelleştirmeleri sürdürmek için ayrıcalıklı uç noktası (CESARETLENDİRİCİ) kullanmak için bkz: [izlemek Azure kullanan ayrıcalıklı uç noktasını yığınında güncelleştirmeleri](azure-stack-monitor-update.md).
- Güncelleştirme yönetimi Azure yığınında genel bakış için bkz: [yönetmek güncelleştirmeleri Azure yığın genel bakış](azure-stack-updates.md).
- Azure yığın güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md).
