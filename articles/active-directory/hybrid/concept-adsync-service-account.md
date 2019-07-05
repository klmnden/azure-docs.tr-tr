---
title: 'Azure AD Connect: Ad eşitleme hizmeti hesabı | Microsoft Docs'
description: Bu konu, ADSync hizmeti hesabı açıklar ve hesabı ile ilgili en iyi uygulamalar sağlanır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f228da5afc5998d8fa59ce2d720cec4c9f955b67
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478725"
---
# <a name="adsync-service-account"></a>Ad eşitleme hizmeti hesabı
Azure AD Connect, Active Directory ve Azure Active Directory Eşitlemesi düzenleyen bir şirket içi hizmetini yükler.  Şirket içi ortamınızdaki bir sunucuda Microsoft Azure AD eşitleme eşitleme hizmeti (ADSync) çalıştırır.  Hizmet kimlik bilgilerini, Express yüklemeleri varsayılan olarak ayarlanmış, ancak Kurumsal güvenlik gereksinimlerinizi karşılayacak şekilde özelleştirilebilir.  Bu kimlik bilgileri, şirket içi ormanları veya Azure Active Directory'ye bağlanmak için kullanılmaz.

ADSync seçme hizmet hesabı Azure AD Connect'i yüklemeden önce yapmak için bir önemli planlama kararıdır.  Yüklemeyi başlatmak, başarısız olan hizmeti neden olur sonra kimlik bilgilerini değiştirmek eşitleme veritabanına erişim hakkını kaybetmesini ve bağlı dizinlerinizi (Azure ve AD DS) ile kimlik doğrulaması gerçekleştiremeyen yapmaya.  Özgün kimlik bilgilerini geri yüklenene kadar eşitleme meydana gelir.

## <a name="the-default-adsync-service-account"></a>Varsayılan ad eşitleme hizmeti hesabı

Bir üye sunucuda çalıştırıldığında zaman AdSync hizmeti sanal hizmet hesabı (VSA) bağlamında çalıştırılır.  Bir ürün kısıtlamasından dolayı bir özel hizmet hesabı bir etki alanı denetleyicisine yüklendiğinde oluşturulur.  Hızlı ayarları hizmet hesabı, Kuruluş güvenliği gereksinimlerinizi karşılamıyorsa, Azure AD Connect Özelleştir seçeneğini belirleyerek dağıtın.  Kuruluşunuzun gereksinimlerini karşılayan hizmet hesabı seçeneğini belirleyin.

>[!NOTE]
>Bir etki alanı denetleyicisine yüklendiğinde varsayılan hizmet hesabı Domain\AAD_InstallationIdentifier şeklidir.  Bu hesabın parolasını rastgele oluşturulmuş ve kurtarma ve parola döndürme için önemli zorluklar teşkil etmektedir.  Microsoft'un önerdiği hizmet hesabı bir etki alanı denetleyicisinin bir tek başına kullanabilir veya grup yönetilen hizmet hesabını ilk yükleme sırasında özelleştirme (smsa'yı / gMSA)

|Azure AD Connect konumu|Hizmet hesabı oluşturuldu|
|-----|-----|
|Üye sunucu|NT SERVICE\ADSync|
|Etki alanı denetleyicisi|Domain\AAD_74dc30c01e80 (bkz. Not)|

## <a name="custom-adsync-service-accounts"></a>Özel ADSync hizmeti hesapları
Sanal hizmet hesabı veya bir tek başına bağlamında hizmet ya da Grup yönetilen hizmet hesabını ADSync çalıştıran Microsoft önerir.  Etki alanı yöneticinizle, belirli bir kuruluş güvenliği gereksinimlerinizi karşılamak için sağlanan bir hizmet hesabı oluşturmak de tercih edebilirsiniz.   Yükleme sırasında kullanılan hizmet hesabının özelleştirmek için aşağıdaki hızlı ayarlar sayfasındaki Özelleştir seçeneğini seçin.   Aşağıdaki seçenekler kullanılabilir:

- Varsayılan hesap – Azure AD Connect yukarıda açıklandığı gibi hizmet hesabı sağlayacak.
- Yönetilen hizmet hesabı – bir tek başına kullanın veya yöneticiniz tarafından sağlanan Grup
- etki alanı hesabı – bir etki alanı hizmet yöneticiniz tarafından sağlanan hesabı kullanın

![](media/concept-adsync-service-account/adsync1.png)

![](media/concept-adsync-service-account/adsync2.png)

## <a name="diagnosing-adsync-service-account-changes"></a>Tanılama ADSync hizmeti hesap değişiklikleri
Yüklemeyi başlatmak, başarısız olan hizmeti neden olur sonra ADSync hizmeti için kimlik bilgilerini eşitleme veritabanına erişim hakkını kaybetmesini ve bağlı dizinlerinizi (Azure ve AD DS) ile kimlik doğrulaması gerçekleştiremeyen değiştiriliyor.  Veritabanı erişebilmesi için yeni ad eşitleme hizmeti hesabı bu sorundan kurtulmak için yeterli değil. Özgün kimlik bilgilerini geri yüklenene kadar eşitleme meydana gelir.

Başlatamadı olduğunda ADSync hizmeti olay günlüğüne düzeyinde hata iletisi verir.  İleti içeriğini yerleşik veritabanı (localdb) veya tam SQL kullanımda olmasına bağlı olarak değişir.  Mevcut bir olay günlüğü girişleri örnekleri aşağıda verilmiştir.

### <a name="example-1"></a>Örnek 1

AdSync hizmeti şifreleme anahtarları bulunamadı ve yeniden oluşturuldu.  Bu sorun düzeltilene kadar eşitleme gerçekleşmez.

Bu sorunu Microsoft Azure AD eşitleme sorunlarını giderme, şifreleme anahtarlarını AdSync hizmeti kimlik bilgileri oturum açma değiştirilirse erişilemez hale gelecektir.  Kimlik bilgileri değiştirildiyse, oturum açma hesabı geri (örn. ilk olarak yapılandırılmış değeri değiştirmek için hizmetler uygulamasını kullanın NT SERVICE\AdSync) ve hizmeti yeniden başlatın.  Bu, hemen AdSync hizmeti doğru işlem geri yükler.

Lütfen aşağıdaki bölüme bakın [makale](https://go.microsoft.com/fwlink/?linkid=2086764) daha fazla bilgi için.

### <a name="example-2"></a>Örnek 2

Hizmet (localdb) yerel veritabanına bir bağlantı kurulamadı çünkü başlatamadı.

Bu sorunu Microsoft Azure AD eşitleme hizmeti sorunlarını giderme, oturum açma AdSync hizmeti kimlik bilgileri değiştiyse, yerel veritabanı sağlayıcısı erişim izni kaybedilmesine neden olur.  Kimlik bilgilerini değiştirilmiş olsa bile oturum açma hesabı geri (örn. ilk olarak yapılandırılmış değeri değiştirmek için hizmetler uygulamasını kullanın NT SERVICE\AdSync) ve hizmeti yeniden başlatın.  Bu, hemen AdSync hizmeti doğru işlem geri yükler.

Lütfen aşağıdaki bölüme bakın [makale](https://go.microsoft.com/fwlink/?linkid=2086764) daha fazla bilgi için.

Aşağıdaki hata bilgilerini sağlayıcı tarafından döndürülen ek ayrıntılar:
 

``` 
OriginalError=0x80004005 OLEDB Provider error(s): 
Description  = 'Login timeout expired'
Failure Code = 0x80004005
Minor Number = 0 
Description  = 'A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online.'
```
## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.