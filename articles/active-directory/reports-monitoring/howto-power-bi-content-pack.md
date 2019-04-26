---
title: Azure Active Directory Power BI İçerik Paketi'ni kullanma | Microsoft Docs
description: Azure Active Directory Power BI İçerik Paketi'ni kullanmayı öğrenin
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 934562147fedcc81b16fd1ad2534af5662ef4b78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60437700"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Azure Active Directory Power BI içerik Paketi'ni kullanma

|  |
|--|
|Azure AD Power BI içerik paketi şu anda Azure AD kiracınızdaki verileri almak için Azure AD Graph API'lerini kullanmaktadır. Sonuç olarak içerik paketinde mevcut olan verilerle [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md) kullanarak aldığınız veriler arasında farklılık olabilir. |
|  |

Power BI İçerik Paketi için Azure Active Directory (Azure AD) yardımcı olmak için önceden oluşturulmuş raporları içerir, kullanıcılarınızın benimseyin ve nasıl Azure AD özellikleri anlayın. Bu, Power BI'da zengin görselleştirme deneyiminden faydalanarak dizininizdeki tüm etkinliklerin bir anlayış kazanmak sağlar. Ayrıca, kendi panonuzu oluşturabilir ve kuruluşunuzdaki herkesle paylaşabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

İçerik paketini kullanmak için bir Azure AD premium (ö1/ö2) lisansı gerekir. Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için.

## <a name="install-the-content-pack"></a>İçerik Paketi yükleyin

Kullanıma [hızlı](quickstart-install-power-bi-content-pack.md) Azure AD Power BI içerik Paketi'ni yüklemek için.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Bu Azure AD günlükleri İçerik Paketi sürümünde yer alan raporlar

Aşağıdaki raporlar, Azure AD Power BI içerik paketine dahil edilir. Verilerden raporlar içeren **son 30 gün**.

**Uygulama kullanımını ve Eğilimleri raporu**:  Bu rapor, kuruluşunuzda kullanılan uygulamaları bir anlayış verir. En popüler uygulamalar listesini almak veya kuruluşunuzda son zamanlarda toplu bir uygulamayı nasıl kullanıldığını anlayın. Bu, izleme ve kullanım zaman içinde geliştirilmeye sağlar.

**Konuma ve kullanıcılara göre oturum açma**: Bu rapor, tüm oturum açma Azure kimliği kullanılarak gerçekleştirilen işlemleri hakkında veri sağlar. Bu rapor, tek tek oturum açma aşağı inebilir ve şunun gibi sorular sorun:

- Burada bu kullanıcı gelen oturum?
- Hangi kullanıcı en çok oturum açma sayısına sahip ve nereden oturum açıyor? 
- Oturum açma işlemi başarılı olmuş mu?  
 
Ayrıca, belirli bir tarih veya konumu seçerek sonuçlarını filtreleyebilirsiniz.

**Uygulama başına benzersiz kullanıcı**:  Bu rapor, belirli bir uygulamayı kullanan tüm benzersiz kullanıcıları bir görünümünü sağlar. Yalnızca sahip kullanıcılar içerir "*başarıyla*" bir uygulamada oturum açmış.

**Cihaz oturum açma işlemleri**: Bu rapor, kuruluşunuzda kullanılan cihaz profillerini anlayabilir ve kullanıma göre cihaz ilkeleri belirleyebilirsiniz yardımcı olur. Bu veriler dahil olmak üzere kullanıcılar hakkında ayrıntılı bilgiyle beraber uygulamalara oturum açmak için kullanılan tarayıcılar ve işletim sistemi türü sağlar:

- User Name
- IP Adresi
- Location 
- Oturum açma durumu 

**SSPR huni**: Bu rapor SSPR aracının kuruluşunuzda nasıl kullanıldığını anlamanıza yardımcı olur. Kaç parola sıfırlama, SSPR aracıyla bulunulması görebilir ve kaç tanesinin başarılı. Ayrıca, parola sıfırlama hatalarının inin ve hunisini neden anlama. 

## <a name="customize-azure-ad-activity-content-pack"></a>Azure AD etkinlik içerik paketini özelleştirme

**Görselleştirme Değiştir**:  Tıklayarak raporun görsellerini değiştirebilirsiniz **Raporu Düzenle** ve istediğiniz görselleştirmeyi seçin.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/10.png) 

**Ek alanları dahil**:  Rapor için bir alan eklemek veya alan eklemek/kaldırmak istediğiniz görsele tıklayarak kaldırın. Örneğin, aşağıda gösterildiği gibi Tablo görünümüne "oturum açma durumu" alanını ekleyebilirsiniz. 
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/11.png) 

**Görselleştirmeleri bir panoya sabitleme**:  Panoya sabitleme ve kendi görselleştirmelerinizi rapora dahil olmak üzere, panoyu özelleştirebilirsiniz. 

![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/13.png) 
 
**Pano paylaşmak**: Ayrıca, kuruluşunuzdaki kullanıcılarla Pano paylaşabilirsiniz. Raporu paylaştığınızda raporda seçtiğiniz alanları kullanıcılar görebilir.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Power BI raporunuzun her gün yenilenmesini zamanlayabilirsiniz.

Power BI raporunuzun her gün yenilenmesi zamanlamak için Git **veri kümeleri** > **ayarları** > **yenilemeyi zamanla** ve aşağıda gösterildiği gibi ayarlayın.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>İçerik paketinin daha yeni sürüme güncelleştirin

İçerik paketinizi yeni sürüme güncelleştirmek istiyorsanız:

- Yeni içerik paketini indirin ve bu makaledeki yönergeleri kullanarak ayarlayın.

- Git, kurduktan sonra **veri kaynağı** > **ayarları** > **veri kaynağı kimlik bilgileri** ve kimlik bilgilerinizi yeniden girin.

    ![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/16.png) 

İçerik paketinin yeni sürümü beklendiği gibi çalıştığını doğruladıktan sonra bağlantılı raporlara ve veri kümeleri söz konusu içerik paketiyle ilişkili silerek gerekirse eski sürümü kaldırabilirsiniz.

## <a name="troubleshoot-content-pack-errors"></a>İçerik Paketi hatalarını giderme

İçerik Paketi ile çalışırken aşağıdaki hatalarla karşılaşırsanız çalıştırmak mümkündür: 

- [Yenileme başarısız oldu](#refresh-failed) 
- [Veri kaynağı kimlik bilgileri güncelleştirilemedi](#failed-to-update-data-source-credentials) 
- [Veri alma çok uzun sürüyor](#data-import-is-too-slow) 

Power BI hakkında genel yardım için bu [yardım makalelerini inceleyin](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>Yenileme başarısız oldu 
 
**Bu hatanın nasıl kullanıma sunulur**: Power BI veya yenileme geçmişi başarısız durumunda e-posta. 


| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| İçerik paketine bağlanma kullanıcıların kimlik bilgilerini sıfırlama ancak içerik paketi bağlantı ayarlarında güncelleştirilmemiş hataları nedeniyle başarısız oldu yenileyin. | Power BI'da Azure AD etkinlik günlüklerini panoya karşılık gelen bulun (**Azure Active Directory etkinlik günlükleri**), yenileme zamanlama seçin ve ardından Azure AD kimlik bilgilerinizi girin. |
| Yenileme, temel alınan içerik paketindeki veri sorunları nedeniyle başarısız olabilir. | [Bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>Veri kaynağı kimlik bilgileri güncelleştirilemedi 
 
**Bu hatanın nasıl kullanıma sunulur**: Azure AD etkinlik günlükleri içerik paketine yeniden bağlandığınızda Power BI,. 

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Bağlanan kullanıcının genel yönetici veya güvenlik okuyucusu veya güvenlik yöneticisi değil. | Genel yönetici veya güvenlik okuyucusu veya Güvenlik Yöneticisi içerik paketlerine erişmek için bir hesap kullanın. |
| Kiracınızın Premium Kiracı değil veya dosya Premium lisansı ile en az bir kullanıcı yok. | [Bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>Veri içeri aktarma çok yavaş 
 
**Bu hatanın nasıl kullanıma sunulur**: İçerik paketiniz bağlandıktan sonra Power BI'da veri içeri aktarma işlemi için Azure AD etkinlik panonuzu hazırlamak başlar. günlükleri. Şu iletiyle karşılaşırsınız: **Veri alınıyor...**  herhangi bir gelişme olmadan.  

| Nedeni | Nasıl düzeltileceğini |
| ---   | ---        |
| Kiracınızın boyutuna bağlı olarak, bu adımı her yerde birkaç dakika veya 30 dakika sürebilir. | İleti bir saat içinde Panonuzda gösterecek şekilde değişmezse [bir destek bileti](../fundamentals/active-directory-troubleshooting-support-howto.md).|
  
## <a name="next-steps"></a>Sonraki adımlar

* [Power BI içerik Paketi'ni yüklemek](quickstart-install-power-bi-content-pack.md).
* [Azure AD raporlar nedir? ](overview-reports.md).
