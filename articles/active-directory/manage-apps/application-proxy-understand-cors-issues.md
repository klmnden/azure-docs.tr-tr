---
title: Anlama ve Azure AD uygulama ara sunucusu CORS sorunları çözme
description: Azure AD uygulama proxy'si ve CORS sorunları çözmek üzere nasıl CORS bir anlayış sağlar.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: 2b6adcf4231aa44a4f28d277e963efa16de8af81
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399338"
---
# <a name="understand-and-solve-azure-active-directory-application-proxy-cors-issues"></a>Anlama ve Azure Active Directory Uygulama Proxy CORS sorunları çözme

[Çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/) bazen Azure Active Directory Uygulama proxy'si ile yayımladığınız uygulamalar ve API'ler için zorluklar çıkarabilir. Bu makalede, Azure AD uygulama ara sunucusu CORS sorunlar ve çözümler açıklanmaktadır.

Tarayıcı güvenlik genellikle bir web sayfası başka bir etki alanına AJAX istekleri yapmasını engeller. Bu kısıtlama adlı *aynı çıkış noktası İlkesi*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen, web API'si çağırma diğer sitelere izin vermek isteyebilirsiniz. CORS aynı çıkış noktası İlkesi gevşeyin ve bazı çıkış noktaları arası istekleri izin verirken diğerlerini bir izin veren bir W3C standardıdır.

## <a name="understand-and-identify-cors-issues"></a>CORS sorunları belirlemek ve anlama

İki URL aynı düzenleri, konaklar ve bağlantı noktaları varsa aynı kaynağa sahip ([RFC 6454](https://tools.ietf.org/html/rfc6454)), örneğin:

-   http:\//contoso.com/foo.html
-   http:\//contoso.com/bar.html

Aşağıdaki URL'ler önceki değerinden farklı kaynakları iki vardır:

-   http:\//contoso.net - farklı bir etki alanı
-   http:\//contoso.com:9000/foo.html - farklı bir bağlantı noktası
-   https:\//contoso.com/foo.html - farklı düzeni
-   http:\//www.contoso.com/foo.html - farklı bir alt etki alanı

Aynı çıkış noktası İlkesi uygulamaların doğru erişim denetim üstbilgileri kullandıkları sürece diğer kaynaklardan kaynaklara erişmesini engeller. CORS üstbilgilerini eksik veya yanlış ise, çıkış noktaları arası istekleri başarısız olur. 

Tarayıcı hata ayıklama araçlarını kullanarak CORS'yi sorunları belirleyebilir:

1. Tarayıcıyı başlatın ve web uygulamasına göz atın.
1. Tuşuna **F12** hata ayıklama Konsolu.
1. Deneyin işlemin yeniden ve konsol iletisini gözden geçirin. CORS ihlalinin kaynağını hakkında bir konsol hata üretir.

Aşağıdaki ekran görüntüsünde seçerek **deneyin** düğmesi neden CORS hata iletisi, https:\//corswebclient-contoso.msappproxy.net Access-Control-Allow-Origin başlığı bulunamadı.

![CORS sorunu](./media/application-proxy-understand-cors-issues/image3.png)

## <a name="cors-challenges-with-application-proxy"></a>Uygulama Ara sunucusu ile CORS zorlukları

Aşağıdaki örnek, tipik bir gösterir. Azure AD uygulama ara sunucusu CORS senaryo. İç sunucu ana bilgisayarlar bir **CORSWebService** web API denetleyicisi ve bir **CORSWebClient** çağrılarının **CORSWebService**. Bir AJAX isteğinden yoktur **CORSWebClient** için **CORSWebService**.

![Şirket içi aynı çıkış noktası isteği](./media/application-proxy-understand-cors-issues/image1.png)

Bu şirket içi ancak yük başarısız veya hata Azure AD uygulama proxy'si aracılığıyla yayımlandığında barındırdığınızda CORSWebClient uygulama çalışır. CORSWebClient ve CORSWebService uygulamalar ayrı olarak farklı uygulamalarla uygulama ara sunucusu üzerinden yayımladığınız, farklı etki alanlarında iki uygulama barındırılır. Çıkış noktaları arası istek CORSWebClient bir AJAX isteği CORSWebService olduğu ve devreder.

![Uygulama proxy'si CORS isteği](./media/application-proxy-understand-cors-issues/image2.png)

## <a name="solutions-for-application-proxy-cors-issues"></a>Uygulama proxy'si CORS sorunlarına yönelik çözümleri

Yollarından biri önceki CORS sorunu çözebilirsiniz.

### <a name="option-1-set-up-a-custom-domain"></a>1\. seçenek: Özel etki alanını ayarlama

Bir Azure AD uygulama proxy'si kullanın [özel etki alanı](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) uygulama kaynakları, kod veya üst bilgileri için herhangi bir değişiklik yapmak zorunda kalmadan aynı kaynaktan alınan yayımlanacak. 

### <a name="option-2-publish-the-parent-directory"></a>2\. seçenek: Üst dizini yayımlama

Üst dizini de her iki uygulama yayımlayın. Web sunucusu üzerinde yalnızca iki uygulamalar varsa, özellikle de iyi bu çözümü çalışır. Ayrı ayrı her bir uygulama yayımlama yerine, aynı başlangıca sonuçları ortak bir üst dizin yayımlayabilirsiniz.

Aşağıdaki örnekler, portal CORSWebClient Azure AD uygulama proxy'si sayfasını gösterir.  Zaman **İç URL** ayarlanır *contoso.com/CORSWebClient*, uygulama için başarılı istekler yapamaz *contoso.com/CORSWebService* dizin, çünkü Bunlar, çıkış noktaları arası. 

![Tek tek uygulama yayımlama](./media/application-proxy-understand-cors-issues/image4.png)

Bunun yerine, **İç URL** her ikisini de içeren üst dizini yayımlamak için *CORSWebClient* ve *CORSWebService* dizinleri:

![Üst dizine yayımlama](./media/application-proxy-understand-cors-issues/image5.png)

Sonuçta elde edilen uygulama URL'lerini etkili bir şekilde CORS sorunu:

- https:\//corswebclient-contoso.msappproxy.net/CORSWebService
- https:\//corswebclient-contoso.msappproxy.net/CORSWebClient

### <a name="option-3-update-http-headers"></a>Seçenek 3: HTTP üst bilgilerini güncelleştir

Kaynak isteğiyle eşleşmesi için web hizmeti bir özel HTTP yanıt üst bilgisi ekleyin. Internet Information Services (IIS) çalıştıran Web siteleri için üst bilgi değiştirmek için IIS Yöneticisi'ni kullanın:

![IIS Yöneticisi'nde özel bir yanıt üstbilgisi Ekle](./media/application-proxy-understand-cors-issues/image6.png)

Bu değişiklik, herhangi bir kod değişikliği gerektirmez. Fiddler izlemelerinde doğrulayabilirsiniz:

**Posta üst bilgisi ekleme**\
HTTP/1.1 200 OK\
Cache-Control: no-cache\
Pragma: no-cache\
İçerik türü: text/plain; Charset = utf-8\
Süre sonu:-1\
Farklılık gösterir: Kabul Encoding\
Sunucu: Microsoft-IIS/8.5 Microsoft-HTTPAPI/2.0\
**Access-Control-Allow-Origin: https://corswebclient-contoso.msappproxy.net** \
X-AspNet-Version: 4.0.30319\
X-desteklenen-tarafından: ASP.NET\
İçerik uzunluğu: 17

### <a name="option-4-modify-the-app"></a>Seçenek 4: Uygulamayı değiştirme

Access-Control-Allow-Origin üstbilgiyle uygun değerleri ekleyerek CORS desteği için uygulamanızı değiştirebilirsiniz. Üst bilgi Ekle şekilde uygulamanın kod diline bağlıdır. Çoğu çaba gerektirdiğinden kod değiştirme işlemi en az önerilen seçenek gerçekleşir.

### <a name="option-5-extend-the-lifetime-of-the-access-token"></a>5\. seçenek: Erişim belirteci ömrü genişletme

Bazı CORS sorunları, ne zaman uygulamanızı yeniden yönlendirir gibi çözümlenemiyor *login.microsoftonline.com* kimliğini doğrulamak için ve erişim belirtecinin süresi dolar. CORS çağırın, sonra başarısız oluyor. Bu senaryo için geçici bir çözüm, bir kullanıcının oturumu sırasında süresinin dolmasını engellemek için erişim belirteci ömrü genişletmek sağlamaktır. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz. [Azure AD'de yapılandırılabilir belirteç ömrünü](../develop/active-directory-configurable-token-lifetimes.md).

## <a name="see-also"></a>Ayrıca bkz.
- [Öğretici: Azure Active Directory Uygulama proxy'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md) 
- [Bir Azure AD uygulama ara sunucusu dağıtımını planlama](application-proxy-deployment-plan.md) 
- [Azure Active Directory Uygulama proxy'si aracılığıyla şirket içi uygulamalara uzaktan erişim](application-proxy.md) 
