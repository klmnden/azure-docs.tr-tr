---
title: Azure yığın 1712 güncelleştirme | Microsoft Docs
description: Azure yığın 1712 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: b14f79ad-025f-45d8-9e1d-e53d2b420bb1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: brenduns
ms.openlocfilehash: c3cb8ab8a838a3f831ece617fcf3e218a9510ea5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-stack-1712-update"></a>Azure yığın 1712 güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye Bu güncelleştirme paketi giderir. Bilinen sorunlar bilinen sorunlar bölünen doğrudan güncelleştirme işlemi ile ilgili ve bilinen sorunlar yapı (yükleme sonrası) sahip.

> [!IMPORTANT]
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemler için geçerlidir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="build-reference"></a>Derleme başvurusu

Azure yığın 1712 güncelleştirme yapı numarası **180106.1**. Bir müşteri dağıttıysanız **180103.2** daha önce geçerli gerekmez **180106.1**.

## <a name="before-you-begin"></a>Başlamadan önce

> [!IMPORTANT]
> 1712 güncelleştirme yükleme işlemi sırasında sanal makineler oluşturmak çalışmayın. Bkz: [Azure yığın genel bakış güncelleştirmeleri yönetmek](https://docs.microsoft.com/azure/azure-stack/azure-stack-updates#plan-for-updates) daha fazla ayrıntı için.

### <a name="prerequisites"></a>Önkoşullar

Azure yığın yüklemelisiniz [1711 güncelleştirme](https://docs.microsoft.com/azure/azure-stack/azure-stack-update-1711) bu güncelleştirmeyi uygulamadan önce.

### <a name="post-update-steps"></a>Güncelleştirme sonrası adımlar

Bu güncelleştirme, ayrıca 1712 Azure yığın güncelleştirme yüklemesini tamamladıktan sonra OEM ortağından Bellenim güncelleştirmeleri yükleme gerektirir.

> [!NOTE]
> Güncelleştirmeleri yüklemek için OEM iş ortağı Web sitesine bakın.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Bu güncelleştirme aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.

#### <a name="new-features"></a>Yeni Özellikler

- Test-AzureStack cmdlet'i Azure yığın bulut ayrıcalıklı uç noktası aracılığıyla kullanılabilir doğrulamak için
- Bağlantısı kesilmiş bir dağıtımı Azure yığınının kaydetmek için özelliği
- Sertifika ve kullanıcı hesabı süre sonu için izleme uyarıları
- Eklenen kümesi BmcPassword cmdlet'te CESARETLENDİRİCİ BMC parola döndürme için
- İsteğe bağlı günlük kaydını desteklemek için ağ günlük güncelleştirmeler
- Yeniden görüntü oluşturma işlemi için sanal makine ölçek kümeleri (VMSS) desteği
- CloudAdmin oturum açma için bilgi noktası modu ERCS VM'de etkinleştir
- Kiracılar Windows sanal makineleri otomatik olarak etkinleştirebilir

#### <a name="fixes"></a>Düzeltmeler

- Onarım çalıştırılırken bakım düğümü işletimsel durumunu göstermek için düzeltme
- Doğru genel IP kullanım kayıtları saat ve tarih damgası için düzeltme
- Çeşitli diğer performans, sağlamlık ve güvenlik düzeltmeleri
- TimeSource ve Defender uç nokta modülü hata düzeltmeleri ayrıcalıklı

#### <a name="windows-server-2016-new-features-and-fixes"></a>Windows Server 2016 yeni özellikler ve düzeltmeler

- [Ocak, 3-2018 — KB4056890 (işletim sistemi yapı 14393.2007)](https://support.microsoft.com/help/4056890/windows-10-update-kb4056890)
    - Bu güncelleştirme tarafından açıklanan sektör çapında güvenlik sorunu yazılım düzeltmeler içerir [MSRC güvenlik önerisi ADV 180002](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002).

### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

Bu bölümde 1712 güncelleştirme yüklemesi sırasında karşılaşabileceğiniz bilinen sorunları içerir.

1. **Belirti:** Azure yığın işleçleri güncelleştirme işlemi sırasında şu hata karşılaşabilirsiniz: *"türü 'CheckHealth' rolünün 'VirtualMachines' yükseltilmiş bir özel durum: \n\nVirtual makine sistem durumu denetimi aşağıdaki hatalar ACS01 üretilen için-. \nThere ana bilgisayarlarını VM bilgileri alınırken bir hata oluştu. Özel durum ayrıntıları: \nGet-VM: 'başarısız Node03' bilgisayardaki işlemi: WS-Management hizmeti isteği işleyemiyor. WMI \nservice veya WMI sağlayıcısı bilinmeyen bir hata döndürdü: HRESULT 0x8004106c. "*
    1. **Neden:** bu sorunu sonraki pencere sunucu güncelleştirmelerinde ele alınması için tasarlanmıştır bir Windows Server sorunu nedeniyle oluşur.
    2. **Çözüm:** kişi Microsoft Müşteri Hizmetleri ve desteği (CSS) Yardım için.
<br><br>
2. **Belirti:** Azure yığın işleçleri güncelleştirme işlemi sırasında şu hata karşılaşabilirsiniz:*"VM başarısız bir hata ile ana bilgisayar Node03 düğümünde çekirdek halkası etkinleştirme: [ana bilgisayar-Node03] bağlanmak için Uzak sunucu ana bilgisayar Node03 başarısız oldu Aşağıdaki hata iletisi: WinRM istemcisi HTTP sunucu hata durumu (500) aldı, ancak uzak hizmet diğer bilgileri nedeni hakkında hata içermiyordu. "*
    1. **Neden:** bu sorunu sonraki pencere sunucu güncelleştirmelerinde ele alınması için tasarlanmıştır bir Windows Server sorunu nedeniyle oluşur. 
    2. **Çözüm:** kişi Microsoft Müşteri Hizmetleri ve desteği (CSS) Yardım için.
<br><br>

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu bölümde yükleme sonrası ile ilgili bilinen sorunlar yapı içeren **180106.1**.

#### <a name="portal"></a>Portal

- Yönetici portalı'nda işlem ve depolama kaynaklarını görüntülemek mümkün olmayabilir. Bu güncelleştirme yükleme sırasında bir hata oluştu ve güncelleştirme yanlış başarılı olarak bildirildi olduğunu gösterir. Bu sorun devam ederse, lütfen Yardım için Microsoft CSS başvurun.
- Boş bir portal panosunda görebilirsiniz. Pano kurtarmak için portalın sağ üst köşesindeki dişli simgesini seçin ve ardından **varsayılan ayarları geri**.
- Bir kaynak grubu özelliklerini görüntülediğinizde **taşıma** düğmesi devre dışıdır. Bu davranış beklenir. Kaynak grupları abonelikler arasında taşıma şu anda desteklenmiyor.
- Burada abonelik, kaynak grubu veya konum aşağı açılan listesinde seçtiğiniz herhangi bir iş akışı için bir veya daha fazla aşağıdaki sorunlarla karşılaşabilirsiniz:

   - Boş bir satır listesi üstündeki görebilirsiniz. Hala beklendiği gibi bir öğe seçin yapabiliyor olmanız gerekir.
   - Açılan listedeki öğeleri listesi kısaysa, öğe adlarının herhangi biri görmeye olmayabilir.
   - Birden çok kullanıcı aboneliğiniz varsa, kaynak grubu aşağı açılan listesi boş olabilir.

        > [!NOTE]
        > Son iki sorunlarını geçici olarak çözmek için abonelik veya kaynak grubu (biliyorsanız) adını yazın veya bunun yerine PowerShell kullanabilirsiniz.

- Kullanıcı abonelikleri yalnız bırakılmış kaynakları sonuçlarında siliniyor. Geçici bir çözüm olarak ilk kullanıcı kaynakları veya tüm kaynak grubunu silme ve kullanıcı abonelikleri silin.
- Aboneliğiniz Azure yığın Portal kullanımı için izinleri görüntülemek mümkün değildir. Geçici bir çözüm olarak izinleri PowerShell kullanarak doğrulayabilirsiniz.
- **Hizmet durumu** dikey başarısız yüklenemiyor. Yönetici veya Kullanıcı Portalı'nda Azure yığın hizmet durumu dikey penceresini açtığınızda bir hata görüntüler ve bilgi yüklemez. Bu beklenen bir davranıştır. Seçin ve hizmetin sistem durumunu açın, ancak bu özellik henüz kullanılabilir değil ancak Azure yığın gelecek bir sürümünde uygulanacaktır.

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

- Bir altyapı rol örneğini yeniden başlatırsanız, yeniden başlatma başarısız olduğunu belirten bir ileti alabilirsiniz. Bununla birlikte, yeniden başlatma gerçekten başarılı oldu.

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
- VM oluşturulur ve bu IP adresi ile ilişkili sonra bir sanal makineden (VM) genel bir IP adresi ilişkisini olamaz. Disassociation çalışmak için görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır. Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur. Şu anda, yeni VM oluşturmak için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.
- Azure yığın işleçleri dağıtmak, Sil, Vnet veya ağ güvenlik gruplarını değiştirmek olabilir. Bu sorun öncelikle aynı paketin sonraki güncelleştirme denemelerinde görülür. Bu, şu an araştırma altında bir güncelleştirme paketleme sorun kaynaklanır.
- İç yük dengeleyici (ILB) MAC adresleri Linux örnekleri kıran arka uç VM'ler için yanlış bir şekilde işler.

#### <a name="sqlmysql"></a>SQL/MySQL
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir.
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.

    > [!NOTE]
    > Etkisi var olan SQL veya MySQL kaynak sağlayıcı kullanıcılarınız için Azure yığın tümleşik sistemlerinizi 1712 sürüme güncelleştirirken olmalıdır. Yeni bir Azure yığın güncelleştirme kullanılabilir hale gelene kadar geçerli SQL veya MySQL kaynak sağlayıcı derlemeleriniz kullanmaya devam edebilirsiniz.

#### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

#### <a name="identity"></a>Kimlik

Azure Active Directory Federasyon Hizmetleri (ADFS içinde) ortamlarında, dağıtılan **azurestack\azurestackadmin** hesabıdır artık varsayılan sağlayıcı aboneliğin sahibi. Oturum açmayı yerine **Yönetici portalı / adminmanagement endpoint** ile **azurestack\azurestackadmin**, kullanabileceğiniz **azurestack\cloudadmin** hesabı, bunu yönetebilir ve varsayılan sağlayıcı aboneliği kullanmanız gerektiğini.

> [!IMPORTANT]
> Olsa bile **azurestack\cloudadmin** hesabıdır dağıtılan ADFS ortamlarda varsayılan sağlayıcı aboneliğin sahibi, konak RDP için izinleri yok. Kullanmaya devam **azurestack\azurestackadmin** hesabı veya oturum açma, erişim ve gerektiğinde konak yönetmek için yerel yönetici hesabı.

## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle

Azure yığın 1712 güncelleştirme paketinden indirebilirsiniz [burada](https://aka.ms/azurestackupdatedownload).

## <a name="more-information"></a>Daha fazla bilgi

Microsoft, izlemek ve ayrıcalıklı uç noktası (güncelleştirme 1712 ile yüklü CESARETLENDİRİCİ) kullanarak güncelleştirmeleri sürdürmek için bir yol sağlamıştır.

- Bkz: [izlemek Azure ayrıcalıklı endpoint belgelerini kullanarak yığınında güncelleştirmeleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update). 

## <a name="see-also"></a>Ayrıca bkz.

- Bkz [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md) Azure yığınında güncelleştirme yönetimi genel bakış.
- Bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md) Azure yığın güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için.
