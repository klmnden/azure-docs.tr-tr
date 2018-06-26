---
title: Azure Application Insights analizleri sorunlarını giderme | Microsoft Docs
description: 'Application Insights analytics sorunları? Buradan başlayın. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2016
ms.author: mbullwin
ms.openlocfilehash: eeda0fa6ad8faa05baf0a9344e958d298fb80d8e
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936692"
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Application Insights Analiz Sorunlarını Giderme
Sorun [uygulama Öngörüler Analytics](app-insights-analytics.md)? Buradan başlayın. Analytics Azure Application Insights güçlü arama aracıdır.

## <a name="limits"></a>Sınırlar
* Şu anda geçmiş verileri bir hafta içinde yalnızca sorgu sonuçları sınırlıdır.
* Biz test tarayıcılar: Chrome, sınır ve Internet Explorer'ın en son sürümleri.

## <a name="known-incompatible-browser-extensions"></a>Bilinen uyumlu tarayıcı uzantıları
* Ghostery

Uzantıyı devre dışı bırakmak veya farklı bir tarayıcı kullanın.

## <a name="e-a"></a> "Beklenmeyen hata"
![Beklenmeyen hata ekranı](./media/app-insights-analytics-troubleshooting/010.png)

İç hata portal çalışma zamanı sırasında – işlenmeyen bir özel durum oluştu.

* Tarayıcının önbelleğini temizlemek. 

## <a name="e-b"></a>403 ...Lütfen yeniden deneyin
![403 ...Lütfen yeniden deneyin](./media/app-insights-analytics-troubleshooting/020.png)

Kimlik doğrulamasıyla ilgili bir hata oluştu (kimlik doğrulama veya erişim belirteci oluşturma sırasında). Portal tarayıcı ayarları değiştirmeden kurtarma yolu olabilir.

* Doğrulama [üçüncü taraf tanımlama bilgilerini etkin](#cookies) tarayıcıda. 

## <a name="authentication"></a>403 ...güvenlik bölgesi doğrulayın
![403 ...verify güvenlik bölgesi](./media/app-insights-analytics-troubleshooting/030.png)

Kimlik doğrulamasıyla ilgili bir hata oluştu (kimlik doğrulama veya erişim belirteci oluşturma sırasında). Portal tarayıcı ayarları değiştirmeden kurtarma yolu olabilir.

1. Doğrulama [üçüncü taraf tanımlama bilgilerini etkin](#cookies) tarayıcıda. 
2. Analytics portalını açmak için sık kullanılan, yer işareti veya kayıtlı bağlantılardan birini mi kullandınız? Bağlantıyı kaydettiğinizde kullandığınızdan farklı kimlik bilgileriyle mi oturum açtınız?
3. InPrivate/gizli tarayıcı penceresi kullanmayı deneyin (tüm ilgili pencereleri kapattıktan sonra). Kimlik bilgilerinizi sağlamanız gerekir. 
4. Başka bir (normal) tarayıcı penceresi açın ve gidin [Azure](https://portal.azure.com). Oturumunuzu kapatın. Sonra bağlantınızı açın ve doğru kimlik bilgileriyle oturum açın.
5. Edge ve Internet Explorer kullanıcıları, güvenilir bölge ayarlarının desteklenmediği durumlarda da bu hatayı alabilir.
   
    Her ikisi de doğrulayın [Analytics portalı](https://portal.azure.com) ve [Azure Active Directory portalında](https://portal.azure.com) aynı güvenlik bölgesinde şunlardır:
   
   * Internet Explorer'da açın **Internet Seçenekleri**, **güvenlik**, **Güvenilen siteler**, **siteleri**:
     
     ![Internet Seçenekleri iletişim kutusunda, Güvenilen siteler için bir site ekleme](./media/app-insights-analytics-troubleshooting/033.png)
     
     Web Siteleri listesinde aşağıdaki URL’lerden herhangi bir varsa diğerlerinin de listeye eklendiğinden emin olun:
     
     https://analytics.applicationinsights.io<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Kaynak bulunamadı
![404 ... kaynak bulunamadı](./media/app-insights-analytics-troubleshooting/040.png)

Uygulama kaynağı Application Insights silindi ve artık kullanılamıyor. URL Analytics sayfasına kaydettiyseniz, bu durum oluşabilir.

## <a name="e-e"></a>403 ... Yok
![403 ... yetkili değil](./media/app-insights-analytics-troubleshooting/050.png)

Bu uygulamayı analizleri açmak için izniniz yok.

* Bağlantıyı birisinden mı aldınız? İçinde olduğundan emin olmak için isteyin [okuyucular veya katkıda bulunanlar bu kaynak grubu için](app-insights-resources-roles-access-control.md).
* Farklı kimlik bilgileri kullanarak bağlantı kaydettiniz? Açık [Azure portal](https://portal.azure.com), oturumu kapatın ve ardından doğru kimlik bilgilerini sağlayarak bu bağlantıyı yeniden deneyin.

## <a name="html-storage"></a>403 ... HTML5 depolaması
HTML5 localStorage ve sessionStorage portalımıza kullanır.

* Chrome: Ayarları, gizlilik, içerik ayarları.
* Internet Explorer: Internet Seçenekleri, Gelişmiş sekmesinde, güvenlik, DOM depolama etkinleştir

![403 ... HTML5 depolaması etkinleştirmeyi deneyin](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Abonelik bulunamadı
![404 ... Abonelik bulunamadı](./media/app-insights-analytics-troubleshooting/070.png)

URL geçersiz. 

* Uygulama kaynağı açın [Application Insights portalındaki](https://portal.azure.com). Ardından Analytics düğmesini kullanın.

## <a name="e-h"></a>404 ... sayfa yok
![404 ... Sayfa yok](./media/app-insights-analytics-troubleshooting/080.png)

URL geçersiz.

* Uygulama kaynağı açın [Application Insights portalındaki](https://portal.azure.com). Ardından Analytics düğmesini kullanın.

## <a name="cookies"></a>Üçüncü taraf tanımlama bilgilerini etkinleştirin
  Bkz: [üçüncü taraf tanımlama bilgilerini devre dışı bırakma](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ancak ihtiyacımız için bildirim **etkinleştirmek** bunları.


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

