---
title: Video Indexer hesabınız yönetme
titlesuffix: Azure Media Services
description: Bu makalede, Azure'a bağlı bir Video Indexer hesabın nasıl yönetileceği gösterilmektedir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 1515a026ae297a960f220a97449d2258c0b75e58
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553614"
---
# <a name="manage-a-video-indexer-account-connected-to-azure"></a>Azure'a bağlı bir Video Indexer hesabına yönetme

Bu makalede, Azure aboneliğinize bağlı bir Video Indexer hesabına ve bir Azure Media Services hesabına nasıl yönetileceği gösterilmektedir.

> [!NOTE]
> Hesap bu konuda tartışılan yapılandırma ayarlamalarını yapmanız Video Indexer hesap sahibi olmanız gerekir.

## <a name="prerequisites"></a>Önkoşullar

Video Indexer hesabınız açıklandığı gibi yalnızca Azure'a bağlanmak [Azure'a bağlı](connect-to-azure.md). 

Takip ettiğinizden emin olun [önkoşulları](connect-to-azure.md#prerequisites) ve gözden geçirme [konuları](connect-to-azure.md#considerations) makaledeki.

## <a name="examine-account-settings"></a>Hesap ayarları inceleyin

Bu bölüm, Video Indexer hesabınız ayarları inceler.

Ayarları görüntülemek için:

1. Sağ üst köşesinde kullanıcı simgesine tıklayıp **ayarları**.

    ![Ayarlar](./media/manage-account-connected-to-azure/select-settings.png)

2. Üzerinde **ayarları** sayfasında **hesabı** sekmesi.

Video Indexer hesabınız Azure'a bağlıysa, aşağıdakilere bakın:

* Temel alınan Azure Media Services hesabı adı.
* Dizin oluşturma işleri çalıştıran ve sıraya alınmış sayısı.
* Sayısı ve türü, ayrılmış ayrılmış birim.

Hesabınızda bazı ayarlamalar gerekiyorsa ilgili hatalar ve uyarılar hakkında Hesap yapılandırmanızı üzerinde görürsünüz **ayarları** sayfası. İletileri Azure portalında tam basamak değişiklik gerek duyduğunuz senaryolara bağlantılar içerir. Daha fazla bilgi için [hataları ve Uyarıları](#errors-and-warnings) aşağıdaki bölümü.

## <a name="auto-scale-reserved-units"></a>Otomatik ölçeklendirme ayrılmış birimleri

**Ayarları** sayfası otomatik ölçeklendirme, medya ayrılmış birimi (RU) olanak tanır. Seçenek ise **üzerinde**, RU sayısı ayırmak ve Video Indexer durdurur/RU'ları otomatik olarak başlar, emin olun. Bu seçenek, boşta kalma süresi fazla para ödemeyin ancak dizin oluşturma işleri dizin oluşturma yükü yüksek olduğunda uzun tamamlanması için de beklemez.

Otomatik ölçeklendirme, 1 ölçeklendirme değil RU veya Media Services hesabı varsayılan sınırının üzerinde. Sınırı artırmak için bir hizmet isteği oluşturun. Kotalar ve sınırlamalar ve Destek bileti açmak nasıl hakkında daha fazla bilgi için bkz. [kotaları ve sınırlamaları](../../media-services/previous/media-services-quotas-and-limitations.md).

![Kaydolma](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Hatalar ve uyarılar

Hesabınızda bazı ayarlamalar gerekiyorsa ilgili hatalar ve uyarılar, hesap yapılandırması hakkında gördüğünüz **ayarları** sayfası. İletileri Azure portalında tam basamak değişiklik gerek duyduğunuz senaryolara bağlantılar içerir. Bu bölüm, hata ve uyarı iletilerini hakkında daha fazla ayrıntı sağlar.

* Event Grid

    Azure portalını kullanarak EventGrid kaynak sağlayıcısını kaydetmeniz gerekir. İçinde [Azure portalında](https://portal.azure.com/)Git **abonelikleri** > [. abonelik] > **ResourceProviders** > **Microsoft.EventGrid**. Değilse de **kayıtlı** durumu, tıklayın **kaydetme**. Bu işlem birkaç dakika kaydedilecek götürür. 

* Akış Uç Noktası

    Varsayılan temel Media Services hesabı olduğundan emin olun **akış uç noktası** başlatılmış bir durumda. Aksi takdirde, bu Media Services hesabı veya Video Indexer videoları mümkün olmayacaktır.

* Medya Ayrılmış Birimleri 

    Medya ayrılmış birimleri, medya hizmeti kaynağına dizin videolara sırayla ayırmalısınız. Dizin oluşturma en iyi performans için en az 10 S3 ayrılmış birimi ayırmak önerilir. Fiyatlandırma bilgileri için bkz. SSS bölümünü [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfası.   

## <a name="next-steps"></a>Sonraki adımlar

Program aracılığıyla, deneme hesabıyla ve/veya'ndaki yönergeleri takip ederek azure'a bağlı Video Indexer hesaplarınızı ile etkileşim kurabilirsiniz: [API'leri kullanan](video-indexer-use-apis.md).

Azure'a bağlanırken kullandığınız aynı Azure AD kullanıcı kullanmanız gerekir.
