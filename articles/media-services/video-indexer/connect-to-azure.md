---
title: Video Indexer hesabınız, Azure portalında oluşturma
titlesuffix: Azure Media Services
description: Bu makalede, Azure portalında bir Video Indexer hesabına oluşturulacağını gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: 49e05047d58cc5b6bb5e0099c24a131a26dc8af1
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292490"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Azure'a bağlı bir Video Indexer hesabı oluşturun

Video Indexer hesabınızı oluştururken ücretsiz bir deneme hesabı (belirli sayıda ücretsiz dizin oluşturma dakikası elde edersiniz) veya ücretli bir seçenek (kota sınırlaması olmaz) arasından seçim yapabilirsiniz. Ücretsiz deneme kullanıldığında Video Indexer, web sitesi kullanıcılarına 600 dakikaya kadar ve API kullanıcılarına ise 2400 dakikaya kadar ücretsiz dizin oluşturma olanağı sunar. Ücretli seçeneği ile Azure aboneliğinize bağlı bir Video Indexer hesabınız ve Azure Media Services hesabı oluşturun. Dizin oluşturma faaliyeti yapılan dakika sayısının yanı sıra Medya Hesabı ile ilgili ücretler için ödeme yaparsınız. 

Bu makalede bir Azure aboneliğine bağlı bir Video Indexer hesabınız ve Azure Media Services hesabı oluşturma işlemini gösterir. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. 

    Henüz Azure aboneliğiniz yoksa kaydolmak [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

* Bir Azure Active Directory (AD) etki alanı. 

    Azure AD etki alanı yoksa, bu etki alanı, Azure aboneliğiniz ile oluşturun. Daha fazla bilgi için [Azure Active Directory'de özel etki alanı adlarını yönetme](../../active-directory/users-groups-roles/domains-manage.md)

* Kullanıcı ve Azure AD etki alanınızı üye. Bu üye, Video Indexer hesabınız Azure'a bağlanırken kullanacaksınız.

    Bu kullanıcı, bu ölçütleri karşılamalıdır:

    * Bir iş veya Okul hesabı, kişisel hesabı değil, outlook.com, live.com veya hotmail.com gibi bir Azure AD kullanıcısı olabilir.
        
        ![Tüm AAD kullanıcıları](./media/create-account/all-aad-users.png)

    *  Azure aboneliğinizde bir sahip rolü veya katkıda bulunan hem de kullanıcı erişimi yöneticisi rol üyesi olabilir. Bir kullanıcının ile 2 rolü iki kez eklenebilir. Katkıda bulunan ve bir kez kullanıcı erişimi Yöneticisi ile bir kez.

        ![Erişim denetimi](./media/create-account/access-control-iam.png)

* Azure portalını kullanarak EventGrid kaynak sağlayıcısını kaydedin.

    İçinde [Azure portalında](https://portal.azure.com/)Git **abonelikleri** > [. abonelik] > **ResourceProviders** > **Microsoft.EventGrid**. Durumda değil "Kaydedildi" ise, tıklayın **kaydetme**. Bu işlem birkaç dakika kaydedilecek götürür. 

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.

2. Tıklayarak **Azure'a bağlanma** düğmesi:

    ![Azure'a bağlanma](./media/create-account/connect-to-azure.png)

3. Abonelik listesi göründüğünde, kullanmak istediğiniz aboneliği seçin. 

    ![Video Indexer'ı Azure'a bağlanma](./media/create-account/connect-vi-to-azure-subscription.png)

4. Desteklenen konumlardan Azure bölgesini seçin: Batı ABD 2, Kuzey Avrupa ve Doğu Asya.
5. Altında **Azure Media Services hesabı**, aşağıdaki seçeneklerden birini seçin:

    * Yeni bir Media Services hesabı oluşturmak için Seç **yeni kaynak grubu oluştur**. Kaynak grubunuz için bir ad sağlayın.

        Azure, yeni bir Azure depolama hesabı dahil aboneliğinizde, yeni hesabı oluşturur. Varsayılan başlangıç yapılandırmasını akış uç noktası ve 10 S3 ayrılmış birim ile yeni Media Services hesabınız var.
    * Mevcut bir Media Services hesabını kullanmayı tercih **var olan kaynağı kullanın**. Hesapları listesinden hesabınızı seçin.

        Media Services hesabınızı, Video Indexer hesabınız ile aynı bölgede olması gerekir. Dizin oluşturma süresi ve aktarım hızının düşük olmasını en aza indirmek için ayrılmış birim sayısı ve türü ayarlamak **10 S3 ayrılmış birim** Media Services hesabı.
    * Bağlantınız el ile yapılandırmak için tıklayın **el ile yapılandırmaya geçiş**. 
    
        Bağlantınızı tamamlamak otomatik seçeneğini herhangi bir nedenden dolayı başarısız olursa veya kurulumu ve yapılandırması ise sık karşılaşılan durumlarda farklı veya ayarları üzerinde tam görünürlük ve denetim olmasını istediğiniz el ile yapılandırmak isteyebilirsiniz. 
        
        İçinde **bağlanın, Video Indexer bir Azure aboneliğine**, aşağıdaki bilgileri sağlayın.

        |Ayar|Açıklama|
        |---|---|
        |Video Indexer hesabının bölgesi|Video Indexer hesap bölgesi adı. Daha iyi performans ve düşük maliyetlerden için Azure Media Services kaynağınız ve Azure depolama hesabının bulunduğu bölge adını belirtmek için önemle tavsiye edilir. |
        |Azure Active Directory (AAD) kiracısı|Azure AD kiracısı, örneğin "contoso.onmicrosoft.com" adı. Kiracı bilgileri, Azure portalından alınabilir. İmlecinizi üst oturum açan kullanıcı adının üzerine sağ alt köşesinde yerleştirin.|
        |Abonelik Kimliği|Azure aboneliği altında bu bağlantının oluşturulması. Abonelik kimliği, Azure portalından alınabilir. Tıklayarak **tüm hizmetleri** sol bölme ve "abonelikler" arayın. SELECT, **abonelikleri** ve istenen kimliği aboneliklerinizi listesinden seçin.|
        |Azure Media Services kaynak grubu adı|İçin Media Services hesabı, kaynak grubunun adı zaten var.|
        |Medya hizmeti kaynak adı|Azure Media Services kaynağı adı.|
        |Uygulama Kimliği|Azure AD uygulama kimliği ile belirtilen Media Services hesabı için izinler. Daha fazla bilgi için [kullanım hizmet sorumlusu kimlik doğrulaması](../../media-services/previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).|
        |Uygulama Anahtarı|Daha fazla bilgi için [kullanım hizmet sorumlusu kimlik doğrulaması](../../media-services/previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).|

6. İşiniz bittiğinde seçin **Connect**. Bu işlem birkaç dakika sürebilir. 

    Azure'a bağlandıktan sonra yeni Video Indexer hesabınız hesap listesinde görüntülenir:

    ![Yeni hesap](./media/create-account/new-account.png)

7. Yeni hesabınıza gidin: 

    ![Video Indexer hesabınız](./media/create-account/vi-account.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki Azure Media Services ilgili önemli noktalar geçerlidir:

* Yeni bir Media Services hesabına bağladıysanız, Azure aboneliğinizde yeni bir kaynak grubu, Media Services hesabı ve depolama hesabı görürsünüz.
* Yeni bir Media Services hesabına bağladıysanız, Video Indexer ortam ayarlamanız **ayrılmış birim** 10 S3 birimleri:

    ![Media Services için ayrılmış birimleri](./media/create-account/ams-reserved-units.png)

* Video Indexer mevcut bir Media Services hesabına bağladıysanız, var olan medya değiştirmez **ayrılmış birim** yapılandırma.

    Medya sayısını ve türünü ayarlamak ihtiyacınız olabilecek **ayrılmış birim**, planlanan yük göre. Yük yüksektir ve yeterli birimleri veya hızı yoksa, video işleme zaman aşımı hataları neden göz önünde bulundurun.

* Yeni bir Media Services hesabına bağlı değilse, Video Indexer varsayılan otomatik olarak başlatır. **akış uç noktası** da:

    ![Media Services akış uç noktası](./media/create-account/ams-streaming-endpoint.png)

* Video Indexer, var olan bir Media Services hesabına bağladıysanız, varsayılan akış uç noktası yapılandırmasını değiştirmez. Hiçbir çalışan varsa **akış uç noktası**, bu Media Services hesabı veya Video Indexer videoları mümkün olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Deneme hesabınız ile ve/veya'ndaki yönergeleri takip ederek azure'a bağlı Video Indexer hesaplarınızı ile program aracılığıyla etkileşim kurabilir: [kullanım API'leri](video-indexer-use-apis.md).

Azure'a bağlanırken kullandığınız aynı Azure AD kullanıcı kullanmanız gerekir.


