---
title: Azure Storage (Önizleme) statik Web sitesi barındırma | Microsoft Docs
description: Azure Storage şimdi (Önizleme) barındırma, modern web uygulamalarını barındırmak için uygun maliyetli ve ölçeklenebilir bir çözüm sağlayarak statik Web sitesi sunar.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 06/26/18
ms.author: mihauss
ms.openlocfilehash: df1661b5fe7a2c0e37deef5259d6b5842ed6ee5e
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131618"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Azure Storage (Önizleme) statik Web sitesi barındırma
Azure Storage şimdi (Önizleme) barındırma, Azure üzerinde uygun maliyetli ve ölçeklenebilir modern web uygulamalarını dağıtmasına olanak sağlayarak statik Web sitesi sunar. Statik bir Web sitesinde, statik içerik ve JavaScript veya başka bir istemci-tarafı kodu Web sayfalarını içerir. Bunun aksine, dinamik Web siteleri sunucu tarafı kodu bağlıdır ve kullanarak barındırılan [Azure Web Apps](/app-service/app-service-web-overview.md).

Esnek, düşük maliyetli modelleri doğru dağıtımları shift gibi web içeriği sunucu yönetimi gerek kalmadan ulaştırmak için kritik yeteneğidir. Azure depolama alanında statik Web sitesi barındırma giriş bu yararlanarak sunucusuz mimarileri ile zengin arka uç özellikleri etkinleştirme mümkün kılar [Azure işlevleri](/azure-functions/functions-overview.md) ve diğer PaaS Hizmetleri.

## <a name="how-does-it-work"></a>Nasıl çalışır?
Depolama hesabınıza statik Web sitelerine etkinleştirdiğinizde, yeni bir web hizmeti uç noktası şeklinde oluşturulur `<account-name>.<zone-name>.web.core.windows.net`.

Web hizmeti uç noktası her zaman anonim okuma erişimini sağlar, hizmet hatalarına yanıt olarak biçimlendirilmiş HTML sayfaları döndürür ve yalnızca okuma işlemleri nesne sağlar. Web hizmeti uç noktası dizini belgesinde kök ve tüm alt dizinler için istenen dizin döndürür. Depolama hizmeti 404 hatası döndürdüğünde, yapılandırdıysanız web uç noktası bir özel hata belgeyi döndürür.

Statik Web siteniz için içerik "$web" adlı özel bir kapsayıcıda barındırılır. Zaten yoksa, etkinleştirme işleminin parçası olarak, "$web" sizin için oluşturulur. "$Web" içeriğinde hesap kök web uç noktası kullanılarak erişilebilir. Örneğin `https://contoso.z4.web.core.windows.net/` bu adda bir belge $web kök dizininde bulunuyorsa, Web siteniz için yapılandırdığınız dizin belgeyi döndürür.

İçerik Web sitenize yüklemek, blob storage uç kullanın. 'Hesap kök dizininde erişilebilir image.jpg' adlı bir blob karşıya yüklemek için aşağıdaki URL'yi kullanın `https://contoso.blob.core.windows.net/$web/image.jpg`. Karşıya yüklenen görüntü karşılık gelen web uç noktada bir web tarayıcısında görüntülenebilen `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Özel etki alanı adları
Web içeriğini barındırmak için özel bir etki alanı kullanabilirsiniz. Bunu yapmak için yönergeleri [Azure depolama hesabınız için bir özel etki alanı adı yapılandırma](storage-custom-domain-name.md). HTTPS üzerinden bir özel etki alanı adına barındırılan Web sitenizi erişmek için bkz: [BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için Azure CDN kullanarak](storage-https-custom-domain-cdn.md).

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Statik Web sitesi barındırma hiçbir ek ücret sağlanır. Azure Blob Storage fiyatları hakkında daha fazla ayrıntı için kullanıma [Azure Blob Depolama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Hızlı Başlangıç
### <a name="azure-portal"></a>Azure portalına
Web uygulamanızda Azure Storage barındırma başlatmak için Azure Portalı'nı kullanarak özellik yapılandırabilir ve sol gezinti çubuğunda "Ayarlar" altında "Statik Web sitesinde (Önizleme)"'i tıklatın. "Etkin"'i tıklatın ve dizin belge ve (isteğe bağlı) özel hata belge yol adını girin.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Statik Web sitesi etkinleştirme bir parçası oluşturulmuş "$web" kapsayıcısı web varlıklarınızı yükleyin. Bunu doğrudan Azure Portalı'nda yapabilirsiniz veya özelliklerden yararlanabilirsiniz [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/) tüm dizin yapıları karşıya yüklemek için. Bir dizini belgesinde yapılandırdığınız adla eklediğinizden emin olun. Bu örnekte, belgenin "index.html" adıdır.

> [!NOTE]
> Belge adı büyük küçük harfe duyarlıdır ve bu nedenle depolama dosyasının adı tam olarak eşleşmesi gerekiyor.

Son olarak, Web sitenizi test etmek için web bitiş noktasına gidin.

## <a name="faq"></a>SSS
**Statik Web sitelerine tüm depolama hesap türleri için kullanılabilir mi?**  
Hayır, statik Web sitesi barındırma yalnızca GPv2 standart depolama hesapları kullanılabilir.

**Depolama VNET ve yeni web uç noktası üzerinde desteklenen güvenlik duvarı kurallarını misiniz?**  
Evet, yeni web uç noktası depolama hesabı için yapılandırılan sanal ağ ve güvenlik duvarı kuralları obeys.

## <a name="next-steps"></a>Sonraki adımlar
* [BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Blob veya web uç noktanız için özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure-functions/functions-overview.md)
* [Azure Web Apps](/app-service/app-service-web-overview.md)
* [İlk sunucusuz web uygulamanızı oluşturun](https://aka.ms/static-serverless-webapp)
