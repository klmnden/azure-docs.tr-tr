---
title: "Öğretici: Etki alanınızı ve alt etki alanınızı Azure DNS'de barındırma"
description: Bu öğreticide Azure DNS'yi DNS bölgelerinizi barındıracak şekilde yapılandırmayı öğreneceksiniz.
services: dns
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: tutorial
ms.date: 6/13/2018
ms.author: victorh
ms.openlocfilehash: 44f5bf9a28d56e85bae1d50136c50868ec96eb4e
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205450"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Öğretici: Azure DNS’te etki alanınızı barındırma

Azure DNS'yi DNS etki alanınızı barındırmak ve DNS kayıtlarınızı yönetmek için kullanabilirsiniz. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. 

Örneğin, contoso.net etki alanını bir etki alanı adı kayıt şirketinden satın aldığınızı ve Azure DNS'de contoso.net adlı bir bölge oluşturduğunuzu varsayalım. Etki alanının sahibi olduğunuzdan, kayıt şirketiniz size etki alanınız için ad sunucusu (NS) kayıtlarını yapılandırma seçeneğini sunar. Kayıt kuruluşu bu NS kayıtlarını .net üst alanında depolar. Ardından, dünya genelindeki İnternet kullanıcıları contoso.net’teki DNS kayıtlarını çözümlemeye çalışırken Azure DNS bölgesindeki etki alanınıza yönlendirilir.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * DNS bölgesi oluşturma
> * Ad sunucularının listesini alma
> * Etki alanını devretme
> * Devretme özelliğinin çalıştığını doğrulama


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

1. Azure Portal’da oturum açın.
1. Sol üstten **Kaynak oluştur** > **Ağ** > **DNS bölgesi**'ni seçerek **DNS bölgesi oluşturma** sayfasını açın.

   ![DNS bölgesi](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. **DNS bölgesi oluştur** sayfasında aşağıdaki değerleri girin ve **Oluştur**’u seçin:

   | **Ayar** | **Değer** | **Ayrıntılar** |
   |---|---|---|
   |**Ad**|[etki alanı adınız] |Satın aldığınız etki alanı adı. Bu öğreticide örnek olarak contoso.net kullanılmıştır.|
   |**Abonelik**|[Aboneliğiniz]|Bölgenin oluşturulacağı bir abonelik seçin.|
   |**Kaynak grubu**|**Yeni oluştur:** contosoRG|Bir kaynak grubu oluşturun. Kaynak grubu adı, seçtiğiniz abonelik içinde benzersiz olmalıdır. |
   |**Konum**|Doğu ABD||

> [!NOTE]
> Kaynak grubunun konumunu ifade eder ve DNS bölgesini etkilemez. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

## <a name="retrieve-name-servers"></a>Ad sunucularını alma

DNS bölgenizi Azure DNS'ye devretmeden önce, bölgenizin ad sunucularını bilmeniz gerekir. Azure DNS, her bölge oluşturmada bir havuzdan ad sunucuları ayırır.

1. Oluşturulan DNS bölgesiyle, Azure Portal **Sık Kullanılanlar** bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasında DNS bölgenizi seçin. Seçtiğiniz abonelikte zaten çeşitli kaynaklar varsa, uygulama ağ geçidine kolaylıkla erişmek için **Ada göre filtrele** kutusuna etki alanı adınızı girebilirsiniz. 

1. DNS bölgesi sayfasından ad sunucularını alın. Bu örnekte, contoso.net bölgesine *ns1-01.azure-dns.com*, *ns2-01.azure-dns.net*, *ns3-01.azure-dns.org* ve *ns4-01.azure-dns.info* ad sunucuları atanmıştır:

   ![Ad sunucularının listesi](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

Azure DNS, bölgenizdeki yetkili NS kayıtlarını atanan ad sunucularını içerecek şekilde otomatik olarak oluşturur.


## <a name="delegate-the-domain"></a>Etki alanını devretme

Artık DNS bölgesi oluşturulduğuna ve ad sunucularınız olduğuna göre, üst etki alanını Azure DNS ad sunucularıyla güncelleştirmeniz gerekir. Her kayıt şirketi, bir etki alanının ad sunucusu kayıtlarını değiştirmek için kendi DNS yönetim araçlarına sahiptir. Kayıt şirketinin DNS yönetim sayfasında NS kayıtlarını düzenleyin ve NS kayıtlarını Azure DNS ad sunucularıyla değiştirin.

Bir etki alanını Azure DNS'ye devrederken Azure DNS tarafından sağlanan ad sunucularını kullanmanız gerekir. Etki alanınızın adından bağımsız olarak dört ad sunucusunun tamamını kullanmanız önerilir. Etki alanı temsilcisi, bir ad sunucusunun etki alanınızla aynı üst düzey etki alanını kullanmasını gerektirmez.

Kendi bölgenizdeki ad sunucularını kullanan ve bazen *gösterim ad sunucuları* olarak adlandırılan temsilci seçimleri şu anda Azure DNS'de desteklenmemektedir.

## <a name="verify-that-the-delegation-is-working"></a>Devretme özelliğinin çalıştığını doğrulama

Temsilci seçmeyi tamamladıktan sonra, bölgenizin Yetki Başlangıcı (SOA) kaydını sorgulamak için *nslookup* gibi bir araç kullanarak çalışıp çalışmadığını doğrulayabilirsiniz. SOA kaydı, bölge oluşturulurken otomatik olarak oluşturulur. Başarılı bir şekilde çalışıp çalışmadığını kontrol edebilmek için devretme işlemini tamamladıktan sonra 10 dakika veya daha fazla beklemeniz gerekebilir. Değişikliklerin DNS sisteminde yayılması daha uzun sürebilir.

Azure DNS ad sunucularını belirtmeniz gerekmez. Temsilci seçimi doğru ayarlanmışsa normal DNS çözümleme işlemi ad sunucularını otomatik olarak bulur.

Komut isteminden aşağıdakine benzer bir nslookup komutu yazın:

```
nslookup -type=SOA contoso.net
```

Aşağıda önceki komuttan bir yanıt örneği gösterilmektedir:

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

Bir sonraki öğreticiye geçmeyi düşünüyorsanız **contosoRG** kaynak grubunu tutabilirsiniz. Aksi halde **contosoRG** kaynak grubunu silerek bu öğreticide oluşturulan kaynakları silebilirsiniz. Bunu yapmak **contosoRG** kaynak grubuna ve ardından **Kaynak grubunu sil**'e tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide etki alanınız için bir DNS bölgesi oluşturdunuz ve bunu Azure DNS'ye devrettiniz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında bir web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
