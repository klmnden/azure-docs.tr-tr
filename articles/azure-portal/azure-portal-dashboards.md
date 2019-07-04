---
title: Azure portalı panoları oluşturma ve paylaşma | Microsoft Docs
description: Bu makalede, oluşturma, özelleştirme, yayımlama ve Azure portalında panoları paylaşma açıklar.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/01/2019
ms.author: kfollis
ms.openlocfilehash: 8dd1349ca9ab62484eb6693291e3b869ff079dc1
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537214"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Azure portalında panoları oluşturma ve paylaşma

Panolar, bulut kaynaklarınızı Azure portalında odaklı ve düzenli bir görünüm oluşturmak için bir yol sağlar. Panoları burada hızla gündelik işlemleri için görevler başlatabilir ve kaynakları izleme çalışma alanı olarak kullanın.  Projeleri, görevler veya kullanıcı rolleri, örneğin dayalı özel panolar oluşturun.  Azure portalı, başlangıç noktası olarak varsayılan bir Pano sağlar. Varsayılan panoyu düzenleyebilir, oluşturun ve ek panoları özelleştirin ve yayımlama ve diğer kullanıcılar için kullanılabilir hale getirmek için panoları paylaşma. Bu makalede, yeni bir pano oluşturun, özelleştirebilir ve yayımlama ve pano paylaşma açıklar.

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma

Bu örnekte, yeni, özel bir pano oluşturun ve bir ad atayın. Başlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **Pano** sol kenar üst bölümünden. Varsayılan görünümünüzü panoya zaten ayarlanmış olabilir.
1. Seçin **+ yeni Pano**.

    ![Varsayılan panosunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-new.png)

4. Bu eylem açar **kutucuk Galerisi**, kendisinden seçtiğiniz kutucukları ve burada, düzenleme kutucukları boş bir kılavuzdan.

    ![Kutucuk Galerisi ve boş kılavuz ekran görüntüsü](./media/azure-portal-dashboards/dashboard-name.png)

5. Seçin **Panom'u** Panoda metin etiketi ve özel Pano kolayca belirlemenize yardımcı olacak bir ad girin.
1. Seçin **özelleştirme Bitti** düzenleme modundan çıkmak için sayfa üstbilgisi.

Pano görünümü artık boş panonuzu gösterir. Kullanımınıza sunulan panolar görmek için Pano adının yanındaki açılan seçin: liste diğer kullanıcıların oluşturulan ve paylaşılan panolar içerebilir.

## <a name="edit-a-dashboard"></a>Bir panoyu Düzenle

Şimdi, ekleme, yeniden boyutlandırma ve Azure kaynaklarınızı temsil eden kutucukları düzenlemek için panoyu düzenlemenize izin verir.

### <a name="add-tiles"></a>Kutucuk Ekle

Kutucukları bir panoya eklemek için bu adımları izleyin:
1. Seçin ![Düzenle simgesi](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Düzenle** sayfası başlığındaki.

    ![Düzenleme vurgulama panosunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-edit.png)

2. Gözat **kutucuk Galerisi** veya istediğiniz kutucuğu bulmak için arama alanını kullanın.
1. Seçin **Ekle** varsayılan boyut ve konum ile bir Pano kutucuğu otomatik olarak eklemek için. Veya, kutucuğu kılavuza sürükleyin ve istediğiniz yere yerleştirin.

Birçok kaynak sayfalarında ("dikey pencereleri" olarak da bilinir), Komut çubuğundaki Raptiye simgesi bulunur. Simgeyi seçerseniz, kaynak sayfası temsil eden bir kutucuk şu anda etkin olan panoya sabitlenir. Bu yöntem, kutucuk, panonuza eklemek için alternatif bir yoludur.

![Raptiye simgesi ile sayfa komut çubuğunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-pin-blade.png)

> [!TIP]
> Birden fazla kuruluş ile çalışıyorsanız, ekleme **kuruluş kimlik** hangi kuruluş kaynakları açıkça göstermek için panonuza kutucuk aittir.
>
>
### <a name="resize-or-rearrange-tiles"></a>Kutucukları yeniden veya yeniden boyutlandırma
Bir döşeme boyutunu değiştirmek için veya bir Panodaki kutucukları yeniden düzenlemek için aşağıdaki adımları izleyin:

1. Seçin ![Düzenle simgesi](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Düzenle** sayfası başlığındaki.
1. Bir kutucuğun sağ alt köşesinde bağlam menüsünü seçin. Ardından bir döşeme boyutunu seçin. Destekleyen herhangi bir boyutta kutucuklar da kutucuğu istediğiniz boyuta sürükleyin sağlayan sağ alt köşesinde "tanıtıcısı" ekleyin.

   ![Döşeme boyutu menüsü açık olan panosunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-tile-resize.png)

3. Bir kutucuk seçin ve kılavuzda Panonuzda düzenlemek için yeni bir konuma sürükleyin.

### <a name="additional-tile-configuration"></a>Ek kutucuk yapılandırma

Bazı kutucuklar istediğiniz bilgileri göstermek için daha fazla yapılandırma gerekebilir. Örneğin, **ölçüm grafiği** kutucuğa sahip gelen bir ölçüm görüntülemek için ayarlanacak **Azure İzleyici**. Kutucuk verilerini Panodaki varsayılan saat ayarlarını geçersiz kılmak için de özelleştirebilirsiniz.

Ekranları ayarlanması gereken herhangi bir kutucuğa bir **Yapılandır kutucuğu** kutucuğu özelleştirme kadar kapak sayfası. Bu başlığı seçin ve ardından gerekli kurulumu yapın.

![Yapılandırma gerektiren kutucuğunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Markdown kutucuğunu panonuza özel, statik içeriği görüntülemenizi sağlar. Bu temel yönergeler, görüntü, bir dizi köprüler olabilir veya bile iletişim bilgileri. Markdown kutucuğu kullanma hakkında daha fazla bilgi için bkz. [özel markdown kutucuğunu kullanın](azure-portal-markdown-tile.md).
>
>
### <a name="customize-tile-data"></a>Kutucuk verilerini Özelleştir

Panodaki veriler, son 24 saat için etkinlik otomatik olarak gösterir. Bu kutucuk için farklı bir süre göstermek için bu adımları izleyin:

1. Seçin ![filtre simgesine](./media/azure-portal-dashboards/dashboard-filter.png) sol üst köşesinde kutucuk veya seçin, filtre simgesini **kutucuk verilerini Özelleştir** bağlam menüsünden.

   ![Kutucuk bağlam menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

2. Onay kutusunu seçin **kutucuk düzeyinde pano saat ayarlarını geçersiz kılma**.

   ![Kutucuk saat ayarlarını yapılandırmak için iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

3. Bu kutucuğun göstermek için zaman aralığını seçin. Son 30 gün için son 30 dakika seçin veya özel bir aralık tanımlayın.
1. Görüntülenecek zaman ayrıntı düzeyi seçin. Bu gibi durumlarda, bir dakikalık artışlarla herhangi bir yerde bir aylık gösterebilirsiniz.
1. **Uygula**’yı seçin.

## <a name="delete-a-tile"></a>Bir kutucuğu Sil

Bir kutucuğu panodan kaldırmak için bu adımları izleyin:

* Kutucuğun sağ üst köşesinde bağlam menüsünü seçin ve ardından **panodan kaldırmak**. veya,
* Seçin ![Düzenle simgesi](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Düzenle** özelleştirme moduna girmek için. Kutucuğun sağ üst köşesinde üzerine gelin ve ardından ![Sil simgesi](./media/azure-portal-dashboards/dashboard-delete-icon.png) Sil simgesi kutucuğu panodan kaldırmak için.

   ![Kutucuğu panodan Kaldır gösteren ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Bir Panoya kopyalama

Mevcut bir panoya yeni bir Pano için bir şablon olarak kullanmak için aşağıdaki adımları izleyin:

1. Pano görünümü kopyalamak istediğiniz panoyu gösterildiğini doğrulayın.
1. Sayfa üst bilgisindeki seçin ![Kopyala simgesi](./media/azure-portal-dashboards/dashboard-clone.png) **kopya**.
1. Adlı bir panonun kopyasını "kopyası *Pano adınızı*" açılarak düzenleme modunda. Yukarıdaki adımlar bu makalede yeniden adlandırabilir ve panoyu özelleştirme kullanın.

## <a name="publish-and-share-a-dashboard"></a>Yayımlama ve pano paylaşma

Bir panoyu oluşturduğunuzda, görebileceğiniz tek olduğunuz anlamına gelir. varsayılan olarak özeldir. Panolar başkalarının kullanılabilir hale getirmek için bunları diğer kullanıcılarla paylaşabilir. İlk olarak, Pano bir Azure kaynağı olarak yayımlamanız gerekir. Yayımlama ve özel bir Pano paylaşmak için bu adımları izleyin:

1. Seçin ![Paylaş simgesi](./media/azure-portal-dashboards/dashboard-share-icon.png) **paylaşmak** sayfası başlığındaki. **Paylaşım + erişim denetimi** formu görüntülenir.
1. Doğru Pano adının gösterildiğini onaylayın.
1. Seçin bir **abonelik adı**. Abonelik erişimi olan kullanıcılar, paylaşılan panoyu kullanabilirsiniz. Tek tek kutucuklara tarafından temsil edilen kaynaklara erişimi, Azure rol tabanlı access control tarafından belirlenir.
1. 'Pano' kaynak grubu seçili abonelik için bu panoyu yayımlamak için onay kutusunu seçin. Veya, onay kutusunu temizleyin ve bunun yerine mevcut bir kaynak grubuna yayımlamak seçin.
1. Pano kaynak için bir konum seçin. Diğer kaynaklarla panoyu bulun öneririz. Not: var olan kaynak gruplarından seçerseniz, panoyu bu kaynak grubu ile otomatik olarak bulunur.
1. **Yayımla**’yı seçin.

   ![Panoyu yayımlama iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-publish.png)

### <a name="set-access-control-on-a-shared-dashboard"></a>Paylaşılan bir Panodaki erişim denetimini ayarlama

Pano yayımladıktan sonra aşağıdaki adımları izleyerek panoya kimlerin yönetin:

1. İçinde **paylaşım + erişim denetimi** bölmesinde **kullanıcıları yönetme**.

   ![Ekran görüntüsü Pano paylaşım ve erişim denetimi iletişim kutusu](./media/azure-portal-dashboards/dashboard-share-access-control.png)

2. **Erişim denetimi** sayfası açılır. Bu sayfada, birisi için erişim düzeyini gözden geçirin veya yeni bir rol ataması Ekle. Bir rol ataması eklediğinizde, Pano izinleri verdiğiniz.

> [!NOTE]
> Kutucuklar, kuruluşunuzdaki kaynakların temsili görünümlerdir. Kaynaklara erişim rol tabanlı erişim denetimi atama yönetilir ve kaynak aşağı abonelikten izinler devralınmıştır. Bir panoya erişimi verme işlemi otomatik olarak izinleri Panoda gösterilen kaynaklara atamaz. Paylaşılan panolara ve kaynaklar için rol tabanlı erişim denetimi izinleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi ile panoları paylaşma](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Paylaşılan bir panoyu açın.

Bulmak ve paylaşılan bir panoyu açmak için bu adımları izleyin:

1. Pano adının yanındaki açılan listeyi seçin.
1. Pano görüntülenen listeden seçin veya **tüm panolara Gözat** açmak istediğiniz panoyu listede yoksa.

   ![Pano seçimi menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-browse.png)

3. İçinde **türü** alanın, Seç **paylaşılan panolar**.
1. Bir veya daha fazla abonelik seçin. Pano adına göre filtrelemek için metin de girebilirsiniz.
1. Bir pano, paylaşılan panoları listesinden seçin.

## <a name="delete-a-dashboard"></a>Bir panoyu silme

Özel veya paylaşılan bir panoyu kalıcı olarak silmek için aşağıdaki adımları izleyin:

1. Pano adının yanında bulunan aşağı açılan listeden silmek istediğiniz panoyu seçin.
1. Seçin ![Sil simgesi](./media/azure-portal-dashboards/dashboard-delete-icon.png) **Sil** sayfası başlığındaki.
1. Özel bir Pano seçin **Tamam** onay iletişim kutusunda, panoyu kaldırma. Onay iletişim kutusunda, paylaşılan bir panoyu yayımlanan Pano artık başkaları tarafından görülebilir olduğunu onaylamak için onay kutusunu seçin. Sonra **Tamam**’ı seçin.

   ![Silme onayının ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Rol tabanlı erişim denetimi ile panoları paylaşma](azure-portal-dashboard-share-access.md)
* [Program aracılığıyla Azure panoları oluşturma](azure-portal-dashboards-create-programmatically.md)
