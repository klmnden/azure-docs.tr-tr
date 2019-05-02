---
title: Azure Depolama'da statik Web sitesi barındırma
description: Barındırma, modern web uygulamalarını barındırmak için uygun maliyetli ve ölçeklenebilir bir çözüm sağlayarak azure depolama statik Web sitesi.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/29/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 21944c62f09518e20619313cd6ac28fb2ad600c7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925273"
---
# <a name="static-website-hosting-in-azure-storage"></a>Azure Depolama'da statik Web sitesi barındırma
Azure depolama ve GPv2 hesapları doğrudan adlı bir depolama kapsayıcısındaki statik içerik (HTML, CSS, JavaScript ve görüntü dosyaları) sunmak izin *$web*. Azure Depolama'da barındırma yararlanma dahil olmak üzere sunucusuz mimarileri kullanmanıza olanak verir [Azure işlevleri](/azure/azure-functions/functions-overview) ve diğer PaaS Hizmetleri.

Statik Web sitesi barındırma aksine, sunucu tarafı kodu bağımlı dinamik siteleri en iyi kullanarak barındırılan [Azure App Service](/azure/app-service/overview).

## <a name="how-does-it-work"></a>Nasıl çalışır?
Statik Web sitesi etkinleştirdiğinizde, depolama hesabı, barındırma, varsayılan dosyanızın adını seçin ve isteğe bağlı olarak Özel 404 sayfa yolu sağlayın. Özelliğin etkin olduğu gibi bir kapsayıcı adlı *$web* önceden yoksa oluşturulur.

Dosyalar *$web* kapsayıcı şunlardır:

- Anonim erişim istekleri aracılığıyla sunulan
- yalnızca okuma işlemlerini nesnesi aracılığıyla kullanılabilir
- büyük küçük harfe duyarlı
- Bu düzen aşağıdaki genel Web kullanılabilir:
    - `https://<ACCOUNT_NAME>.<ZONE_NAME>.web.core.windows.net/<FILE_NAME>`
- Bu düzen aşağıdaki Blob Depolama uç noktası aracılığıyla kullanılabilir:
    - `https://<ACCOUNT_NAME>.blob.core.windows.net/$web/<FILE_NAME>`

Blob Depolama uç noktası, dosyaları karşıya yüklemek için kullanın. Örneğin, dosyayı bu konuma karşıya:

```bash
https://contoso.blob.core.windows.net/$web/image.png
```

Tarayıcı şunun gibi bir konumda kullanılabilir:

```bash
https://contoso.z4.web.core.windows.net/image.png
```

Bir dosya adı sağlanmazsa, seçilen varsayılan dosya adı, kök ve alt dizinleri kullanılır. Sunucu bir 404 döndürür ve bir hata belgesi yolu belirtmezseniz varsayılan 404 sayfa kullanıcıya döndürülür.

> [!NOTE]
> Dosyaları için varsayılan genel erişim düzeyi özel ' dir. Dosyaları anonim erişim istekleri aracılığıyla sunulan olduğundan, bu ayar yoksayılır. Tüm dosyalara genel erişimi yoktur ve RBAC izinlerinin göz ardı edilir.

## <a name="cdn-and-ssl-support"></a>CDN ve SSL desteği

Statik Web sitesi dosyalarınızı, özel etki alanı hem de HTTPS üzerinden kullanılabilmesi için bkz: [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md). Bu işlemin bir parçası olarak, gerek *CDN'NİZİN web uç noktası* blob uç noktası değil. CDN yapılandırma hemen çalıştırılmadı olarak içeriğinizi görünür hale gelmeden önce birkaç dakika beklemeniz gerekebilir.

Statik Web sitenizi güncelleştirdiğinizde, önbelleğe alınmış içerikleri CDN uç sunucularda CDN uç noktasını Temizleme tarafından Temizle emin olun. Daha fazla bilgi için bkz. [Azure CDN uç noktasını temizleme](../../cdn/cdn-purge-endpoint.md).

> [!NOTE]
> HTTPS, hesap web uç noktası aracılığıyla yerel olarak desteklenir. HTTPS üzerinden özel etki alanları şu anda Azure CDN kullanımı gerektirir. 
>
> HTTPS üzerinden genel hesap web uç noktası: `https://<ACCOUNT_NAME>.<ZONE_NAME>.web.core.windows.net/<FILE_NAME>`

## <a name="custom-domain-names"></a>Özel etki alanı adları

Yapabilecekleriniz [Azure depolama hesabınız için bir özel etki alanı adı yapılandırma](storage-custom-domain-name.md) statik Web sitenizi özel bir etki alanını kullanılabilir hale getirmek için. Şirket etki alanınızı barındırmaya ayrıntılı bir bakış için [Azure, bkz: etki alanınızı Azure DNS'de konak](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Fiyatlandırma
Statik Web sitesi barındırma etkinleştirme ücretsizdir. Müşteriler, kullanılan blob depolama ve işlem maliyetleri için sizden ücret alınır. Azure Blob Depolama fiyatları hakkında daha fazla ayrıntı için kullanıma [Azure Blob Depolama fiyatlandırması sayfasını](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Hızlı Başlangıç

### <a name="azure-portal"></a>Azure portal
Adresinden Azure portalında açarak başlamak https://portal.azure.com ve GPv2 depolama hesabınızda aşağıdaki adımları çalıştırın:

1. Tıklayarak **ayarları**
2. Tıklayarak **statik Web sitesi**
3. Girin bir *dizin belgesi adı*. (Bir ortak değer *index.html)*
4. İsteğe bağlı olarak girin bir *hata belgesi yolu* için Özel 404 sayfa. (Bir ortak değer *404.html)*

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Sonra varlıklarınız için karşıya yükleme *$web* Azure portalından veya ile kapsayıcı [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) tüm dizinleri karşıya yüklemek için. Eşleşen bir dosya eklediğinizden emin olun *dizin belgesi adı* özelliği etkinleştirirken seçtiğiniz.

Son olarak, Web sitenizi test etmek için web bitiş noktasına gidin.

### <a name="azure-cli"></a>Azure CLI
Depolama Önizleme uzantıyı yükleyin:

```azurecli-interactive
az extension add --name storage-preview
```
Birden fazla aboneliğiniz olması durumunda, CLI etkinleştirmek istediğiniz GPv2 depolama hesabına aboneliğine ayarlayın:

```azurecli-interactive
az account set --subscription <SUBSCRIPTION_ID>
```
Özelliğini etkinleştirin. Kendi değerlerinizle köşeli ayraçlar dahil tüm yer tutucu değerlerini değiştirdiğinizden emin olun:

```azurecli-interactive
az storage blob service-properties update --account-name <ACCOUNT_NAME> --static-website --404-document <ERROR_DOCUMENT_NAME> --index-document <INDEX_DOCUMENT_NAME>
```
Sorgu için web uç noktası URL'si:

```azurecli-interactive
az storage account show -n <ACCOUNT_NAME> -g <RESOURCE_GROUP> --query "primaryEndpoints.web" --output tsv
```

Nesnelere karşıya *$web* kaynak dizinden kapsayıcı. Başvuru için doğru kaçış mutlaka *$web* komutu kapsayıcıda. Örneğin, Azure portalında Azure CLI'dan CloudShell kullanıyorsanız, kaçış *$web* gösterildiği gibi kapsayıcı:

```azurecli-interactive
az storage blob upload-batch -s <SOURCE_PATH> -d \$web --account-name <ACCOUNT_NAME>
```

## <a name="deployment"></a>Dağıtım

İçerik için bir depolama kapsayıcısı dağıtmak için kullanılabilen yöntemler şunlardır:

- [AzCopy](../common/storage-use-azcopy.md)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
- [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/)
- [Visual Studio Code uzantısı](https://code.visualstudio.com/tutorials/static-website/getting-started)

Her durumda, dosyaları kopyalamak emin *$web* kapsayıcı.

## <a name="metrics"></a>Ölçümler

Statik Web sitesi sayfalarınıza ölçümleri etkinleştirmek üzere tıklayın **ayarları** > **izleme** > **ölçümleri**.

Ölçüm verilerini farklı ölçümlerinde API'leri takma tarafından oluşturulur. Portal, yalnızca belirli bir zaman dilimi içinde yalnızca veri döndüren üyelerde odaklanabilmek için kullanılan API üyelerini görüntüler. Gerekli API üye seçebilir emin olmak için zaman aralığını genişletmek için ilk adım olacaktır.

Zaman çerçevesini düğmesine tıklayıp **son 24 saat** ve ardından **Uygula**

![Azure depolama statik Web siteleri ölçümleri zaman aralığı](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

Ardından, **Blob** gelen *Namespace* açılır.

![Azure depolama statik Web siteleri ölçümleri ad alanı](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

Ardından **çıkış** ölçümü.

![Azure depolama statik Web siteleri ölçümleri ölçüm](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

Seçin **toplam** gelen *toplama* Seçici.

![Azure depolama statik Web siteleri ölçümleri toplama](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

Ardından, **Filtre Ekle** düğmesini tıklatın ve seçin **API adı** gelen *özelliği* Seçici.

![Azure depolama statik Web siteleri ölçümleri API adı](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

Son olarak, yanındaki kutuyu işaretleyin **GetWebContent** içinde *değerleri* ölçümleri raporu doldurmak için Seçici.

![Azure depolama statik Web siteleri ölçümleri GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

Etkinleştirildikten sonra dosyalarda istatistikleri trafiği *$web* kapsayıcı ölçümleri panosunda bildirilir.

## <a name="faq"></a>SSS

**Statik Web siteleri özelliği, tüm depolama hesap türleri için kullanılabilir mi?**  
Hayır, statik Web sitesi barındırma, yalnızca GPv2 standart depolama hesaplarında kullanılabilir.

**Depolama VNET ve yeni web uç noktada desteklenen güvenlik duvarı kuralları?**  
Evet, yeni web uç noktası, depolama hesabı için yapılandırılmış sanal ağ ve güvenlik duvarı kuralları ilişkiden.

**Web uç noktası, büyük/küçük harf duyarlıdır?**  
Evet, web uç noktası yalnızca blob uç noktası gibi büyük küçük harfe duyarlıdır.

**Web uç noktası, HTTP ve HTTPS erişilebilir?**
Evet, web uç noktası hem HTTP hem de HTTPS erişilebilir. Ancak, depolama hesabı, HTTPS üzerinden güvenli aktarım gerektir için yapılandırılmışsa, kullanıcıların HTTPS uç noktasını kullanmanız gerekir. Daha fazla bilgi için [Azure depolamadaki güvenli aktarım gerektir](../common/storage-require-secure-transfer.md).

## <a name="next-steps"></a>Sonraki adımlar
* [HTTPS üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Blob veya web uç noktanız için özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [İlk sunucusuz web uygulamanızı oluşturun](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Öğretici: Etki alanınızı Azure DNS'de barındırın](../../dns/dns-delegate-domain-azure-dns.md)
