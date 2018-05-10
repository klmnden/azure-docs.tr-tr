---
title: 404 durum kodunu döndürür Azure CDN uç noktası sorunlarını giderme | Microsoft Docs
description: Azure CDN uç noktası ile 404 yanıt kodlarını sorunlarını giderin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1cffef5bbda475032ee7ff07188ab0d9d52846ea
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-azure-cdn-endpoints-that-return-a-404-status-code"></a>404 durum kodunu döndürür Azure CDN uç noktası sorunlarını giderme
Bu makalede, 404 HTTP yanıtı durum kodları dönüş Azure içerik teslim ağı (CDN) uç noktaları ile ilgili sorunları giderme sağlar.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **destek alın**.

## <a name="symptom"></a>Belirti
CDN profili ve uç oluşturdunuz, ancak içeriğinizi CDN üzerinde kullanılabilir olması için görünmemektedir. İçeriğinizi CDN URL'sine aracılığıyla erişmeye çalışan kullanıcılar bir HTTP 404 durum kodu alır. 

## <a name="cause"></a>Nedeni
Dahil birkaç olası nedenleri şunlardır:

* Dosyanın kaynağından CDN ile görünür değil.
* Uç nokta, yanlış yerinde aramak CDN neden yanlış yapılandırılmıştır.
* Ana bilgisayarın ana bilgisayar üstbilgisi CDN reddediyor.
* Uç nokta CDN yayılması zaman vardı kurmadı.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
> [!IMPORTANT]
> Kaydın yayılması zaman yararlanırken bir CDN uç noktası oluşturduktan sonra onu hemen kullanılabilir olmaz:
> - İçin **Azure CDN standart Microsoft** profilleri yayma genellikle on dakika içinde tamamlanır. 
> - İçin **akamai'den Azure CDN standart** profilleri yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - İçin **verizon'dan Azure CDN standart** ve **verizon'dan Azure CDN Premium** profilleri yayma işlemi genellikle 90 dakika içinde tamamlanır. 
> 
> Bu belgedeki adımları tamamlayın ve 404 yanıtlarını hala almanızı, destek bileti açmadan önce denetlemek için birkaç saat bekleyen düşünün.
> 
> 

### <a name="check-the-origin-file"></a>Kaynak dosyanın denetleyin
İlk olarak, dosyayı önbelleğe kaynak sunucuda kullanılabilir ve internet'te genel olarak erişilebilir olduğunu doğrulayın. Bunu yapmak için hızlı bir özel veya incognito oturumu ve doğrudan dosyaya Gözat bir tarayıcıda açmak için yoludur. Yazın veya URL adres kutusuna yapıştırın ve beklediğiniz dosyasında sonuçlarını doğrulayın. Örneğin, bir dosyada bir Azure Storage hesabı, https erişilebilir olduğunu varsayalım:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt. Bu dosyanın içeriğini başarıyla yükleyebilir, test geçirir.

![Başarılı!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Bu doğrulamanın dosyanızı genel olarak kullanılabilir, bazı ağ yapılandırmaları, kuruluşunuzda onu görünen yapabilir hızlı ve kolay bir yolu olsa da bu, aslında, (Bu örnekte barındırılmaktadır olsa bile yalnızca görünür ağınızdaki kullanıcılara bir dosyayı genel kullanıma açık olduğunda Azure). Bu durumda olmadığından emin olmak için kuruluşunuzun ağ ya da azure'daki bir sanal makineye bağlı olmayan bir mobil aygıtı gibi bir dış tarayıcı dosyasıyla sınayın.
> 
> 

### <a name="check-the-origin-settings"></a>Kaynak ayarlarını kontrol edin
İnternet'te genel kullanıma açık bir dosyadır doğrulandıktan sonra kaynak ayarlarınızı doğrulayın. İçinde [Azure Portal](https://portal.azure.com), CDN profilinize gidin ve sorun giderme uç nokta seçin. Elde edilen gelen **Endpoint** sayfasında, kaynak seçin.  

![Vurgulanan kaynağına sahip uç nokta sayfası](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Kaynak** sayfası görüntülenir. 

![Kaynak Sayfası](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Kaynak türü ve ana bilgisayar adı
Doğrulayın değerlerini **kaynak türü** ve **kaynak ana bilgisayar adı** doğrudur. Bu örnekte, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, URL hostname bölümüdür *cdndocdemo.blob.core.windows.net*, doğru olduğu. Azure Storage, Web App ve bulut hizmeti çıkış için bir açılır liste değer kullandığından **kaynak ana bilgisayar adı** alanında, yanlış yazım bir sorun değildir. Ancak, özel bir kaynak kullanırsanız, ana bilgisayar adı doğru yazıldığından emin olun.

#### <a name="http-and-https-ports"></a>HTTP ve HTTPS bağlantı noktaları
Denetleyin, **HTTP** ve **HTTPS bağlantı noktalarını**. Çoğu durumda, 80 ve 443 doğru olduğundan ve hiçbir değişiklik yapılmasını gerektirir.  Ancak, kaynak sunucu farklı bir bağlantı noktasında dinleme yapıyorsanız, burada gösterilen gerekecektir. Emin değilseniz, kaynak dosyanın URL'sini görüntüleyin. HTTP ve HTTPS belirtimleri 80 ve 443 numaralı bağlantı noktalarını varsayılan olarak kullanın. Örnek URL, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, bir bağlantı noktası, varsayılan 443 olduğu varsayılır ve ayarları doğru şekilde belirtilmedi.  

Ancak, daha önce test kaynak dosyanın http için URL varsayalım:\//www.contoso.com:8080/file.txt. Not *: 8080* hostname segment sonunda bölümü. Sayı www.contoso.com web sunucusuna bağlanmak için bağlantı noktası 8080 kullanılacak tarayıcı bildirir, bu nedenle girmeniz gerekecek *8080* içinde **HTTP bağlantı noktası** alan. Bu bağlantı noktası ayarlarını kaynaktan bilgi almak için uç nokta yalnızca hangi bağlantı noktasını kullanır etkileyen dikkate almak önemlidir.

> [!NOTE]
> **Azure CDN standart akamai'den** uç noktaları kaynakları için tam TCP bağlantı noktası aralığı izin vermez.  İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Uç nokta ayarlarını kontrol edin
Üzerinde **Endpoint** sayfasında, **yapılandırma** düğmesi.

![Yapılandırma düğmesi vurgulanan uç nokta sayfası](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

CDN uç noktası **yapılandırma** sayfası görüntülenir.

![Yapılandırma sayfası](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoller
İçin **protokolleri**, istemcileri tarafından kullanılan protokol seçili olduğundan emin olun. İstemci tarafından kullanılan aynı protokol kaynağa erişmek için kullanılan bir olduğundan, önceki bölümde doğru yapılandırılmış kaynak bağlantı noktaları olması önemlidir. CDN uç noktası, yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarındaki (80 ve 443), kaynak bağlantı noktalarının bağımsız olarak dinler.

Şimdi kuramsal örneğimizde http ile dönmek:\//www.contoso.com:8080/file.txt.  Contoso unutmayın olarak belirtilen *8080* kendi HTTP bağlantı noktası, ancak aynı zamanda belirtilen varsayalım *44300* kendi HTTPS bağlantı noktası olarak.  Adlı bir uç nokta oluşturduysanız *contoso*, kendi CDN uç noktası ana bilgisayar adı olacaktır *contoso.azureedge.net*.  Http isteği:\//contoso.azureedge.net/file.txt olan bir HTTP isteği uç HTTP bağlantı noktası 8080 üzerinde kaynaktan almak için kullanmanız.  HTTPS, https üzerinden güvenli bir isteği: \/ /contoso.azureedge.net/file.txt, dosyayı kaynaktan alınırken 44300 bağlantı noktasında HTTPS kullanmak uç nokta neden.

#### <a name="origin-host-header"></a>Kaynak barındırma üst bilgisi
**Kaynak ana bilgisayar üstbilgisi** olan her istekle kaynağa gönderilen barındırma üst bilgisi değeri.  Çoğu durumda, bu aynı olmalıdır **kaynak ana bilgisayar adı** biz doğrulamıştınız.  Bu alandaki yanlış bir değere 404 durumları genellikle neden olmaz ancak ne kaynağını bekliyor bağlı olarak diğer 4xx durumların neden olabilir.

#### <a name="origin-path"></a>Kaynak yolu
Son olarak, kimliğinizi doğrulamanız gerekir bizim **kaynak yolu**.  Varsayılan olarak bu boştur.  CDN üzerinde kullanılabilir hale getirmek istediğiniz kaynak barındırılan kaynakları kapsamını sınırlamak istiyorsanız, bu alan yalnızca kullanmanız gerekir.  

Örnek uç biz kullanılabilir olması için depolama hesabındaki tüm kaynaklara istediği şekilde **kaynak yolu** boş bırakılır.  Bu, https isteğine anlamına gelir:\//cdndocdemo.azureedge.net/publicblob/lorem.txt sonuçları istekleri cdndocdemo.core.windows.net uç noktasından bağlantı içinde */publicblob/lorem.txt*.  Benzer şekilde, https için bir istek:\//cdndocdemo.azureedge.net/donotcache/status.png sonuçları isteyen uç nokta */donotcache/status.png* kaynaktan.

Ancak ne kaynağınıza her yolda için CDN kullanmak istemiyorsanız?  Yalnızca istediğinizi kullanıma sunmak söyleyin *publicblob* yolu.  Biz girerseniz, */publicblob* içinde **kaynak yolu** eklemek uç nokta neden olacak alan */publicblob* kaynağa yapılan her isteği önce.  Bu istek için https anlamına gelir:\//cdndocdemo.azureedge.net/publicblob/lorem.txt şimdi gerçekte alacak URL isteği kısmı */publicblob/lorem.txt*ve ilave */publicblob* başına. Bu istek için sonuçlanır */publicblob/publicblob/lorem.txt* kaynaktan.  Bu yol için gerçek bir dosya sorunu çözmezse, kaynak 404 durumu döndürür.  Bu örnekte lorem.txt almak için doğru URL https gerçekte olacaktır:\//cdndocdemo.azureedge.net/lorem.txt.  Biz içerme Not */publicblob* tümü, yolda URL isteği kısmı olduğundan */lorem.txt* ve uç nokta ekler */publicblob*, sonuç */publicblob/lorem.txt* geçirilen kaynağa istek bırakılıyor.

