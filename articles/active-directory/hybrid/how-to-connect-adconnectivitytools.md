---
title: 'Azure AD Connect: ADConnectivityTool PowerShell Modülü nedir | Microsoft Docs'
description: Bu belgede yeni ADConnectivity PowerShell modülüne tanıtır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b2471b28132b26c41844b620f6dcf49e3802a6e
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436719"
---
# <a name="what-is-the-adconnectivitytool-powershell-module"></a>ADConnectivityTool PowerShell Modülü nedir?

Aşağıdakilerden biri kullanılan bir PowerShell modülü ADConnectivity aracıdır:

- Yükleme sırasında bir ağ bağlantısı sorunu olduğunda sihirbazda sağlanan kullanıcının başarılı Active Directory kimlik doğrulamasını engeller.
- Bir PowerShell oturumundan işlevleri çağıran bir kullanıcı tarafından yükleme gönderin.

Aracı bulunur: **C:\Program Files\Microsoft Azure Active Directory Connect\Tools\ ADConnectivityTool.psm1** 

## <a name="adconnectivitytool-during-installation"></a>Yükleme sırasında ADConnectivityTool

Üzerinde **dizinlerinizi bağlama** sayfa, Azure AD Connect bir ağ sorunu oluşursa ADConnectivityTool otomatik olarak Sihirbazı'nda birini kullanın işlevleri neler olduğunu belirlemek için.  Aşağıdakilerden herhangi biri ağ sorunları kabul edilebilir:

- Kullanıcı tarafından sağlanan orman adı yanlış yazıldı veya söz konusu orman yok 
- Kullanıcı tarafından sağlanan orman ile ilişkili etki alanı denetleyicileri, UDP bağlantı noktası 389 kapalı
- 'AD ormanı hesabını' penceresinde sağlanan kimlik bilgileri hedef orman ile ilişkili etki alanı denetleyicilerini almak için ayrıcalıklara sahip değil
- Herhangi bir TCP bağlantı noktası 53, 88 veya 389 kullanıcı sağlanan orman ile ilişkili etki alanı denetleyicileri, kapalı 
- Hem 389 UDP ve TCP bağlantı noktası (veya bağlantı noktaları) kapalı
- DNS yüklenemedi, ilişkili etki alanı denetleyicileri için sağlanan orman/veya çözümlenen

Bu sorunlardan biri bulunduğunda AADConnect Sihirbazı'nda ilgili hata iletisi görüntülenir:


![Hata](media/how-to-connect-adconnectivitytools/error1.png)

Örneğin, ne zaman biz çalıştığınız bir dizin eklemek **dizinlerinizi bağlama** ekran, Azure AD Connect bunu doğrulamak gereken ve 389 bağlantı noktası üzerinden etki alanı denetleyicisi ile iletişim kurabilmesi bekliyor.  Erişilemiyorsa, yukarıdaki ekran görüntüsünde gösterilen hata göreceğiz.  

Arka planda gerçekten gerçekleşip olan Azure AD Connect aradığı `Start-NetworkConnectivityDiagnosisTools` işlevi.  Bir ağ bağlantısı sorunu nedeniyle kimlik doğrulama başarısız olduğunda bu işlev çağrılır.

Son olarak, her aracın sihirbazından çağrıldığında ayrıntılı günlük dosyası oluşturulur. Günlük bulunan **C:\ProgramData\AADConnect\ADConnectivityTool-\<tarih >-\<zaman > .log**

## <a name="adconnectivitytools-post-installation"></a>ADConnectivityTools yükleme sonrası
Azure AD Connect yüklendikten sonra herhangi bir ADConnectivityTools PowerShell modülündeki işlevleri kullanılabilir.  

İşlevler hakkında başvuru bilgisi bulabilirsiniz [ADConnectivityTools başvurusu](reference-connect-adconnectivitytools.md)

### <a name="start-connectivityvalidation"></a>Başlangıç ConnectivityValidation

Mümkün olduğundan bu işlevi çağırmak için kullanacağız **yalnızca** çağrılabilir el ile ADConnectivityTool.psm1 PowerShell içinde içeri aktarıldıktan sonra. 

Bu işlev, belirtilen AD kimlik bilgilerini doğrulamak için Azure AD Connect Sihirbazı'nı çalıştıran aynı mantığı yürütür.  Ancak sorun ve önerilen bir çözüm hakkında daha ayrıntılı bir açıklama sağlar. 

Bağlantı doğrulama aşağıdaki adımlardan oluşur:
-   Etki alanı FQDN'si (tam etki alanı adı) nesnesini alın
-   Kullanıcı 'Hesap oluştur yeni AD' seçtiyseniz, bu kimlik bilgileri Enterprise Administrators grubuna ait, doğrula
-   Get orman FQDN'si nesnesi
-   Bu en az bir etki alanına daha önce elde edilen orman FQDN'si nesneyle ilişkili erişilebilir olduğunu doğrulayın
-   Orman işlev düzeyini Windows Server 2003 veya daha büyük olduğundan emin olun.

Kullanıcılar bu eylemler başarıyla yürütüldü bir dizin ekleyin.

Bir sorun çözüldükten sonra bu işlevi kullanıcının çalıştırıyorsa (veya hiç sorun değil varsa) çıktı kullanıcının Azure AD Connect Sihirbazı için geri dönün ve kimlik bilgilerini yeniden eklemeyi deneyin gösterir.



## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect: Hesaplar ve izinler](reference-connect-accounts-permissions.md)
- [Hızlı yükleme](how-to-connect-install-express.md)
- [Özel yükleme](how-to-connect-install-custom.md)
- [ADConnectivityTools başvurusu](reference-connect-adconnectivitytools.md)

