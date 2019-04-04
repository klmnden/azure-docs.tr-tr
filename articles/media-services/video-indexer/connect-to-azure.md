---
title: Video Indexer hesabınız, Azure portalında oluşturma
titlesuffix: Azure Media Services
description: Bu makalede, Azure portalında bir Video Indexer hesabına oluşturulacağını gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 01/12/2019
ms.author: juliako
ms.openlocfilehash: affa6f9a808543401b7d57812c7d2bef4324a83c
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894226"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Azure'a bağlı bir Video Indexer hesabı oluşturun

Video Indexer hesabınızı oluştururken ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası elde edersiniz) veya ücretli bir seçenek (kota sınırlaması olmaz) arasından seçim yapabilirsiniz. Ücretsiz deneme kullanıldığında Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ve API kullanıcılarına ise 2400 dakikaya kadar ücretsiz dizin oluşturma olanağı sunar. Ücretli seçeneğiyle Azure aboneliğinize bağlı bir Video Indexer hesabınız ve Azure Media Services hesabı oluşturun. Dizin oluşturma faaliyeti yapılan dakika sayısının yanı sıra Medya Hesabı ile ilgili ücretler için ödeme yaparsınız. 

Bu makalede bir Azure aboneliğine bağlı bir Video Indexer hesabınız ve Azure Media Services hesabı oluşturma işlemini gösterir. Konu adımları Azure'a bağlanmak için otomatik (varsayılan) akışı kullanarak sağlar. Ayrıca el ile Azure'a bağlanmak nasıl gösterir (Gelişmiş).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği.

    Henüz Azure aboneliğiniz yoksa kaydolmak [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

* Bir Azure Active Directory (AD) etki alanı.

    Azure AD etki alanı yoksa, bu etki alanı, Azure aboneliğiniz ile oluşturun. Daha fazla bilgi için [Azure Active Directory'de özel etki alanı adlarını yönetme](../../active-directory/users-groups-roles/domains-manage.md)

* Kullanıcı ve Azure AD etki alanınızı üye. Bu üye, Video Indexer hesabınız Azure'a bağlanırken kullanacaksınız.

    Bu kullanıcının iş veya Okul hesabı, kişisel hesabı değil, outlook.com, live.com veya hotmail.com gibi bir Azure AD kullanıcısı olması gerekir.

    ![Tüm AAD kullanıcıları](./media/create-account/all-aad-users.png)

### <a name="additional-prerequisites-for-automatic-flow"></a>Otomatik bir akış için ek Önkoşullar

Kullanıcı ve Azure AD etki alanınızı üye. Bu üye, Video Indexer hesabınız Azure'a bağlanırken kullanacaksınız.

Bu kullanıcı Azure aboneliğinize ya da bir üyesi olmalıdır bir **sahibi** rolüne ya da her ikisini de **katkıda bulunan** ve **kullanıcı erişimi Yöneticisi** rolleri. Bir kullanıcının ile 2 rolü iki kez eklenebilir. Katkıda bulunan ve bir kez kullanıcı erişimi Yöneticisi ile bir kez.

![Erişim denetimi](./media/create-account/access-control-iam.png)

### <a name="additional-prerequisites-for-manual-flow"></a>El ile akış için ek Önkoşullar

Azure portalını kullanarak EventGrid kaynak sağlayıcısını kaydedin.

İçinde [Azure portalında](https://portal.azure.com/)Git **abonelikleri**[. abonelik] -> ->**ResourceProviders**. 

Arama **Microsoft.Media** ve **Microsoft.EventGrid**. Durumda değil "Kaydedildi" ise, tıklayın **kaydetme**. Bu işlem birkaç dakika kaydedilecek götürür.

![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.

2. Tıklayarak **yeni hesap oluşturma** düğmesi:

    ![Azure'a bağlanma](./media/create-account/connect-to-azure.png)

3. Abonelik listesi göründüğünde, kullanmak istediğiniz aboneliği seçin.

    ![Video Indexer'ı Azure'a bağlanma](./media/create-account/connect-vi-to-azure-subscription.png)

4. Bir Azure bölgesi desteklenen konumlardan seçin: Batı ABD 2, Kuzey Avrupa veya Güneydoğu Asya.
5. Altında **Azure Media Services hesabı**, aşağıdaki seçeneklerden birini seçin:

    * Yeni bir Media Services hesabı oluşturmak için Seç **yeni kaynak grubu oluştur**. Kaynak grubunuz için bir ad sağlayın.

        Azure, yeni bir Azure depolama hesabı dahil aboneliğinizde, yeni hesabı oluşturur. Varsayılan başlangıç yapılandırmasını akış uç noktası ve 10 S3 ayrılmış birim ile yeni Media Services hesabınız var.
    * Mevcut bir Media Services hesabını kullanmayı tercih **var olan kaynağı kullanın**. Hesapları listesinden hesabınızı seçin.

        Media Services hesabınızı, Video Indexer hesabınız ile aynı bölgede olması gerekir. 

        > [!NOTE]
        > Dizin oluşturma süresi ve aktarım hızının düşük olmasını en aza indirmek için tür ve sayıda ayarlamak için önerilir [ayrılmış birim](../previous/media-services-scale-media-processing-overview.md ) için **10 S3 ayrılmış birim** Media Services hesabı. Bkz: [ayrılmış birimlerini değiştirmek için portalı kullanma](../previous/media-services-portal-scale-media-processing.md).

    * Bağlantınız el ile yapılandırmak için tıklayın **el ile yapılandırmaya geçiş** bağlantı.

        Ayrıntılı bilgi için bkz. [Azure'a el ile bağlanmanız](#connect-to-azure-manually-advanced-option) izler (Gelişmiş seçenek) bölümü.
6. İşiniz bittiğinde seçin **Connect**. Bu işlem birkaç dakika sürebilir. 

    Azure'a bağlandıktan sonra yeni Video Indexer hesabınız hesap listesinde görüntülenir:

    ![Yeni hesap](./media/create-account/new-account.png)

7. Yeni hesabınıza gidin

## <a name="connect-to-azure-manually-advanced-option"></a>El ile Azure'a bağlanabilirsiniz (Gelişmiş Seçenekler)

Bir Azure bağlantısı başarısız olursa, el ile bağlanarak sorunu gidermek deneyebilirsiniz.

> [!NOTE]
> Aşağıdaki üç hesapları aynı bölgede olması önemle tavsiye edilir: Media Services hesabı, yanı sıra Azure depolama hesabı bağlandığınız Video Indexer hesabınız aynı Media Services hesabına bağlı.

### <a name="create-and-configure-a-media-services-account"></a>Bir Media Services hesabını oluşturma ve yapılandırma

1. Kullanım [Azure](https://portal.azure.com/) bölümünde anlatıldığı gibi bir Azure Media Services hesabı oluşturmak için portalı [hesap oluşturma](../previous/media-services-portal-create-account.md).

    Media Services hesabınız için bir depolama hesabı oluştururken **StorageV2** hesap türü için ve **coğrafi olarak yedekli (RGS)** çoğaltma alanlar için.

    ![Yeni AMS hesabının](./media/create-account/create-ams-account1.png)

    > [!NOTE]
    > Media Services kaynağı ve hesap adlarını not aldığınızdan emin olun. Sonraki bölümde yer alan adımlar için gerekir.

2. Tür ve sayıda ayarlamak [ayrılmış birim](../previous/media-services-scale-media-processing-overview.md ) için **10 S3 ayrılmış birim** oluşturduğunuz Media Services hesabı. Bkz: [ayrılmış birimlerini değiştirmek için portalı kullanma](../previous/media-services-portal-scale-media-processing.md).
3. Video Indexer web uygulamasında videolarınızı yürütebilirsiniz önce varsayılan başlangıç **akış uç noktası** yeni Media Services hesabı.

    Yeni medya hizmetleri hesabı **akış uç noktaları**. Akış uç noktası ve ENTER tuşuna Başlat'ı seçin.

    ![Yeni AMS hesabının](./media/create-account/create-ams-account2.png)

4. Video Indexer'ın ile Media Services API'sine kimliğini doğrulamak bir AD uygulaması oluşturulması gerekir. Azure AD kimlik doğrulama işlemini açıklanan aşağıdaki adımları Kılavuzu [Azure portalını kullanarak Azure AD kimlik doğrulamasını kullanmaya başlama](../previous/media-services-portal-get-started-with-aad.md):

    1. Yeni Media Services hesabı seçin **API erişimi**.
    2. Seçin [hizmet sorumlusu kimlik doğrulaması yöntemi](../previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).
    3. İstemci Kimliğini ve istemci gizli anahtarı açıklandığı alma [istemci Kimliğini ve istemci gizli anahtarını almak](../previous/media-services-portal-get-started-with-aad.md#get-the-client-id-and-client-secret) bölümü.

        Seçtikten sonra **ayarları**->**anahtarları**, ekleme **açıklama**, basın **Kaydet**, anahtar değeri ile doldurulur.

        Anahtarı süresi dolarsa Video Indexer desteğe anahtarını yenilemek için hesap sahibi gerekir.

        > [!NOTE]
        > Anahtar değeri ve uygulama kimliği aşağı aldığınızdan emin olun Sonraki bölümde yer alan adımlar için gerekir.

### <a name="connect-manually"></a>El ile bağlanma

İçinde **bağlanın, Video Indexer bir Azure aboneliğine** iletişim, [Video Indexer](https://www.videoindexer.ai/) sayfasında **el ile yapılandırmaya geçiş** bağlantı.

İletişim kutusunda aşağıdaki bilgileri sağlayın:

|Ayar|Açıklama|
|---|---|
|Video Indexer hesabının bölgesi|Video Indexer hesap bölgesi adı. Daha iyi performans ve düşük maliyetlerden için Azure Media Services kaynağınız ve Azure depolama hesabının bulunduğu bölge adını belirtmek için önemle tavsiye edilir. |
|Azure Active Directory (AAD) kiracısı|Azure AD kiracısı, örneğin "contoso.onmicrosoft.com" adı. Kiracı bilgileri, Azure portalından alınabilir. Sağ üst köşedeki oturum açan kullanıcı adının üzerine imleci yerleştirin. Sağındaki adını bulma **etki alanı**.|
|Abonelik Kimliği|Azure aboneliği altında bu bağlantının oluşturulması. Abonelik kimliği, Azure portalından alınabilir. Tıklayarak **tüm hizmetleri** sol bölme ve "abonelikler" arayın. Seçin **abonelikleri** ve istenen kimliği aboneliklerinizi listesinden seçin.|
|Azure Media Services kaynak grubu adı|Media Services hesabı oluşturduğunuz kaynak grubunun adı.|
|Medya hizmeti kaynak adı|Önceki bölümde oluşturduğunuz Azure Media Services hesabı adı.|
|Uygulama Kimliği|Önceki bölümde oluşturduğunuz Azure AD uygulama kimliği (ile belirtilen Media Services hesabı için izinler).|
|Uygulama Anahtarı|Önceki bölümde oluşturduğunuz Azure AD uygulama anahtarı. |

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki Azure Media Services ilgili önemli noktalar geçerlidir:

* Yeni bir kaynak grubu, Media Services hesabı ve depolama hesabı otomatik olarak bağlarsanız, Azure aboneliğinizde bakın.
* Otomatik olarak bağlarsanız, Video Indexer medya ayarlar **ayrılmış birim** 10 S3 birimleri:

    ![Media Services için ayrılmış birimleri](./media/create-account/ams-reserved-units.png)

* Video Indexer mevcut bir Media Services hesabına bağlanırsanız, var olan medya değiştirmez **ayrılmış birim** yapılandırma.

   Medya ayrılmış birimi sayısını ve türünü ayarlamak planlanan yük göre gerekebilir. Yük yüksektir ve yeterli birimleri veya hızı yoksa, video işleme zaman aşımı hataları neden göz önünde bulundurun.

* Yeni bir Media Services hesabına bağlanın, Video Indexer varsayılan otomatik olarak başlatılır. **akış uç noktası** da:

    ![Media Services akış uç noktası](./media/create-account/ams-streaming-endpoint.png)

    Akış uç noktalarını önemli başlangıç süresi vardır. Bu nedenle, videolarınızı akışa ve Video Indexer web uygulamasında izlenen kadar Azure'a hesabınıza bağlı saatten birkaç dakika sürebilir.

* Var olan bir Media Services hesabına bağlanın, Video Indexer varsayılan akış uç noktası yapılandırmasını değiştirmez. Hiçbir çalışan varsa **akış uç noktası**, bu Media Services hesabı veya Video Indexer videoları mümkün olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Program aracılığıyla, deneme hesabıyla ve/veya'ndaki yönergeleri takip ederek azure'a bağlı Video Indexer hesaplarınızı ile etkileşim kurabilirsiniz: [API'leri kullanan](video-indexer-use-apis.md).

Azure'a bağlanırken kullandığınız aynı Azure AD kullanıcı kullanmanız gerekir.


