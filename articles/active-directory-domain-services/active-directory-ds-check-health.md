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
ms.date: 02/12/2018
ms.author: ergreenl
ms.openlocfilehash: a9421ace7abf1f3d45b1f8cd810067d79faa92ec
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD etki alanı Hizmetleri yönetilen etki alanının durumunu denetleyin

## <a name="overview-of-the-health-page"></a>Sistem durumu sayfasına genel bakış
Sağlık durumu sayfasında, Azure portaldaki, yönetilen etki alanınızda neler olduğunu güncel tutmak kullanabilirsiniz. Bu makalede, sağlık durumu sayfasında öğelerini aracılığıyla anlatılmaktadır.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>Yönetilen etki alanınızı durumunu görüntüleme
1. Gidin [Azure AD etki alanı Hizmetleri sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
2. Durumunu görüntülemek istediğiniz etki alanını tıklatın.
3. Sol gezinti bölmesinde **sistem durumu**.

Aşağıdaki resimde bir örnek sistem durumu sayfası gösterilmektedir: ![örnek sistem durumu sayfası](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Yönetilen etki alanınızın sistem durumu, her saat değerlendirilir. Yönetilen etki alanınıza değişiklikleri yaptıktan sonra yönetilen etki alanınızın güncelleştirilmiş durumunu görüntülemek için sonraki değerlendirme döngüsünde kadar bekleyin. Sağ üst köşedeki "Son hesaplanan" timestamp, yönetilen etki alanınızı durumunu en son ne zaman değerlendirildiğinde gösterir.
>

### <a name="status-of-your-managed-domain"></a>Yönetilen etki alanınızın durumuyla
Üst durum sağlığını sağda sayfası, yönetilen etki alanınız genel durumunu gösterir. Tüm etki alanınızdaki var olan uyarılar durum Etkenler. Bu gibi durumlarda, etki alanınızın durumuyla ayrıca Azure AD Etki Alanı Hizmetleri'ne genel bakış sayfasında görüntüleyebilirsiniz.

| Durum | Simge | Açıklama |
| --- | :----: | --- |
| Çalışıyor | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Yönetilen etki alanınızı düzgün çalıştığından ve herhangi bir kritik veya uyarı uyarı yok. Bu etki alanının bilgilendirici uyarılar olabilir. |
| İlgilenilmesi (uyarı) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Yönetilen etki alanınızda hiçbir kritik uyarılar vardır, ancak ele alınması gereken bir veya daha fazla uyarı bildirimleri vardır. |
| İlgilenilmesi (kritik) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Yönetilen etki alanınızda bir veya daha fazla kritik uyarılar vardır. Uyarı ve/veya bilgilendirici uyarılar da sahip olabilirsiniz. |
| Dağıtılıyor | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Dağıtılan sürecinde, etki alanıdır. |

## <a name="monitors"></a>İzleyiciler
İzlemeler Azure AD etki alanı Hizmetleri düzenli olarak izler, yönetilen etki alanınız yönlerini olduğundan. İzleyici iyi durumda tutmak için en iyi yönetilen etki alanınız için etkin uyarı çözümlemek için yoludur.

Azure AD etki alanı Hizmetleri şu anda aşağıdaki izler:
 - Backup
 - Azure AD ile eşitleme

### <a name="the-backup-monitor"></a>'Yedek' İzleyicisi
Bu, yönetilen etki alanınızın düzenli yedeklemeler gerçekleştirilen olup olmadığını izler. Aşağıdaki tabloda yedekleme İzleyicisi'nin ayrıntılar sütununda beklenmesi gerekenler açıklanmaktadır:

| Ayrıntı değeri | Açıklama |
| --- | --- |
|"Hiçbir zaman yedeklenen" | Yeni oluşturulan bir yönetilen etki alanı için bu durum normaldir. Genellikle, ilk yedekleme 24 saat yönetilen etki alanınızı sağlandıktan sonra oluşturulur. Yönetilen etki alanınızı yeni oluşturulmadı veya anormal bir süre boyunca, bu durum gördüğünüz [desteğine başvurun](active-directory-ds-contact-us.md). |
| 1-14 gün önce son yedeğin alındığı | Genel olarak, bu değer için yedekleme İzleyicisi beklenir. |
| Son yedekleme birden fazla 14 gün önce yapılmadı. | İki haftadan uzun dilediğiniz zaman, son yedeklemeden bu yana olağandışı uzunlukta bir süre ' dir. Etkin kritik uyarılar, yönetilen etki alanınız düzenli olarak yedeklenen engellemek. İlk olarak, tüm etkin uyarıları, yönetilen etki alanınız için ve ardından Sorun oluşmaya devam ederse, çözümleme [desteğine başvurun](active-directory-ds-contact-us.md). |


### <a name="the-synchronization-with-azure-ad-monitor"></a>'Azure AD ile Eşitleme' İzleyicisi
Microsoft Azure Active Directory ile yönetilen etki alanınız ne sıklıkta eşitlenir izler. Nesneler (kullanıcılar ve gruplar) ve Azure AD dizininizi yaptığından son eşitleme hem de bir eşitleme dönemi ne kadar sürebilir etkileyebilecek değişiklikler sayısı sayısı. Yönetilen etki alanınızı üzerinde üç gün önce son eşitlenmişse [desteğine başvurun](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Uyarılar
Azure AD etki alanı hizmetleri çalıştırmak sırayla ele alınması gereken, yönetilen etki alanınızda sorunlar için uyarıları üretilir. Her uyarı sorunu açıklar ve sorunu çözmek için belirli adımlar özetlenmektedir çözümleme URL'si sağlar. Tüm uyarıları ve bunların çözümleri görüntülemek için ziyaret [sorun giderme uyarıları](active-directory-ds-troubleshoot-alerts.md) makalesi.

### <a name="alert-severity"></a>Uyarı önem derecesi
Uyarılar üç önem derecesi düzeyleri kategorilere: kritik, uyarı ve bilgi.

 * **Kritik uyarılar** ciddi bir şekilde, yönetilen etki alanınız etkisi sorunlardır. Microsoft edemez izlemek, yönetmek, düzeltme eki ve yönetilen etki alanınızı eşitlemek gibi bu uyarılar hemen ilgilenilmesi gerekir. 
 * **Uyarı bildirimleri** , yönetilen etki alanınızı gelecekte etkileyebilecek sorunları size bildiren. Bu uyarılar, yönetilen etki alanınız güvenliğini sağlamak için öneriler sunar.
 * **Bilgilendirici uyarılar** etki alanınızın olumsuz etkileyen değil bildirimler. Bilgilendirici uyarılar, etki alanı ve Azure AD etki alanı Hizmetleri olup bitenler hakkında bilgili korumak için tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar
- [Uyarıları, yönetilen etki alanınızda Çözümle](active-directory-ds-troubleshoot-alerts.md)
- [Azure AD etki alanı hizmetleri hakkında daha fazla bilgi](active-directory-ds-overview.md)
- [Ürün ekibine başvurun](active-directory-ds-contact-us.md)
