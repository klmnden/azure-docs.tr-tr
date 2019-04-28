---
title: 'Öğretici: SSL ile özel etki alanını Azure CDN - Azure depolama kullanarak statik Web sitesi üzerinde etkinleştirmek'
description: Statik Web sitesi barındırmak için özel bir etki alanı yapılandırma konusunda bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: tamram
ms.custom: seodec18
ms.openlocfilehash: 6ccd33805fe4b62d3456121321edc4eec3bff2e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61427526"
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

1. Açık [Azure portalında](https://portal.azure.com/) web tarayıcınızda. 
1. Depolama hesabınızı bulun ve hesabına genel bakış görüntüler.
1. Seçin **Azure CDN** altında **Blob hizmeti** Azure CDN yapılandırmak için menüsünde.
1. İçinde **yeni uç nokta** bölümünde, yeni bir CDN uç noktası oluşturmak için alanları doldurun.
1. Bir uç nokta adı gibi girin *mystaticwebsiteCDN*.
1. Web sitesi etki alanınızı CDN uç noktanız için ana bilgisayar girin.
1. Kaynak konak adı için girin, statik Web sitesi uç nokta demektir. Statik Web sitesi uç noktanızı bulmak için gidin **statik Web sitesi** bölümünde depolama hesabınız için ve uç noktasını kopyalayın. 
1. CDN uç noktanıza giderek test *mywebsitecdn.azureedge.net* tarayıcınızda.

## <a name="enable-custom-domain-and-ssl"></a>Özel etki alanı ve SSL etkinleştir

1. Özel etki alanınızı CDN uç noktasına yönlendirmek için etki alanı adı sağlayıcınızla bir CNAME kaydı oluşturun. CNAME kaydı *www* alt etki alanı aşağıdakine benzer olmalıdır:

    ![Www alt etki alanı için CNAME kaydı belirtin](media/storage-blob-static-website-custom-domain/subdomain-cname-record.png)

1. Azure portalında, özel etki alanı ve SSL sertifikası yapılandırmak için yeni oluşturulan uç noktasına tıklayın.
1. Seçin **özel etki alanı Ekle** ve etki alanı adınızı girin ve ardından tıklayın **Ekle**.
1. Bir SSL sertifikasını sağlamak için yeni oluşturulan özel etki alanı seçin.
1. Ayarlama **özel etki alanı HTTPS** için **ON**. Seçin **CDN yönetilen** Azure CDN SSL sertifikanızı yönetmek için. **Kaydet**’e tıklayın.
1. Web sitesi URL'niz erişerek Web sitenizi test edin.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu iki, statik Web siteniz için SSL Azure CDN ile özel bir etki alanı yapılandırma hakkında bilgi edindiniz.

Azure depolama üzerinde statik Web sitesi barındırma hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Statik Web siteleri hakkında daha fazla bilgi edinin](storage-blob-static-website.md)
