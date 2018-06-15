---
title: Azure için bağlı bir Video dizin oluşturucu hesabı oluşturma | Microsoft Docs
description: Bu makalede, Azure için bağlı bir Video dizin oluşturucu hesabının nasıl oluşturulacağı gösterilmektedir.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: ac9093d41a2e70905ea82c6d11f020696488ff27
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355865"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Azure için bağlı bir Video dizin oluşturucu hesabı oluşturun

Video dizin oluşturucu ücretsiz bir deneme hesabı kullanırken, sayısız dizinini oluşturabilir ve kota sınırlı. Bu makalede, bu sınırlamaları uygulanmaya boşaltır ve Kullandıkça Öde fiyatlandırma kullanan bir Azure aboneliği için bağlantılı bir Video dizin oluşturucu hesabının nasıl oluşturulacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. 

    Henüz Azure aboneliğiniz yoksa kaydolmak [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

* Bir Azure Active Directory (AD) etki alanı. 

    Azure AD etki alanı yoksa, bu etki alanı Azure aboneliğinizle oluşturun.

* Kullanıcı ve Azure AD etki alanınızdaki üye. Bu üye Video dizin oluşturucu hesabınız için Azure bağlanırken kullanacaksınız.

    Bu kullanıcının bu ölçütleri karşılamalıdır:

    * Bir iş veya Okul hesabı, kişisel hesabı değil, outlook.com, live.com veya hotmail.com gibi bir Azure AD kullanıcısının olabilir.
        
        ![Tüm AAD kullanıcılar](./media/create-account/all-aad-users.png)

    *  Sahip rolü veya katkıda bulunan ve kullanıcı erişimi yöneticisi rolleri Azure aboneliğinizle üyesi olabilir.

        ![Erişim denetimi](./media/create-account/access-control-iam.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma

1. Oturum, kullanıcı oturum ve tıklayın **Azure Bağlan** düğmesi:

    ![Azure'a bağlanma](./media/create-account/connect-to-azure.png)

2. Abonelik listesi göründüğünde, kullanmak istediğiniz aboneliği seçin. 

    ![Video dizin oluşturucu için Azure connect](./media/create-account/connect-vi-to-azure-subscription.png)

3. Desteklenen konumlardan bir Azure bölgesi seçin: 2 Batı ABD, Kuzey Avrupa veya Doğu Asya.
4. Altında **Azure Media Services hesabı**, aşağıdaki seçeneklerden birini seçin:

    * Yeni bir Media Services hesabı oluşturmak için seçin **yeni kaynak grubu oluştur**. Kaynak grubunuz için bir ad sağlayın.

        Azure, aboneliğinizde yeni bir Azure depolama hesabı dahil olmak üzere, yeni hesap adınız oluşturur. Yeni bir Media Services hesap adınız varsayılan ilk yapılandırmayla bir akış uç noktası ve 10 S3 ayrılan birimler sahiptir.
    * Varolan bir Media Services hesabı kullanmak için **olan kaynağı kullanın**. Hesapları listesinden hesabınızı seçin.

        Media Services hesabınızı Video dizin oluşturucu hesabınızı aynı bölgede olması gerekir. Dizin oluşturma süresi ve düşük verimliliği en aza indirmek için ayrılmış birim sayısını ve türünü ayarlamak **10 S3 ayrılan birimler** , Media Services hesabı.
    * El ile bağlantınızı yapılandırmak için tıklatın **geçiş için el ile yapılandırma** bağlamak ve gerekli bilgileri sağlayın:

    ![Video dizin oluşturucu için Azure connect](./media/create-account/connect-vi-to-azure-subscription-2.png)

5. İşiniz bittiğinde seçin **Bağlan**. Bu işlem birkaç dakika sürebilir. 

    Azure'a bağlandıktan sonra yeni Video dizin oluşturucu hesabınız hesap listede görünür:

    ![Yeni hesabı](./media/create-account/new-account.png)

6. Yeni hesabınız göz atın: 

    ![Video dizin oluşturucu hesabı](./media/create-account/vi-account.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki Azure Media Services ilgili önemli noktalar geçerlidir:

* Yeni bir Media Services hesabına bağladıysanız, Azure aboneliğinizde yeni bir kaynak grubu, Media Services hesabı ve bir depolama hesabı görürsünüz.
* Yeni bir Media Services hesabına bağladıysanız, Video dizin oluşturucu medya kümesi **ayrılan birimler** 10 S3 birimleri için:

    ![Media Services ayrılan birimler](./media/create-account/ams-reserved-units.png)

* Varolan bir Media Services hesabı bağladıysanız, var olan medya Video dizin oluşturucu değiştirmez **ayrılan birimler** yapılandırma.

    Medya sayısını ve türünü ayarlamak gerekebilecek **ayrılan birimler**, planlı yük göre. Yük yüksek ise ve yeterli birimleri veya hızı yoksa, işleme videolar zaman aşımı hatalarına neden olduğunu aklınızda bulundurun.

* Yeni bir Media Services hesabına bağladıysanız, Video dizin oluşturucu otomatik olarak başlayan bir **akış uç noktası** da:

    ![Media Services akış uç noktası](./media/create-account/ams-streaming-endpoint.png)

* Varolan bir Media Services hesabı bağladıysanız, Video dizin oluşturucu akış uç noktalarını yapılandırma değiştirmez. Hiçbir çalışan varsa **akış uç noktası**, bu Media Services hesabı veya Video Oluşturucudaki videoları izleyin mümkün olmaz.

## <a name="use-video-indexer-apis-v2"></a>Video dizin oluşturucuyu kullanın API v2

Program aracılığıyla deneme hesabınızla ve/veya'ndaki yönergeleri izleyerek azure'a bağlı Video dizin oluşturucu hesaplarınızı ile etkileşim kurabilirsiniz: [kullanım API'leri](video-indexer-use-apis.md).

Azure'a bağlanırken kullanılan aynı Azure AD kullanıcısının kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Çıkış JSON ayrıntılarını inceleyin](video-indexer-output-json-v2.md).

