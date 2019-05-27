---
title: 'Öğretici: SSL ile özel etki alanını Azure CDN - Azure depolama kullanarak statik Web sitesi üzerinde etkinleştirmek'
description: Statik Web sitesi barındırmak için özel bir etki alanı yapılandırma konusunda bilgi edinin.
services: storage
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: normesta
ms.reviewer: seguler
ms.custom: seodec18
ms.openlocfilehash: 2b0bb94be2ba8ea983cda8fd015d05fcd532f2bc
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226164"
---
# <a name="tutorial-use-azure-cdn-to-enable-a-custom-domain-with-ssl-for-a-static-website"></a>Öğretici: Statik bir Web sitesi için SSL ile özel etki alanı etkinleştirmek için Azure CDN'yi kullanma

Bu öğretici, bir dizinin ikinci bölümüdür. İçinde statik bir Web siteniz için SSL ile bir özel etki alanı endpoint etkinleştirmeyi öğrenin. 

Öğreticinin nasıl kullanılacağını gösterir [Azure CDN](../../cdn/cdn-overview.md) özel etki alanı için uç nokta statik Web sitenizi yapılandırmak için. Azure CDN ile özel SSL sertifikalarını sağlama, özel bir etki alanı kullanın ve tümünü aynı anda özel yeniden yazma kuralları yapılandırın. Azure CDN yapılandırma ek ücretlere neden olur, ancak tutarlı düşük gecikme süreleriyle Web sitenize yerden sağlar dünyanın. Azure CDN, ayrıca kendi sertifikanızı ile SSL şifrelemesi sağlar. Azure CDN fiyatlandırması hakkında daha fazla bilgi için bkz: [Azure CDN fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/).

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Statik Web sitesi uç noktada bir CDN uç noktası oluşturma
> * Özel etki alanı ve SSL etkinleştir

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce bir bölümü tamamlayın [Öğreticisi: Blob Depolama üzerinde statik Web sitesi barındırma](storage-blob-static-website-host.md). 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Oturum [Azure portalında](https://portal.azure.com/) kullanmaya başlamak için.

## <a name="create-a-cdn-endpoint-on-the-static-website-endpoint"></a>Statik Web sitesi uç noktada bir CDN uç noktası oluşturma

1. Azure portalında depolama hesabınızı bulun ve hesabına genel bakış görüntüler.
1. Seçin **Azure CDN** altında **Blob hizmeti** Azure CDN yapılandırmak için menüsünde.
1. İçinde **CDN profili** bölümünde, yeni veya var olan bir CDN profili belirtin. Daha fazla bilgi için [hızlı başlangıç: Bir Azure CDN profili ve uç noktası oluşturma](../../cdn/cdn-create-new-endpoint.md).
1. CDN uç noktası için bir fiyatlandırma Katmanı'nı belirtin. Bu öğreticide **standart Akamai** fiyatlandırma katmanından olduğundan, hızlı bir şekilde, genellikle birkaç dakika içinde yayar. Diğer fiyatlandırma katmanlarından daha uzun sürebilir yayılması, ancak aynı zamanda diğer avantajlar sunabilir. Daha fazla bilgi için [karşılaştırma Azure CDN ürün özellikleri](../../cdn/cdn-features.md).
1. İçinde **CDN uç noktası adı** alanı, CDN uç noktanız için bir ad belirtin. CDN uç noktası Azure genelinde benzersiz olmalıdır.
1. Statik Web sitesi uç noktada işiniz belirtin **kaynak konak adı** alan. Statik Web sitesi uç noktanızı bulmak için gidin **statik Web sitesi** depolama hesabınız için ayarlar. Birincil uç noktaya kopyalayın ve protokolü tanımlayıcısı kaldırma CDN yapılandırmanın yapıştırın (*örn*, HTTPS).

    Aşağıdaki görüntüde bir örnek uç nokta yapılandırması gösterilmektedir:

    ![Ekran gösteren örnek CDN uç nokta yapılandırması](media/storage-blob-static-website-custom-domain/add-cdn-endpoint.png)

1. CDN uç noktası oluşturun ve yayması için bekleyin.
1. CDN uç noktası düzgün yapılandırıldığını doğrulamak için uç nokta ayarlarına giderek tıklayın. CDN'ye genel bakış depolama hesabınız için uç nokta ana bilgisayar adını bulun ve aşağıdaki görüntüde gösterildiği gibi uç noktasına gidin. CDN uç noktanıza biçimi şuna benzeyecektir `https://staticwebsitesamples.azureedge.net`.

    ![Ekran gösterme CDN uç noktası genel bakış](media/storage-blob-static-website-custom-domain/verify-cdn-endpoint.png)

    CDN uç noktayı yayma işlemi tamamlandıktan sonra CDN uç noktasına giderek daha önce statik Web sitenize karşıya index.html dosyanın içeriğini görüntüler.

1. CDN uç noktanız için başlangıç ayarlarını gözden geçirmek için gidin **kaynak** altında **ayarları** CDN uç noktanız için bölüm. Göreceksiniz **kaynak türü** ayarlanmış *özel kaynak* ve **kaynak konak adı** alan statik Web sitesi uç noktanızı görüntüler.

    ![CDN uç noktası için kaynak ayarları gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/verify-cdn-origin.png)

## <a name="enable-custom-domain-and-ssl"></a>Özel etki alanı ve SSL etkinleştir

1. Özel etki alanınızı CDN uç noktasına yönlendirmek için etki alanı adı sağlayıcınızla bir CNAME kaydı oluşturun. CNAME kaydı *www* alt etki alanı aşağıdakine benzer olmalıdır:

    ![Www alt etki alanı için CNAME kaydı belirtin](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. Azure portalında, CDN uç noktanız için ayarları görüntüleyin. Gidin **özel etki alanları** altında **ayarları** özel etki alanı ve SSL sertifikası yapılandırmak için.
1. Seçin **özel etki alanı Ekle** ve etki alanı adınızı girin ve ardından tıklayın **Ekle**.
1. Bir SSL sertifikasını sağlamak için yeni özel etki alanı eşlemesi'ni seçin.
1. Ayarlama **özel etki alanı HTTPS** için **ON**, ardından **Kaydet**. Bu özel etki alanınızı yapılandırmanız için birkaç saat sürebilir. Portal, aşağıdaki görüntüde gösterildiği gibi ilerleme durumunu görüntüler.

    ![Özel etki alanı yapılandırmanın ilerleme durumunu gösteren ekran görüntüsü](media/storage-blob-static-website-custom-domain/configure-custom-domain-https.png)

1. Özel etki alanınız için URL erişerek statik sitenize özel etki alanınızı eşleme test edin.

Özel etki alanları için HTTPS'yi etkinleştirme hakkında daha fazla bilgi için bkz. [Öğreticisi: Bir Azure CDN özel etki alanı üzerinde HTTPS yapılandırma](../../cdn/cdn-custom-ssl.md).

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu iki, statik Web siteniz için SSL Azure CDN ile özel bir etki alanı yapılandırma hakkında bilgi edindiniz.

Yapılandırma ve Azure CDN'yi kullanma hakkında daha fazla bilgi için bkz. [Azure CDN nedir?](../../cdn/cdn-overview.md).