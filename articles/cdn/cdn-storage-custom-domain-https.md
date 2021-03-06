---
title: Öğretici - Depolama bloblarına HTTPS üzerinden Azure CDN özel etki alanı kullanarak erişme | Microsoft Docs
description: ''
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: e3b10760b95662570c8a6e802a57e1073e2faa0a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593377"
---
# <a name="tutorial-access-storage-blobs-using-an-azure-cdn-custom-domain-over-https"></a>Öğretici: HTTPS üzerinden bir Azure CDN özel etki alanını kullanarak depolama blob'lara erişim

Azure depolama hesabınızı Azure Content Delivery Network (CDN) ile tümleştirdikten sonra, özel bir etki alanı ekleyebilir ve özel blob depolama uç noktanız için bu etki alanında HTTPS’yi etkinleştirebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticideki adımları tamamlamadan önce, ilk olarak Azure depolama hesabınızı Azure CDN ile tümleştirmeniz gerekir. Daha fazla bilgi için [hızlı başlangıç: Bir Azure depolama hesabını Azure CDN ile tümleştirme](cdn-create-a-storage-account-with-cdn.md).

## <a name="add-a-custom-domain"></a>Özel etki alanı ekleme
Profilinizde bir CDN uç noktası oluşturduğunuzda, varsayılan olarak CDN içeriği sunmak için URL’nize azureedge.net adresinin alt etki alanı olan uç nokta adı eklenir. Bir CDN uç noktası ile özel etki alanını ilişkilendirme seçeneğiniz de vardır. Bu seçeneği kullanarak URL’nizde bir uç nokta adı yerine özel etki alanı ile içerik sunabilirsiniz. Uç noktanıza özel etki alanı eklemek için bu öğreticideki yönergeleri izleyin: [Azure CDN uç noktanıza özel etki alanı ekleme](cdn-map-content-to-custom-domain.md).

## <a name="configure-https"></a>HTTPS’yi yapılandırma
Özel etki alanınızda HTTPS protokolünü kullanarak, verilerinizin TLS/SSL şifrelemesi aracılığıyla güvenli bir şekilde teslim edilmesini sağlarsınız. Web tarayıcınız HTTPS üzerinden bir web sitesine bağlanırken, web sitesinin güvenlik sertifikasını doğrular ve bu sertifikanın yasal bir sertifika yetkilisi tarafından verildiğini doğrular. Özel etki alanınızda HTTPS yapılandırmak için bu öğreticideki yönergeleri izleyin: [Bir Azure CDN özel etki alanı üzerinde HTTPS yapılandırma](cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Paylaşılan Erişim İmzaları
Blob depolama uç noktanız anonim okuma erişimini engellemek için yapılandırılmışsa, özel etki alanınıza yaptığınız her istekte bir [Paylaşılan Erişim İmzası (SAS)](cdn-sas-storage-support.md) belirteci sağlamanız gerekir. Varsayılan olarak, blob depolama uç noktaları anonim okuma erişimini engeller. SAS hakkında daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../storage/blobs/storage-manage-access-to-resources.md).

Azure CDN SAS belirtecine eklenen kısıtlamaları yok sayar. Örneğin, tüm SAS belirteçlerinin bir sona erme zamanı vardır, yani içerik CDN varlık noktası (POP) sunucularından temizlenene kadar içeriğe süresi dolmuş bir SAS ile hala erişilebilir. Önbellek yanıtı üst bilgisini ayarlayarak verilerin Azure CDN üzerinde ne kadar süreyle önbelleğe alınacağını denetleyebilirsiniz. Daha fazla bilgi için, bkz. [Azure CDN’de Azure Depolama bloblarının sona erme tarihini yönetme](cdn-manage-expiration-of-blob-content.md).

Aynı blob uç noktası için birden çok SAS URL’si oluşturursanız, sorgu dizesi önbelleğe almayı göz önünde bulundurun. Bunu yapmak her URL’nin benzersiz bir varlık olarak kabul edilmesini sağlar. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](cdn-query-string.md).

## <a name="http-to-https-redirection"></a>HTTP’den -HTTPS’ye yeniden yönlendirme
[Azure CDN kuralları altyapısı](cdn-verizon-premium-rules-engine.md) ile bir [URL Yeniden Yönlendirme kuralı](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect) oluşturarak HTTP trafiğini HTTPS’ye yeniden yönlendirebilirsiniz. Bu seçenek bir **Verizon’dan Azure CDN Premium** profili gerektirir.

![URL yeniden yönlendirme kuralı](./media/cdn-storage-custom-domain-https/cdn-url-redirect-rule.png)

Bu kuralda, *Cdn-endpoint-name* CDN uç noktanız için yapılandırdığınız, açılır listeden seçebileceğiniz addır. *origin-path* değeri statik içeriğinizin bulunduğu kaynak depolama hesabı içindeki yola başvurur. Tüm statik içeriği tek bir kapsayıcıda barındırıyorsanız, *origin-path* değerini bu kapsayıcının adıyla değiştirin.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Azure CDN üzerinden bloblara eriştiğinizde, POP sunucuları ve kaynak (Blob depolama) arasında trafik için [Blob depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) ve POP sunucularından erişilen veriler için [Azure CDN fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/) üzerinden ücret ödersiniz.

Örneğin, ABD’de Azure CDN kullanılarak erişilen bir depolama hesabınız varsa ve Avrupa’da bulunan biri Azure CDN üzerinden bu depolama hesabına erişmeye çalışırsa, Azure CDN önce bu blob için Avrupa’ya en yakın POP sunucusunu denetler. Bulunursa, Azure CDN blobun bu kopyasına erişir ve Azure CDN üzerinden erişildiği için CDN fiyatlandırmasını kullanır. Bulunamazsa, Azure CDN blobu POP sunucusuna kopyalar (bu işlem Blob depolama fiyatlandırmasında belirtilen çıkış ve işlem ücretlerine tabidir) ve ardından POP sunucusu üzerinde dosyaya erişir (bu işlem Azure CDN faturalandırmasıyla sonuçlanır).

## <a name="next-steps"></a>Sonraki adımlar
[Öğretici: Azure CDN önbelleğe alma kuralları ayarlayın](cdn-caching-rules-tutorial.md)




