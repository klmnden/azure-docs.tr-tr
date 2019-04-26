---
title: Azure AD Connect Health Sürüm Geçmişi
description: Bu belgede, Azure AD Connect Health ve bu sürümlerde dahil yayınlar açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/20/2019
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46e70850ba9e5984e36643f1b9ecc9db29eec149
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386916"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Sürüm Yayınlama Geçmişi
Azure Active Directory ekibi, düzenli olarak yeni özellikler ve işlevler ile Azure AD Connect Health güncelleştirir. Bu makalede sürümleri ve yayımlanan özellikler listelenmektedir.  

> [!NOTE]
> Connect Health aracıları yeni sürümü yayımlandığında otomatik olarak güncelleştirilir. Lütfen otomatik yükseltme ayarları, Azure portalından etkinleştirildiğinden emin olun. 
>

Azure AD Connect Health eşitleme için Azure AD Connect yüklemesi ile tümleşiktir. Daha fazla bilgi edinin [Azure AD Connect sürüm geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history) , özellikle ilgili düşüncelerinizi için oy [Connect Health User Voice kanal](https://feedback.azure.com/forums/169401-azure-active-directory/filters/new?category_id=165591)

## <a name="march-2019"></a>Mart 2019
**Aracı güncelleştirmesi:** 
* (Sürüm 3.1.41.0) AD DS için Azure AD Connect Health Aracısı 
* .NET sürüm koleksiyonu.
* Belirli kategorileri eksik olduğunda performans sayacı koleksiyonu geliştirme.
* Birden çok İzleme Aracısı örneği UNICODE önleme üzerinde hata düzeltmesi.

* AD FS (sürüm 3.1.41.0) için Azure AD Connect Health Aracısı 
* Tümleştirme ve AD FS test betikleri ADFSToolBox kullanarak yükseltin.
* .NET sürüm koleksiyonu.
* Belirli kategorileri eksik olduğunda performans sayacı koleksiyonu geliştirme.
* Birden çok İzleme Aracısı örneği UNICODE önleme üzerinde hata düzeltmesi.


## <a name="november-2018"></a>Kasım 2018
**Yeni GA özellikler:** 
* Azure AD Connect Health eşitleme için - tanılama ve Portalı'ndan yinelenen öznitelik eşitleme hatalarını Düzelt

**Aracı güncelleştirmesi:** 
* (Sürüm 3.1.24.0) AD DS için Azure AD Connect Health Aracısı 
* Aktarım Katmanı Güvenliği (TLS) Protokolü sürüm 1.2 uyumluluk ve zorlama
* Genel katalog uyarı gürültüsünü azaltmak
* Sistem Durumu Aracısı kayıt hata düzeltmeleri

* AD FS (sürüm 3.1.24.0) için Azure AD Connect Health Aracısı
* Aktarım Katmanı Güvenliği (TLS) Protokolü sürüm 1.2 uyumluluk ve zorlama
* Yerelleştirilmiş işletim sistemi ADFSRequestToken Test desteği
* Tanılama Aracısı EventHandler kilitleme sorunu çözüldü
* Sistem Durumu Aracısı kayıt hata düzeltmeleri

## <a name="august-2018"></a>Ağustos 2018 
*  Azure AD Connect sürümü 1.1.880.0 yayımlanan eşitleme (sürüm 3.1.7.0) için Azure AD Connect Health Aracısı    
   1. Düzeltme [İzleme Aracısı ile .NET Framework KB'ın yüksek CPU sorunu serbest bırakır](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)

## <a name="june-2018"></a>Haziran 2018 
**Yeni Önizleme özellikleri:** 
* Azure AD Connect Health eşitleme için - tanılama ve Portalı'ndan yinelenen öznitelik eşitleme hatalarını Düzelt 

**Aracı güncelleştirmesi:** 
* (Sürüm 3.1.7.0) AD DS için Azure AD Connect Health Aracısı    
  1. Düzeltme [İzleme Aracısı ile .NET Framework KB'ın yüksek CPU sorunu serbest bırakır](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   
* AD FS (sürüm 3.1.7.0) için Azure AD Connect Health Aracısı  
  1. Düzeltme [İzleme Aracısı ile .NET Framework KB'ın yüksek CPU sorunu serbest bırakır](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
  2. İkincil sunucuda ADFS sunucusu 2016 düzeltmeleri test sonuçları
   
* AD FS (sürüm 3.1.2.0) için Azure AD Connect Health Aracısı  
  1. Aracı bellek yönetimi ve ilgili uyarıları 3.0.244.0 sürümü için düzeltme


## <a name="may-2018"></a>Mayıs 2018
**Aracı güncelleştirmesi:**
* (Sürüm 3.0.244.0) AD DS için Azure AD Connect Health Aracısı
  1. Aracı gizlilik geliştirme  
  2. Hata düzeltmeleri ve genel iyileştirmeler

* AD FS (sürüm 3.0.244.0) için Azure AD Connect Health Aracısı
  1. Tanılama aracısı ve ilgili PowerShell modülü geliştirmeleri
  2. Aracı gizlilik geliştirme  
  3. Hata düzeltmeleri ve genel iyileştirmeler

* Azure AD Connect sürümü 1.1.819.0 yayımlanan eşitleme (sürüm 3.0.164.0) için Azure AD Connect Health Aracısı 
  1. Aracı gizlilik geliştirme  
  2. Hata düzeltmeleri ve genel iyileştirmeler


## <a name="march-2018"></a>Mart 2018
**Yeni Önizleme özellikleri:**
* Azure AD Connect Health AD FS - riskli IP raporu ve uyarı.

**Aracı güncelleştirmesi:**

* (Sürüm 3.0.176.0) AD DS için Azure AD Connect Health Aracısı
  1. Aracı kullanılabilirlik geliştirmeleri 
  2. Hata düzeltmeleri ve genel iyileştirmeler
* AD FS (sürüm 3.0.176.0) için Azure AD Connect Health Aracısı
  1. Aracı kullanılabilirlik geliştirmeleri 
  2. Hata düzeltmeleri ve genel iyileştirmeler
* Azure AD Connect sürümü 1.1.750.0 yayımlanan eşitleme (sürüm 3.0.129.0) için Azure AD Connect Health Aracısı  
  1. Aracı kullanılabilirlik geliştirmeleri 
  2. Hata düzeltmeleri ve genel iyileştirmeler

## <a name="december-2017"></a>Aralık 2017
**Aracı güncelleştirmesi:**

* (Sürüm 3.0.145.0) AD DS için Azure AD Connect Health Aracısı
  1. Aracı kullanılabilirlik geliştirmeleri 
  2. Yeni aracı sorun giderme komutlar eklendi
  3. Hata düzeltmeleri ve genel iyileştirmeler
* AD FS (sürüm 3.0.145.0) için Azure AD Connect Health Aracısı
  1. Yeni aracı sorun giderme komutlar eklendi
  2. Aracı kullanılabilirlik geliştirmeleri 
  3. Hata düzeltmeleri ve genel iyileştirmeler
  
## <a name="october-2017"></a>Ekim 2017
**Aracı güncelleştirmesi:**

 * Azure AD Connect sürümü 1.1.649.0 yayımlanan eşitleme (sürüm 3.0.129.0) için Azure AD Connect Health Aracısı
<br></br> Azure AD Connect ve Azure AD Connect Health aracısını eşitleme için arasında bir sürüm uyumluluk sorunu düzeltildi. Bu sorunu, Azure AD Connect yerinde yükseltme sürümüne 1.1.647.0 icraatta bulunan müşterileri etkiler, ancak şu anda sistem durumu aracısı sürümü 3.0.127.0 sahiptir. Yükseltmeden sonra sistem durumu aracısı artık Azure AD Connect eşitleme hizmeti ile ilgili sağlık verilerini Azure AD Health hizmetine gönderebilirsiniz. Bu düzeltmeyle, sistem durumu aracısı sürümü 3.0.129.0 Azure AD Connect yerinde yükseltme sırasında yüklenir. Sistem Durumu Aracısı sürümü 3.0.129.0 Azure AD Connect sürümü 1.1.649.0 ile uyumluluk sorunu yok.

## <a name="july-2017"></a>Temmuz 2017
**Aracı güncelleştirmesi:**

* (Sürüm 3.0.68.0) AD DS için Azure AD Connect Health Aracısı
  1. Hata düzeltmeleri ve genel iyileştirmeler
  2. Bağımsız bulut desteği
* AD FS (sürüm 3.0.68.0) için Azure AD Connect Health Aracısı
  1. Hata düzeltmeleri ve genel iyileştirmeler
  2. Bağımsız bulut desteği
* Azure AD Connect sürümü 1.1.614.0 yayımlanan eşitleme (sürüm 3.0.68.0) için Azure AD Connect Health Aracısı
  1. Microsoft Azure kamu Bulutu ve Microsoft bulut Almanya için destek

## <a name="april-2017"></a>Nisan 2017      
**Aracı güncelleştirmesi:**

* AD FS (sürüm 3.0.12.0) için Azure AD Connect Health Aracısı
  1. Hata düzeltmeleri ve genel iyileştirmeler
* (Sürüm 3.0.12.0) AD DS için Azure AD Connect Health Aracısı
  1. Performans sayaçları geliştirmeleri karşıya yükleme
  2. Hata düzeltmeleri ve genel iyileştirmeler

## <a name="october-2016"></a>Ekim 2016
**Aracı güncelleştirmesi:**

* AD FS (sürüm 2.6.408.0) için Azure AD Connect Health Aracısı
* Kimlik doğrulama isteklerinin istemci IP adreslerini algılama yönelik geliştirmeler
* Uyarılarla ilgili hata düzeltmeleri
* (Sürüm 2.6.408.0) AD DS için Azure AD Connect Health Aracısı
* Uyarılarla ilgili hata düzeltmeleri.
* Azure AD Connect 1.1.281.0 sürümü ile yayımlanan eşitleme (sürüm 2.6.353.0) için Azure AD Connect Health Aracısı
* Eşitleme hatası raporları için gerekli verileri sağlar
* Uyarılarla ilgili hata düzeltmeleri

**Yeni Önizleme özellikleri:**

* Azure AD için eşitleme hatası raporları bağlanma

**Yeni özellikler:**

* Azure AD Connect Health AD FS için-IP adres alanını hatalı kullanıcı adı/parola ile ilk 50 kullanıcıya ilişkin rapor kullanıma sunulmuştur.

## <a name="july-2016"></a>Temmuz 2016
**Yeni Önizleme özellikleri:**

* [Azure AD Connect Health'i AD DS için](how-to-connect-health-adds.md).

## <a name="january-2016"></a>2016 Ocak
**Aracı güncelleştirmesi:**

* AD FS (sürüm 2.6.91.1512) için Azure AD Connect Health Aracısı

**Yeni özellikler:**

* [Test Bağlantısı aracı için Azure AD Connect Health aracıları](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>2015 Kasım
**Yeni özellikler:**

* Destek [rol tabanlı erişim denetimi](how-to-connect-health-operations.md#manage-access-with-role-based-access-control)

**Yeni Önizleme özellikleri:**

* [Azure AD Connect Health eşitleme](how-to-connect-health-sync.md).

**Giderilen sorunlar:**

* Aracı kaydı sırasında görülen hataları için hata düzeltmeleri.

## <a name="september-2015"></a>Eylül 2015
**Yeni özellikler:**

* AD FS için kullanıcı adı parola rapor yanlış
* Kimliği doğrulanmamış bir HTTP Ara sunucusunu yapılandırmak için destek
* Sunucu Çekirdeğinde aracısını yapılandırmak için destek
* AD FS için uyarılar geliştirmeleri
* Bağlantı ve veriler için AD FS için Azure AD Connect Health Aracısı geliştirmeleri karşıya yükleyin.

**Giderilen sorunlar:**

* AD FS hata türleri için kullanım öngörüleri hata düzeltmeleri.

## <a name="june-2015"></a>Haziran 2015
**Azure AD Connect Health AD FS ve AD FS Ara sunucusu için ilk sürümü.**

**Yeni özellikler:**

* AD FS ve AD FS Ara sunucularının e-posta bildirimleri ile izleme için uyarıları.
* AD FS topolojisi ve AD FS performans sayaçlarının desenleri kolay erişim.
* Uygulamaları, kimlik doğrulama yöntemleri, ağ konumu vb. istek tarafından gruplandırılmış AD FS sunucularında başarılı belirteci isteklerini eğilimi.
* AD FS sunucularında hata türleri vb. uygulamalar tarafından gruplandırılmış başarısız istek eğilimleri.
* Azure AD genel yönetici kimlik bilgilerini kullanarak daha basit aracı dağıtımı.  

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [bulutta, şirket içi kimlik altyapınızı ve eşitleme hizmetlerinizi izleme](whatis-hybrid-identity-health.md).

