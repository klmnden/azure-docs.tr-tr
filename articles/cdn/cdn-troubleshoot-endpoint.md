---
title: Bir 404 durum kodu döndüren bir Azure CDN uç noktası sorunlarını giderme | Microsoft Docs
description: Azure CDN uç noktası ile 404 yanıt kodlarını sorunlarını giderin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b665c2f72f50b2d72fd625b49c4212785ab3301d
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593268"
---
# <a name="troubleshooting-azure-cdn-endpoints-that-return-a-404-status-code"></a>Bir 404 durum kodu döndüren bir Azure CDN uç noktası sorunlarını giderme
Bu makalede, 404 HTTP yanıtı durum kodları iade Azure Content Delivery Network (CDN) uç noktaları ile ilgili sorunları giderme sağlar.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **destek al**.

## <a name="symptom"></a>Belirti
CDN profili ve uç nokta oluşturdunuz, ancak içerik CDN'de kullanılabilir olmasını desteklemiyor. İçeriğinizi aracılığıyla CDN URL'sine erişmeye çalışan kullanıcılar bir HTTP 404 durum kodu alır. 

## <a name="cause"></a>Nedeni
Birkaç olası nedeni vardır:

* Dosyanın kaynağından CDN için görünür değildir.
* Neden yanlış yere aramak CDN uç noktası hatalı yapılandırılmış.
* Ana bilgisayar, ana bilgisayar üstbilgisi CDN reddediyor.
* Uç nokta CDN yayılması zaman sahip.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
> [!IMPORTANT]
> Kaydın CDN'de yayılması zaman alır bir CDN uç noktası oluşturduktan sonra bunu hemen kullanılmaya kullanılabilir olacaktır değil:
> - **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle on dakikada tamamlanır. 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır. 
> 
> Bu belgedeki adımları tamamlayın ve hala geçmesine karşın 404 yanıtlarını karşılaşacağınız, bir destek bileti açmadan önce kontrol etmek için birkaç saat bekleniyor göz önünde bulundurun.
> 
> 

### <a name="check-the-origin-file"></a>Kaynak dosyayı denetleyin
İlk olarak, dosya önbelleği için kaynak sunucuda kullanılabilir olduğunu ve internet'te genel olarak erişilebilir olduğunu doğrulayın. Bunu yapmanın en hızlı yolu bir tarayıcı doğrudan dosyaya göz atın ve bir özel veya gizli oturum açmaktır. Yazın veya URL adres kutusuna yapıştırın ve dosyayı beklediğiniz sonuçları doğrulayın. Örneğin, bir Azure depolama hesabında, https erişilebilir bir dosya olduğunu varsayalım:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt. Bu dosyanın içeriğini başarıyla yükleyebilir, test başarılı olur.

![Başarılı!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Bu dosyanızı herkes tarafından kullanılabilir, kuruluşunuzdaki bazı ağ yapılandırmaları, görünen hale getirebilecek doğrulamak için hızlı ve kolay bir yolu olsa da, aslında, (içinde barındırıldığı olsa bile yalnızca görünür ağınızdaki kullanıcılara olduğunda bir dosyayı genel kullanıma açıktır Azure için). Bu durumda olmadığından emin olmak için dosya, kuruluşunuzun ağa veya azure'daki bir sanal makinenin bağlı olmayan bir mobil cihazı gibi dış tarayıcı ile test edin.
> 
> 

### <a name="check-the-origin-settings"></a>Kaynak ayarlarını kontrol edin
Dosya internet üzerinde herkes tarafından kullanılabilir doğruladıktan sonra kaynak ayarlarınızı doğrulayın. İçinde [Azure portalı](https://portal.azure.com), CDN profilinize gidin ve sorun giderme uç noktayı seçin. Bulunan değerden **uç nokta** sayfasında, kaynak seçin.  

![Vurgulanan kaynak uç noktası sayfası](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Kaynak** sayfası görüntülenir. 

![Başlangıç sayfası](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Kaynak türü ve ana bilgisayar adı
Doğrulayın değerlerini **kaynak türü** ve **kaynak konak adı** doğrudur. Bu örnekte, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, URL konak adı kısmı *cdndocdemo.blob.core.windows.net*, doğru olduğu. Azure depolama, Web uygulaması ve bulut hizmeti kaynakları için bir açılan liste değer kullandığından **kaynak konak adı** alan, yanlış yazım bir sorun değildir. Ancak özel kaynak kullanırsanız, konak adı doğru yazıldığından emin olun.

#### <a name="http-and-https-ports"></a>HTTP ve HTTPS bağlantı noktaları
Denetleme, **HTTP** ve **HTTPS bağlantı noktaları**. Çoğu durumda, 80 ve 443 doğru olduğundan ve hiçbir değişiklik yapılmasını gerektirir.  Ancak, kaynak sunucu farklı bir bağlantı noktasında dinliyorsa, burada gösterilen gerekecektir. Emin değilseniz, kaynak dosyanız için URL görüntüleyin. HTTP ve HTTPS özellikleri varsayılan olarak 80 ve 443 bağlantı noktalarını kullanın. Örnek URL, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, bir bağlantı noktası, varsayılan 443 olduğu varsayılır ve ayarları doğru şekilde belirtilmedi.  

Daha önce test kaynak dosyası için http URL'sini ancak varsayalım:\//www.contoso.com:8080/file.txt. Not *: 8080* hostname segmentin sonuna bölümü. Sayı www web sunucusuna bağlanmak için bağlantı noktası 8080 kullanılacak tarayıcı bildirir\.contoso.com, bu nedenle gereken girin *8080* içinde **HTTP bağlantı noktası** alan. Yalnızca hangi bağlantı noktası uç noktanın kaynaktan bilgi almak için kullanır. Bu bağlantı noktası ayarlarını etkileyen dikkat edin önemlidir.

> [!NOTE]
> **Azure CDN standart Akamai** uç noktaları kaynakları için tam TCP bağlantı noktası aralığına izin vermez.  İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](/previous-versions/azure/mt757337(v=azure.100)).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Uç nokta ayarlarını kontrol edin
Üzerinde **uç nokta** sayfasında **yapılandırma** düğmesi.

![Uç nokta sayfasıyla Yapılandır düğmesi](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

CDN uç noktası **yapılandırma** sayfası görüntülenir.

![Yapılandır sayfası](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoller
İçin **protokolleri**, istemcileri tarafından kullanılan protokol'ın seçili olduğundan emin olun. Kaynağa erişmek için kullanılan bir istemci tarafından kullanılan aynı protokol olduğu için önceki bölümde doğru yapılandırılmış kaynak bağlantı noktalarının olması önemlidir. CDN uç noktasına, yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarındaki (80 ve 443), kaynak bağlantı noktalarının bakılmaksızın dinler.

Şimdi kuramsal örneğimizde http ile geri dönün:\//www.contoso.com:8080/file.txt.  Contoso, anımsanır gibi belirtilen *8080* , HTTP bağlantı noktası, ancak aynı zamanda belirtilen varsayalım *44300* kendi HTTPS bağlantı noktası olarak.  Bunlar adlı bir uç nokta oluşturduysanız *contoso*, kendi CDN uç noktası ana bilgisayar adı olacaktır *contoso.azureedge.net*.  Http isteği:\//contoso.azureedge.net/file.txt olan bir HTTP isteği için uç nokta, kaynaktan almak için 8080 bağlantı noktasında HTTP kullanırsınız.  HTTPS, https üzerinden güvenli bir istek: \/ /contoso.azureedge.net/file.txt, HTTPS bağlantı noktasına 44300 dosyayı kaynaktan alınırken kullanılacak uç nokta neden.

#### <a name="origin-host-header"></a>Kaynak barındırma üst bilgisi
**Kaynak barındırma üst bilgisi** her isteğin birlikte kaynağa gönderilen ana bilgisayar üstbilgisi değeri.  Çoğu durumda, bu aynı olmalıdır **kaynak konak adı** daha önce doğrulanır.  Bu alandaki yanlış bir değere 404 durumu genellikle neden olmaz, ancak hangi kaynak bekliyor bağlı olarak diğer 4xx durum neden olabilir.

#### <a name="origin-path"></a>Kaynak yolu
Son olarak, biz doğrulamalıdır bizim **kaynak yolu**.  Varsayılan olarak bu boştur.  CDN'de kullanılabilir olmasını istediğiniz kaynak barındırılan kaynakları kapsamını daraltmak isterseniz, bu alan yalnızca kullanmalısınız.  

Örnek uç nokta istedik kullanılabilir olması için depolama hesabındaki tüm kaynaklar için **kaynak yolu** boş bırakılır.  Bir https isteği buna:\//cdndocdemo.azureedge.net/publicblob/lorem.txt sonuçları bir bağlantı uç noktasından ister cdndocdemo.core.windows.net */publicblob/lorem.txt*.  Benzer şekilde, https için bir istek:\//cdndocdemo.azureedge.net/donotcache/status.png sonuçları uç nokta isteyen */donotcache/status.png* kaynaktan.

Ancak ne kaynağınıza üzerinde her yol için bir CDN kullanmayı istemiyorsanız?  Söyleyin yalnızca istediğinizi göstermek *publicblob* yolu.  Biz girerseniz */publicblob* içinde **kaynak yolu** eklemek uç nokta neden olacak alan */publicblob* kaynağa yapılan her isteği önce.  Bu istek için https anlamına gelir:\//cdndocdemo.azureedge.net/publicblob/lorem.txt artık gerçekte alacağınız isteği bölümü URL'SİNİN */publicblob/lorem.txt*ve ekleme */publicblob* başına. Bir istek için sonuçlanır */publicblob/publicblob/lorem.txt* kaynaktan.  Bu yol için gerçek bir dosya sorunu çözmezse, kaynağı bir 404 durum döndürür.  Bu örnekte lorem.txt almak için doğru URL, https gerçekten olacaktır:\//cdndocdemo.azureedge.net/lorem.txt.  Unutmayın, biz içermez */publicblob* hiç yolu isteği bölümü URL'SİNİN olduğundan */lorem.txt* ve uç nokta ekler */publicblob*, imzalanmayarak */publicblob/lorem.txt* kaynağa geçirilen istek.

