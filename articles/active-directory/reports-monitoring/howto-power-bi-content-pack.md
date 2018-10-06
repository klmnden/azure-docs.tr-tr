---
title: Azure Active Directory Power BI İçerik Paketi'ni kullanma | Microsoft Docs
description: Azure Active Directory Power BI İçerik Paketi'ni kullanmayı öğrenin
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c0326a6b611d5f3d5633db2d2b64b8cdc15e10a7
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816693"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Azure Active Directory Power BI İçerik Paketi'ni kullanma

|  |
|--|
|Şu anda Azure AD Power BI içerik paketi, Azure AD kiracınızdan verileri almak için Azure AD Graph API kullanır. Sonuç olarak, içerik paketindeki veri kullanarak alınan verileri arasındaki bazı girdilerinde görebilirsiniz [raporlama için Microsoft Graph API'lerini](concept-reporting-api.md). |
|  |

BT yöneticisi, kullanıcılarınıza nasıl benimseyin ve Azure Active Directory özelliklerine anlamanız gerekir. Bu, BT altyapınız ve iletişim artırmalarını ve en iyi Azure AD özellikleri dışında almak için plana sağlar. Azure Active Directory için Power BI içerik paketi, verilerinizi içine dizininizle neler olduğunu daha zengin içgörüler toplamak için daha fazla analiz olanağı sağlar. Kolayca tümleşme, Azure Active Directory API'lerini Power bı'a, önceden oluşturulmuş içerik paketi indirebilir ve Azure Active Power BI'ı sunan zengin görselleştirme deneyiminden faydalanarak Directory'niz tüm etkinlikleri öngörü. Kendi panonuzu oluşturabilir ve kuruluşunuzdaki herkesle kolayca paylaşabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

İçerik paketini kullanmak için bir Azure AD premium (ö1/ö2) lisansı gerekir. 

## <a name="install-the-content-pack"></a>İçerik Paketi yükleyin

Kullanıma [hızlı](quickstart-install-power-bi-content-pack.md) Azure AD Power BI içerik Paketi'ni yüklemek için.

## <a name="what-can-i-do-with-this-content-pack"></a>Bu içerik paketiyle ne yapabilirim?

Bu içerik paketiyle yapabileceklerinizi anlatmadan önce paketteki raporlar hakkında kısa bilgiler vermek istiyoruz. Rapor verileri **son 30 günü** kapsar.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Bu Azure Active Directory günlükleri İçerik Paketi sürümünde yer alan raporlar

**Uygulama Kullanımı ve Eğilim Raporu**: Kuruluşunuzda kullanılan uygulamalar ve hangisinin en çok ve hangi zamanlarda kullanıldığıyla ilgili öngörülere ulaşın. Bu raporu kullanarak kuruluşunuzda kısa süre önce kullanıma sunduğunuz bir uygulama hakkında öngörüye sahip olabilir veya popüler uygulamaları görebilirsiniz. Bu sayede kullanılmayan uygulamaların kullanılmasını teşvik edebilirsiniz.

**Konuma ve kullanıcılara göre oturum açma sayıları**: Azure kimliği kullanılarak gerçekleştirilen tüm oturum açma işlemleri ve kullanıcıların kimliği hakkında öngörüler toplayın. Bu bilgiyle oturum açma işlemleri hakkında ayrıntılı bilgilere ulaşabilir ve şu soruların yanıtlarını bulabilirsiniz:

- Bu kullanıcı en çok nereden oturum açmış?
- Hangi kullanıcı en çok oturum açma sayısına sahip ve nereden oturum açıyor? 
- Oturum açma işlemi başarılı olmuş mu?  
 
Belirli bir tarihe veya konuma tıklayarak ayrıntılara ulaşabilirsiniz.

**Uygulama başına benzersiz kullanıcı sayısı**: Belirli bir uygulamayı kullanan tüm benzersiz kullanıcıları görüntüleyin. Buna yalnızca uygulamada “*başarıyla*” oturum açmış kullanıcılar dahil edilir.

**Cihazdan oturum açma işlemleri**: Kuruluşunuzdaki kullanıcıların işletim sistemleri ve tarayıcıları hakkında bilgi edinin ve kullanıcılarla ilgili şu ayrıntılı bilgilere ulaşın:

- User Name
- IP Adresi
- Konum 
- Oturum açma durumu 

Bu rapor türüyle kuruluşunuzda kullanılan cihaz profillerini anlayabilir ve kullanıma göre cihaz ilkeleri belirleyebilirsiniz

**SSPR Hunisi**: Kuruluşunuzda parola sıfırlama işlemlerinin nasıl gerçekleştirildiğini kavrayın. SSPR aracıyla kaç parola sıfırlama girişimi yapıldığını ve kaçının başarılı olduğunu görün. SSPR hunisini kullanarak Parola sıfırlama işlemlerinin ayrıntılarına inin ve başarısız olan işlemlerin nedenini öğrenin. Bu rapor SSPR aracının kuruluşunuzda nasıl kullanıldığını göstererek doğru kararlar almanızı sağlar.

## <a name="customizing-azure-ad-activity-content-pack"></a>Azure AD Etkinlik içerik paketini özelleştirme

**Görselleri Değiştirme**: **Raporu Düzenle**'ye tıklayıp istediğiniz görünümü seçerek raporun görsellerini değiştirebilirsiniz.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/09.png) 
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/10.png) 

**Ek alanları dahil etme**: Alan eklemek/kaldırmak istediğiniz görsele tıklayarak rapora alan ekleyebilir veya var olan alanları kaldırabilirsiniz. Aşağıdaki örnekte tablo görünümüne "oturum açma durumu" alanını ekliyorum. 
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/11.png) 

**Görselleri panonuza sabitleyin**: Panonuzu özelleştirebilir ve kendi görselleştirmelerinizi rapora ekleyip panoya sabitleyebilirsiniz. Aşağıdaki örnekte "Oturum Açma Durumu" adında yeni bir filtre ekledim ve rapora dahil ettim. Ayrıca çubuk grafik görselini çizgi grafik olarak değiştirdim be bu yeni görseli panoya sabitleyebilirim.

![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/12.png) 

![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/13.png) 
 

 


**Panonuzu paylaşma**: İstediğiniz içeriği oluşturduktan sonra panoyu kuruluşunuzdaki kullanıcılarla paylaşabilirsiniz. Raporu paylaştığınızda raporda belirlediğiniz alanları görebileceklerini unutmayın.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Power BI raporunuzun her gün yenilenmesini planlama

Power BI raporunuzun her gün yenilenmesi için **Veri Kümeleri > Ayarlar > Yenilemeyi Zamanla** sayfasına gidin ve aşağıdaki gibi ayarlayın.
 
![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>İçerik paketini yeni sürüme güncelleştirme

İçerik paketinizi yeni sürüme güncelleştirmek istiyorsanız:

- Yeni içerik paketini indirin ve bu makaledeki talimatlara göre kurun.

- Kurduktan sonra **Veri Kaynağı > Ayarlar > Veri kaynağı kimlik bilgileri** sayfasına gidin ve aşağıdaki gibi kimlik bilgilerinizi tekrar girin

    ![Azure Active Directory Power BI İçerik Paketi](./media/howto-power-bi-content-pack/16.png) 

İçerik paketinin yeni sürümü çalışmaya başladığında ihtiyaç duymanız halinde eski içerik paketiyle ilişkilendirilmiş raporları ve veri kümelerini silerek onu kaldırabilirsiniz.

## <a name="still-having-issues"></a>Sorun yaşamaya devam mı ediyorsunuz? 

[Sorun giderme kılavuzumuzu](troubleshoot-content-pack.md) inceleyin. Power BI hakkında genel yardım için bu [yardım makalelerini inceleyin](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Sonraki adımlar

* [Power BI içerik Paketi'ni yüklemek](quickstart-install-power-bi-content-pack.md).
* [İçerik Paketi hatalarında sorun giderme](troubleshoot-content-pack.md).
* [Azure AD raporlar nedir? ](overview-reports.md).
