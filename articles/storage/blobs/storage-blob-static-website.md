---
title: Azure Depolama'da statik Web sitesi barındırma
description: Barındırma, modern web uygulamalarını barındırmak için uygun maliyetli ve ölçeklenebilir bir çözüm sağlayarak azure depolama statik Web sitesi.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.reviewer: seguler
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 36cc8cebdb567cb9650ad1ad3baf72a0b5478247
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427967"
---
# <a name="static-website-hosting-in-azure-storage"></a>Azure Depolama'da statik Web sitesi barındırma

Statik içerik (HTML, CSS, JavaScript ve görüntü dosyaları) doğrudan adlı bir depolama kapsayıcısındaki verebilen *$web*. Azure depolama, içeriğinizi barındıran içeren sunucusuz mimarileri kullanmanıza olanak sağlar [Azure işlevleri](/azure/azure-functions/functions-overview) ve başka bir Platform olarak hizmet (PaaS) Hizmetleri.

> [!NOTE]
> Sitenizi sunucu tarafındaki koda bağımlıdır kullanırsanız [Azure App Service](/azure/app-service/overview) yerine.

## <a name="setting-up-a-static-website"></a>Statik bir Web sitesi ayarlama

Statik Web sitesi barındırma depolama hesabında etkinleştirmek için olan bir özelliktir.

Statik Web sitesi barındırma etkinleştirmek için varsayılan dosyanızın adını seçin ve isteğe bağlı olarak Özel 404 sayfa yolu sağlayın. Adlı bir blob depolama kapsayıcısı, **$web** zaten yoksa hesabında bir sizin için oluşturulur. Sitenizin dosyalar bu kapsayıcıya ekleyin.

Adım adım yönergeler için bkz. [Azure depolamadaki statik Web sitesi barındırma](storage-blob-static-website-how-to.md).

![Azure depolama statik Web siteleri ölçümleri ölçüm](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Dosyalar **$web** kapsayıcı küçük harfe duyarlıdır, anonim erişim isteklerini ve yalnızca okuma işlemleri kullanılabilir aracılığıyla sunulur.

## <a name="uploading-content"></a>İçeriği karşıya yükleniyor

İçeriği karşıya yüklemek için bu araçlardan herhangi birini kullanabilirsiniz **$web** kapsayıcı:

> [!div class="checklist"]
> * [Azure CLI](storage-blob-static-website-how-to.md#cli)
> * [Azure PowerShell Modülü](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/)
> * [Visual Studio Code uzantısı](https://code.visualstudio.com/tutorials/static-website/getting-started)

## <a name="viewing-content"></a>İçerik görüntüleme

Kullanıcılar, Web sitesinin genel URL'yi kullanarak bir tarayıcıdan site içeriği görüntüleyebilir. URL, Azure portal, Azure CLI veya PowerShell kullanarak bulabilirsiniz. Bu tabloyu kılavuz olarak kullanın.

|Aracı| Rehber |
|----|----|
|**Azure portal** | [Azure portalını kullanarak Web sitesi URL'si bulunamadı](storage-blob-static-website-how-to.md#portal-find-url) |
|**Azure CLI** | [Azure CLI kullanarak Web sitesi URL'si bulunamadı](storage-blob-static-website-how-to.md#cli-find-url) |
|**Azure PowerShell Modülü** | [PowerShell kullanarak Web sitesi URL'si bulunamadı](storage-blob-static-website-how-to.md#powershell-find-url) |

Sitenizin URL'sini bölgesel kodunu içerir. Örneğin URL `https://contosoblobaccount.z22.web.core.windows.net/` bölge kodu içeren `z22`.

URL kod kalmalıdır, yalnızca dahili kullanım içindir ve diğer herhangi bir yolla kod kullanmak zorunda kalmazsınız.

Statik Web sitesi barındırma, etkinleştirdiğinizde, belirttiğiniz dizin belgenin görünür kullanıcılar sitesini açın ve belirli bir dosyayı belirtmeyin (örneğin: `https://contosoblobaccount.z22.web.core.windows.net`).  

Sunucu bir 404 hatası döndürür ve Web sitesi etkinleştirildiğinde bir hata belgesi belirtmediniz varsayılan 404 sayfa kullanıcıya döndürülür.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Ayarın etkisi web kapsayıcısı genel erişim düzeyi

Genel erişim düzeyini değiştirebilir **$web** kapsayıcı, ancak bu olan herhangi bir etkisi birincil statik Web sitesi uç noktada çünkü bu dosyalar Anonim erişim istekleri aracılığıyla sunulur. Tüm dosyalara genel (salt okunur) erişim anlamına gelir.

Aşağıdaki ekran görüntüsünde, Azure portalında genel erişim düzeyi ayarı gösterir:

![Portalda genel erişim düzeyi ayarlama işlemini gösteren ekran görüntüsü](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Birincil statik Web sitesi uç noktası etkilenmez, ancak genel erişim düzeyi değişiklik birincil blob Hizmeti uç noktası etkiler.

Örneğin, genel erişim düzeyi değiştirirseniz **$web** kapsayıcısından **özel (anonim erişim yok)** için **Blob (yalnızca BLOB'lar için anonim okuma erişimi)** , ardından Birincil statik Web sitesi uç noktası genel erişim düzeyini `https://contosoblobaccount.z22.web.core.windows.net/index.html` değiştirmez.

Ancak, genel erişim birincil blob Hizmeti uç noktası `https://contosoblobaccount.blob.core.windows.net/$web/index.html` özel ortak olarak değiştirin. Artık kullanıcılar, bu iki uç nokta birini kullanarak bu dosyayı açabilir.

## <a name="content-delivery-network-cdn-and-secure-socket-layer-ssl-support"></a>Content Delivery Network (CDN) ve Güvenli Yuva Katmanı (SSL) desteği

Statik Web sitesi dosyalarınızı, özel etki alanı hem de HTTPS üzerinden kullanılabilmesi için bkz: [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md). Bu işlemin bir parçası olarak, CDN'NİZİN birincil siteye yönlendirmeniz gerekir *statik Web sitesi* aksine birincil uç nokta *blob hizmeti* uç noktası. CDN yapılandırma hemen çalıştırılmadı olarak içeriğinizi görünür hale gelmeden önce birkaç dakika beklemeniz gerekebilir.

Statik Web sitenizi güncelleştirdiğinizde, önbelleğe alınmış içerikleri CDN uç sunucularda CDN uç noktasını Temizleme tarafından Temizle emin olun. Daha fazla bilgi için bkz. [Azure CDN uç noktasını temizleme](../../cdn/cdn-purge-endpoint.md).

> [!NOTE]
> Web uç noktası, HTTP ve HTTPS erişilebilir olacak şekilde HTTPS hesabı web uç noktası aracılığıyla yerel olarak desteklenir. Ancak, depolama hesabı, HTTPS üzerinden güvenli aktarım gerektir için yapılandırılmışsa, kullanıcıların HTTPS uç noktasını kullanmanız gerekir. Daha fazla bilgi için [Azure depolamadaki güvenli aktarım gerektir](../common/storage-require-secure-transfer.md).
>
> HTTPS üzerinden özel etki alanları şu anda Azure CDN kullanımı gerektirir.

## <a name="custom-domain-names"></a>Özel etki alanı adları

Statik Web sitenizi kullanılabilir bir özel etki alanı yapabilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabınız için bir özel etki alanı adı yapılandırma](storage-custom-domain-name.md).

Etki alanınızı azure'da barındırmaya bir derinlemesine bakış için bkz: [etki alanınızı Azure DNS'de konak](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Fiyatlandırma

Statik Web sitesi barındırma etkinleştirebilirsiniz ücretsiz olarak. Yalnızca sitenizi yararlanan blob depolama ve işlem maliyetleri için faturalandırılırsınız. Azure Blob Depolama fiyatları hakkında daha fazla ayrıntı için kullanıma [Azure Blob Depolama fiyatlandırması sayfasını](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>Ölçümler

Statik Web sitesi sayfalarında ölçümleri etkinleştirebilirsiniz. Ölçümleri etkinleştirdikten sonra dosyalarda istatistikleri trafiği **$web** kapsayıcı ölçümleri panosunda bildirilir.

Statik Web sitesi sayfalarınıza ölçümleri etkinleştirmek için bkz: [etkinleştirme statik Web sitesi sayfalarında ölçümleri](storage-blob-static-website-how-to.md#metrics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama'daki statik Web sitesi barındırma](storage-blob-static-website-how-to.md)
* [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Blob veya web uç noktanız için özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure/azure-functions/functions-overview)
* [Azure uygulama hizmeti](/azure/app-service/overview)
* [İlk sunucusuz web uygulamanızı oluşturun](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Öğretici: Etki alanınızı Azure DNS'de barındırın](../../dns/dns-delegate-domain-azure-dns.md)
