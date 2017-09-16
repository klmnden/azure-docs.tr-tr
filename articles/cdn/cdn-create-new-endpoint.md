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
ms.date: 01/23/2017
ms.author: mazha
ms.translationtype: Human Translation
ms.sourcegitcommit: bdf6e27463fcc6186a3b15a55653fa468da91bdc
ms.openlocfilehash: d263e911d0d0b3cdc1e48e300a3c8a0994b38c39
ms.contentlocale: tr-tr
ms.lasthandoff: 01/24/2017


---
# <a name="getting-started-with-azure-cdn"></a>Azure CDN kullanmaya başlama
Bu konu başlığında, yeni bir CDN profili ve uç noktası oluşturarak Azure CDN'yi etkinleştirme işlemi boyunca size yol gösterilecektir.

> [!IMPORTANT]
> CDN'nin nasıl çalıştığına ilişkin giriş bilgileri ve özelliklerin listesi için bkz. [CDN'ye Genel Bakış](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Yeni bir CDN profili oluşturma
CDN profili, CDN uç noktaları koleksiyonudur.  Her bir profil, bir veya daha fazla CDN uç noktası içerir.  CDN uç noktalarınızı İnternet etki alanı, web uygulaması veya başka ölçütlere göre düzenlemek için birden çok profil kullanmak isteyebilirsiniz.

> [!NOTE]
> Varsayılan olarak, tek bir Azure aboneliği sekiz CDN profili ile sınırlıdır. Her CDN profili, on CDN uç noktası ile sınırlıdır.
> 
> CDN fiyatlandırması, CDN profili düzeyinde uygulanır. Azure CDN fiyatlandırma katmanlarının bir karışımını kullanmak istiyorsanız birden çok CDN profili kullanmanız gerekir.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Yeni bir CDN uç noktası oluşturma
**Yeni bir CDN uç noktası oluşturmak için**

1. [Azure portalında](https://portal.azure.com), CDN profilinize gidin.  Önceki adımda bunu panoya sabitlemiş olabilirsiniz.  Sabitlemediyseniz bunu bulmak için **Gözat**'a, ardından **CDN profilleri**'ne ve uç noktanızı eklemeyi planladığınız profile tıklayabilirsiniz.
   
    CDN profili dikey penceresi görünür.
   
    ![CDN profili][cdn-profile-settings]
2. **Uç Nokta Ekle** düğmesine tıklayın.
   
    ![Uç nokta ekle düğmesi][cdn-new-endpoint-button]
   
    **Uç nokta ekleme** dikey penceresi görünür.
   
    ![Uç nokta ekleme dikey penceresi][cdn-add-endpoint]
3. Bu CDN uç noktası için bir **Ad** girin.  Bu ad, `<endpointname>.azureedge.net` etki alanındaki önbelleğe alınmış kaynaklarınıza erişmek için kullanılır.
4. **Başlangıç noktası türü** açılan menüsünde, başlangıç noktası türünüzü seçin.  Azure Storage hesabı için **Depolama**, Azure Bulut Hizmeti için **Bulut hizmeti**, Azure Web Uygulaması için **Web Uygulaması** veya genel olarak erişilebilen herhangi bir web sunucusu kaynağı (Azure'da veya başka bir konumda barındırılan) için **Özel kaynak** seçeneğini belirleyin.
   
    ![CDN kaynak türü](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. **Kaynak ana bilgisayar adı** açılan menüsünde, kaynak etki alanınızı seçin veya yazın.  4. Adım'da belirttiğiniz türdeki tüm kullanılabilir kaynaklar açılır listede listelenir.  **Kaynak türü** tercihiniz olarak *Özel kaynak* öğesini seçtiyseniz özel kaynağınızın etki alanını yazmanız gerekir.
6. **Kaynak yolu** metin kutusunda, önbelleğe almak istediğiniz kaynakların yolunu girin veya 5. adımda belirttiğiniz etki alanındaki herhangi bir kaynağın önbelleğe alınmasına izin vermek için bu alanı boş bırakın.
7. **Kaynak ana bilgisayar üst bilgisi** içinde, CDN'nin her bir istekle göndermesini istediğiniz ana bilgisayar üst bilgisini girin veya varsayılan değeri bırakın.
   
   > [!WARNING]
   > Azure Storage ve Web Apps gibi bazı kaynak türleri, ana bilgisayar üst bilgisinin kaynağın etki alanı ile eşleşmesini gerektirir. Etki alanından farklı ana bilgisayar üst bilgisi gerektiren bir kaynağa sahip değilseniz varsayılan değeri bırakmanız gerekir.
   > 
   > 
8. **Protokol** ve **Kaynak bağlantı noktası** alanında, kaynak üzerindeki kaynaklarınıza erişmek için kullanılan protokolleri ve bağlantı noktalarını belirtin.  En az bir protokol (HTTP veya HTTPS) seçilmelidir.
   
   > [!NOTE]
   > **Kaynak bağlantı noktası** yalnızca, uç noktanın kaynaktan bilgi almak için hangi bağlantı noktasını kullanacağını etkiler.  Uç noktanın kendisi, **Kaynak bağlantı noktasından** bağımsız olarak, yalnızca varsayılan HTTP ve HTTPS bağlantı noktalarındaki (80 ve 443) uç istemciler tarafından kullanılabilir.  
   > 
   > **Akamai'den Azure CDN** uç noktaları, kaynaklar için tam TCP bağlantı noktası aralığına izin vermez.  İzin verilmeyen kaynak bağlantı noktalarının listesi için bkz. [Akamai'den Azure CDN İzin Verilen Kaynak Bağlantı Noktaları](https://msdn.microsoft.com/library/mt757337.aspx).  
   > 
   > CDN içeriğine HTTPS kullanarak erişilmesi şu kısıtlamalara tabidir:
   > 
   > * CDN tarafından sağlanan SSL sertifikasını kullanmanız gerekir. Üçüncü taraf sertifikalar desteklenmez.
   > * HTTPS içeriğine erişmek için CDN tarafından sağlanan etki alanını (`<endpointname>.azureedge.net`) kullanmanız gerekir. CDN şu an için özel sertifikaları desteklemediğinden özel etki alanı adları (CNAME'ler) için HTTPS desteği mevcut değildir.
   > 
   > 
9. Yeni uç noktayı oluşturmak için **Ekle** düğmesine tıklayın.
10. Uç nokta oluşturulduktan sonra, profile yönelik uç noktalar listesinde görünür. Liste görünümünde, kaynak etki alanının yanı sıra, önbelleğe alınan içeriğe erişmek için kullanılacak URL gösterilir.
    
    ![CDN uç noktası][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > Kaydın CDN'de yayılması zaman alacağından, uç nokta hemen kullanılabilir olmaz.  <b>Akamai'den Azure CDN</b> profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır.  <b>Verizon'dan Azure CDN</b> profilleri için yayma işlemi genellikle 90 dakika içinde tamamlanır ancak bazı durumlarda daha uzun sürebilir.
    > 
    > Uç nokta yapılandırması POP'lere yayılmadan önce CDN etki alanı adını kullanmayı deneyen kullanıcılar, HTTP 404 yanıt kodlarını alır.  Uç noktanızı oluşturmanızın ardından birkaç saat geçmesine karşın 404 yanıtlarını almaya devam ediyorsanız lütfen bkz. [404 durumu döndüren CDN uç noktası sorunlarını giderme](cdn-troubleshoot-endpoint.md).
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

