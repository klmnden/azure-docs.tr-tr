---
title: Microsoft Azure yığın Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeler, düzeltmeler ve Azure yığın Geliştirme Seti için bilinen sorunlar
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: 2aa6663ae598b32979411fd10e7bb8a2eacd21de
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure yığın Geliştirme Seti sürüm notları
Bu sürüm notları geliştirmeleri, düzeltmeler ve Azure yığın Geliştirme Seti bilinen sorunlar hakkında bilgi sağlar. Çalıştırdığınız hangi sürümünün emin değilseniz, yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](.\.\azure-stack-updates.md#determine-the-current-version).

> Abone tarafından ASDK yenilikler ile güncel kalmasını [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).


## <a name="build-201805131"></a>Yapı 20180513.1

### <a name="new-features"></a>Yeni Özellikler 
Bu yapı aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.  

- <!-- 1759172 - IS, ASDK --> **More granular administrative subscriptions**. With version 1804 and later, the Default Provider subscription is now complemented with two additional subscriptions. The additions facilitate separating the management of core infrastructure, additional resource providers, and workloads. The following three subscriptions are available:
  - *Sağlayıcı abonelik varsayılan*. Bu abonelik, yalnızca çekirdek altyapıyı kullanın. Kaynaklar veya kaynak sağlayıcıları bu abonelikte dağıtmayın.
  - *Abonelik ölçümü*. Bu abonelik, kaynak sağlayıcısı dağıtım için kullanın. Bu abonelikte dağıtılan kaynak sizden ücret istenmese.
  - *Tüketim abonelik*. Bu abonelik, dağıtmak istediğiniz tüm diğer iş yükü için kullanın. Burada dağıtılan kaynak normal kullanım fiyatlar sizden ücret kesilir.


### <a name="fixed-issues"></a>Giderilen sorunlar
- <!-- IS, ASDK -->  In the admin portal, you no longer have to refresh the Update tile before it displays information. 

- <!-- 2050709 - IS, ASDK -->  You can now use the admin portal to edit storage metrics for Blob service, Table service, and Queue service.

- <!-- IS, ASDK --> Under **Networking**, when you click **Connection** to set up a VPN connection, **Site-to-site (IPsec)** is now the only available option. 

- **Çeşitli düzeltmeleri** performans, sağlamlık, güvenlik ve Azure yığını tarafından kullanılan işletim sistemi için

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Bu güncelleştirmeyle zaman aşımına ek sürümler  
Aşağıdaki artık kullanılabilir, ancak Azure yığın güncelleştirme 1804 gerektirmez.
- **Güncelleştirme izleme paketi Microsoft Azure yığın System Center Operations Manager**. Yeni bir sürümünü (1.0.3.0) Microsoft System Center Operations Manager izleme paketi Azure yığın kullanılabilir [karşıdan](https://www.microsoft.com/download/details.aspx?id=55184). Bağlı bir Azure yığın dağıtımına eklediğinizde bu sürümle birlikte, hizmet asıl adı kullanabilirsiniz. Bu sürüm ayrıca Operations Manager içinde düzeltme eyleme doğrudan izin veren bir güncelleştirme yönetim deneyimi sunar. Kaynak sağlayıcıları görüntülemek, birimleri ölçeklendirme ve birim düğümleri ölçeklendirme yeni panolar vardır.

- **Yeni Azure yığın yönetici PowerShell sürümü 1.2.12**.  Azure yığın PowerShell 1.2.12 yükleme için kullanıma sunulmuştur. Bu sürüm Azure yığın yönetmek tüm yönetim kaynak sağlayıcıları için komutları sağlar.  Bu sürümle birlikte, bazı içerikler Azure yığın araçları Github'dan kullanım dışı kalacaktır [depo](https://github.com/Azure/AzureStack-Tools). 

   Yükleme ayrıntılarını izleyin [yönergeleri](.\.\azure-stack-powershell-install.md) veya [yardımcı](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.2.12) Azure yığın modülü 1.2.12 için içerik. 

- **İlk Azure yığın API Rest başvurusu sürümü**. Tüm Azure yığın yönetici kaynak sağlayıcısı için API Başvurusu şimdi yayımlanır

### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> The ability [to open a new support request from the dropdown](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) from within the administrator portal isn’t available. Instead, use the following link:     
    - Azure yığın Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> You might not have use of the horizontal scroll bar along the bottom of the admin and user portals. If you can’t access the horizontal scroll bar, use the breadcrumbs to navigate to a previous blade in the portal by selecting the name of the blade you want to view from the breadcrumb list found at the top left of the portal.
  ![İçerik haritası](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete user subscriptions.

- <!-- TBD -  IS ASDK --> You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use PowerShell to verify permissions.

-   <!-- TBD -  IS ASDK --> In the admin portal, you might see a critical alert for the Microsoft.Update.Admin component. The Alert name, description, and remediation all display as:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

    Bu uyarı güvenle yoksayılabilir. 

#### <a name="compute"></a>İşlem
- <!-- TBD -  IS ASDK --> Scaling settings for virtual machine scale sets are not available in the portal. As a workaround, you can use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Because of PowerShell version differences, you must use the `-Name` parameter instead of `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> When you create virtual machines on the Azure Stack user portal, the portal displays an incorrect number of data disks that can attach to a DS series VM. DS series VMs can accommodate as many data disks as the Azure configuration.

- <!-- TBD -  IS ASDK --> When a VM image fails to be created, a failed item that you cannot delete might be added to the VM images compute blade.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

- <!-- TBD -  IS ASDK --> If provisioning an extension on a VM deployment takes too long, users should let the provisioning time-out instead of trying to stop the process to deallocate or delete the VM.  

- <!-- 1662991 - IS ASDK --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 

#### <a name="networking"></a>Ağ
- <!-- 1766332 - IS, ASDK --> Under **Networking**, if you click **Create VPN Gateway** to set up a VPN connection, **Policy Based** is listed as a VPN type. Do not select this option. Only the **Route Based** option is supported in Azure Stack.

- <!-- 2388980 -  IS ASDK --> After a VM is created and associated with a public IP address, you can't disassociate that VM from that IP address. Disassociation appears to work, but the previously assigned public IP address remains associated with the original VM.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

- <!-- 2292271 - IS ASDK --> If you raise a Quota limit for a Network resource that is part of an Offer and Plan that is associated with a tenant subscription, the new limit is not applied to that subscription. However, the new limit does apply to new subscriptions that are created after the quota is increased. 

  Bu sorunu geçici olarak çözmek için plan zaten bir aboneliği ile ilişkili olduğunda ağ Kotayı artırmak için bir eklenti planı kullanın. Daha fazla bilgi için bkz: nasıl yapılır [bir eklenti planı kullanılabilir hale](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> You cannot delete a subscription that has DNS Zone resources or Route Table resources associated with it. To successfully delete the subscription, you must first delete DNS Zone and Route Table resources from the tenant subscription. 


- <!-- 1902460 -  IS ASDK --> Azure Stack supports a single *local network gateway* per IP address. This is true across all tenant subscriptions. After the creation of the first local network gateway connection, subsequent attempts to create a local network gateway resource with the same IP address are blocked.

- <!-- 16309153 -  IS ASDK --> On a Virtual Network that was created with a DNS Server setting of *Automatic*, changing to a custom DNS Server fails. The updated settings are not pushed to VMs in that Vnet.
 
- <!-- TBD -  IS ASDK --> Azure Stack does not support adding additional network interfaces to a VM instance after the VM is deployed. If the VM requires more than one network interface, they must be defined at deployment time.


#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- <!-- TBD - ASDK --> The database hosting servers must be dedicated for use by the resource provider and user workloads. You cannot use an instance that is being used by any other consumer, including App Services.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Users must register the storage resource provider before they create their first Azure Function in the subscription.

- <!-- TBD -  IS ASDK --> In order to scale out infrastructure (workers, management, front-end roles), you must use PowerShell as described in the release notes for Compute.
 
#### <a name="usage"></a>Kullanım  
- <!-- TBD -  IS ASDK --> Usage Public IP address usage meter data shows the same *EventDateTime* value for each record instead of the *TimeDate* stamp that shows when the record was created. Currently, you can’t use this data to perform accurate accounting of public IP address usage.

<!-- #### Identity -->






## <a name="build-201803291"></a>Yapı 20180329.1

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Yeni özellikleri ve düzeltmeleri Azure tümleşik sistemleri sürüm 1803 uygulamak için Azure yığın Geliştirme Seti yığınının yayımladı. Bkz: [yeni özellikler](.\.\azure-stack-update-1803.md#new-features) ve [giderilen sorunlar](.\.\azure-stack-update-1803.md#fixed-issues) Azure yığın 1803 bölümlerini Güncelleştirme ayrıntıları için sürüm notları.  
> [!IMPORTANT]    
> Listelenen öğelerin bazıları **yeni özellikler** ve **giderilen sorunlar** bölümleri yalnızca Azure tümleşik yığını sistemler için ilgili.

### <a name="changes"></a>Değişiklikler
- Yeni oluşturulan bir tekliften durumunu değiştirmek için yol *özel* için *ortak* veya *yetkisi alınmış* değişti. Daha fazla bilgi için bkz: [teklifi oluşturmak](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- Özelliği [aşağı açılır listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure yığın Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Gördüğünüz bir **gerekli etkinleştirme** Azure yığın Geliştirme Seti kaydetmek için öneren bir uyarı bildirimi. Bu davranış beklenir.

- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nı panosunda güncelleştirmeler hakkında bilgi görüntülemek güncelleştirme döşeme başarısız olur. Bu sorunu çözmek için yenilemeniz kutucuğa tıklayın.

-   Yönetim Portalı'nda Microsoft.Update.Admin bileşeni için kritik bir uyarı görebilirsiniz. Uyarı adı, açıklama ve düzeltmesi tümü olarak görüntülenir:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

    Bu uyarı güvenle yoksayılabilir. 



#### <a name="marketplace"></a>Market
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.
 
#### <a name="compute"></a>İşlem
- Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal DS serisi VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

-  VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Ağ
- Altında **ağ**, tıklatırsanız **bağlantı** bir VPN bağlantısı kurmak için **VNet-VNet** olası bağlantı türü olarak listelenir. Bu seçeneği belirlemeyin. Şu anda yalnızca **siteden siteye (IPSec)** seçeneği desteklenir.

- Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.



- Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.
 
- Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.



#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- Kullanıcıların veritabanlarını yeni bir SQL veya MySQL SKU oluşturabilmeniz için önce en çok bir saat sürebilir.

- Veritabanı sunucuları barındırma kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan örneğini kullanamazsınız.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** or **Tier** names when you create a SKU for the SQL and MySQL resource providers.

#### <a name="app-service"></a>App Service
- Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.
 
#### <a name="usage"></a>Kullanım  
- Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan indirme
- Kullanırken *çağırma webrequest* Azure yığın indirmek için PowerShell cmdlet araçları Github'dan, hata iletisi:     
    -  *çağırma webrequest: istek iptal edildi: SSL/TLS güvenli kanalı oluşturulamadı.*     

  Bir son GitHub destek kullanımdan Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan) nedeniyle bu hata oluşur. Daha fazla bilgi için bkz: [zayıf şifreleme standartları kaldırma bildirimi](https://githubengineering.com/crypto-removal-notice/).

  Bu sorunu çözmek için ekleme `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` TLSv1.2 GitHub havuzların indirirken kullanmak için PowerShell konsolunu zorlamak için komut dosyasının en üstüne.






## <a name="build-201803021"></a>Yapı 20180302.1

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Bkz: [yeni özellikler ve düzeltmeler](.\.\azure-stack-update-1802.md#new-features-and-fixes) Azure yığınının Azure yığın 1802 güncelleştirme Sürüm Notları bölümünü tümleşik sistemler.

> [!IMPORTANT]    
> Listelenen öğelerin bazıları **yeni özellikler ve düzeltmeler** bölüm yalnızca Azure tümleşik yığını sistemler için ilgili.


### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="portal"></a>Portal
- Özelliği [aşağı açılır listeden yeni bir destek isteği açma](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) gelen içinde Yönetici portalı kullanılamıyor. Bunun yerine, aşağıdaki bağlantıyı kullanın:     
    - Azure yığın Geliştirme Seti için kullanmak https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Gördüğünüz bir **gerekli etkinleştirme** Azure yığın Geliştirme Seti kaydetmek için öneren bir uyarı bildirimi. Bu davranış beklenir.

- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.

- Azure yığın Portal kullanımı aboneliğinizi izinleri görüntüleyemezsiniz. Geçici bir çözüm olarak, izinleri doğrulamak için PowerShell kullanın.

- Yönetim Portalı'nı panosunda güncelleştirmeler hakkında bilgi görüntülemek güncelleştirme döşeme başarısız olur. Bu sorunu çözmek için yenilemeniz kutucuğa tıklayın.

-   Yönetim Portalı'nda Microsoft.Update.Admin bileşeni için kritik bir uyarı görebilirsiniz. Uyarı adı, açıklama ve düzeltmesi tümü olarak görüntülenir:  
    - *HATA - FaultType ResourceProviderTimeout için şablon eksik.*

    Bu uyarı güvenle yoksayılabilir. 

- Hem Yönetim Portalı ve Kullanıcı Portalı, daha eski bir API sürümüyle oluşturulmuş depolama hesapları için genel bakış dikey seçtiğinizde yüklemeye genel bakış dikey penceresinde başarısız (örnek: 2015-06-15). 

  Geçici bir çözüm olarak çalıştırmak için PowerShell kullanın **başlangıç ResourceSynchronization.ps1** için depolama hesabı ayrıntıları erişimi geri yüklemek için komut dosyası. [Komut dosyası Github'dan edinilebilir]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)ve ASDK kullanırsanız, Hizmet Yöneticisi kimlik bilgileriyle Geliştirme Seti ana bilgisayarda çalıştırmanız gerekir.  

- **Hizmet durumu** dikey başarısız yüklenemiyor. Yönetici veya Kullanıcı Portalı'nda Azure yığın hizmet durumu dikey penceresini açtığınızda bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açın, ancak bu özellik henüz kullanılabilir değil ancak Azure yığın gelecek bir sürümünde uygulanacaktır.

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme
Azure yığın Yönetim Portalı'nda ada sahip bir kritik uyarı görebilirsiniz **dış sertifika sona erme bekleyen**.  Bu uyarı güvenle yoksayılabilir ve Azure yığın Geliştirme Seti işlemleri etkilemez. 


#### <a name="marketplace"></a>Market
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.
 
#### <a name="compute"></a>İşlem
- Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

- Azure yığını yalnızca sabit türü VHD'lerin destekler. Dinamik VHD Azure yığında Market üzerinden sunulan bazı görüntüleri kullanır ancak bu kaldırıldı. Ekli dinamik bir diski bir sanal makine (VM) yeniden boyutlandırma VM başarısız durumda bırakır.

  Bu sorunu azaltmak için VM'in disk, VHD blob depolama hesabındaki silmeden VM silin. Ardından VHD'yi dinamik bir disk, sabit bir diske Dönüştür ve sanal makine yeniden oluşturun.

- Sanal makineler Azure yığın kullanıcı portalında oluşturduğunuzda, portal DS serisi VM iliştirebilirsiniz veri diskleri yanlış sayıda görüntüler. DS serisi VM'ler sayıda veri diski Azure yapılandırması sağlayabilir.

- Oluşturulacak bir VM görüntüsü başarısız olduğunda, VM görüntüleri işlem dikey penceresine eklenen silemezsiniz başarısız bir öğe.

  Geçici bir çözüm olarak, Hyper-V ile oluşturulan bir kukla VHD ile yeni bir VM görüntüsü oluşturma (yeni-VHD-yol C:\dummy.vhd-- SizeBytes sabit 1 GB). Bu işlem başarısız öğesini silmeden engeller sorunu çözer. Ardından, kukla görüntüsünü oluşturduktan sonra 15 dakika başarılı bir şekilde silebilirsiniz.

  Ayrıca, daha önce başarısız VM görüntüsü yeniden indirin daha sonra deneyebilirsiniz.

-  VM dağıtımı üzerinde uzantı sağlama çok uzun sürerse, kullanıcıların serbest bırakma veya VM silme işlemi durdurulmaya çalışılırken yerine sağlama zaman aşımı izin vermemelisiniz.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Ağ
- Altında **ağ**, tıklatırsanız **bağlantı** bir VPN bağlantısı kurmak için **VNet-VNet** olası bağlantı türü olarak listelenir. Bu seçeneği belirlemeyin. Şu anda yalnızca **siteden siteye (IPSec)** seçeneği desteklenir.

- Bir VM oluşturulur ve bir ortak IP adresi ile ilişkili sonra bu VM IP adresinden ilişkisini olamaz. Çalışmak için disassociation görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır.

  Şu anda, oluşturduğunuz yeni VM'ler için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

  Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur.

-   IP iletimi özelliği portalda görünür, ancak IP iletimini etkinleştirme etkisi yoktur. Bu özellik henüz desteklenmiyor.

- Azure yığın destekleyen tek bir *yerel ağ geçidi* her bir IP adresi. Bu Kiracı abonelikler arasında geçerlidir. Aynı IP adresiyle bir yerel ağ geçidi kaynağı oluşturmak ilk yerel ağ geçidi bağlantısı sonraki oluşturulmasını denemesinden sonra engellenir.

- Bir DNS sunucusu ayarı ile oluşturulan bir sanal ağda *otomatik*, özel bir DNS sunucusunun başarısız olması için değiştirilmesi. Bu Vnet vm'lerinin güncelleştirilmiş ayarları gönderilir değil.
 
- Azure yığın VM dağıtıldıktan sonra ek ağ arabirimleri VM örneğine eklemeyi desteklemez. VM birden fazla ağ arabirimi gerektiriyorsa, dağıtım sırasında tanımlanmalıdır.



#### <a name="sql-and-mysql"></a>SQL ve MySQL 
- Kullanıcıların veritabanlarını yeni bir SQL veya MySQL SKU oluşturabilmeniz için önce en çok bir saat sürebilir.

- Veritabanı sunucuları barındırma kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan örneğini kullanamazsınız.

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers.

#### <a name="app-service"></a>App Service
- Kullanıcılar, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

- (Çalışanları, yönetim, ön uç rolleri) altyapıyı ölçeklendirme için PowerShell işlem için Sürüm Notları'nda açıklandığı şekilde kullanmanız gerekir.
 
#### <a name="usage"></a>Kullanım  
- Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Azure yığın araçları Github'dan indirme
- Kullanırken *çağırma webrequest* Azure yığın indirmek için PowerShell cmdlet araçları Github'dan, hata iletisi:     
    -  *çağırma webrequest: istek iptal edildi: SSL/TLS güvenli kanalı oluşturulamadı.*     

  Bir son GitHub destek kullanımdan Tlsv1 ve Tlsv1.1 şifreleme standartları (PowerShell için varsayılan) nedeniyle bu hata oluşur. Daha fazla bilgi için bkz: [zayıf şifreleme standartları kaldırma bildirimi](https://githubengineering.com/crypto-removal-notice/).

  Bu sorunu çözmek için ekleme `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` TLSv1.2 GitHub havuzların indirirken kullanmak için PowerShell konsolunu zorlamak için komut dosyasının en üstüne.

