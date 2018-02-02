---
title: "Uygulama proxy'si sorunlarını giderme | Microsoft Docs"
description: "Azure AD uygulama proxy'si hataların nasıl giderileceği kapsar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 970caafb-40b8-483c-bb46-c8b032a4fb74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 4291d765bec94ca1edd50b8df0c414524f29fba2
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Uygulama proxy'si sorunları ve hata iletileri sorunlarını giderme
Yayımlanmış bir uygulamanın erişme veya yayımlama uygulamalarda hatalar meydana gelirse, Microsoft Azure AD uygulama proxy'si düzgün çalışıp çalışmadığını görmek için aşağıdaki seçeneklerden denetleyin:

* Windows Hizmetleri konsolunu açın ve doğrulayın **Microsoft AAD Application Proxy Connector** hizmetidir etkin ve çalışıyor. Ayrıca uygulama proxy'si hizmeti özellikleri sayfanın aramak aşağıdaki resimde gösterildiği gibi istediğiniz:  
  ![Microsoft AAD uygulama Proxy Bağlayıcısı özellikleri penceresinin ekran görüntüsü](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)
* Olay Görüntüleyicisi'ni açın ve uygulama Proxy Bağlayıcısı olayları arayın **uygulama ve hizmet günlükleri** > **Microsoft** > **AadApplicationProxy** > **bağlayıcı** > **yönetici**.
* Gerekli olursa, daha ayrıntılı günlükler tarafından [uygulama Proxy Bağlayıcısı oturum açtığında kapatma](application-proxy-understand-connectors.md#under-the-hood).

Azure AD sorun giderme aracı hakkında daha fazla bilgi için bkz: [bağlayıcı ağ Önkoşullar doğrulamak için sorun giderme aracı](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites).

## <a name="the-page-is-not-rendered-correctly"></a>Sayfa doğru işlenmez
İşleme veya yanlış özel hata iletileri almadan çalışmıyor uygulamanız ile ilgili sorunlar olabilir. Bu makale yolu yayımlanan ancak uygulama dışında yol mevcut içeriği gerektiriyor ortaya çıkabilir.

Örneğin, yol https://yourapp/app yayımlama ancak uygulama içinde https://yourapp/media görüntüleri çağırır, bunlar işlenip olmaz. Tüm ilgili içerik içermesi gereken en yüksek düzey yolu kullanarak uygulama yayımlama emin olun. Bu örnekte, http://yourapp/ olacaktır.

Başvurulan içerik dahil, ancak yine de daha derin bir bağlantıyı yolunda güden kullanıcılara gerekir, yolunu değiştirirseniz, blog gönderisine bakın [paneli ve Office 365 uygulama Başlatıcı Azure AD uygulama proxy'si uygulamalarda erişmek için doğru bağlantı ayarı](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="connector-errors"></a>Bağlayıcı hataları

Kullanım [Azure AD uygulama Proxy Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/) Bağlayıcınızı uygulama proxy'si hizmeti ulaşabilir doğrulanamadı. En azından, Orta ABD bölgesi ve size en yakın bölgeyi tüm yeşil onay işaretli olduğundan emin olun. Bunun ötesinde, daha fazla yeşil onay işaretleri büyük esneklik anlamına gelir. 

Kayıt sırasında Bağlayıcı Sihirbazı'nı yükleme başarısız olursa, başarısızlığın nedenini görüntülemek için iki yolu vardır. Ya da konum altındaki olay günlüğü **uygulamaları ve Hizmetleri Logs\Microsoft\AadApplicationProxy\Connector\Admin**, veya aşağıdaki Windows PowerShell komutunu çalıştırın:

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

Olay günlüğünden bağlayıcı hata bulduktan sonra bu sorunu gidermek için yaygın hataların bu tabloyu kullanın:

| Hata | Önerilen adımlar |
| ----- | ----------------- |
| Bağlayıcı kaydı başarısız oldu: etkin Azure Yönetim Portalı'nda uygulama proxy'si ve Active Directory kullanıcı adınızı ve parolanızı doğru girdiğinizden emin olun. Hata: 'bir veya daha fazla hata oluştu.' | Azure AD ile oturum açma olmadan kaydı penceresi kapattıysanız, bağlayıcı Sihirbazı'nı yeniden çalıştırın ve bağlayıcı kaydedin. <br><br> Kayıt penceresi açar ve oturum açmanızı vermeden hemen kapatılıyor, büyük olasılıkla bu hatayı alırsınız. Sisteminizde bir ağ hatası olduğunda bu hata oluşur. Tarayıcıdan ortak bir Web sitesine bağlanmak mümkündür ve bağlantı noktaları belirtildiği şekilde açık olduğundan emin olun [uygulama ara sunucusu önkoşulları](active-directory-application-proxy-enable.md). |
| NET hata kaydı penceresinde görüntülenir. Devam edilemiyor. | Bu hatayı görürseniz ve penceresi kapanır yanlış kullanıcı adı ya da parola girdi. Yeniden deneyin. |
| Bağlayıcı kaydı başarısız oldu: etkin Azure Yönetim Portalı'nda uygulama proxy'si ve Active Directory kullanıcı adınızı ve parolanızı doğru girdiğinizden emin olun. Hata: ' AADSTS50059: hiçbir Kiracı tanımlama bilgileri ya da istek bulunamadı veya asıl URI başarısız oldu, kimlik bilgileri ve arama hizmeti tarafından sağlanan herhangi tarafından kapsanan. | Bir Microsoft Account ve Kuruluş Kimliği erişmeye çalıştığınız dizininin parçası olan bir etki alanı değil kullanarak oturum açmaya çalışıyorsunuz. Yönetici, Kiracı etki alanı etki alanı adıyla aynı parçasıdır, örneğin, Azure AD etki alanı contoso.com ise, yönetici olması gerekir emin olun admin@contoso.com. |
| PowerShell komut dosyaları çalıştırmak için geçerli yürütme İlkesi alınamadı. | Bağlayıcı yükleme başarısız olursa, PowerShell yürütme ilkesini devre dışı olduğundan emin olmak için kontrol edin. <br><br>1. Grup İlkesi Düzenleyicisi'ni açın.<br>2. Git **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri** > **Windows PowerShell** çift tıklayın ve **komut dosyası yürütme kapatma**.<br>3. Yürütme ilkesi için ya da ayarlanabilir **yapılandırılmamış** veya **etkin**. Varsa kümesine **etkin**seçenekleri altında olduğundan emin olun, yürütme İlkesi ayarlandığından **yerel ve uzak imzalanmış komut dosyalarını izin** veya **tüm komut dosyalarına izin**. |
| Bağlayıcı yapılandırması yüklenemedi. | Kimlik doğrulaması için kullanılır, bağlayıcı'nın istemci sertifikasının süresi doldu. Bir proxy'nin arkasında yüklü bağlayıcı varsa, bu da oluşabilir. Bu durumda, bağlayıcı Internet'e erişemez ve uzak kullanıcılar için uygulamaları sağlamak mümkün olmaz. Güven kullanarak el ile yenileme `Register-AppProxyConnector` Windows PowerShell cmdlet'i. Bağlayıcınızı bir proxy'nin arkasında ise Internet erişimi bağlayıcı hesaplarına "Ağ Hizmetleri" ve "yerel sistem." vermek gerekli. Bu, onları Proxy için erişim verme veya onları proxy sunucusunu atlamak için ayarlayarak gerçekleştirilebilir. |
| Bağlayıcı kaydı başarısız oldu: Bağlayıcısı'nı kaydetmek için Active Directory genel Yöneticisi olduğundan emin olun. Hata: 'kayıt isteği reddedildi.' | Bu etki alanı yönetici ile oturum açmaya çalıştığınız diğer ad değil. Bağlayıcınızı kullanıcının etki alanı sahibi olan dizin için her zaman yüklenir. Oturum eklemeye çalıştığınız yönetici hesabı için Azure AD kiracısı genel izinlere sahip olduğundan emin olun. |

## <a name="kerberos-errors"></a>Kerberos hataları

Bu tablo Kerberos kurulumu ve yapılandırması gelen daha sık karşılaşılan kapsayan ve çözümü için öneriler içerir.

| Hata | Önerilen adımlar |
| ----- | ----------------- |
| PowerShell komut dosyaları çalıştırmak için geçerli yürütme İlkesi alınamadı. | Bağlayıcı yükleme başarısız olursa, PowerShell yürütme ilkesini devre dışı olduğundan emin olmak için kontrol edin.<br><br>1. Grup İlkesi Düzenleyicisi'ni açın.<br>2. Git **Bilgisayar Yapılandırması** > **Yönetim Şablonları** > **Windows bileşenleri** > **Windows PowerShell** çift tıklayın ve **komut dosyası yürütme kapatma**.<br>3. Yürütme ilkesi için ya da ayarlanabilir **yapılandırılmamış** veya **etkin**. Varsa kümesine **etkin**seçenekleri altında olduğundan emin olun, yürütme İlkesi ayarlandığından **yerel ve uzak imzalanmış komut dosyalarını izin** veya **tüm komut dosyalarına izin**. |
| 12008 - azure AD, izin verilen Kerberos kimlik doğrulama girişimlerini arka uç sunucusuna üst sınırını aştı. | Bu hata yanlış yapılandırma Azure AD arasında gösterebilir ve arka uç uygulama sunucusu ya da her iki makinede saat ve tarih yapılandırmasında bir sorun. Arka uç sunucusuna Azure AD tarafından oluşturulan Kerberos biletini reddetti. Doğrulayın, Azure AD ve arka uç uygulama sunucusu doğru şekilde yapılandırılır. Olduğundan emin olun saat ve tarih yapılandırmasını Azure AD temel ve arka uç uygulama sunucusu eşitlenir. |
| 13016 - sınır belirteci veya erişim tanımlama bilgisi yok UPN olduğundan azure AD kullanıcı adına bir Kerberos anahtarı alınamıyor. | STS yapılandırmasında bir sorun yoktur. UPN talep yapılandırma içinde STS düzeltin. |
| 13019 - azure AD kullanıcı adına bir Kerberos anahtarı şu genel API hata nedeniyle alınamıyor. | Bu olay Azure AD arasında yanlış yapılandırma gösterebilir ve etki alanı denetleyici sunucusu veya her iki makinede saat ve tarih yapılandırmasında bir sorun. Etki alanı denetleyicisi Azure AD tarafından oluşturulan Kerberos biletini reddetti. Doğrulayın, Azure AD ve arka uç uygulama sunucusu özellikle SPN yapılandırma doğru yapılandırılmış. Azure AD etki alanı denetleyicisi Azure AD ile güven oluşturur emin olmak için etki alanı denetleyicisi aynı etki alanı için etki alanına katılmış olduğundan emin olun. Olduğundan emin olun saat ve tarih yapılandırmasını Azure AD temel ve etki alanı denetleyicisi eşitlenir. |
| 13020 - arka uç sunucu SPN'si tanımlanmadığından azure AD kullanıcı adına bir Kerberos anahtarı alınamıyor. | Bu olay Azure AD arasında yanlış yapılandırma gösterebilir ve etki alanı denetleyici sunucusu veya her iki makinede saat ve tarih yapılandırmasında bir sorun. Etki alanı denetleyicisi Azure AD tarafından oluşturulan Kerberos biletini reddetti. Doğrulayın, Azure AD ve arka uç uygulama sunucusu özellikle SPN yapılandırma doğru yapılandırılmış. Azure AD etki alanı denetleyicisi Azure AD ile güven oluşturur emin olmak için etki alanı denetleyicisi aynı etki alanı için etki alanına katılmış olduğundan emin olun. Olduğundan emin olun saat ve tarih yapılandırmasını Azure AD temel ve etki alanı denetleyicisi eşitlenir. |
| 13022 - Kerberos kimlik doğrulama girişimlerini bir HTTP 401 hata ile arka uç sunucusuna yanıtlaması çünkü azure AD kullanıcı kimlik doğrulaması yapamaz. | Bu olay Azure AD arasında yanlış yapılandırma gösterebilir ve arka uç uygulama sunucusu ya da her iki makinede saat ve tarih yapılandırmasında bir sorun. Arka uç sunucusuna Azure AD tarafından oluşturulan Kerberos biletini reddetti. Doğrulayın, Azure AD ve arka uç uygulama sunucusu doğru şekilde yapılandırılır. Olduğundan emin olun saat ve tarih yapılandırmasını Azure AD temel ve arka uç uygulama sunucusu eşitlenir. |

## <a name="end-user-errors"></a>Son kullanıcı hataları

Bu liste uygulamasına erişmesi ve başarısız çalıştığınızda, son kullanıcılarınızın karşılaşabileceğiniz hataları kapsar. 

| Hata | Önerilen adımlar |
| ----- | ----------------- |
| Web sayfası görüntülenemiyor. | Kullanıcı, uygulama bir IWA uygulama ise, yayımlanan uygulamaya erişmeye çalışırken bu hatayı alabilirsiniz. Bu uygulama için tanımlanmış SPN yanlış olabilir. IWA uygulamaları için bu uygulama için yapılandırılan SPN doğru olduğundan emin olun. |
| Web sayfası görüntülenemiyor. | Kullanıcı, uygulamayı bir OWA uygulama ise, yayımlanan uygulamaya erişmeye çalışırken bu hatayı alabilirsiniz. Bunun nedeni aşağıdakilerden biri:<br><li>Bu uygulama için tanımlanmış SPN doğru değil. Bu uygulama için yapılandırılan SPN doğru olduğundan emin olun.</li><li>Uygulamaya erişmeyi denedi kullanıcının oturum açmak için kullandığını uygun şirket hesabı yerine bir Microsoft hesabı veya bir Konuk kullanıcı olduğu. Yayımlanan uygulama etki alanını eşleşen kendi şirket hesabını kullanarak oturum açtığında emin olun. Microsoft Account kullanıcıları ve Konuk IWA uygulamaları erişemez.</li><li>Uygulama erişmeye çalıştığınız kullanıcı, şirket içi tarafında bu uygulama için düzgün şekilde tanımlı değil. Bu kullanıcının şirket içi makinede bu arka uç uygulaması için tanımlanan uygun izinlere sahip olduğundan emin olun. |
| Bu şirket uygulama erişilemiyor. Bu uygulamaya erişmek için yetkili değil. Yetkilendirme başarısız oldu. Bu uygulamaya erişimi olan kullanıcı atadığınızdan emin olun. | Kullanıcıların Microsoft hesaplarını yerine şirket hesabındaki oturum açmak için kullandıkları, yayımlanan uygulamaya erişmeye çalışırken bu hatayı alabilirsiniz. Konuk kullanıcılar da bu hatayı alabilirsiniz. Microsoft Account kullanıcılar ve konuklar IWA uygulamaları erişemez. Yayımlanan uygulama etki alanını eşleşen kendi şirket hesabını kullanarak oturum açtığında emin olun.<br><br>Bu uygulama için kullanıcı atanmamış. Git **uygulama** sekmesinde ve altında **kullanıcılar ve gruplar**, bu uygulama için bu kullanıcı veya kullanıcı grubuna atayın. |
| Bu şirket uygulama şu anda erişilemiyor. Daha sonra yeniden deneyin... Bağlayıcı zaman aşımına uğradı. | Kullanıcılarınızın, bunların düzgün şekilde şirket içi tarafında bu uygulama için tanımlanmazsa, yayımlanan uygulamaya erişmeye çalışırken bu hatayı alabilirsiniz. Kullanıcılarınızın şirket içi makinede bu arka uç uygulaması için tanımlanan uygun izinlere sahip olduğunuzdan emin olun. |
| Bu şirket uygulama erişilemiyor. Bu uygulamaya erişmek için yetkili değil. Yetkilendirme başarısız oldu. Kullanıcının Azure Active Directory Premium veya Basic için bir lisans olduğundan emin olun. | Kullanıcılarınızın, bunlar açıkça bir Premium/temel lisansına sahip abonenin yönetici tarafından doğru atanmışsa, yayımlanan uygulamaya erişmeye çalışırken bu hatayı alabilirsiniz. Abonenin Active Directory gidin **lisansları** sekmesinde ve bu kullanıcı veya kullanıcı grubunun bir Premium veya Basic lisansı atanmış olduğundan emin olun. |

## <a name="my-error-wasnt-listed-here"></a>My hata burada listelenen değildi

Bir hata veya sorun bu sorun giderme Kılavuzu'nda listelenmiyor Azure AD uygulama proxy'si ile karşılaşırsanız, bu konuda duymak isteriz. E-posta Gönder bizim [geri bildirim takım](mailto:aadapfeedback@microsoft.com) ayrıntılarıyla karşılaşıldı.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory Uygulama Ara sunucusunu etkinleştirme](active-directory-application-proxy-enable.md)
* [Uygulama proxy'si ile uygulama yayımlama](active-directory-application-proxy-publish.md)
* [Çoklu oturum açmayı etkinleştir](active-directory-application-proxy-sso-using-kcd.md)
* [Koşullu erişimi etkinleştirme](application-proxy-enable-remote-access-sharepoint.md)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
