---
title: Azure depolama (Önizleme) statik Web sitesi barındırma | Microsoft Docs
description: Azure depolama artık (Önizleme) barındırma, modern web uygulamalarını barındırmak için uygun maliyetli ve ölçeklenebilir bir çözüm sağlayarak statik Web sitesi sunar.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 08/17/18
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 65a1cd85baf18ac1f0d193e7e6d6c3139919fb59
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617406"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Azure depolama (Önizleme) statik Web sitesi barındırma
Azure depolama artık (Önizleme) barındırma, Azure üzerinde uygun maliyetli ve ölçeklenebilir, modern web uygulamaları dağıtmanıza olanak sağlayan statik Web sitesi sunar. Statik içerik ve JavaScript veya diğer istemci tarafı kod, statik bir Web sitesinde Web sayfalarını içerir. Bunun tersine, dinamik Web siteleri sunucu tarafı kodu bağlıdır ve kullanarak barındırılan [Azure Web Apps](/azure/app-service/app-service-web-overview).

Web içerik sunucu yönetimine gerek kalmadan sağlama olanağı, esnek ve ekonomik modelleri doğru dağıtımları kaydırma gibi kritik öneme sahiptir. Azure Depolama'da statik Web sitesi barındırma da başlamasıyla bu ile sunucusuz mimarileri yararlanarak zengin bir arka uç özellikleri etkinleştirme mümkün kılar [Azure işlevleri](/azure/azure-functions/functions-overview) ve diğer PaaS Hizmetleri.

## <a name="how-does-it-work"></a>Nasıl çalışır?
Depolama hesabınızı statik Web sitelerini etkinleştirdiğinizde, yeni bir web hizmeti uç noktası biçiminde oluşturulur `<account-name>.<zone-name>.web.core.windows.net`.

Web hizmeti uç noktası her zaman anonim okuma erişimi verir, biçimlendirilmiş HTML sayfalarını hizmet hatalara yanıt verir ve yalnızca okuma işlemlerini nesnesi sağlar. Web hizmeti uç noktası, hem kök ve alt dizinlerdeki istenilen dizinde bulunan dizin belgeyi döndürür. Depolama hizmeti bir 404 hatası döndürdüğünde, yapılandırdığınız bir özel hata belge web uç noktası döndürür.

Statik Web siteniz için içerik "$web" adlı özel bir kapsayıcı içinde barındırılır. Zaten yoksa, etkinleştirme işleminin bir parçası olarak "$web" sizin için oluşturulur. Web uç noktası kullanarak hesap kökünde "$web" içeriğinde erişilebilir. Örneğin `https://contoso.z4.web.core.windows.net/` $web kök dizininde bu ada sahip bir belge zaten varsa, Web siteniz için yapılandırdığınız dizin belgeyi döndürür.

Web sitenize içeriği karşıya yüklerken, blob depolama uç noktasını kullanın. 'Hesap kök dizininde erişilebilir image.jpg' adlı bir blob karşıya yüklemek için şu URL'yi kullanın `https://contoso.blob.core.windows.net/$web/image.jpg`. Karşıya yüklenen görüntüyü karşılık gelen web uç noktasında bir web tarayıcısında görüntülenebilen `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Özel etki alanı adları
Özel bir etki alanı, web içeriğini barındırmak için kullanabilirsiniz. Bunu yapmak için yönergeleri izleyin. [Azure depolama hesabınız için bir özel etki alanı adı yapılandırma](storage-custom-domain-name.md). HTTPS üzerinden özel etki alanı barındırılan Web sitenize güvenle erişin için bkz: [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md). CDN'NİZİN blob uç noktası yerine web uç noktası ve CDN yapılandırma anında, gerçekleşmez içeriğinizi görülebilir birkaç dakika beklemeniz gerekebilir. Bu nedenle unutmayın.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Statik Web sitesi barındırma, ek ücret alınmadan sağlanır. Azure Blob Depolama fiyatları hakkında daha fazla ayrıntı için kullanıma [Azure Blob Depolama fiyatlandırması sayfasını](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Hızlı Başlangıç
### <a name="azure-portal"></a>Azure portal
Henüz kaydolmadıysanız [GPv2 depolama hesabı oluşturma](../common/storage-quickstart-create-account.md) web uygulamanızı barındırmaya başlamak için Azure portalını kullanarak özellik yapılandırabilir ve sol gezinti çubuğunda "Ayarlar" altında "Statik Web sitesinde (Önizleme)"'a tıklayın. "Etkin"'i tıklatın ve dizin belge ve (isteğe bağlı) özel hata belgesi yolu adını girin.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Statik Web sitesi etkinleştirme bir parçası oluşturulmuş "$web" kapsayıcı web varlıklarınızı yükleyin. Bunu doğrudan Azure Portalı'nda yapabilirsiniz veya avantajlarından yararlanabilirsiniz [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) tüm dizin yapılarını yüklenecek. Bir dizin belgesi yapılandırdığınız adla eklediğinizden emin olun. Bu örnekte, belgenin "index.html" dir.

> [!NOTE]
> Belge adı büyük/küçük harfe duyarlıdır ve bu nedenle depolamadaki dosyanın adı tam olarak eşleşmesi gerekir.

Son olarak, Web sitenizi test etmek için web bitiş noktasına gidin.

### <a name="azure-cli"></a>Azure CLI
Depolama Önizleme uzantıyı yükleyin:

```azurecli-interactive
az extension add --name storage-preview
```
Özelliği etkinleştirin:

```azurecli-interactive
az storage blob service-properties update --account-name <account-name> --static-website --404-document <error-doc-name> --index-document <index-doc-name>
```
Sorgu için web uç noktası URL'si:

```azurecli-interactive
az storage account show -n <account-name> -g <resource-group> --query "primaryEndpoints.web" --output tsv
```

Nesneleri $web kapsayıcısına yükleyin:

```azurecli-interactive
az storage blob upload-batch -s deploy -d $web --account-name <account-name>
```

## <a name="faq"></a>SSS
**Statik Web siteleri için tüm depolama hesabı türleri kullanılabilir mi?**  
Hayır, statik Web sitesi barındırma, yalnızca GPv2 standart depolama hesaplarında kullanılabilir.

**Depolama VNET ve yeni web uç noktada desteklenen güvenlik duvarı kuralları?**  
Evet, yeni web uç noktası, depolama hesabı için yapılandırılmış sanal ağ ve güvenlik duvarı kuralları ilişkiden.

**Web uç noktası, büyük küçük harfe duyarlı mi?**  
Evet, web uç noktası yalnızca blob uç noktası gibi büyük küçük harfe duyarlı. 

## <a name="next-steps"></a>Sonraki adımlar
* [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Blob veya web uç noktanız için özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure/azure-functions/functions-overview)
* [Azure Web Apps](/azure/app-service/app-service-web-overview)
* [İlk sunucusuz web uygulamanızı oluşturun](https://aka.ms/static-serverless-webapp)
