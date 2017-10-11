---
title: "404 durumu döndüren Azure CDN uç noktası sorunlarını giderme | Microsoft Docs"
description: "Azure CDN uç noktası ile 404 yanıt kodlarını sorunlarını giderin."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f59fbd18413fb44026d8c92b7f6940ed2f8a00a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>404 durumları döndüren CDN uç noktası sorunlarını giderme
Bu makale ile ilgili sorunları gidermenize yardımcı [CDN uç noktası](cdn-create-new-endpoint.md) 404 hataları döndürüyor.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve tıklayın **destek alın**.

## <a name="symptom"></a>Belirti
CDN profili ve uç oluşturdunuz, ancak içeriğinizi CDN üzerinde kullanılabilir olması için görünmemektedir.  İçeriğinizi CDN URL'sine aracılığıyla erişme girişimi kullanıcılar HTTP 404 durum kodlarını alır. 

## <a name="cause"></a>Nedeni
Dahil birkaç olası nedenleri şunlardır:

* Dosyanın kaynağından CDN ile görünür değil
* Uç nokta yanlış yerinde aramak CDN neden yanlış yapılandırılmış
* Ana bilgisayarın ana bilgisayar üstbilgisi CDN reddediyor
* Uç nokta CDN yayılması zaman sahip olmayan

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
> [!IMPORTANT]
> Kaydın yayılması zaman yararlanırken bir CDN uç noktası oluşturduktan sonra onu hemen kullanılabilir olmaz.  İçin <b>akamai'den Azure CDN</b> profilleri yayma işlemi genellikle bir dakika içinde tamamlanır.  <b>Verizon'dan Azure CDN</b> profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır ancak bazı durumlarda daha uzun sürebilir.  Bu belgedeki adımları tamamlayın ve 404 yanıtlarını hala almanızı, destek bileti açmadan önce denetlemek için birkaç saat bekleyen düşünün.
> 
> 

### <a name="check-the-origin-file"></a>Kaynak dosyanın denetleyin
İlk olarak, biz istiyoruz önbelleğe alınmış dosya doğrulayın bizim kaynak üzerinde bulunan ve genel olarak erişilebilir olan.  Bunu yapmak için hızlı bir giriş-özel veya Incognito oturumunda bir tarayıcı açın ve doğrudan dosyasına gözatın yoludur.  Yalnızca yazın veya URL adres kutusuna yapıştırın ve beklediğiniz dosyasında sonuçlarının varsa bkz.  Bu örnekte, sahibim adresindeki erişilebilir bir Azure depolama hesabındaki bir dosyayı kullanmak için yapacağım `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Gördüğünüz gibi test başarıyla geçirir.

![Başarılı!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Bazı ağ yapılandırmaları, kuruluşunuzda bu dosyanızı genel kullanıma açık doğrulamak için hızlı ve kolay şekilde olmakla birlikte, bu dosya, aslında (Azure'da barındırıldığı olsa bile) yalnızca görünür ağınızdaki kullanıcılara olduğunda genel kullanıma açık olduğunu çalışabilmesine sağlayabilir.  İçinden, kuruluşunuzun ağ ya da, azure'daki bir sanal makineye bağlı olmayan bir mobil aygıtı gibi test edebilirsiniz dış tarayıcı varsa, en iyi olacaktır.
> 
> 

### <a name="check-the-origin-settings"></a>Kaynak ayarlarını kontrol edin
Biz Internet üzerinde genel kullanıma açık bir dosyadır doğrulandı, biz kaynak ayarları doğrulamanız gerekir.  İçinde [Azure Portal](https://portal.azure.com), CDN profilinize gidin ve sorun giderme uç noktasına tıklayın.  Sonuç olarak **Endpoint** dikey penceresinde kaynak'ı tıklatın.  

![Vurgulanan kaynağına sahip uç nokta dikey penceresi](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

**Kaynak** dikey penceresi görünür. 

![Kaynak dikey penceresi](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Kaynak türü ve ana bilgisayar adı
Doğrulama **kaynak türü** düzeltin ve doğrulama **kaynak ana bilgisayar adı**.  My örnekte `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, URL ana bilgisayar adı kısmı `cdndocdemo.blob.core.windows.net`.  Aşağıdaki ekran görüntüsünde gördüğünüz gibi bu doğrudur.  Azure Storage, Web App ve bulut hizmeti kaynakları **kaynak ana bilgisayar adı** alandır açılan listesinde, böylece biz doğru yazım hakkında endişelenmeniz gerekmez.  Ancak, özel bir kaynak kullanıyorsanız, olmasından *kesinlikle kritik* , ana bilgisayar adı doğru yazıldığından!

#### <a name="http-and-https-ports"></a>HTTP ve HTTPS bağlantı noktaları
İşte denetlemek için başka bir şey, **HTTP** ve **HTTPS bağlantı noktalarını**.  Çoğu durumda, 80 ve 443 doğru olduğundan ve hiçbir değişiklik yapılmasını gerektirir.  Ancak, kaynak sunucu farklı bir bağlantı noktasında dinleme yapıyorsanız, burada gösterilen gerekecektir.  Emin değilseniz, yalnızca kaynak dosyanızı URL'sini bakın.  HTTP ve HTTPS belirtimleri varsayılan olarak 80 ve 443 numaralı bağlantı noktalarını belirtin. My URL'de `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, bir bağlantı noktası, varsayılan 443 olduğu varsayılır ve ayarlarımı doğru şekilde belirtilmedi.  

Ancak, daha önce test, kaynak dosya için URL söyleyin `http://www.contoso.com:8080/file.txt`.  Not `:8080` hostname segment sonunda.  Bağlantı noktası kullanmak için tarayıcı söyler `8080` web sunucusuna bağlanmak için `www.contoso.com`, 8080 de girmeniz gerekir böylece **HTTP bağlantı noktası** alan.  Bu bağlantı noktası ayarlarını uç nokta kaynaktan bilgi almak için kullandığı hangi bağlantı noktasının etkiler olduğunu dikkate almak önemlidir.

> [!NOTE]
> **Akamai'den Azure CDN** uç noktaları, kaynaklar için tam TCP bağlantı noktası aralığına izin vermez.  İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Uç nokta ayarlarını kontrol edin
Geri **Endpoint** dikey penceresinde tıklatın **yapılandırma** düğmesi.

![Uç nokta dikey Yapılandırma düğmesi vurgulanan](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Uç noktanın **yapılandırma** dikey penceresi görünür.

![Dikey yapılandırın](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoller
İçin **protokolleri**, istemcileri tarafından kullanılan protokol seçili olduğundan emin olun.  Önceki bölümde doğru yapılandırılmış kaynak bağlantı noktaları olmak önemlidir, kaynağa erişmek için kullanılan bir istemci tarafından kullanılan aynı protokol olacaktır.  Uç nokta yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarındaki (80 ve 443), kaynak bağlantı noktalarının bağımsız olarak dinler.

Şimdi kuramsal örneğimizde dönmek `http://www.contoso.com:8080/file.txt`.  Contoso unutmayın olarak belirtilen `8080` kendi HTTP bağlantı noktası, ancak aynı zamanda belirtilen varsayalım `44300` kendi HTTPS bağlantı noktası olarak.  Adlı bir uç nokta oluşturduysanız `contoso`, kendi CDN uç noktası ana bilgisayar adı olacaktır `contoso.azureedge.net`.  Bir istek için `http://contoso.azureedge.net/file.txt` uç nokta, kaynaktan almak için bağlantı noktası 8080 üzerinde HTTP kullanırsınız bir HTTP isteğinin olduğundan.  HTTPS üzerinden güvenli bir isteği `https://contoso.azureedge.net/file.txt`, 44300 bağlantı noktasında HTTPS kullanmak uç nokta neden olduğunda retriving kaynak dosyadan.

#### <a name="origin-host-header"></a>Kaynak ana bilgisayar üstbilgisi
**Kaynak ana bilgisayar üstbilgisi** olan her istekle kaynağa gönderilen barındırma üst bilgisi değeri.  Çoğu durumda, bu aynı olmalıdır **kaynak ana bilgisayar adı** biz doğrulamıştınız.  Bu alandaki yanlış bir değere 404 durumları genellikle neden olmaz ancak ne kaynağını bekliyor bağlı olarak diğer 4xx durumların neden olabilir.

#### <a name="origin-path"></a>Kaynak yolu
Son olarak, kimliğinizi doğrulamanız gerekir bizim **kaynak yolu**.  Varsayılan olarak bu boştur.  CDN üzerinde kullanılabilir hale getirmek istediğiniz kaynak barındırılan kaynakları kapsamını sınırlamak istiyorsanız, bu alan yalnızca kullanmanız gerekir.  

Örneğin, Noktam tüm kaynaklar ı sol şekilde kullanılabilmesi için depolama hesabımdaki istediğim **kaynak yolu** boş.  Bir istek buna `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` uç noktası'na bir bağlantıdan sonuçlanıyor `cdndocdemo.core.windows.net` isteklerine `/publicblob/lorem.txt`.  Benzer şekilde, bir istek için `https://cdndocdemo.azureedge.net/donotcache/status.png` sonuçları isteyen uç nokta `/donotcache/status.png` kaynaktan.

Ancak ne my kaynağındaki her yolu için CDN kullanmak istemiyorsanız?  I söyleyin yalnızca istediği kullanıma sunmak `publicblob` yolu.  I girerseniz, */publicblob* içinde my **kaynak yolu** eklemek uç nokta neden olacak alan */publicblob* kaynağa yapılan her isteği önce.  İstek için buna `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` şimdi URL isteği kısmı gerçekte sürer `/publicblob/lorem.txt`ve ilave `/publicblob` başına. Bu istek için sonuçlanır `/publicblob/publicblob/lorem.txt` kaynaktan.  Bu yol için gerçek bir dosya sorunu çözmezse, kaynak 404 durumu döndürür.  Bu örnekte lorem.txt almak için doğru URL gerçekte olacaktır `https://cdndocdemo.azureedge.net/lorem.txt`.  Biz içerme Not */publicblob* tümü, yolda URL isteği kısmı olduğundan `/lorem.txt` ve uç nokta ekler `/publicblob`, sonuç olarak `/publicblob/lorem.txt` geçirilen kaynağa istek bırakılıyor.

