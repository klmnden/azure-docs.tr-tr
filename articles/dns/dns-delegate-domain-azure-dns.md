---
title: "Öğretici: Etki alanınızı ve alt etki alanınızı Azure DNS'de barındırma"
description: Bu öğreticide Azure DNS'yi DNS bölgelerinizi barındıracak şekilde yapılandırmayı öğreneceksiniz.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: c0c5c5fe899c9b9b898973a88c7dac4256959ee4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60563247"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Öğretici: Azure DNS’te etki alanınızı barındırma

Azure DNS'yi DNS etki alanınızı barındırmak ve DNS kayıtlarınızı yönetmek için kullanabilirsiniz. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Örneğin, contoso.net etki alanını bir etki alanı adı kayıt şirketinden satın aldığınızı ve Azure DNS'de contoso.net adlı bir bölge oluşturduğunuzu varsayalım. Etki alanının sahibi olduğunuzdan, kayıt şirketiniz size etki alanınız için ad sunucusu (NS) kayıtlarını yapılandırma seçeneğini sunar. Kayıt kuruluşu bu NS kayıtlarını .net üst alanında depolar. Contoso.NET'teki DNS kayıtlarını çözümlemeye çalıştıklarında dünyanın dört bir yanındaki kullanıcılara Internet, Azure DNS bölgesindeki etki alanınıza yönlendirilir.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir DNS bölgesi oluşturun.
> * Ad sunucularının bir listesini alın.
> * Etki alanı temsilcisi.
> * Temsilci seçmeyi çalıştığını doğrulayın.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure DNS'de barındırabilir test etmek bir etki alanı adı olmalıdır. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Bu öğreticide kullanılan örnek etki alanı, contoso.net olmakla birlikte, kendi etki alanı adını kullanın.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure Portal’da oturum açın.
1. Sol üstten **Kaynak oluştur** > **Ağ** > **DNS bölgesi**'ni seçerek **DNS bölgesi oluşturma** sayfasını açın.

   ![DNS bölgesi](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’u seçin:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|[etki alanı adınız] |Satın aldığınız etki alanı adı. Bu öğreticide örnek olarak contoso.net kullanılmıştır.|
   |**Abonelik**|[Aboneliğiniz]|Bölgenin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçtiğiniz abonelik içinde benzersiz olmalıdır.<br>Kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman "Genel" şeklindedir ve gösterilmiyor.|
   |**Konum**|Doğu ABD||

## <a name="retrieve-name-servers"></a>Ad sunucularını alma

DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucularını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında DNS bölgenizi seçin. Zaten seçili aboneliği çeşitli kaynaklar varsa, etki alanı adınızı girin **ada göre filtrele** uygulama ağ geçidine kolaylıkla erişmek için kutusu. 

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, contoso.net bölgesine *ns1-01.azure-dns.com*, *ns2-01.azure-dns.net*, *ns3-01.azure-dns.org* ve *ns4-01.azure-dns.info* ad sunucuları atanmıştır:

   ![Ad sunucularının listesi](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

Azure DNS, bölgenizdeki yetkili NS kayıtlarını atanan ad sunucularını içerecek şekilde otomatik olarak oluşturur.

## <a name="delegate-the-domain"></a>Etki alanını devretme

Artık DNS bölgesi oluşturulduğuna ve ad sunucularınız olduğuna göre, üst etki alanını Azure DNS ad sunucularıyla güncelleştirmeniz gerekir. Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. 

1. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS ad sunucularıyla değiştirin.

1. Bir etki alanını Azure DNS'ye devretme, Azure DNS tarafından sağlanan ad sunucularını kullanmanız gerekir. Etki alanınızın adından bağımsız olarak tüm dört ad sunucusunun kullanın. Etki alanı temsilcisi, bir ad sunucusunun etki alanınızla aynı üst düzey etki alanını kullanmasını gerektirmez.

> [!NOTE]
> Ad sunucusu adreslerini kopyalarken adresinin sonundaki noktayı da kopyaladığınızdan emin olun. Sondaki nokta bir tam etki alanı adının sonuna gösterir. NS adın sonunda yoksa, bazı kaydedicilerin dönemin sonuna ekleyin. DNS RFC ile uyumlu olması için sondaki nokta içerir.

Kendi bölgenizdeki ad sunucularını kullanan temsilciler olarak da adlandırılır *gösterim ad sunucuları*, Azure DNS'de şu anda desteklenmemektedir.

## <a name="verify-the-delegation"></a>Temsilci seçmeyi doğrulayın

Temsilci seçmeyi tamamladıktan sonra bir aracı gibi kullanarak çalışmakta olduğunu doğrulayabilirsiniz *nslookup* bölgenizin yetki başlangıcı (SOA) kaydı sorgulanamıyor. SOA kaydı, bölge oluşturulurken otomatik olarak oluşturulur. 10 dakika beklemeniz gerekebilir veya başarıyla önce temsilci seçmeyi tamamladıktan sonra daha fazla çalışır durumda olduğunu doğrulayın. Değişikliklerin DNS sisteminde yayılması daha uzun sürebilir.

Azure DNS ad sunucularını belirtmeniz gerekmez. Temsilci seçimi doğru ayarlanmışsa normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur.

1. Bir komut isteminden, aşağıdaki örneğe benzer bir nslookup komutu girin:

   ```
   nslookup -type=SOA contoso.net
   ```

1. Yanıtınız nslookup aşağıdaki çıktıya benzer doğrulayın:

   ```
   Server: ns1-04.azure-dns.com
   Address: 208.76.47.4

   contoso.net
   primary name server = ns1-04.azure-dns.com
   responsible mail addr = msnhst.microsoft.com
   serial = 1
   refresh = 900 (15 mins)
   retry = 300 (5 mins)
   expire = 604800 (7 days)
   default TTL = 300 (5 mins)
   ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçmeyi düşünüyorsanız **contosoRG** kaynak grubunu tutabilirsiniz. Aksi halde **contosoRG** kaynak grubunu silerek bu öğreticide oluşturulan kaynakları silebilirsiniz.

- Seçin **contosoRG** kaynak grubunu ve ardından **kaynak grubunu Sil**. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, etki alanınız için DNS bölgesi oluşturduğunuz ve Azure DNS için temsilci. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)