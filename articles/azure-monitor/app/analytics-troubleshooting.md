---
title: Azure Application Insights analiz sorunlarını giderme | Microsoft Docs
description: 'Application Insights analytics ile sorunları mı yaşıyorsunuz? Buradan başlayın. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: mbullwin
ms.openlocfilehash: ecf0638aa999208331603ac30ccf4eb17b3c4500
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60692390"
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Application Insights Analiz Sorunlarını Giderme
Sorun [Application Insights Analytics](analytics.md)? Buradan başlayın. Analytics, Azure Application Insights'ın güçlü bir arama aracıdır.

## <a name="limits"></a>Limits
* Şu anda geçmiş verilerin bir hafta içerisinde yalnızca sorgu sonuçları sınırlıdır.
* Şu test tarayıcılar: en son Chrome, Microsoft Edge ve Internet Explorer sürümleri.

## <a name="known-incompatible-browser-extensions"></a>Bilinen uyumlu tarayıcı uzantıları
* Ghostery

Uzantı devre dışı bırakın veya farklı bir tarayıcı kullanın.

## <a name="e-a"></a> "Beklenmeyen bir hata"
![Beklenmeyen bir hata ekranı](media/analytics-troubleshooting/010.png)

Portal çalışma zamanı işlenmeyen özel durum sırasında iç hata oluştu.

* Tarayıcının önbelleğini temizleyin.

## <a name="e-b"></a>403 ...Lütfen yeniden deneyin
![403 ...Lütfen yeniden deneyin](media/analytics-troubleshooting/020.png)

Kimlik doğrulamasıyla ilgili bir hata oluştu (kimlik doğrulama veya erişim belirteci oluşturma sırasında). Portal, tarayıcı ayarları değiştirilmeden kurtarılamayabilir yolu olabilir.

* Doğrulama [üçüncü taraf tanımlama bilgilerini etkin](#cookies) tarayıcıda. 

## <a name="authentication"></a>403 ...güvenlik bölgesi doğrulayın
![403 ...verify güvenlik bölgesi](media/analytics-troubleshooting/030.png)

Kimlik doğrulamasıyla ilgili bir hata oluştu (kimlik doğrulama veya erişim belirteci oluşturma sırasında). Portal, tarayıcı ayarları değiştirilmeden kurtarılamayabilir yolu olabilir.

1. Doğrulama [üçüncü taraf tanımlama bilgilerini etkin](#cookies) tarayıcıda. 
2. Analytics portalını açmak için sık kullanılan, yer işareti veya kayıtlı bağlantılardan birini mi kullandınız? Bağlantıyı kaydettiğinizde kullandığınızdan farklı kimlik bilgileriyle mi oturum açtınız?
3. InPrivate/gizli tarayıcı penceresi kullanmayı deneyin (tüm ilgili pencereleri kapattıktan sonra). Kimlik bilgilerinizi sağlamanız gerekir. 
4. Başka bir (normal) bir tarayıcı penceresi açın ve gidin [Azure](https://portal.azure.com). Oturumunuzu kapatın. Sonra bağlantınızı açın ve doğru kimlik bilgileriyle oturum açın.
5. Güvenilir Bölge ayarlarının desteklenmediği durumlarda kullanıcıların Microsoft Edge ve Internet Explorer da bu hatayı alabilirsiniz.
   
    Her ikisi de doğrulayın [analiz portalı](https://portal.azure.com) ve [Azure Active Directory portalında](https://portal.azure.com) aynı güvenlik bölgesinde bulunan:
   
   * Internet Explorer'da açın **Internet Seçenekleri**, **güvenlik**, **Güvenilen siteler**, **siteleri**:
     
     ![Internet Seçenekleri iletişim kutusunda, bir siteyi Güvenilen siteler için ekleme](media/analytics-troubleshooting/033.png)
     
     Web Siteleri listesinde aşağıdaki URL’lerden herhangi bir varsa diğerlerinin de listeye eklendiğinden emin olun:
     
     https://analytics.applicationinsights.io<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Kaynak bulunamadı
![404 ... kaynak bulunamadı](media/analytics-troubleshooting/040.png)

Uygulama kaynağı Application Insights'tan silindi ve artık kullanılamaz. Analytics sayfanın URL'sini kaydedilirse, bu durum oluşabilir.

## <a name="e-e"></a>403 ... Yetkilendirme yok
![403 ... yetkili değil](media/analytics-troubleshooting/050.png)

Analytics'te bu uygulamayı açmak için izniniz yok.

* Bağlantıyı birisinden mu? İçinde olduğundan emin olmak için isteyin [okuyucular veya bu kaynak grubu için katkıda bulunanlar](../../azure-monitor/app/resources-roles-access-control.md).
* Farklı kimlik bilgileri kullanılarak bağlantı kaydettiniz? Açık [Azure portalında](https://portal.azure.com), oturumu kapatın ve ardından doğru kimlik bilgilerini girerek bu bağlantıyı yeniden deneyin.

## <a name="html-storage"></a>403 ... HTML5 depolama
HTML5 localStorage ve sessionStorage portalımıza kullanır.

* Chrome: Ayarları, gizlilik, içerik ayarlar.
* Internet Explorer: Internet Seçenekleri, Gelişmiş sekmesinde, güvenlik, DOM depolama etkinleştir

![403 ... HTML5 depolaması etkinleştirmeyi deneyin](media/analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Abonelik bulunamadı
![404 ... Abonelik bulunamadı](media/analytics-troubleshooting/070.png)

URL geçersiz. 

* Uygulama kaynağı açın [Application Insights portalında](https://portal.azure.com). Ardından Analytics düğmesini kullanın.

## <a name="e-h"></a>404 ... sayfa yok
![404 ... Sayfa yok](media/analytics-troubleshooting/080.png)

URL geçersiz.

* Uygulama kaynağı açın [Application Insights portalında](https://portal.azure.com). Ardından Analytics düğmesini kullanın.

## <a name="cookies"></a>Üçüncü taraf tanımlama bilgilerini etkinleştirin
  Bkz: [üçüncü taraf tanımlama bilgilerini devre dışı bırakma](https://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ancak ihtiyacımız için bildirim **etkinleştirme** bunları.


[!INCLUDE [app-insights-analytics-footer](../../../includes/app-insights-analytics-footer.md)]

