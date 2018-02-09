---
title: "Azure CDN kullanmaya başlama | Microsoft Docs"
description: "Bu konu başlığında, Azure Content Delivery Network’ün (CDN) nasıl etkinleştirileceği gösterilmektedir. Öğretici, yeni bir CDN profili ve uç noktası oluşturma işlemi boyunca size yol gösterecektir."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2018
ms.author: mazha
ms.openlocfilehash: 81a88f6495ca9092ca3b55b8ffb3e41def3b4623
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="getting-started-with-azure-cdn"></a>Azure CDN kullanmaya başlama
Bu makalede yeni bir CDN profili ve uç noktası oluşturarak Azure Content Delivery Network’ü (CDN) nasıl etkinleştireceğiniz anlatılmaktadır.

> [!IMPORTANT]
> CDN tanıtımı ve özellik listesi için bkz. [CDN'ye Genel Bakış](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Yeni bir CDN profili oluşturma
CDN profili, CDN uç noktaları koleksiyonudur. Her bir profil, bir veya daha fazla CDN uç noktası içerebilir. CDN uç noktalarınızı internet etki alanı, web uygulaması veya başka ölçütlere göre düzenlemek için birden çok profil kullanabilirsiniz.

> [!NOTE]
> Azure aboneliği aşağıdaki kaynaklar için varsayılan sınırlara sahiptir:
> - Oluşturulabilecek CDN profili sayısı
> - Bir CDN profilinde oluşturulabilecek uç nokta sayısı 
> - Bir uç noktasına eşlenebilecek özel etki alanı sayısı
>
> CDN aboneliği sınırları hakkında daha fazla bilgi için bkz. [CDN sınırları](https://docs.microsoft.com/azure/azure-subscription-service-limits#cdn-limits).
>
> CDN fiyatlandırması, CDN profili düzeyinde uygulanır. Bu yüzden, Azure CDN fiyatlandırma katmanlarının bir karışımını kullanmak istiyorsanız birden çok CDN profili oluşturmanız gerekir.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Yeni bir CDN uç noktası oluşturma
**Yeni bir CDN uç noktası oluşturmak için**

1. [Azure portalında](https://portal.azure.com), CDN profilinize gidin. Önceki adımda bunu panoya sabitlemiş olabilirsiniz. Aksi takdirde, **Tüm hizmetler**’i, sonra da **CDN profilleri**’ni seçerek bulabilirsiniz. **CDN profilleri** bölmesinde, uç noktanızı eklemeyi planladığınız profili seçin. 
   
    CDN profili bölmesi görünür.
   
    ![CDN profili][cdn-profile-settings]

2. **Uç nokta**’yı seçin.
   
    ![Uç nokta ekle düğmesi][cdn-new-endpoint-button]
   
    **Uç nokta ekleyin** bölmesi görünür.
   
    ![Uç nokta ekleyin bölmesi][cdn-add-endpoint]

3. **Ad** için, yeni CDN uç noktasına yönelik benzersiz bir ad girin. Bu ad, `<endpointname>.azureedge.net` etki alanındaki önbelleğe alınmış kaynaklarınıza erişmek için kullanılır.

4. **Kaynak türü** için, bir kaynak türü seçin. Azure Storage hesabı için **Depolama**, Azure Bulut Hizmeti için **Bulut hizmeti**, Azure Web Uygulaması için **Web Uygulaması** veya genel olarak erişilebilen herhangi bir web sunucusu kaynağı (Azure'da veya başka bir konumda barındırılan) için **Özel kaynak** seçeneğini belirleyin.
   
    ![CDN kaynak türü](./media/cdn-create-new-endpoint/cdn-origin-type.png)

5. **Kaynak ana bilgisayar adı** için, kaynak etki alanınızı seçin veya girin. 4. Adım'da belirttiğiniz türdeki tüm kullanılabilir kaynaklar açılan listede listelenir. Çıkış noktası türünüz olarak **Özel çıkış noktasını** seçtiyseniz, özel çıkış noktanızın etki alanını girin.
    
6. **Kaynak yolu** için, önbelleğe almak istediğiniz kaynakların yolunu girin veya 5. adımda belirttiğiniz etki alanındaki herhangi bir kaynağın önbelleğe alınmasına izin vermek için bu alanı boş bırakın.
    
7. **Kaynak ana bilgisayar üst bilgisi** için, Azure CDN'nin her bir istekle göndermesini istediğiniz ana bilgisayar üst bilgisini girin veya varsayılan değeri bırakın.
   
   > [!WARNING]
   > Azure Storage ve Web Apps gibi bazı kaynak türleri, ana bilgisayar üst bilgisinin kaynağın etki alanı ile eşleşmesini gerektirir. Etki alanından farklı ana bilgisayar üst bilgisi gerektiren bir kaynağa sahip değilseniz varsayılan değeri bırakmanız gerekir.
   > 
    
8. **Protokol** ve **Kaynak bağlantı noktası** alanında, kaynak üzerindeki kaynaklarınıza erişmek için kullanılan protokolleri ve bağlantı noktalarını belirtin. En az bir protokol (HTTP veya HTTPS) seçilmelidir. HTTPS içeriğine erişmek için CDN tarafından sağlanan etki alanını (`<endpointname>.azureedge.net`) kullanın. 
   
   > [!NOTE]
   > **Kaynak bağlantı noktası** değeri yalnızca, uç noktanın kaynaktan bilgi almak için kullandığı bağlantı noktasını belirler. Uç noktanın kendisi, **Kaynak bağlantı noktası** değerinden bağımsız olarak, yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarındaki (80 ve 443) uç istemciler tarafından kullanılabilir.  
   > 
   > **Akamai'den Azure CDN** profillerindeki uç noktalar, kaynak bağlantı noktaları için tam TCP bağlantı noktası aralığına izin vermez. İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > HTTPS kullanarak CDN içeriğine eriştiğinizde, şu sınırlamalarla karşılaşırsınız:
   > 
   > * CDN tarafından sağlanan SSL sertifikasını kullanın. Üçüncü taraf sertifikalar desteklenmez.
   > * Azure CDN özel etki alanları için HTTPS desteği yalnızca **Azure CDN from Verizon** ürünleri (Standard ve Premium) için mevcuttur. **Azure CDN from Akamai** ürünlerinde desteklenmez. Daha fazla bilgi için bkz. [Azure CDN özel etki alanı üzerinde HTTPS'yi yapılandırma](cdn-custom-ssl.md).
    
9. Yeni uç nokta oluşturmak için **Ekle**’yi seçin.
   
   Uç nokta oluşturulduktan sonra, profile yönelik uç noktalar listesinde görünür.
    
   ![CDN uç noktası][cdn-endpoint-success]
    
   > [!IMPORTANT]
   > Kaydın yayılması zaman alacağından, uç nokta hemen kullanılabilir olmaz. **Akamai'den Azure CDN** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. **Verizon'dan Azure CDN** profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır ancak bazı durumlarda daha uzun sürebilir.
    > 
    > Uç nokta yapılandırması POP'lere yayılmadan önce CDN etki alanı adını kullanmayı denerseniz, HTTP 404 yanıtı alabilirsiniz. Uç noktanızı oluşturmanızın ardından birkaç saat geçmesine karşın 404 yanıtlarını almaya devam ediyorsanız bkz. [404 durumu döndüren CDN uç noktası sorunlarını giderme](cdn-troubleshoot-endpoint.md).
    > 
    > 

## <a name="see-also"></a>Ayrıca Bkz.
* [İsteklerin önbelleğe alınma davranışını sorgu dizeleriyle denetleme](cdn-query-string.md)
* [CDN İçeriğini Özel Etki Alanı ile Eşleme](cdn-map-content-to-custom-domain.md)
* [Azure CDN uç noktasında varlıkları önceden yükleme](cdn-preload-endpoint.md)
* [Azure CDN Uç Noktasını Temizleme](cdn-purge-endpoint.md)
* [404 durumları döndüren CDN uç noktası sorunlarını giderme](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
