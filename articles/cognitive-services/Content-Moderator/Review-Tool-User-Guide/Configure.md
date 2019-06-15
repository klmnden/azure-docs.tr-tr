---
title: Gözden geçirme aracı ayarları - Content Moderator'ı yapılandırma
titlesuffix: Azure Cognitive Services
description: Gözden geçirme aracı, yapılandırma veya Content Moderator için takım, etiketler, bağlayıcı, iş akışları ve kimlik bilgilerini almak için kullanın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/15/2019
ms.author: sajagtap
ms.openlocfilehash: f88ccbabc925b651abbc06f571a9d4220ed8aeb2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61285614"
---
# <a name="configure-the-review-tool"></a>Gözden Geçirme aracını yapılandırma

[Gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com) aracılığıyla erişebileceğiniz çeşitli önemli özelliklere sahip **ayarları** Panosu menüsünde.

![Moderator gözden geçirme çok içerik ayarlar menüsü](images/settings-1.png)

## <a name="manage-team-and-subteams"></a>Takım ve alt takımlar yönetme

**Takım** sekmesinde takım ve alt takımlar yönetmenize olanak tanır&mdash;belirli olduğunda bildirim kullanıcı gruplarını [incelemelere](../review-api.md#reviews) başlatılır. Yalnızca (gözden geçirme aracı ile oturum açtığınızda oluşturduğunuz) bir ekibi olabilir, ancak birden çok alt ekipler oluşturabilirsiniz. Takım Yöneticisi üyeler davet, izinlerini ayarlayın ve bunları farklı alt takımlar için atayın.

![Gözden geçirme aracı team ayarları](images/settings-2-team.png)

Alt ekipler, yükseltme ekipler veya belirli kategorileri içeriğinin gözden geçirme için adanmış ekipler oluşturmak için kullanışlıdır. Örneğin, daha fazla inceleme için ayrı bir ekibe yetişkinlere yönelik içeriğe gönderebilir.

Bu bölümde, alt ekipler oluşturma ve hızlı bir şekilde gözden geçirmeleri hareket halindeyken atama açıklanmaktadır. Ancak, kullanabileceğiniz [iş akışları](workflows.md) belirli ölçütlere göre incelemeleri atamak için.

### <a name="create-a-subteam"></a>Bir alt oluşturma

Git **alt ekipler** tıklayın ve bölüm **ekleme alt**. İletişim kutusunda, alt adınızı girin ve tıklayın **Kaydet**.

![Alt ad](images/1-Teams-2.PNG)

#### <a name="invite-teammates"></a>Ekip arkadaşlarınızın davet edin

Gözden geçirenler için varsayılan takım eklemeniz gerekir. Bu nedenle zaten varsayılan takım üyesi yoksa, birisi için bir alt atayamazsınız. Tıklayın **davet** üzerinde **takım** sekmesi.

![Kullanıcıları davet](images/invite-users.png)

#### <a name="assign-teammates-to-subteam"></a>Takım arkadaşları subteam için atama

Tıklayın **Üye Ekle** üyeleri varsayılan takımınız için bir veya daha fazla alt ekipler atamak için düğme. Bu gibi durumlarda, mevcut kullanıcıları yalnızca bir alt için ekleyebilirsiniz. Gözden geçirme aracı olmayan yeni kullanıcılar eklemek için onları takım Ayarları sayfasında "Davet et" düğmesini kullanarak davet edin.

![Alt üye atama](images/1-Teams-3.PNG)

### <a name="assign-reviews-to-subteams"></a>Ata alt takımlar için gözden geçirmeleri

Sonra alt ekipler oluşturdunuz ve üyeleri atanan, içeriği atama başlayabilir [incelemeleri](../review-api.md#reviews) bu alt takımlar için. Bu yapılır **gözden geçirme** sitenin sekmesi.
İçerik için bir alt atamak için select sağ üst köşesindeki üç nokta simgesine tıklayın **Taşı**ve bir alt seçin.

![Görüntü gözden subteam atayın](images/3-review-image-subteam-1.png)

### <a name="switch-between-subteams"></a>Alt ekipler arasında geçiş yapın

Birden fazla alt üyesiyseniz, hangi içerik incelemeleri, görüntülenme şeklini değiştirmek için bu alt ekipler arasında geçiş yapabilirsiniz. İçinde **gözden geçirme** sekmesinde, etiketli açılan menüyü seçin **varsayılan** seçip **seçin alt**. Farklı alt ekipler, ancak yalnızca biri için içerik incelemeleri görüntüleyebilir, üyesi olduğunuz.

![Alt ekipler arasında geçiş yapın](images/3-review-image-subteam-2.png)

## <a name="tags"></a>Tags

**Etiketleri** sekmesi iki varsayılan denetimi etiketler yanı sıra özel denetimi etiketleri tanımlamanızı sağlar&mdash;**isadult** (**bir**) ve **isracy**  (**r**). Özel bir etiket oluşturduğunuzda, varsayılan etiketler yanı sıra incelemelerde kullanılabilir hale gelir. Görünürlük ayarlarına geçiş yaparak hangi etiketlerin incelemelerde görünmesini değiştirebilirsiniz.

!["Görünür olduğundan" onay kutularını dahil olmak üzere etiketleri görüntüleme](images/tags-4-disable.png)

### <a name="create-custom-tags"></a>Özel etiketler oluşturma

Yeni bir etiket oluşturmak için bir kısa kodu, ad ve açıklama ilgili alanlara girmeniz gerekir.

- **Kısa kod**: İki harfli kod için etiketi girin. Örnek: **Kal**
- **Ad**: Küçük boşluksuz kısa ve açıklayıcı bir etiket adı girin. Örnek: **isbullying**.
- **Açıklama**: içerik türü, bir açıklama (isteğe bağlı) girin, etiket hedeflerinizi. Örnek: **Bulunduran veya siber bullying, örnekleri**.

Tıklayın **Ekle** bir etiket ekleyin ve **Kaydet** işiniz bittiğinde etiketler oluşturma.

![Gözden geçirme aracı yeni etiket iletişim kutusu oluşturma](images/settings-3-tags.png)

### <a name="delete-tags"></a>Etiketleri Sil

Özel etiketler, etiket listesindeki girdiler yanındaki Çöp Kutusu simgesini seçerek silebilirsiniz, ancak varsayılan etiketleri silemezsiniz.

## <a name="connectors"></a>Bağlayıcılar

**Bağlayıcılar** sekme, içerik içeriği bir parçası olarak farklı şekilde işleyebilen hizmete özgü artırmasını bağlayıcılarınızı yönetmenize olanak tanır [iş akışları](../review-api.md#workflows).

Bir iş akışı oluşturduğunuzda varsayılan içerik olarak işaretleyebilirsiniz Content Moderator bağlayıcı Bağlayıcıdır **yetişkinlere yönelik** veya **müstehcen**küfür bulun ve benzeri. Ancak, kendi ilgili hizmetler için kimlik bilgilerine sahip olduğu sürece burada listelenen diğer bağlayıcılara kullanabilirsiniz (yüz tanıma API'si Bağlayıcısı'nı kullanmak için örneğin, almanız gerekir bir [yüz tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/face/overview) abonelik anahtarı).

[Gözden geçirme aracı](./human-in-the-loop.md) aşağıdaki bağlayıcılar içerir:

- Duygu Tanıma API'si
- Yüz tanıma API'si
- PhotoDNA bulut hizmeti
- Metin Analizi API’si

### <a name="add-a-connector"></a>Bağlayıcı Ekle

Bir bağlayıcı eklemek için (ve içeriği kullanmak için kullanılabilir hale getirmek [iş akışları](../review-api.md#workflows)), uygun seçin **Connect** düğmesi. Sonraki iletişim kutusunda, bu hizmet için abonelik anahtarınızı girin. Yeni Bağlayıcınızı, işiniz bittiğinde, sayfanın en üstündeki görüntülenmesi gerekir.

![Content Moderator bağlayıcı ayarları](images/settings-4-connectors.png)

## <a name="workflows"></a>İş Akışları

**İş akışları** sekmesini yönetmenize olanak tanır, [iş akışları](../review-api.md#workflows). İçerik için filtreler bulut tabanlı iş akışlarıdır ve içeriği farklı şekillerde sıralama ve uygun eylemleri gerçekleştirmek için bağlayıcılar ile çalışırlar. Burada, tanımlayabilirsiniz, düzenleme ve iş akışlarınızı test edin. Bkz: [tanımlama ve kullanma iş akışları](Workflows.md) bunun nasıl yapılacağı hakkında yönergeler için.

![Content Moderator iş akışı ayarları](images/settings-5-workflows.png)

## <a name="credentials"></a>Kimlik Bilgileri

**Kimlik bilgilerini** sekme denetimi hizmetlerinden herhangi birinin bir REST çağrısı veya istemci SDK'sı erişmesi gerekebilir, Content Moderator abonelik anahtarı hızlı erişim sağlar.

![Content Moderator kimlik bilgileri](images/settings-6-credentials.png)

### <a name="use-external-credentials-for-workflows"></a>İş akışları için Dış kimlik bilgilerini kullan

[Gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com) kaydolduktan, ancak varolan kullanacak şekilde de yapılandırabilirsiniz Azure Content Moderator Hizmetleri Azure hesabınızdan anahtar için ücretsiz bir deneme sürümü anahtarı oluşturur. Ücretsiz deneme anahtarlarının katı kullanım sınırları sahip olarak, bu büyük ölçekli senaryolar için önerilir ([fiyatlandırma ve limitler](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/)).

Oluşturduysanız bir [Content Moderator kaynak](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) Azure'da, Azure portal ve select gitmek **anahtarları** dikey penceresi. Anahtarlarınızdan birini kopyalayın.

![Azure portalında Content Moderator anahtarları](images/credentials-azure-portal-keys.PNG)

İçinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com)'s **kimlik bilgilerini** sekmesine gidin **iş akışı ayarları** bölmesinde **Düzenle**, anahtarınızı yapıştırın**Ocp-Apim-Subscription-Key** alan. Şimdi, yönetim API'lerini çağıran iş akışlarını Azure kimlik bilgilerinizi kullanır.

> [!NOTE]
> Diğer iki alanlar **iş akışı ayarları** bölmesi olan özel terim ve görüntü listeleri için. Bkz: [özel terimleri](../try-terms-list-api.md) veya [özel görüntüleri](../try-image-list-api.md) kılavuzları bunlar hakkında bilgi edinmek için.

### <a name="use-your-azure-account-with-the-review-apis"></a>Azure hesabınızı İnceleme API'lerini kullanın.

Azure anahtarınızı İnceleme API'lerini kullanmak için kaynak kimliğinizi almak gerekir Azure portal ve select Content Moderator kaynağınıza gidin **özellikleri** dikey penceresi. Kaynak Kimliği değerini kopyalayın ve yapıştırın **beyaz listeye kaynak kimlikleri** gözden geçirme Aracı'nın alanını **kimlik bilgilerini** sekmesi.

![Azure portalında Content Moderator kaynak kimliği](images/credentials-azure-portal-resourceid.PNG)

Her iki yerde de abonelik anahtarınızı girdiyseniz, gözden geçirme aracı hesabınızla birlikte gelen deneme anahtarı kullanılmayacak ancak sunulmaya devam edecektir.

## <a name="next-steps"></a>Sonraki adımlar

İzleyin [gözden geçirme aracı hızlı](../quick-start.md) içerik denetleme senaryolarda İnceleme aracını kullanmaya başlamak için.