---
title: "Microsoft Azure yığın Geliştirme Seti sürüm notları | Microsoft Docs"
description: "Geliştirmeler, düzeltmeler ve Azure yığın Geliştirme Seti için bilinen sorunlar"
services: azure-stack
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: andredm
ms.openlocfilehash: 56d8ad4fecf14dfa69ade43438672c31a4954209
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure yığın Geliştirme Seti sürüm notları

*Uygulandığı öğe: Azure yığın Geliştirme Seti*

Bu sürüm notları geliştirmeleri, düzeltmeler ve Azure yığın Geliştirme Seti bilinen sorunlar hakkında bilgi sağlar. Çalıştırdığınız hangi sürümünün emin değilseniz, yapabilecekleriniz [denetlemek için portal'ı kullanmanızı](azure-stack-updates.md#determine-the-current-version).

## <a name="build-201711221"></a>Yapı 20171122.1

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

- Bkz: [yeni özellikler ve düzeltmeler](azure-stack-update-1711.md#new-features-and-fixes) Azure yığınının Azure yığın 1711 güncelleştirme Sürüm Notları bölümünü tümleşik sistemler.

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
- İçinde **gerekli etkinleştirme** uyarı ayrıntılarını uyarı, bağlantısı olmayan **AzureBridge** bileşeni. Bunu yaparsanız, **genel bakış** dikey yüklemek, başarısız bir şekilde çalışır ve zaman aşımı olmaz.
- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.
- Azure yığın portalları kullanarak aboneliğinize izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri PowerShell kullanarak doğrulayabilirsiniz.
 
#### <a name="marketplace"></a>Market
- Kullanarak Azure yığın Market öğeleri eklemeye çalıştığınızda **azure'dan Ekle** seçeneği, tüm öğeleri olabilir indirme için görünür.
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
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.

#### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.
 
#### <a name="usage-and-billing"></a>Kullanım ve faturalandırma
- Ortak IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

#### <a name="identity"></a>Kimlik

Azure Active Directory Federasyon Hizmetleri (ADFS içinde) ortamlarında, dağıtılan **azurestack\azurestackadmin** hesabıdır artık varsayılan sağlayıcı aboneliğin sahibi. Oturum açmayı yerine **Yönetici portalı / adminmanagement endpoint** ile **azurestack\azurestackadmin**, kullanabileceğiniz **azurestack\cloudadmin** hesabı, bunu yönetebilir ve varsayılan sağlayıcı aboneliği kullanmanız gerektiğini.

> [!IMPORTANT]
> Hatta **azurestack\cloudadmin** hesabıdır dağıtılan ADFS ortamlarda varsayılan sağlayıcı aboneliğin sahibi, konak RDP için izinleri yok. Kullanmaya devam **azurestack\azurestackadmin** hesabı veya oturum açma, erişim ve gerektiğinde konak yönetmek için yerel yönetici hesabı.


## <a name="build-201710201"></a>Yapı 20171020.1

### <a name="improvements-and-fixes"></a>Geliştirmeleri ve düzeltmeler

Geliştirmeleri ve 20171020.1 yapı düzeltmeler listesini görmek için bkz: [geliştirmeleri ve düzeltmeleri](azure-stack-update-1710.md#improvements-and-fixes) Azure yığın 1710 Sürüm Notları bölümünü tümleşik sistemler. "Ek kalite ve düzeltmeleri" bölümünde listelenen öğelerin bazıları, yalnızca tümleşik sistemler için uygundur.

Ayrıca, aşağıdaki düzeltmeleri yapıldı:
- İşlem kaynak Sağlayıcı bilinmeyen bir duruma görüntülendiği bir sorun düzeltilmiştir.
- Bir sorun, bunları oluşturmanız ve plan ayrıntılarını görüntülemek daha sonra deneyin sonra burada kotaları Yönetici portalı'nda görünmeyebilir sabit.

### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="powershell"></a>PowerShell
- AzureRM 1.2.11 PowerShell modülü sürümü yeni değişiklikler ile ilgili bir listesi bulunur. 1.2.10 yükseltme hakkında bilgi için sürüm, bkz: [Geçiş Kılavuzu](https://aka.ms/azspowershellmigration).
 
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
- İçinde **gerekli etkinleştirme** uyarı ayrıntılarını uyarı, bağlantısı olmayan **AzureBridge** bileşeni. Bunu yaparsanız, **genel bakış** dikey yüklemek, başarısız bir şekilde çalışır ve zaman aşımı olmaz.
- Yönetici portalı'nda görebileceğiniz bir **kiracılar getirilirken hata** hata **bildirimleri** alanı. Bu hata güvenle yok sayabilirsiniz.
- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.
- Azure yığın portalları kullanarak aboneliğinize izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri PowerShell kullanarak doğrulayabilirsiniz.
 
#### <a name="marketplace"></a>Market
- Kullanarak Azure yığın Market öğeleri eklemeye çalıştığınızda **azure'dan Ekle** seçeneği, tüm öğeleri olabilir indirme için görünür.
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
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.

#### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.
 
#### <a name="usage-and-billing"></a>Kullanım ve faturalandırma
- Ortak IP adresi kullanım ölçüm verileri gösterilir aynı *olay tarihi-saati* yerine her kayıt için değer *TimeDate* kaydının oluşturulduğu gösterilir Damga. Şu anda, ortak IP adresi kullanımının doğru hesap gerçekleştirmek için bu verileri kullanamazsınız.

## <a name="build-201709283"></a>Yapı 20170928.3

### <a name="known-issues"></a>Bilinen sorunlar

#### <a name="powershell"></a>PowerShell
- AzureRM 1.2.11 PowerShell modülü sürümü yeni değişiklikler ile ilgili bir listesi bulunur. 1.2.10 yükseltme hakkında bilgi için sürüm, bkz: [Geçiş Kılavuzu](https://aka.ms/azspowershellmigration).

#### <a name="deployment"></a>Dağıtım
- Dağıtım sırasında bir saat sunucusu IP adresi ile belirtmeniz gerekir.

 #### <a name="infrastructure-management"></a>Altyapı Yönetimi
- Altyapı yedekleme üzerinde etkinleştirmeyin **altyapı yedekleme** dikey.
- İşlem kaynak sağlayıcısı bilinmeyen durumu görüntüler.
- Model ve temel kart yönetim denetleyicisi (BMC) IP adresini bir ölçek birimi düğümün temel bilgileri gösterilmez. Bu davranış Azure yığın Development Kit'te beklenir. 
   
#### <a name="portal"></a>Portal
- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.
- Bir kaynak grubu özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenir. Kaynak grupları abonelikler arasında taşıma şu anda desteklenmiyor.
- Göreceğiniz bir **gerekli etkinleştirme** Azure yığın Geliştirme Seti kaydetmek için öneren bir uyarı bildirimi. Bu davranış beklenir.
- İçinde **gerekli etkinleştirme** uyarı ayrıntılarını uyarı, bağlantısı olmayan **AzureBridge** bileşeni. Bunu yaparsanız, **genel bakış** dikey yüklemek, başarısız bir şekilde çalışır ve zaman aşımı olmaz.
- Bunları oluşturabilir ve ardından sonraki görünüme deneyin sonra kotaları Yönetici portalı'nda plan ayrıntılarını görünmeyebilir. Geçici bir çözüm olarak, **Hizmetleri ve kotaları**, tıklatın **Ekle**ve yeni bir giriş ekleyin.
- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.
- Aboneliğiniz Azure yığın Portal kullanımı için izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri Powershell kullanarak doğrulayabilirsiniz.
  
#### <a name="marketplace"></a>Market
- Kullanıcılar bir abonelik olmadan tam Market göz atabilir ve planları ve teklifleri gibi yönetim öğelerini görebilirsiniz. Bu öğeler kullanıcılara işlevsiz.
 
#### <a name="compute"></a>İşlem
- Kullanıcılar coğrafi olarak yedekli depolama ile sanal makine oluşturmak için seçeneği sunulur. Bu yapılandırma, sanal makine oluşturma başarısız olmasına neden olur.
- Yalnızca bir hata etki alanı ve bir bir güncelleştirme etki alanı ile bir sanal makine kullanılabilirlik yapılandırabilirsiniz.
- Sanal makine ölçek kümeleri oluşturmak için hiçbir Market deneyim yoktur. Bir şablonu kullanarak bir ölçek oluşturabilirsiniz.

#### <a name="networking"></a>Ağ
- Portalı kullanarak bir ortak IP adresi ile bir yük dengeleyicisi oluşturulamıyor. Geçici bir çözüm olarak, yük dengeleyici oluşturmak için PowerShell'i kullanabilirsiniz.
- Ağ Yük Dengeleyici oluşturduğunuzda, bir ağ adresi çevirisi (NAT) kuralı oluşturmanız gerekir. Bunu yapmazsanız, yük dengeleyici oluşturulduktan sonra bir NAT kuralı eklemeye çalıştığınızda bir hata alırsınız.
- Altında **ağ**, tıklatırsanız **bağlantı** bir VPN bağlantısı kurmak için **VNet-VNet** olası bağlantı türü olarak listelenir. Bu seçeneği belirlemeyin. Şu anda yalnızca **siteden siteye (IPSec)** seçeneği desteklenir.
- VM oluşturulur ve bu IP adresi ile ilişkili sonra bir sanal makineden (VM) genel bir IP adresi ilişkisini olamaz. Disassociation çalışmak için görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır. Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış (bazen denir bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur. Şu anda, yeni VM oluşturmak için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.

#### <a name="sqlmysql"></a>SQL/MySQL
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.

#### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.
