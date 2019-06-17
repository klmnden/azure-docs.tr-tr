---
title: Azure AD etki alanı Hizmetleri - yönetilen etki alanınıza durumunu kontrol edin | Microsoft Docs
description: Azure portalında sistem durumu sayfasını kullanarak yönetilen etki alanınızın sistem durumunu denetleyin.
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: c3ff0b7daeaef077ea0d83782aeac6cda56fc659
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246487"
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD Domain Services yönetilen etki alanının sistem durumunu denetle

## <a name="overview-of-the-health-page"></a>Sistem Durumu Sayfası Genel Bakış
Sistem durumu sayfası, Azure Portal'da kullanarak, yönetilen etki alanınızda neler olduğunu güncel tutmak kullanabilirsiniz. Bu makalede, sistem durumu sayfası öğelerinde gösterilmektedir.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>Yönetilen etki alanınıza durumunu görüntüleme
1. Gidin [Azure AD Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
2. Durumunu görüntülemek istediğiniz etki alanına tıklayın.
3. Sol gezinti bölmesinden **sistem durumu**.

Aşağıdaki resimde bir örnek sistem durumu sayfası gösterilmektedir: ![Örnek sistem durumu sayfası](./media/active-directory-domain-services-alerts/health-page.png)

>[!NOTE]
> Yönetilen etki alanınızın sistem saatte değerlendirilir. Yönetilen etki alanınızla değişiklikleri yaptıktan sonra yönetilen etki alanınızın güncelleştirilmiş durumunu görüntülemek için sonraki değerlendirme döngüsünde kadar bekleyin. Yönetilen etki alanınızın sistem en son ne zaman değerlendirildiği sağ üst köşedeki "Son Değerlendirme" zaman damgası gösterilir.
>

### <a name="status-of-your-managed-domain"></a>Yönetilen etki alanınıza durumu
Üst durum durumunu sağ sayfa yönetilen etki alanınıza genel durumunu gösterir. Tüm etki alanınızdaki var olan uyarı durumu Etkenler. Azure AD Domain Services genel bakış sayfasında, etki alanının durumunu da görüntüleyebilirsiniz.

| Durum | Simge | Açıklama |
| --- | :----: | --- |
| Çalışıyor | <img src= "./media/active-directory-domain-services-alerts/running-icon.png" width = "15"> | Yönetilen etki alanınıza sorunsuz çalışmasını ve herhangi bir kritik veya uyarı uyarı yok. Bu etki alanının bilgilendirici uyarılar olabilir. |
| İlgilenilmesi (uyarı) | <img src= "./media/active-directory-domain-services-alerts/warning-icon.png" width = "15"> | Yönetilen etki alanınızda hiçbir kritik uyarı yok, ancak ele alınması gereken bir veya daha fazla uyarı bildirimleri vardır. |
| İlgilenilmesi (kritik) | <img src= "./media/active-directory-domain-services-alerts/critical-icon.png" width = "15"> | Yönetilen etki alanınızda bir veya daha fazla kritik uyarı var. Uyarı ve/veya bilgilendirici uyarılar da sahip olabilirsiniz. |
| Dağıtma | <img src= "./media/active-directory-domain-services-alerts/deploying-icon.png" width = "15"> | Dağıtılan sürecinde, etki alanıdır. |

## <a name="monitors"></a>İzleyiciler
Azure AD etki alanı Hizmetleri düzenli olarak izler, yönetilen etki alanınız yönlerini izleyiciler var. Yönetilen etki alanınız için tüm etkin uyarıları çözümlemek için İzleyici iyi durumda tutmak için en iyi yolu var.

Azure AD etki alanı Hizmetleri şu anda aşağıdaki izler:
 - Backup
 - Azure AD ile eşitleme

### <a name="the-backup-monitor"></a>'Yedekleme' İzleyici
Bu yönetilen etki alanınızın düzenli yedeklemeler gerçekleştirilen olup olmadığını izler. Aşağıdaki tabloda, yedekleme İzleyici Ayrıntılar sütununda beklenmesi gerekenler açıklanmaktadır:

| Detayı değeri | Açıklama |
| --- | --- |
|"Hiçbir zaman yedeklenen" | Yeni oluşturulan bir yönetilen etki alanı için bu durum normaldir. Genel olarak, ilk yedekleme 24 saat yönetilen etki alanınıza sağlandıktan sonra oluşturulur. Yönetilen etki alanınıza yeni oluşturulmadı veya anormal bir zaman miktarı için bu durum gördüğünüz [desteğe](contact-us.md). |
| 1-14 gün önce son yedekleme yapılmadı | Genel olarak, bu değer yedekleme İzleyicisi için bekleniyor. |
| Son yedeklemeden 14 günden daha önce gerçekleştirildi. | İki haftadan uzun bir zaman, son yedeklemeden bu yana normalden uzun bir süredir. Etkin kritik uyarılar düzenli olarak yedeklenen gelen yönetilen etki alanınıza engelliyor olabilir. İlk olarak, etkin uyarıları için yönetilen etki alanınızı ve Sorun oluşmaya devam sonra devam ederse, çözmek [desteğe](contact-us.md). |


### <a name="the-synchronization-with-azure-ad-monitor"></a>'Azure AD ile Eşitleme' İzleyici
Microsoft, yönetilen etki alanınızı Azure Active Directory ile eşitlenen ne sıklıkta izler. Nesne (kullanıcılar ve gruplar) ve Azure AD dizininizde son eşitleme hem de eşitleme süresini nasıl uzun sürebilir etkileyebilir bu yana yapılan değişikliklerin sayısı sayısı. Yönetilen etki alanınız üzerinde üç gün önce en son eşitlenmişse [desteğe](contact-us.md).

## <a name="alerts"></a>Uyarılar
Çalıştırmak Azure AD Domain Services için sırayla ele alınması gereken yönetilen etki alanınıza sorunlarını uyarıları üretilir. Her uyarı, sorunu açıklayan ve sorunu çözmek için belirli adımlar özetlenmektedir bir çözüm URL sağlar. Tüm uyarıları ve çözümleri görüntülemek için ziyaret [sorun giderme uyarıları](troubleshoot-alerts.md) makalesi.

### <a name="alert-severity"></a>Uyarı önem derecesi
Uyarılar üç farklı önem düzeyleri kategorilere ayrılmıştır: kritik, uyarı ve bilgilendirici.

 * **Kritik uyarılar** ciddi bir şekilde yönetilen etki alanınızı etkilemeden sorunlardır. Microsoft olamaz izleme, yönetme, düzeltme eki ve yönetilen Etki Alanınızla eşitleme gibi bu uyarılar hemen ilgilenilmesi gerekir. 
 * **Uyarı bildirimleri** yönetilen etki alanınıza gelecekte etkileyebilecek sorunları size bildirir. Bu uyarılar, yönetilen etki alanınıza güvenli hale getirmek için öneriler sunar.
 * **Bilgilendirici uyarılar** etki alanınızı olumsuz etkileyen değil bildirimlerdir. Bilgilendirici uyarılar, etki alanı ve Azure AD Domain Services olup bitenler hakkında bilgili korumak için tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanınızda uyarıları çözümleyin](troubleshoot-alerts.md)
- [Azure AD Domain Services hakkında daha fazla bilgi](overview.md)
- [Ürün ekibine başvurun](contact-us.md)
