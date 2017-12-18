---
title: "Azure Active Directory Power BI İçerik Paketi'ni kullanma | Microsoft Docs"
description: "Azure Active Directory Power BI İçerik Paketi'ni kullanmayı öğrenin"
services: active-directory
author: MarkusVi
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: 
ms.topic: get-started-article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/06/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 503b3f4c576382d8ce965d1f90aadda32c819a0b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Azure Active Directory Power BI İçerik Paketi'ni kullanma

Kullanıcılarınızın Azure Active Directory özelliklerini benimseme ve kullanma şekli BT yöneticileri için çok önemlidir. Kullanımı artırmak ve AAD özelliklerinden en iyi şekilde faydalanmak için BT altyapınızı planlamanızı ve şirket içi iletişimi planlamanızı sağlar. Azure Active Directory için Power BI İçerik Paketi, verilerinizi analiz etmenizi ve Azure Active Directory'nin yoğun kullandığınız özelliklerinin durumu hakkında daha iyi öngörülere sahip olmak amacıyla nasıl kullanabileceğinizi anlamanızı sağlar.  Azure Active Directory API'lerini Power BI ile tümleştirerek önceden oluşturulmuş içerik paketlerini kolayca indirebilir ve Power BI tarafından sunulan zengin görselleştirme deneyiminden faydalanarak Azure Active Directory içindeki tüm işlemler hakkında öngörüye sahip olabilirsiniz. Kendi panonuzu oluşturabilir ve kuruluşunuzdaki herkesle kolayca paylaşabilirsiniz. 

Bu konu içerik paketini ortamınıza yüklemeniz ve kullanmanız için izlemeniz gereken yönergeleri sunmaktadır.

## <a name="installation"></a>Yükleme  

**Power BI İçerik Paketi'ni yüklemek için:**

1. Power BI Hesabınızla (O365 veya Azure AD Hesabınızla aynıdır) [Power BI](https://app.powerbi.com/groups/me/getdata/services) sitesinde oturum açın.

2. Sol gezinti bölmesinin en altında **Veri Al**'ı seçin.

    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. **Hizmetler** kutusunda **Al**'a tıklayın.
   
    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  **Azure Active Directory** aratın.

    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  İstendiğinde Azure AD Kiracı Kimliğinizi girin ve **İleri**'ye tıklayın.

    > [!TIP] 
    > Office 365 / Azure AD kiracınız için Kiracı kimliğinizi öğrenmenin en hızlı yolu, dizinde detaya giderek [**Özellikler** sayfasında](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) **Dizin kimliği**’ni kopyalamaktır.

    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  **Oturum aç**'a tıklayın. 
 
    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  Kullanıcı adınızı ve parolanızı girip **Oturum aç**'a tıklayın.
 
    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  Uygulama onayı iletişim kutusunda **Kabul et**'e tıklayın.
 
9.  Azure Active Directory Etkinlik günlükleri panonuz oluşturulduğunda üzerine tıklayın.
 
    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

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
 
![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

**Ek alanları dahil etme**: Alan eklemek/kaldırmak istediğiniz görsele tıklayarak rapora alan ekleyebilir veya var olan alanları kaldırabilirsiniz. Aşağıdaki örnekte tablo görünümüne "oturum açma durumu" alanını ekliyorum. 
 
![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

**Görselleri panonuza sabitleyin**: Panonuzu özelleştirebilir ve kendi görselleştirmelerinizi rapora ekleyip panoya sabitleyebilirsiniz. Aşağıdaki örnekte "Oturum Açma Durumu" adında yeni bir filtre ekledim ve rapora dahil ettim. Ayrıca çubuk grafik görselini çizgi grafik olarak değiştirdim be bu yeni görseli panoya sabitleyebilirim.

![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


**Panonuzu paylaşma**: İstediğiniz içeriği oluşturduktan sonra panoyu kuruluşunuzdaki kullanıcılarla paylaşabilirsiniz. Raporu paylaştığınızda raporda belirlediğiniz alanları görebileceklerini unutmayın.
 
![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Power BI raporunuzun her gün yenilenmesini planlama

Power BI raporunuzun her gün yenilenmesi için **Veri Kümeleri > Ayarlar > Yenilemeyi Zamanla** sayfasına gidin ve aşağıdaki gibi ayarlayın.
 
![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>İçerik paketini yeni sürüme güncelleştirme

İçerik paketinizi yeni sürüme güncelleştirmek istiyorsanız:

- Yeni içerik paketini indirin ve bu makaledeki talimatlara göre kurun.

- Kurduktan sonra **Veri Kaynağı > Ayarlar > Veri kaynağı kimlik bilgileri** sayfasına gidin ve aşağıdaki gibi kimlik bilgilerinizi tekrar girin

    ![Azure Active Directory Power BI İçerik Paketi](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

İçerik paketinin yeni sürümü çalışmaya başladığında ihtiyaç duymanız halinde eski içerik paketiyle ilişkilendirilmiş raporları ve veri kümelerini silerek onu kaldırabilirsiniz.

## <a name="still-having-issues"></a>Sorun yaşamaya devam mı ediyorsunuz? 

[Sorun giderme kılavuzumuzu](active-directory-reporting-troubleshoot-content-pack.md) inceleyin. Power BI hakkında genel yardım için bu [yardım makalelerini inceleyin](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Sonraki adımlar

Raporlamaya genel bir bakış için bkz. [Azure Active Directory raporlama](active-directory-reporting-azure-portal.md).
