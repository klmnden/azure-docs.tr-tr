---
title: "Azure AD etki alanı Hizmetleri - yönetilen etki alanınızı durumunu denetlemek | Microsoft Docs"
description: "Sistem sağlık durumu sayfasında Azure portalı kullanarak yönetilen etki alanınızı durumunu denetleyin."
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: ergreenl
ms.openlocfilehash: 4adbce0305bdc1a9b261f5cf644fad876d101bc6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---check-the-health-of-your-managed-domain"></a>Azure AD etki alanı Hizmetleri - yönetilen etki alanınızı durumunu denetleyin

## <a name="your-domains-health"></a>Etki alanınızın sistem durumu

Sağlık durumu sayfasında, Azure portaldaki, yönetilen etki alanınızda neler olduğunu güncel tutmak kullanabilirsiniz. Tüm sistem öğeleri adımlara sayfasında ve etki alanınızı emin olmak öğretmek bu konumda makaledir.

### <a name="view-your-health-page"></a>Sistem durumu sayfanızı görüntüleyin

1. Azure AD etki alanı Hizmetleri sayfasına gidin [Azure portalı](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices)
2. Durumunu görüntülemek istediğiniz etki alanını tıklatın.
3. Sol gezinti bölmesinde "Sistem"'i tıklatın.

Aşağıdaki resimde bir örnek sistem durumu sayfasıdır.

![Örnek sistem durumu sayfası](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Etki alanınızın sistem saatte değerlendirilir. Yönetilen etki alanınıza değişiklikleri yaptıktan sonra sistem durumu etki alanınızın görüntülemek için sonraki değerlendirme döngüsünde güncelleştirilmiş kadar beklemelidir. Ne zaman için sağ üst köşede yer alan "Son hesaplanan" zaman damgası kullanarak en son etki alanınızın değerlendirildiğinde kontrol edebilirsiniz.
>

### <a name="status-of-your-managed-domain"></a>Yönetilen etki alanınızın durumuyla

Üst durum sağlığını sağda sayfa etki alanınızın genel durumunu gösterir. Tüm etki alanınızdaki var olan uyarılar durum Etkenler. Bu gibi durumlarda, etki alanınızın durumuyla ayrıca Azure AD Etki Alanı Hizmetleri'ne genel bakış sayfasında görüntüleyebilirsiniz.

Yönetilen bir etki alanına durumları:

| Durum | Simge | Açıklama |
| --- | :----: | --- |
| Çalışıyor | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Yönetilen etki alanınızı düzgün çalıştığından ve herhangi bir kritik veya uyarı uyarı yok. Bu etki alanının bilgilendirici uyarılar olabilir. |
| İlgilenilmesi (uyarı) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Yönetilen etki alanınızda hiçbir kritik uyarılar vardır, ancak ele alınması gereken bir veya daha fazla uyarı bildirimleri vardır. |
| İlgilenilmesi (kritik) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Yönetilen etki alanınızda bir veya daha fazla kritik uyarılar vardır. Uyarı ve/veya bilgilendirici uyarılar da sahip olabilirsiniz. |
| Dağıtılıyor | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Dağıtılan sürecinde, etki alanıdır. |

## <a name="monitors"></a>İzleyiciler

İzleyiciler belirli yönlerden Azure AD etki alanı Hizmetleri gözetmenlik yapar, yönetilen etki alanınız hakkında ayrıntılı olarak açıklanmaktadır. İzleyici iyi durumda tutmak için en iyi yönetilen etki alanınızdaki herhangi bir uyarıyı çözümlemek için yoludur.

Kullanılabilir izleyicileri şunlardır:
 - Backup
 - AAD ile eşitleme

### <a name="backup"></a>Backup

Bu, ne sıklıkta biz yönetilen etki alanınızın yedekleri alması izler. Aşağıdaki yedekleme İzleyicisi'nin ayrıntılar sütunu açıklayan bir tablodur ve ne değerleri beklenmelidir.

| Ayrıntı değeri | Açıklama |
| --- | --- |
|"Hiçbir zaman yedeklenen" | Yeni oluşturulan bir etki alanı için bu durum normaldir. İlk yedek, genellikle 24 saat sonra oluşturulur. Yönetilen etki alanınızı yeni oluşturulmadı veya anormal bir süre boyunca, bu durumda [desteğine başvurun](active-directory-ds-contact-us.md). |
| 1-14 gün önce son yedeğin alındığı | Genel olarak, yedekleme izleyicinin beklenen değer budur. |
| Son yedekleme birden fazla 14 gün önce yapılmadı. | İki haftadan uzun dilediğiniz zaman, son yedeklemeden bu yana olağandışı uzunlukta bir süre ' dir. İlk olarak, yönetilen etki alanı ve ardından IF göre sorun ortaya herhangi bir uyarı hala devam ediyorsa, çözümleme [desteğine başvurun](active-directory-ds-contact-us.md). |


### <a name="synchronization-with-azure-ad"></a>Azure AD ile eşitleme

Microsoft Azure Active Directory ile yönetilen etki alanınız ne sıklıkta eşitlenir izler. Son eşitleme hem de bir eşitleme dönemi ne kadar sürebilir etkileyebilir yaptığından değişikliği miktarının yanı sıra, yönetilen etki alanı kullanıcıları miktarı. Son eşitlemeden bu yana üç günden uzun olması durumunda, genel olarak, önerilir [desteğine başvurun](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Uyarılar

Uyarılar, Azure AD etki alanı hizmetleri çalıştırmak sırayla ele alınması gereken, yönetilen etki alanınızda sorunlardır. Her uyarı sorunu açıklanır ve sorunu çözmek için belirli adımlar özetlenmektedir bir URL sağlar. Tüm uyarıları ve bunların çözümleri görüntülemek için ziyaret [sorun giderme uyarıları](active-directory-ds-troubleshoot-alerts.md) makalesi.

### <a name="severity"></a>Önem Derecesi
Uyarılar üç önem derecesi düzeyleri kategorilere: kritik, uyarı ve bilgi.

 * **Kritik uyarılar** ciddi bir şekilde, yönetilen etki alanınız etkisi sorunlardır. Microsoft edemez izlemek, yönetmek, düzeltme eki ve yönetilen etki alanınızı eşitlemek gibi bu uyarılar hemen ilgilenilmesi gerekir.
 * **Uyarı bildirimleri** etki alanınızın gelecekte etkileyebilir, ancak mutlaka "hizmetinizi ayırırsınız değil" sorunları olabilir. Bu uyarılar en iyi yöntemler anahat ve yönetilen etki alanınızı korumak için önerileri verin.
 * **Bilgilendirici uyarılar** etki alanınızın olumsuz etkileyen değil bildirimler. Bilgilendirici uyarılar, etki alanı ve Azure AD etki alanı Hizmetleri olup bitenler hakkında bilgili korumak için tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar
- [Uyarıları, yönetilen etki alanınızda Çözümle](active-directory-ds-troubleshoot-alerts.md)
- [Azure AD etki alanı hizmetleri hakkında daha fazla bilgi](active-directory-ds-features.md)
- [Bizimle bağlantı kurun](active-directory-ds-contact-us.md)
