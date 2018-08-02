---
title: HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma
description: Azure CDN HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için blob depolama ile tümleştirmeyi öğrenin
services: storage
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 7c4acc7d0832442b94735619ea3a01cb319da993
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398264"
---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma
Azure Content Delivery Network (CDN), HTTPS özel etki alanı adları için artık desteklemektedir. Özel etki alanınızda HTTPS üzerinden kullanarak depolama BLOB'ları erişmek için bu özelliği kullanabilir. Bunu yapmak için önce blob veya web uç noktada Azure CDN'yi etkinleştirme ve CDN özel etki alanı için harita gerekir. Bu adımları uyguladıktan sonra özel etki alanınız için HTTPS'yi etkinleştirme aracılığıyla tek tıklamayla etkinleştirme, eksiksiz sertifika yönetimi ve tüm normal CDN fiyatlandırması için ek ücret ödemeden basitleştirilmiştir.

Bu yetenek, gizlilik ve veri bütünlüğünü aktarım sırasında hassas web uygulama verilerinizi korumanıza olanak sağladığından önemlidir. HTTPS üzerinden trafik hizmet vermek için SSL protokolü kullanarak internet üzerinden gönderildiğinde verilerin şifrelenmesini sağlar. HTTPS, güven ve kimlik doğrulaması sağlar ve web uygulamalarınızı saldırılara karşı korur.

> [!NOTE]  
> SSL desteği için özel etki alanı adlarını sağlamanın yanı sıra Azure CDN, dünya yüksek bant genişlikli içerik dağıtmanıza olanak uygulamanızı ölçeklendirme yardımcı olabilir. Daha fazla bilgi için kullanıma [Azure CDN'ye genel bakış](../../cdn/cdn-overview.md).

## <a name="quick-start"></a>Hızlı başlangıç
Özel bir blob depolama uç noktanız için HTTPS'yi etkinleştirmek için gerekli adımlar şunlardır:

1.  [Bir Azure depolama hesabını Azure CDN ile tümleştirme](../../cdn/cdn-create-a-storage-account-with-cdn.md).
    Bu makalede, bunu zaten yapmadıysanız, bir depolama hesabı Azure Portalı'nda oluşturma işlemini gösterir.

    > [!NOTE]  
    > Azure Depolama'da statik Web sitesi desteği Önizleme boyunca, "özel kaynak", "kaynak türü" açılan menüden depolama web uç noktanız eklemek için seçin. Azure Portalı'nda, bu, CDN profili, depolama hesabınızdaki doğrudan yapmak gerekir.

2.  [Azure CDN içeriğini özel bir etki alanını eşleme](../../cdn/cdn-map-content-to-custom-domain.md).
3.  [Azure CDN özel etki alanı üzerinde HTTPS'yi etkinleştirme](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları
Blob depolama uç noktanız anonim okuma erişimini engellemek için yapılandırılmışsa, sağlamanız gerekir bir [paylaşılan erişim imzası (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) özel etki alanınıza yaptığınız her istekte belirteç. Varsayılan olarak, blob depolama uç noktaları anonim okuma erişimini engeller. Bkz: [kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](storage-manage-access-to-resources.md) paylaşılan erişim imzaları ile ilgili daha fazla bilgi için.

Azure CDN için SAS belirteci eklenen herhangi bir kısıtlama uymaz. Örneğin, tüm SAS belirteçlerini sona erme süresini sahip. Bu içeriği CDN kenar düğümlerinden temizlenir kadar içerik Bunun anlamı süresi dolmuş bir SAS ile hala erişilebilir. Ne kadar süreyle veri CDN'de önbelleğe yanıt üst bilgisi ayarlayarak önbelleğe denetleyebilirsiniz. Bkz: [sona erme tarihini Azure CDN, Azure depolama blobları yönetme](../../cdn/cdn-manage-expiration-of-blob-content.md) yönergeler için.

Aynı blob uç noktası için birden fazla SAS URL'lerini oluşturursanız, sorgu dizesi için Azure CDN önbelleğe alma özelliğini öneririz. Bu, her URL, benzersiz bir varlık olarak değerlendirilir sağlamak içindir. Bkz: [Azure'nın CDN önbelleğe alma davranışını sorgu dizeleriyle denetleme](../../cdn/cdn-query-string.md) daha fazla bilgi için.

## <a name="http-to-https-redirection"></a>HTTP-HTTPS yeniden yönlendirmesi
HTTP trafiğini HTTPS'ye yönlendirmek seçebilirsiniz. Bu, Verizon sunan Azure CDN premium kullanılmasını gerektirir. Şunları yapmanız [geçersiz kılma HTTP davranışı Azure CDN kurallar altyapısı kullanılarak](../../cdn/cdn-rules-engine.md) aşağıdaki kural ile:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Cdn uç noktası adı" CDN uç noktanız için yapılandırdığınız adla ifade eder. Bu değer, açılan listeden seçebilirsiniz. "Kaynak yolu", statik içeriğin bulunduğu kaynak depolama hesabınızda yol gösterir. "Kaynak yolu", tek bir kapsayıcıdaki tüm statik içeriği barındırıyorsanız, bu kapsayıcı adı ile değiştirin.

Kuralları içinde daha ayrıntılı bilgi edinmek için lütfen bkz [Azure CDN kural altyapısı özellikleri](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Bir Azure CDN ile blobları eriştiğinizde, ödeme yaptığınız [Blob Depolama fiyatları](https://azure.microsoft.com/pricing/details/storage/blobs/) kenar düğümlerini ve kaynak (Blob Depolama) arasındaki trafiği ve [CDN fiyatları](https://azure.microsoft.com/pricing/details/cdn/) kenar düğümlerinden erişilen veriler için.

Örneğin, bir Azure CDN kullanarak erişiliyor Batı ABD Bölgesinde bir depolama hesabında olduğunu varsayalım. Birleşik Krallık'ta birisi için CDN üzerinden o depolama hesabındaki BLOB erişim çalışırsa, Azure, blob için UK en yakın kenar düğümüne ilk denetler. Bulundu, bu BLOB bu kopyayı erişir ve CDN fiyatlandırması, CDN'de erişilen çünkü kullanır. Bulunamazsa, Azure Blob Depolama fiyatlandırması belirtilen çıkış ve işlem ücretleri sonuçlanır ve ardından CDN faturalandırma uygulanmasına neden olur kenar düğümündeki dosyaya erişim kenar düğümüne blobu kopyalar.

Bakarak olduğunda [CDN fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cdn/), özel etki alanı adları için HTTPS'yi destekleyecek unutmayın, yalnızca Verizon ürünleri (standart ve Premium) Azure CDN için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Blob Depolama uç noktanız için bir özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure depolama (Önizleme) statik Web sitesi barındırma](storage-blob-static-website.md)
