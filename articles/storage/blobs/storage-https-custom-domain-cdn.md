---
title: HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma
description: Azure CDN HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Blob Depolama ile tümleştirmeyi öğrenin
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: normesta
ms.reviewer: seguler
ms.subservice: blobs
ms.openlocfilehash: da3a6dcb0d125ac4666bc375e843c57cf12fb2fc
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148399"
---
# <a name="use-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma

Azure Content Delivery Network (Azure CDN), HTTPS özel etki alanı adları için artık desteklemektedir. Azure CDN ile HTTPS üzerinden özel etki alanı adınızı kullanarak blobları erişebilirsiniz. Bunu yapmak için blob veya web uç noktada Azure CDN'yi etkinleştirme ve ardından Azure CDN özel etki alanı için harita. Bitirdikten sonra Azure tek tıklamayla erişim ve eksiksiz sertifika yönetimi aracılığıyla özel etki alanınız için HTTPS'yi etkinleştirme basitleştirir. Normal Azure CDN fiyatlandırması, herhangi bir artış yoktur.

Azure CDN, Aktarımdaki olmakla birlikte gizlilik ve veri bütünlüğünü web uygulama verilerinizi korumanızı sağlar. HTTPS üzerinden trafik hizmet vermek için SSL protokolü kullanarak, Azure CDN internet üzerinden gönderildiğinde şifrelenmiş verilerinizi korur. Azure CDN ile HTTPS kullanarak web uygulamalarınızı saldırıya karşı korunmasına yardımcı olur.

> [!NOTE]  
> SSL desteği için özel etki alanı adlarını sağlamanın yanı sıra Azure CDN, dünya yüksek bant genişlikli içerik dağıtmanıza olanak uygulamanızı ölçeklendirme yardımcı olabilir. Daha fazla bilgi için bkz. [genel bakış, Azure CDN](../../cdn/cdn-overview.md).

## <a name="quickstart"></a>Hızlı Başlangıç

Özel Blob Depolama uç noktanız için HTTPS'yi etkinleştirmek için aşağıdakileri yapın:

1.  [Bir Azure depolama hesabını Azure CDN ile tümleştirme](../../cdn/cdn-create-a-storage-account-with-cdn.md).  
    Zaten yapmadıysanız bu makalede Azure portalında bir depolama hesabı oluşturma işleminde size yol gösterir.

    > [!NOTE]  
    > Azure Depolama'daki statik Web sitesi desteği Önizleme sırasında depolama web uç noktası eklemek için seçin **özel kaynak** içinde **kaynak türü** aşağı açılan listesi. Azure portalında Bu, Azure CDN profili, depolama hesabınızdaki doğrudan yapmak gerekir.

2.  [Azure CDN içeriğini özel bir etki alanını eşleme](../../cdn/cdn-map-content-to-custom-domain.md).

3.  [Azure CDN özel etki alanı üzerinde HTTPS'yi etkinleştirme](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları

Varsayılan olarak, Blob Depolama uç noktaları anonim okuma erişimini engeller. Blob Depolama uç noktanız anonim okuma erişimini engellemek için yapılandırılmışsa, sağlayan bir [paylaşılan erişim imzası](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) özel etki alanınıza her istekte belirteç. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](storage-manage-access-to-resources.md).

Azure CDN için paylaşılan erişim imzası belirtecini eklenen herhangi bir kısıtlama uymuyor. Örneğin, tüm paylaşılan erişim imzası belirteçlerinin süresinin dolmasını. Bu içeriği, Azure CDN kenar düğümlerinden temizlenir kadar bir süresi dolmuş bir paylaşılan erişim imzası içerikle erişmeye devam edebilirsiniz. Önbellek yanıtı üst bilgisini ayarlayarak verilerin Azure CDN üzerinde ne kadar süreyle önbelleğe alınacağını denetleyebilirsiniz. Bilgi edinmek için bkz. nasıl [Azure depolama blobları Azure cdn'de kullanım süresini yönetme](../../cdn/cdn-manage-expiration-of-blob-content.md).

İki veya daha fazla paylaşılan erişim imzası URL'lerini aynı blob uç noktası için oluşturursanız, sorgu dizesi için Azure CDN önbelleğe alma özelliğini açmak öneririz. Bu eylem, Azure, benzersiz bir varlık olarak her URL işler sağlar. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](../../cdn/cdn-query-string.md).

## <a name="http-to-https-redirection"></a>HTTP-HTTPS yeniden yönlendirmesi

HTTP trafiğini HTTPS'ye yeniden yönlendirebilirsiniz. Bunun yapılması, Verizon sunan Azure CDN premium kullanılmasını gerektirir. [Azure CDN kurallar altyapısı ile HTTP davranışı geçersiz kılma](../../cdn/cdn-rules-engine.md) aşağıdaki kural uygulayarak:

![HTTP-HTTPS yeniden yönlendirme kuralı](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

*CDN uç noktası adı*, aşağı açılan listesinde seçtiğiniz Azure CDN uç noktanız için yapılandırdığınız adına başvurur. *Kaynak yolu* statik içerik depolandığı kaynak depolama hesabı içindeki yolu gösterir. Tek bir kapsayıcıdaki tüm statik içerik barındırma, yerini *kaynak yolu* ile bu kapsayıcısının adı.

Kuralları içinde daha ayrıntılı bilgi edinmek için bkz. [Azure CDN kural altyapısı özellikleri](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Azure CDN ile blobları eriştiğinizde, ödeme yaptığınız [Blob Depolama fiyatları](https://azure.microsoft.com/pricing/details/storage/blobs/) kenar düğümlerini ve kaynak (Blob Depolama) arasındaki trafiği. Ödeme yaptığınız [Azure CDN fiyatları](https://azure.microsoft.com/pricing/details/cdn/) kenar düğümlerinden erişilen veriler için.

Örneğin, Azure CDN eriştiğiniz Batı ABD bölgesinde bir depolama hesabına sahip varsayalım. Birisi Birleşik Krallık'ta Azure CDN üzerinden o depolama hesabındaki bir blob erişmeye çalıştığında, Azure blob için UK en yakın olan uç düğümünde ilk denetler. Azure blob bulursa bir kopya erişir ve Azure CDN fiyatlandırması, Azure CDN ulaştığından kullanır. Azure blob bulamazsa, kenar düğümüne blobu kopyalar. Bu eylem, Blob Depolama fiyatlandırması belirtildiği gibi çıkış ve işlem ücretleri sonuçlanır. Azure, Azure CDN faturalamasını, sonuçları kenar düğümündeki dosya ardından erişir.

Üzerinde [Azure CDN fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cdn/), özel etki alanı adları için HTTPS desteği yalnızca Verizon standart ve Premium ürünlerinden Azure CDN için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Blob Depolama uç noktanız için bir özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure Depolama'da statik web sitesi barındırma](storage-blob-static-website.md)
