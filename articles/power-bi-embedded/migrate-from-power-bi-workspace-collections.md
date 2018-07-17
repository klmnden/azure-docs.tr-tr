---
title: Power BI çalışma alanı koleksiyonu içeriğini Power BI Embedded geçirme | Microsoft Docs
description: Power BI Embedded için Power BI çalışma alanı koleksiyonları geçirmeyi öğrenin ve uygulamalara içerik faydalanmayı.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: maghan
ms.openlocfilehash: de20d532112ca73f34f7cb603d043579c28179d6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071241"
---
# <a name="how-to-migrate-power-bi-workspace-collection-content-to-power-bi-embedded"></a>Power BI çalışma alanı koleksiyonu içeriğini Power BI Embedded geçirme

Power BI Embedded için Power BI çalışma alanı koleksiyonları geçirmeyi öğrenin. Bu makalede, Azure Power BI çalışma alanı koleksiyonları ' Power BI Embedded geçişine ilişkin yönergeler sağlar. Ayrıca, uygulama değişiklikleri için beklenmesi gerekenler bakacağız.

Power BI çalışma alanı koleksiyonları kaynak Power BI Premium'un genel kullanılabilirlik sürümü sonra sınırlı bir süre için kullanılabilir olmaya devam eder. Kurumsal Anlaşma kapsamındaki müşteriler mevcut anlaşmalarının sonuna kendi mevcut çalışma alanı koleksiyonları erişebilir. Power BI çalışma alanı koleksiyonları doğrudan ya da CSP kanalları edinilen müşteriler, Power BI Premium'un genel kullanıma bir yıl için erişim keyfini çıkarın.

> [!IMPORTANT]
> Geçiş Power BI hizmetinde bir bağımlılık çekerken, yok bir bağımlılık Power BI uygulamanızın kullanıcıları için kullanılırken bir **ekleme belirteci**. Power BI'ı uygulamanıza eklenmiş içeriği görüntülemek kaydolun gerekmez. Bu yaklaşım ekleme, müşterileriniz için Power BI ekleme.

![Power BI Embedded akış](media/migrate-from-power-bi-workspace-collections/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>Geçiş için hazırlama

Power BI çalışma alanı koleksiyonu hizmetinden üzerinden Power BI Embedded için geçişe hazırlanmak için yapmanız gereken birkaç nokta vardır. Bir kiracı kullanılabilir bir Power BI Pro lisansı olan bir kullanıcıya ihtiyacınız vardır.

1. Bir Azure Active Directory (Azure AD) kiracısına erişiminiz olduğundan emin olun.

    Kullanmak için hangi Kiracı?

    * Var olan Kurumsal Power BI kiracınızın kullanılsın mı?
    * Uygulamanız için ayrı bir kiracı kullanılsın mı?
    * Her müşteri için ayrı bir kiracı kullanılsın mı?

    Uygulamanız veya her müşteri için yeni bir kiracı oluşturmaya karar verdiğinizde, şunlardan birini bakın:

    * [Azure Active Directory kiracısı oluşturma](https://powerbi.microsoft.com/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)
    * [Bir Azure Active Directory kiracısı edinme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant).

2. Bu yeni kiracıda uygulama "ana" hesabı olarak davranan bir kullanıcı oluşturun. Hesap Power BI'a kaydolması ve kendisine atanmış bir Power BI Pro lisansına sahip olması gerekir.

## <a name="accounts-within-azure-ad"></a>Azure AD içindeki hesaplar

Aşağıdaki hesapları kiracınızda bulunması gerekir.

> [!NOTE]
> Bu hesapların uygulama çalışma alanlarını kullanabilmesi için Power BI Pro lisansı olması gerekir.

1. Bir kiracı yönetici kullanıcı.

    Katıştırma uygulama çalışma alanının bir üyesi olarak listelenen Kiracı Yöneticisi olduğunu önerilir.

2. İçerik oluşturma analistlerin hesapları.

    Bu kullanıcıların gerektiğinde uygulama çalışma alanlarında atanmalıdır.

3. Bir uygulama *ana* kullanıcı hesabı veya hizmet hesabı.

    Uygulama arka ucu, bu hesabın kimlik bilgilerini depolar. Kullanma *ana* hesabı ile Power BI REST API'lerini kullanmak için Azure AD belirteçlerini almak için. Bu hesap, uygulamanın ekleme belirtecini oluşturmak için kullanılır. *Ana* hesabı eklemek için oluşturulan uygulama çalışma alanının yöneticisi olması gerekir.

    **Yalnızca normal bir kullanıcı hesabı ekleme amacıyla kullanılır, kuruluşunuzdaki hesabıdır.**

## <a name="app-registration-and-permissions"></a>Uygulama kaydı ve izinler

REST API çağrıları gerçekleştirmek için bir uygulamayı Azure AD'ye kaydedin. Ek yapılandırma, Microsoft Azure Portal'ın Power BI uygulama kayıt sayfasına ek olarak uygulanır. Daha fazla bilgi için [Power BI içeriği eklemek üzere bir Azure AD uygulamasını kaydetme](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

Uygulamayı kullanarak uygulamayı kaydetmeniz önerilir **ana** hesabı.

## <a name="create-app-workspaces-required"></a>Uygulama çalışma alanları (gerekli) oluşturma

Uygulamanızı birden çok müşteriye hizmet, daha iyi yalıtım sağlamak için uygulama çalışma alanları yararlanabilir. Panolarınızı ve raporlarınızı müşterileriniz diğerlerinden ayrılır. Daha sonra Power BI hesabınız uygulama çalışma alanı daha fazla uygulama deneyimleri müşterilerinizin arasında ayırmak için kullanabilirsiniz, ancak basit tutmak için yalnızca bir hesap kullanabilirsiniz.

> [!IMPORTANT]
> Kişisel çalışma alanlarını kullanamazsınız (bir "Çalışma Alanım") müşterileriniz için içerik ekleme avantajlarından yararlanmak için.

Power bı'da uygulama çalışma alanı oluşturmak için Pro sürüm lisansı bulunan bir kullanıcı ihtiyacınız vardır. Uygulama çalışma alanını oluşturan Power BI kullanıcı varsayılan olarak, çalışma alanının yöneticisi değil. **Uygulama *ana* hesabının çalışma alanının yöneticisi olması gerekir.**

## <a name="content-migration"></a>İçerik Geçişi

Çalışma alanı koleksiyonlarınızı içeriğinizi Power BI Embedded geçiş için mevcut çözümünüzle paralel yapılabilir ve kapalı kalma süresi gerektirmez.

A **geçiş aracı** , içeriğinizi Power BI çalışma alanı koleksiyonları ' Power BI Embedded kopyalama konusunda yardımcı olmak için kullanılabilir. Özellikle çok sayıda rapor varsa. Daha fazla bilgi için [Power BI Embedded geçiş aracı](migrate-tool.md).

İçerik Geçişi temelde iki API kullanır.

1. İndirme PBIX: Bu API, Ekim 2016'dan sonra Power BI'a yüklenmiş PBIX dosyalarını indirebilirsiniz.
2. Import PBIX: Bu API, pbıx dosyalarını Power BI'a yükler.

İlgili kod parçacıkları için bkz: [geçişi için kod parçacıkları Power BI Embedded içerik](migrate-code-snippets.md).

### <a name="report-types"></a>Rapor türleri

Her farklı geçiş akış gerektiren raporlar, birkaç türü vardır.

#### <a name="cached-dataset-and-report"></a>Önbellekteki veri kümesini ve raporu

Veriler bir canlı bağlantı veya DirectQuery bağlantısının aksine içeri PBIX dosyaları önbelleğe alınmış veri kümeleri bakın.

**Akış**

1. Power BI çalışma alanı koleksiyonu çalışma alanınızdan download PBIX API çağrısı.
2. Pbıx dosyasını kaydedin.
3. Power BI Embedded çalışma alanınız için Import PBIX çağrısı.

#### <a name="directquery-dataset-and-report"></a>DirectQuery veri kümesini ve raporu

**Akış**

1. GET çağrı `https://api.powerbi.com/v1.0/collections/{collection_id}/workspaces/{wid}/datasets/{dataset_id}/Default.GetBoundGatewayDataSources` ve alınan bağlantı dizesini kaydedin.
2. Power BI çalışma alanı koleksiyonu çalışma alanınızdan download PBIX API çağrısı.
3. Pbıx dosyasını kaydedin.
4. Power BI Embedded çalışma alanınız için Import PBIX çağrısı.
5. Bağlantı dizesi çağrısı yaparak - POST güncelleştir  `https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.SetAllConnections`
6. Çağrısı yaparak GW Kimliğini ve veri kaynağı Kimliğini alın: alma `https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.GetBoundGatewayDataSources`
7. Kullanıcı kimlik bilgilerini güncelleştirin - düzeltme eki `https://api.powerbi.com/v1.0/myorg/gateways/{gateway_id}/datasources/{datasource_id}`

#### <a name="old-dataset-and-reports"></a>Eski veri kümeleri ve raporlar

Ekim 2016'dan desteklemez önce PBIX indirme özelliği karşıya yüklenen raporlar. 

**Akış**

1. Geliştirme ortamınızdan (iç kaynak denetiminizden) pbıx dosyasını alın.
2. Power BI Embedded çalışma alanınız için Import PBIX çağrısı.

#### <a name="push-dataset-and-report"></a>Gönderim veri kümesini ve raporu

İndirme PBIX desteklemiyor *Push API* veri kümeleri. Push API veri kümesi Power BI çalışma alanı koleksiyonları ' Power BI Embedded taşınamaz.

**Akış**

1. Power BI Embedded çalışma alanınız için veri kümesini oluşturmak için Json veri kümesi ile "Create dataset" API çağrısı.
2. Rapor için oluşturulan veri kümesi * yeniden oluşturun.

Anında iletme geçirmek için bazı geçici çözümleri kullanarak mümkündür aşağıdaki API rapordan Power BI çalışma alanı koleksiyonları Power BI Embedded için:

1. İşlevsiz pbıx dosyaları, Power BI çalışma alanı koleksiyonu çalışma alanınıza karşıya yükleniyor.
2. Anında iletme kopyalama API'si rapor ve 1. adımdan işlevsiz PBIX dosyasına bağlayın.
3. Push API raporunu işlevsiz PBIX ile indirin.
4. İşlevsiz PBIX Power BI Embedded çalışma alanınıza yükleyin.
5. Gönderim veri kümesini Power BI Embedded çalışma alanınızda oluşturun.
6. Raporu push API veri kümesi için yeniden bağlayın.

## <a name="create-and-upload-new-reports"></a>Oluşturma ve yeni raporlar karşıya yükleme

Power BI çalışma alanı koleksiyonları ' geçirdiğiniz içeriğe ek olarak, rapor ve veri kümelerini Power BI Desktop kullanarak oluşturabilir ve ardından bu raporları bir uygulama çalışma alanında yayımlayabilirsiniz. Raporları yayımlayan son kullanıcı, bir uygulama çalışma alanına yayımlamak için Power BI Pro lisansı olması gerekir.

## <a name="rebuild-your-application"></a>Uygulamanızı yeniden oluşturma

1. Uygulamanızı Power BI REST API'lerini ve powerbi.com içindeki rapor konumunu kullanacak şekilde değiştirin.

2. AuthN/AuthZ kimlik doğrulaması kullanarak yeniden *ana* uygulamanız için hesap. Kullanmanın avantajı uygulayabileceğiniz bir [ekleme belirteci](https://msdn.microsoft.com/library/mt784614.aspx) bu kullanıcının diğer kullanıcıların adına hareket etmesine izin vermek için.

3. Power BI Embedded gelen raporlarınızı uygulamanıza ekleyin. Daha fazla bilgi için [Power BI panolarınızı, raporlar ve kutucuklar ekleme](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

## <a name="map-your-users-to-a-power-bi-user"></a>Kullanıcılarınızı bir Power BI kullanıcısıyla eşleme

Uygulamanızın içinde uygulamaya içinde yönettiğiniz kullanıcıları eşlemek bir *ana* , uygulamanızın amaçları için Power BI kimlik bilgisi. Bu Power BI için kimlik bilgilerini *ana* hesabı uygulamanızın içinde depolanır ve oluşturmak için kullanılan ekleme belirteçleri.

## <a name="what-to-do-when-you-are-ready-for-production"></a>Üretim için hazır olduğunuzda yapmanız gerekenler

Üretim aşamasına geçmeye hazır olduğunuzda, aşağıdakileri yapmanız gerekir:

- Geliştirme için ayrı bir kiracı kullanıyorsanız uygulama çalışma alanlarınızın, panolarınızın ve raporlarınızın üretim ortamınızda kullanılabilir durumda olduğundan emin olmak gerekir. Uygulamayı üretim kiracınızın Azure AD'de oluşturulan ve 1. adımda belirtilen gerekli uygulama izinlerini atadığınızdan emin olun.

- İhtiyaçlarınıza uygun bir kapasite satın alın. Kullanabileceğiniz [katıştırılmış analiz kapasite planlama teknik incelemesi](https://aka.ms/pbiewhitepaper) ihtiyacınız anlamanıza yardımcı olacak. Satın almaya hazır olduğunuzda, bir Power BI Embedded kaynağı Azure portalında satın alabilirsiniz.

- Uygulama çalışma alanını düzenleyin ve Gelişmiş Ayarlar bölümünden bir kapasiteye atayın.

    ![Powerbi.com içindeki bir kapasiteye uygulama çalışma alanı atama](media/migrate-from-power-bi-workspace-collections/embedded-capacity.png)

- Güncelleştirilen uygulamanızı üretim ortamında dağıtın ve Power BI Embedded raporları eklemeye başlayın.

## <a name="after-migration"></a>Geçişten sonra

Bazı temizleme işlemleri, Power BI çalışma alanı koleksiyonları içinde gereklidir.

- Power BI çalışma alanı koleksiyonları Azure hizmetindeki dağıtılan çözümün tüm çalışma alanlarını kaldırın.
- Azure'daki mevcut herhangi bir çalışma alanı koleksiyonlarını silin.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler. Uygulamanız artık Power BI Embedded geçirilir. Power BI panolarınızı, raporlarınızı ve veri ekleme hakkında daha fazla bilgi için bkz. [Power BI panolarınızı, raporlar ve kutucuklar ekleme](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Başka sorunuz mu var? [Power BI Topluluğu'na sorun](http://community.powerbi.com/)