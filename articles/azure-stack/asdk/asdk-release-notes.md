---
title: Microsoft Azure yığın Geliştirme Seti sürüm notları | Microsoft Docs
description: Geliştirmeler, düzeltmeler ve Azure yığın Geliştirme Seti için bilinen sorunlar
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 3335fdd3a1bb20c378bf36307d742491de0e46ad
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure yığın Geliştirme Seti sürüm notları
Bu sürüm notları geliştirmeleri, düzeltmeler ve Azure yığın Geliştirme Seti bilinen sorunlar hakkında bilgi sağlar. Çalıştırdığınız hangi sürümünün emin değilseniz, yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](.\.\azure-stack-updates.md#determine-the-current-version).

> Abone tarafından ASDK yenilikler ile güncel kalmasını [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [akış](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

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

- Bir kaynak veya kaynak grubunun özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenir. Kaynaklar veya kaynak grupları, kaynak grupları veya abonelikler arasında taşıma şu anda desteklenmiyor.
 
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

- Bir kaynak veya kaynak grubunun özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenir. Kaynaklar veya kaynak grupları, kaynak grupları veya abonelikler arasında taşıma şu anda desteklenmiyor.
 
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




## <a name="build-201801032"></a>Yapı 20180103.2

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

- Bkz: [yeni özellikler ve düzeltmeler](.\.\azure-stack-update-1712.md#new-features-and-fixes) Azure yığınının Azure yığın 1712 güncelleştirme Sürüm Notları bölümünü tümleşik sistemler.

    > [!IMPORTANT]
    > Listelenen öğelerin bazıları **yeni özellikler ve düzeltmeler** bölüm yalnızca Azure tümleşik yığını sistemler için ilgili.

### <a name="known-issues"></a>Bilinen sorunlar
 
#### <a name="deployment"></a>Dağıtım
- Dağıtım sırasında bir saat sunucusu IP adresi ile belirtmeniz gerekir.

#### <a name="infrastructure-management"></a>Altyapı Yönetimi
- Altyapı yedekleme üzerinde etkinleştirmeyin **altyapı yedekleme** dikey.
- Model ve temel kart yönetim denetleyicisi (BMC) IP adresini bir ölçek birimi düğümün temel bilgileri gösterilmez. Bu davranış Azure yığın Development Kit'te beklenir.

#### <a name="portal"></a>Portal
- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.
- Bir kaynak grubu özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenir. Kaynak grupları abonelikler arasında taşıma şu anda desteklenmiyor.
-  Burada abonelik, kaynak grubu veya konum aşağı açılan listesinde seçtiğiniz herhangi bir iş akışı için bir veya daha fazla aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Boş bir satır listesi üstündeki görebilirsiniz. Hala beklendiği gibi bir öğe seçin yapabiliyor olmanız gerekir.
   - Açılan listedeki öğeleri listesi kısaysa, öğe adlarının herhangi biri görmeye olmayabilir.
   - Birden çok kullanıcı aboneliğiniz varsa, kaynak grubu aşağı açılan listesi boş olabilir. 

   Son iki sorunlarını geçici olarak çözmek için abonelik veya kaynak grubu (biliyorsanız) adını yazın veya bunun yerine PowerShell kullanabilirsiniz.

- Göreceğiniz bir **gerekli etkinleştirme** Azure yığın Geliştirme Seti kaydetmek için öneren bir uyarı bildirimi. Bu davranış beklenir.
- Varsa **bileşen** bağlantısı birinden tıklatıldığında **altyapı rolü** uyarı, elde edilen **genel bakış** dikey yüklemeye çalışır ve başarısız olur. Ayrıca ** genel bakış ** dikey penceresi zaman aşımına yapar.
- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.
- Azure yığın portalları kullanarak aboneliğinize izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri PowerShell kullanarak doğrulayabilirsiniz.
- **Hizmet durumu** dikey başarısız yüklenemiyor. Yönetici veya Kullanıcı Portalı'nda Azure yığın hizmet durumu dikey penceresini açtığınızda bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açın, ancak bu özellik henüz kullanılabilir değil ancak Azure yığın gelecek bir sürümünde uygulanacaktır.
#### <a name="marketplace"></a>Market
- Bazı Market öğesi uyumluluk sorunları nedeniyle bu sürümde kaldırılıyor. Bunlar daha sonra doğrulama yeniden etkin olacaktır.
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.
 
#### <a name="compute"></a>İşlem
- Kullanıcılar coğrafi olarak yedekli depolama ile sanal makine oluşturmak için seçeneği sunulur. Bu yapılandırma, sanal makine oluşturma başarısız olmasına neden olur. 
- Yalnızca bir hata etki alanı ve bir bir güncelleştirme etki alanı ile bir sanal makine kullanılabilirlik yapılandırabilirsiniz.
- Sanal makine ölçek kümeleri oluşturmak için hiçbir Market deneyim yoktur. Bir şablonu kullanarak bir ölçek oluşturabilirsiniz.
- Sanal makine ölçek kümeleri için ölçeklendirme ayarları portalda kullanılabilir değildir. Geçici bir çözüm olarak, kullandığınız [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). PowerShell sürümü farklılıkları nedeniyle kullanmalısınız `-Name` yerine parametre `-VMScaleSetName`.

#### <a name="networking"></a>Ağ
- Portalı kullanarak bir ortak IP adresi ile bir yük dengeleyicisi oluşturulamıyor. Geçici bir çözüm olarak, yük dengeleyici oluşturmak için PowerShell'i kullanabilirsiniz.
- Ağ Yük Dengeleyici oluşturduğunuzda, bir ağ adresi çevirisi (NAT) kuralı oluşturmanız gerekir. Bunu yapmazsanız, yük dengeleyici oluşturulduktan sonra bir NAT kuralı eklemeye çalıştığınızda bir hata alırsınız.
- Altında **ağ**, tıklatırsanız **bağlantı** bir VPN bağlantısı kurmak için **VNet-VNet** olası bağlantı türü olarak listelenir. Bu seçeneği belirlemeyin. Şu anda yalnızca **siteden siteye (IPSec)** seçeneği desteklenir.
- VM oluşturulur ve bu IP adresi ile ilişkili sonra bir sanal makineden (VM) genel bir IP adresi ilişkisini olamaz. Disassociation çalışmak için görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır. Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur. Şu anda, yeni VM oluşturmak için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.
- Azure yığın işleçleri dağıtmak, Sil, Vnet veya ağ güvenlik gruplarını değiştirmek olabilir. Bu sorun öncelikle aynı paketin sonraki güncelleştirme denemelerinde görülür. Bu, şu an araştırma altında bir güncelleştirme paketleme sorun kaynaklanır.
- İç yük dengeleyici (ILB) MAC adresleri, arka uç ağ paketlerini Linux örnekleri kullanırken bırakma arka uç VM'ler için yanlış bir şekilde işler.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.

#### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.
 
#### <a name="usage"></a>Kullanım  
- Kullanım genel IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

#### <a name="identity"></a>Kimlik

Azure Active Directory Federasyon Hizmetleri (ADFS içinde) ortamlarında, dağıtılan **azurestack\azurestackadmin** hesabıdır artık varsayılan sağlayıcı aboneliğin sahibi. Oturum açmayı yerine **Yönetici portalı / adminmanagement endpoint** ile **azurestack\azurestackadmin**, kullanabileceğiniz **azurestack\cloudadmin** hesabı, bunu yönetebilir ve varsayılan sağlayıcı aboneliği kullanmanız gerektiğini.

> [!IMPORTANT]
> Hatta **azurestack\cloudadmin** hesabıdır dağıtılan ADFS ortamlarda varsayılan sağlayıcı aboneliğin sahibi, konak RDP için izinleri yok. Kullanmaya devam **azurestack\azurestackadmin** hesabı veya oturum açma, erişim ve gerektiğinde konak yönetmek için yerel yönetici hesabı.


