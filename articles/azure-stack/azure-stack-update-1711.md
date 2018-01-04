---
title: "Azure yığın 1711 güncelleştirme | Microsoft Docs"
description: "Azure yığın 1711 güncelleştirmesi nedir hakkında bilgi edinin tümleşik sistemleri, bilinen sorunlar ve güncelleştirme karşıdan yükleme konumu."
services: azure-stack
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: 2b66fe05-3655-4f1a-9b30-81bd64ba0013
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: andredm
ms.openlocfilehash: 578d17bcfbb7e12c9855132772c2068a5cdf1f62
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="azure-stack-1711-update"></a>Azure yığın 1711 güncelleştirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bu makalede yenilikleri ve bilinen sorunlar bu sürüm ve güncelleştirmeyi yüklemek nereye Bu güncelleştirme paketi giderir. Bilinen sorunlar bilinen sorunlar bölünen doğrudan güncelleştirme işlemi ile ilgili ve bilinen sorunlar yapı (yükleme sonrası) sahip.

> [!IMPORTANT]
> Bu güncelleştirme paketi, yalnızca Azure tümleşik yığını sistemler için geçerlidir. Bu güncelleştirme paketi Azure yığın Geliştirme Seti için geçerli değildir.

## <a name="build-reference"></a>Derleme başvurusu

Azure yığın 1711 güncelleştirme yapı numarası **171201.3**.

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="prerequisites"></a>Önkoşullar

Azure yığın yüklemelisiniz [1710 güncelleştirme](https://docs.microsoft.com/azure/azure-stack/azure-stack-update-1710) bu güncelleştirmeyi uygulamadan önce.

### <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler

Bu güncelleştirme aşağıdaki geliştirmeleri ve düzeltmeler için Azure yığınına içerir.

#### <a name="new-features"></a>Yeni Özellikler

- Çözüm şablonları Syndicating desteği
- Azure yığın grafik günlük kaydı ve hata işleme güncelleştirmeleri
- Ana bilgisayar üzerinde ve devre dışı bırakma özelliği
- Kullanıcılar artık Windows sanal makineleri otomatik olarak etkinleştirebilir
- Eklenen ayrıcalıklı uç noktası bekletme amacıyla BitLocker kurtarma anahtarlarını almak için PowerShell cmdlet
- Altyapı güncelleştirirken çevrimdışı görüntülerini güncelleştirme desteği
- Yedekleme hizmetini etkinleştir ile altyapı yedeklemeyi etkinleştirme

#### <a name="fixes"></a>Düzeltmeler

- Alan değiştirilebilen biriminin (FRU) ve aynı zamanda güncelleştirilmiş küme günlüğe kaydetme sırasında DNS'de sabit yarış durumu
- Yeniden başlatma özelliği alan değiştirilebilen biriminin (FRU) içinde devre dışı bırak konağının Düzelt
- Çeşitli diğer performans, güvenlik ve kararlılık düzeltmeler

#### <a name="windows-server-2016-new-features-and-fixes"></a>Windows Server 2016 yeni özellikler ve düzeltmeler

- [14 Kasım 2017 — KB4048953 (işletim sistemi yapı 14393.1884)](https://support.microsoft.com/help/4048953)
 
### <a name="known-issues-with-the-update-process"></a>Güncelleştirme işlemi ile ilgili bilinen sorunlar

Bu bölüm 1711 güncelleştirme yüklemesi sırasında karşılaşabileceğiniz bilinen sorunları içerir.


1. **Belirti:** Azure yığın işleçleri güncelleştirme işlemi sırasında şu hata karşılaşabilirsiniz: *"name: yükleme güncelleştirme.", "Açıklama": "Güncelleştirme ve Infra VM'ler.", "errorMessage": "yazın 'LiveUpdate' rolünün ' VirtualMachines yükseltilmiş bir özel durum: \n\nThere disk.\n\nat üzerinde yeterli alan <ScriptBlock>, <No file>: line22 ","Durum":"Error","startTimeUtc":" 2017-11-10T16:46:59.123Z ","endTimeUtc":" 2017-11-10T19:20:29.669Z ","adımlar": []"*
    2. **Neden:** Bu sorun, bir veya daha fazla Azure yığın alt yapısının parçası olan sanal makineleri boş disk alanı yetersizliği nedeniyle
    3. **Çözüm:** kişi Microsoft Müşteri Hizmetleri ve desteği (CSS) Yardım için.
<br><br>
2. **Belirti:** Azure yığın işleçleri güncelleştirme işlemi sırasında şu hata karşılaşabilirsiniz:*"ExtractToFile" "3" bağımsız değişkenle çağrılırken özel durum: "işlem dosyasına erişemiyor ' <\\<machineName>-ERCS01\C$ \ Files\WindowsPowerShell\Modules\Microsoft.AzureStack.Diagnostics\Microsoft.AzureStack.common.Tools.Diagnostics.AzureStackDiagnostics.dll program >'*
    1. **Neden:** önceden Portalı'ndan bir güncelleştirme devam ediyor ayrıcalıklı uç noktası (CESARETLENDİRİCİ) kullanarak sürdürüldü olduğunda bu soruna neden olur.
    2. **Çözüm:** kişi Microsoft Müşteri Hizmetleri ve desteği (CSS) Yardım için.
<br><br>
3. **Belirti:**Azure yığın işleçleri güncelleştirme işlemi sırasında şu hata karşılaşabilirsiniz:*"'VirtualMachines' rolünün 'CheckHealth' türü için bir özel durum: \n\nVirtual makine sistem durumu denetimi gerçekleşti <machineName>-üretilen ACS01 hataları. \nThere aşağıdaki ana bilgisayarlarını VM bilgileri alınırken bir hata oluştu. Özel durum ayrıntıları: \nGet-VM: 'başarısız Node03' bilgisayardaki işlemi: WS-Management hizmeti isteği işleyemiyor. WMI \nservice veya WMI sağlayıcısı bilinmeyen bir hata döndürdü: HRESULT 0x8004106c ".*
    1. **Neden:** bu sorunu sonraki pencere sunucu güncelleştirmelerinde ele alınması için tasarlanmıştır bir Windows Server sorunu nedeniyle oluşur.
    2. **Çözüm:** kişi Microsoft Müşteri Hizmetleri ve desteği (CSS) Yardım için.
<br><br>
4. **Belirti:**Azure yığın işleçleri güncelleştirme işlemi sırasında şu hata karşılaşabilirsiniz:*"'URP' rolünün 'DefenderUpdate' türü bir özel durum oluşturuldu: başarısız sürümünden alma \\SU1FileServer\SU1_Public\ DefenderUpdates\x64\{dosya adı} kopyalama-AzSDefenderFiles, C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.Defender\Microsoft.AzureStack.Defender.psm1 60 girişimleri sonra .exe: satır 262"*
    1. **Neden:** Windows Defender tanım güncelleştirmelerini başarısız veya eksik arka plan indirmesi tarafından bu soruna neden olur.
    2. **Çözüm:** güncelleştirme 8 saate kadar tamamlandıktan sonra devam etmek için lütfen girişimi ilk güncelleştirmeyi deneyin beri geçtiğinden.

### <a name="known-issues-post-installation"></a>Bilinen sorunlar (yükleme sonrası)

Bu bölümde yükleme sonrası ile ilgili bilinen sorunlar yapı içeren **20171201.3**.

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

#### <a name="health-and-monitoring"></a>Sistem durumu ve izleme

- Bir altyapı rol örneğini yeniden başlatırsanız, yeniden başlatma başarısız olduğunu belirten bir ileti alabilirsiniz. Bununla birlikte, yeniden başlatma gerçekten başarılı oldu.

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
- VM oluşturulur ve bu IP adresi ile ilişkili sonra bir sanal makineden (VM) genel bir IP adresi ilişkisini olamaz. Disassociation çalışmak için görünür, ancak daha önce atanan genel IP adresi orijinal VM ile ilişkili olarak kalır. Yeni bir VM için IP adresi yeniden atama olsa bile bu davranış oluşur (genellikle olarak adlandırılan bir *VIP takası*). Tüm gelecekte bu IP adresi sonucu başlangıçta ilişkili VM değil de yeni bir bağlantı üzerinden bağlanma girişiminde bulunur. Şu anda, yeni VM oluşturmak için yalnızca yeni ortak IP adreslerini kullanmanız gerekir.
- Azure yığın işleçleri dağıtmak, Sil, Vnet veya ağ güvenlik gruplarını değiştirmek olabilir. Bu sorun öncelikle aynı paketin sonraki güncelleştirme denemelerinde görülür. Bu, şu an araştırma altında bir güncelleştirme paketleme sorun kaynaklanır.
- İç yük dengeleyici (ILB) MAC adresleri Linux örnekleri kıran arka uç VM'ler için yanlış bir şekilde işler.
 
#### <a name="sqlmysql"></a>SQL/MySQL
- Bu yeni bir SQL veya MySQL SKU kiracılar veritabanları oluşturabilmeniz için önce bir saate kadar sürebilir. 
- Öğeleri doğrudan SQL ve MySQL kaynak sağlayıcısı tarafından gerçekleştirilen değil sunucularda barındırma oluşturulması desteklenmiyor ve eşleşmeyen bir duruma neden olabilir.
 
#### <a name="app-service"></a>App Service
- Bir kullanıcı, bunlar ilk Azure işlevlerine abonelikte oluşturmadan önce depolama kaynak sağlayıcısı kaydetmeniz gerekir.

#### <a name="identity"></a>Kimlik

Azure Active Directory Federasyon Hizmetleri (ADFS içinde) ortamlarında, dağıtılan **azurestack\azurestackadmin** hesabıdır artık varsayılan sağlayıcı aboneliğin sahibi. Oturum açmayı yerine **Yönetici portalı / adminmanagement endpoint** ile **azurestack\azurestackadmin**, kullanabileceğiniz **azurestack\cloudadmin** hesabı, bunu yönetebilir ve varsayılan sağlayıcı aboneliği kullanmanız gerektiğini.

> [!IMPORTANT]
> Olsa bile **azurestack\cloudadmin** hesabıdır dağıtılan ADFS ortamlarda varsayılan sağlayıcı aboneliğin sahibi, konak RDP için izinleri yok. Kullanmaya devam **azurestack\azurestackadmin** hesabı veya oturum açma, erişim ve gerektiğinde konak yönetmek için yerel yönetici hesabı.

#### <a name="infrastructure-backup-sevice"></a>Altyapı yedekleme hizmeti
<!-- 1974890-->

- **Bulut kurtarma için öncesi 1711 yedeklemeler desteklenmez.**  
  Öncesi 1711 yedeklemeleri bulut Kurtarma ile uyumlu değildir. 1711 için ilk güncelleştirin ve yedeklemeleri etkinleştirmek gerekir. Yedeklemeleri etkinleştirilirse, 1711 için güncelleştirdikten sonra yedekleyin emin olun. Öncesi 1711 yedeklemeleri silinmesi gerekir.

- **Etkinleştirme altyapı ASDK üzerinde yalnızca sınama amacıyla yedeğidir.**  
  Altyapı yedeklemeleri çok düğümlü çözümleri geri yüklemek için kullanılabilir. ASDK altyapı yedekleme etkinleştirebilirsiniz, ancak kurtarma test etmek için bir yolu yoktur.

Daha fazla bilgi için bkz: [altyapı Backup hizmeti ile Azure yığını için yedekleme ve veri kurtarma](C:\Git\MS\azure-docs-pr\articles\azure-stack\azure-stack-backup-infrastructure-backup.md).

## <a name="download-the-update"></a>Güncelleştirme karşıdan yükle

Azure yığın 1711 güncelleştirme paketinden indirebilirsiniz [burada](https://aka.ms/azurestackupdatedownload).

## <a name="more-information"></a>Daha fazla bilgi

Microsoft, izlemek ve ayrıcalıklı uç noktası (güncelleştirme 1711 ile yüklü CESARETLENDİRİCİ) kullanarak güncelleştirmeleri sürdürmek için bir yol sağlamıştır.

- Bkz: [izlemek Azure ayrıcalıklı endpoint belgelerini kullanarak yığınında güncelleştirmeleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update). 

## <a name="see-also"></a>Ayrıca bkz.

- Bkz [yönetmek Azure yığın genel bakış güncelleştirmelerinde](azure-stack-updates.md) Azure yığınında güncelleştirme yönetimi genel bakış.
- Bkz: [Azure yığınında güncelleştirmelerini](azure-stack-apply-updates.md) Azure yığın güncelleştirmeleriyle uygulama hakkında daha fazla bilgi için.
