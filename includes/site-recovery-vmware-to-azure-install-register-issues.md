---
author: rockboyfor
ms.service: site-recovery
ms.topic: include
origin.date: 10/26/2018
ms.date: 12/10/2018
ms.author: v-yeche
ms.openlocfilehash: 9919521c8cb77f23f50a8097c4e630b4467dc725
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119748"
---
### <a name="installation-failures"></a>Yükleme hataları
| **Örnek hata iletisi** | **Önerilen eylem** |
|--------------------------|------------------------|
|HATA   Hesaplar yüklenemedi. Hata: System.IO.ıoexception: Yüklerken ve CS sunucusu kaydedilirken taşıma bağlantısından veriler okunamıyor.| Bilgisayarda TLS 1.0’ın etkin olduğundan emin olun. |

### <a name="registration-failures"></a>Kayıt hataları
**%ProgramData%\ASRLogs** klasöründeki günlükler gözden geçirilerek kayıt hataları ayıklanabilir.

| **Örnek hata iletisi** | **Önerilen eylem** |
|--------------------------|------------------------|
|**09:20:06**: InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Mesaj: ACS50008: SAML belirteci geçersiz.<br>İzleme kimliği: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Bağıntı Kimliği: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97 ><br>Zaman damgası: **2016-12-12 14:50:08Z<br>** | Sistem saatiniz ile yerel saat arasındaki farkın 15 dakikadan uzun olmadığından emin olun. Kayıt işlemini tamamlamak için yükleyiciyi yeniden çalıştırın.|
|**09:35:27** : Seçili sertifika için tüm olağanüstü durum kurtarma kasasını almaya çalışırken DRRegistrationException:: Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message oluşturdu: ACS50008: SAML belirteci geçersiz.<br>İzleme Kodu: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Bağıntı Kimliği: abe9deb8-3e64-464d-8375-36db9816427a<br>Zaman damgası: **2016-05-19 01:35:39Z**<br> | Sistem saatiniz ile yerel saat arasındaki farkın 15 dakikadan uzun olmadığından emin olun. Kayıt işlemini tamamlamak için yükleyiciyi yeniden çalıştırın.|
|06:28:45:Sertifika oluşturulamadı<br>06:28:45:Kurulum devam edemiyor. Site Recovery kimlik doğrulaması için gereken sertifika oluşturulamıyor. Kurulumu yeniden çalıştırın | Kurulumu yerel yönetici olarak çalıştırdığınızdan emin olun. |