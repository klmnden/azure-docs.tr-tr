---
title: "Azure Market uygulamalarda yönetilen | Microsoft Docs"
description: "Azure açıklar yönetilen Market üzerinden kullanılabilir uygulamalar."
services: azure-resource-manager
author: tfitzmac
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: e643c86bfd5a78f21f6d96051e4395168cb7d6e0
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Market'te Azure yönetilen uygulamalar

Satıcılar, Azure kullanabileceğiniz yönetilen tüm Azure Marketi müşterilere çözümleri sunmak için uygulamalar. Bu satıcılar, yönetilen hizmet sağlayıcıları (MSP'ler), bağımsız yazılım satıcılarının (ISV'ler) ve sistem tümleştiricileri (SIS) içerebilir. Yönetilen uygulamalar, Bakım ve müşteriler için ek yükü bakım azaltın. Altyapı ve Market üzerinden yazılım satıcıları satar. Yönetilen uygulamaların bunlar Hizmetleri ve işletimsel destek ekleyebilirsiniz. Daha fazla bilgi için bkz: [yönetilen uygulama genel bakış](overview.md).

Bu makalede Marketi'nde uygulama yayımlama ve müşterilere geniş çapta kullanılabilir duruma nasıl açıklanmaktadır.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Yönetilen bir uygulamayı yayımlamak için Önkoşullar

Bu makalede tamamlamak için zaten .zip dosyası için yönetilen uygulama tanımı olması gerekir. Daha fazla bilgi için bkz: [hizmet Kataloğu uygulaması oluştur](publish-service-catalog-app.md).

Ayrıca, çeşitli iş önkoşulları vardır. Bunlar:

* Şirketiniz veya onun yan burada satış Marketi tarafından desteklenen bir ülkede bulunmalıdır.
* Ürünü Marketi tarafından desteklenen faturalama modelleri ile uyumlu şekilde lisansına sahip olması gerekir.
* Teknik Destek müşterilerine ticari koşulların elverdiği oranda makul bir biçimde erişilebilir. Destek Ücretli, boş veya topluluk desteklemez.
* Lisans yazılımınız ve üçüncü taraf yazılım bağımlılıkları.
* Teklifinizle Market ve Azure portalını listelenmiş ölçütlerini karşılayan içerik sağlar.
* Azure Market katılım ilkeleri ve yayımcı Sözleşmesi koşullarını kabul ediyorsunuz.
* Kullanım koşulları, Microsoft gizlilik bildirimi ve Microsoft Azure sertifikalı Program sözleşmesi uymak kabul etmiş olursunuz.

## <a name="set-up-your-account-for-publishing-portal"></a>Hesabınız yayımlama portalı için ayarlama

Yayımlama Portalı yayımlama ve, tekliflerini yönetmek için kullanılır. Market uygulamayı yayımlamak için Azure Market onaylanmış bir Microsoft Developer olması gerekir. Onaylanan bir hesap için kaydolmadıysanız bkz [Microsoft Developer hesabı oluşturma](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

Onaylanan bir varsa **Microsoft Developer Center'da** hesap, ancak daha önce kullanılmayan [Azure yayımlama portalında](https://cloudpartner.azure.com/), yayımlama portal kaydetmeniz gerekir.

1. Yeni bir Chrome Incognito veya Internet Explorer gözatma oturumu InPrivate, kişisel hesabına açmadınız emin olmak için açın.
2. Git [https://cloudpartner.azure.com/](https://cloudpartner.azure.com/).
3. Yeni bir kullanıcı ve yayımlama için oturum açma ise ilk kez portal, ardından kimliğiyle aynı e-posta olarak Geliştirici Merkezi hesabınızda oturum gerekir. Şimdi, Geliştirici Merkezi hesabına ve yayımlama portal hesabına bağlanır.

Daha sonra şirket olarak diğer üyeleriyle ekleyebilirsiniz bir [ortak yönetici](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md#4-steps-to-add-a-co-admin-in-the-publishing-portal) yayımlama portalında. Yayımlama portalında ortak yönetici olarak eklediyseniz, ortak Yönetici hesabınızla oturum imzalayabilirsiniz.

> [!TIP]
> Katılım ilkeleri üzerinde açıklanan [Azure Web sitesi](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
>
>

## <a name="create-a-new-azure-application-offer"></a>Yeni bir Azure uygulama teklifi oluşturma

Önkoşulları karşılaması sonra yönetilen uygulamayı teklifiniz oluşturmaya hazırsınız.

### <a name="set-up-an-offer"></a>Bir teklif ayarlayın

Teklif yönetilen bir uygulama için bir sınıf bir yayımcıdan sunumu ürünün karşılık gelir. Market kullanılabilir hale getirmek istediğiniz uygulama yeni bir tür varsa, bunu yeni bir teklif ayarlayabilirsiniz. Bir teklif, SKU'ları koleksiyonudur. Her teklif Market'te kendi varlık olarak görünür.

1. Oturum [bulut iş ortağı portalına](https://cloudpartner.azure.com/).

1. Sol gezinti bölmesinde seçin **+ yeni teklif** > **Azure uygulamaları**.

   ![Yeni teklif](./media/publish-marketplace-app/newOffer.png)

1. İçinde **Düzenleyicisi** görünümü, gerekli formlarına bakın. Her form bu makalenin sonraki bölümlerinde açıklanmıştır.

   ![Teklif ayarları](./media/publish-marketplace-app/newOffer_OfferSettings.png)

## <a name="offer-settings-form"></a>Teklif ayarları formu

Alanlar için **teklif ayarları** şeklindedir:

* **Teklif kodu**: Yayımcı profilindeki teklif bu benzersiz tanımlayıcı tanımlar. Bu kimliği ürün URL'ler, Resource Manager şablonları görünür ve faturalama raporlar. Yalnızca küçük harf alfasayısal karakterler veya tire (-) birleştirilebilir. Kimliği, bir tire bitemez. Buna ait sınırlı en çok 50 karakter. Bu alan, bir teklif Canlı göründükten sonra kilitlendi.
* **Yayımcı kimliği**: Bu teklif altında yayımlamak istediğiniz yayımcı profilini seçmek için bu açılan listeyi kullanın. Bu alan, bir teklif Canlı göründükten sonra kilitlendi.
* **Ad**: teklifiniz için bu görünen ad Market ve Portalı'nda görünür. En çok 50 karakter olabilir. Ürününüzün tanınabilir bir marka adını ekleyin. Nasıl pazarlama olmadığı sürece, şirketinizin adını buraya dahil etmeyin. Kendi Web sitesinde bu teklif pazarlama, adı tam olarak, Web sitenizde şu şekilde görünür durumda olduğundan emin olun.

İşiniz bittiğinde, seçin **kaydetmek** ilerleme durumunuzu kaydetmek için.

## <a name="skus-form"></a>SKU'ları formu

Sonraki adım, teklifiniz için SKU'ları eklemektir.

Bir SKU en küçük purchasable bir teklif birimidir. Aynı ürün sınıfı (teklif) içinde bir SKU arasında ayırt etmek için kullanabilirsiniz:

* Desteklenen farklı özellikleri
* Teklif mi yönetilen veya yönetilmeyen
* Desteklenen faturalama modelleri

Bir SKU Market'te üst teklif altında görüntülenir. Azure portalında purchasable kendi varlık olarak görünür.

1. Seçin **SKU'ları** > **yeni SKU**.

   ![Yeni SKU seçin](./media/publish-marketplace-app/newOffer_skus.png)

1. Girin bir **SKU kimliği**. Bir teklif içinde SKU için benzersiz bir tanımlayıcı SKU kimliğidir. Bu kimliği ürün URL'ler, Resource Manager şablonları görünür ve faturalama raporlar. Yalnızca küçük harf alfasayısal karakterler veya tire (-) birleştirilebilir. Kimliği, tire ve buna ait en çok 50 karakter sınırlı bitemez. Bu alan, bir teklif Canlı göründükten sonra kilitlendi. Bir teklif içinde birden çok SKU olabilir. Bir SKU yayımlamayı düşündüğünüz her görüntü için gerekir.

1. Doldurmak **SKU ayrıntıları** aşağıdaki formda bölümü:

   ![Yeni SKU sağlayın](./media/publish-marketplace-app/sku-settings.png)

   Aşağıdaki alanları doldurun:

   * **Başlık**: Bu SKU için bir başlık girin. Bu öğe için galerisinde bu başlığı görüntülenir.
   * **Özet**: kısa bir özeti için bu SKU girin. Bu metin başlığı altında görüntülenir.
   * **Açıklama**: SKU hakkında ayrıntılı bir açıklama girin.
   * **SKU tür**: izin verilen değerler: *yönetilen uygulamayı* ve *çözüm şablonları*. Bu durumda, seçin *yönetilen uygulamayı*.
   * **Ülke/bölge kullanılabilirliği**: yönetilen uygulamayı kullanılabilir olduğu ülkelerin seçin.

      ![Ülke seçin](./media/publish-marketplace-app/select-country.png)

   * **Fiyatlandırma**: uygulama yönetimi için bir fiyat sağlayın. Fiyat ayarlamadan önce kullanılabilir ülke seçin.

1. Yeni bir paket ekleyin. Doldurmak **Paket ayrıntılarını** aşağıdaki formda bölümü:

   ![Paket](./media/publish-marketplace-app/new-package.png)

   Aşağıdaki alanları doldurun:

   * **Geçerli sürüm**: karşıya yüklediğiniz paket için bir sürümü girin. Şu biçimde olmalıdır `{number}.{number}.{number}{number}`.
   * **Bir paket dosyası seçmek**: Bu paket .zip pakete sıkıştırılmış iki gerekli dosyaları içerir. Yönetilen uygulamayı dağıtmak için gereken kaynakları tanımlayan Resource Manager şablonu bir dosyadır. Diğer dosya tanımlar [kullanıcı arabirimi](create-uidefinition-overview.md) tüketiciler Portalı aracılığıyla yönetilen uygulamayı dağıtmak için. Kullanıcı arabiriminde parametre değerlerini sağlamak üzere tüketiciler etkinleştiren öğelerini belirtin.
   * **Principalıd**: Bu özellik bir kullanıcının, kullanıcı grubu veya verilen uygulama Azure Active Directory (Azure AD) tanımlayıcısıdır Müşteri'nin abonelik içindeki kaynaklara erişim. Rol tanımı izinleri açıklar.
   * **Rol tanımı**: Bu özellik Azure AD tarafından sağlanan tüm yerleşik rol tabanlı erişim denetimi (RBAC) rollerini listesidir. Kaynakları müşteri adına yönetmek üzere kullanmak en uygun olan rolü seçebilirsiniz.

Birden çok yetkilerini ekleyebilirsiniz. Bir AD kullanıcı grubu oluşturun ve kendi Kimliğini belirtin öneririz **Principalıd**. Bu şekilde, kullanıcı grubu SKU güncelleştirmeye gerek olmadan daha fazla kullanıcı ekleyebilirsiniz.

RBAC hakkında daha fazla bilgi için bkz: [Azure portalında RBAC ile çalışmaya başlama](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Market formu

Market form üzerinde göster alanların ister [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Önizleme abonelik kimlikleri

Azure aboneliği yayımlandıktan sonra teklif erişebileceği kimlikleri listesini girin. Bunu yapmadan önce önizleme uygulanan teklif test etmek için bu beyaz listelenen abonelikleri kullanabileceğiniz Canlı. İş ortağı portalında en fazla 100 abonelikleri beyaz listesi derleyebilirsiniz.

### <a name="suggested-categories"></a>Önerilen kategorileri

Teklifiniz en iyi ile ilişkilendirilebilir listesinden en fazla beş kategorilerini seçin. Bu kategoriler kullanılabilir olan ürün kategorilerini teklifiniz eşlemek için kullanılır [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portal](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Market

Aşağıdaki alanları, yönetilen uygulamanızın özetini görüntüler:

![Market özeti](./media/publish-marketplace-app/publishvm10.png)

**Genel bakış** aşağıdaki alanları, yönetilen uygulamanızın görüntüler sekmesi:

![Market’e genel bakış](./media/publish-marketplace-app/publishvm11.png)

**Planları + fiyatlandırma** aşağıdaki alanları, yönetilen uygulamanızın görüntüler sekmesi:

![Market planları](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure portalına

Aşağıdaki alanları, yönetilen uygulamanızın özetini görüntüler:

![Portal özeti](./media/publish-marketplace-app/publishvm12.png)

Genel bakış, yönetilen uygulamanızın için aşağıdaki alanları görüntüler:

![Portal genel bakış](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut iş ortağı Portalı'nda karşıya logo aşağıdaki yönergeleri izleyin:

*   Azure tasarım basit renk paletini sahiptir. Logonuzun ikincil renkleri ve birincil sayısını sınırlayın.
*   Tema renkleri portalı beyaz ve siyah. Bu renkler arka plan rengi olarak logonuzun için kullanmayın. Logonuzun portalında belirgin hale getirir bir renk kullanın. Basit birincil renkleri öneririz. *Saydam arka plan kullanırsanız, logo ve metin beyaz, olduğundan emin olun siyah veya mavi.*
*   Gradyan arka planı logosunu kullanmayın.
*   Metin, logo, bile, şirket veya marka adı yerleştirmeyin. Logonuzun Görünüm ve yapısını düz ve gradyan olmaması gerekir.
*   Logo uzatılmış olmadığından emin olun.

#### <a name="hero-logo"></a>Kahramanı logosu

Kahramanı logosu isteğe bağlıdır. Yayımcı kahramanı logosu yüklemek değil seçebilirsiniz. Kahramanı simgesi yüklendikten sonra silinemez. O anda iş ortağı kahramanı simgeler Market yönergeleri izlemeniz gerekir.

Kahramanı logo simgesini için aşağıdaki yönergeleri izleyin:

*   Yayımcı görünen adı, planı başlık ve uzun Özet teklif beyaz görüntülenir. Bu nedenle, açık bir renk kahramanı simgesi arka plan için kullanmayın. Siyah, beyaz veya saydam arka plan kahramanı simgelerini izin verilmiyor.
*   Teklif listelenen sonra öğeleri kahramanı logosunun içinde programlı olarak katıştırılır. Katıştırılmış öğelerini dahil yayımcı görünen adı, planı başlık, uzun Özet teklif ve **oluşturma** düğmesi. Sonuç olarak, kahramanı logosu tasarlarken herhangi bir metin girmeyin. Metni program aracılığıyla bu alana dahil olduğundan boş alanı sağ tarafta bırakın. Metin için boş alan sağdaki 415 x 100 piksel olmalıdır. Soldan 370 piksel uzakta bulunur.

    ![Kahramanı logosu örneği](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Form desteği

Doldurmak **Destek** kişiler şirketinizden desteğiyle formu. Bu bilgileri, kişiler ve müşteri destek ilgili kişisi mühendislik.

## <a name="publish-an-offer"></a>Bir teklifi yayımlama

Tüm bölümleri doldurduktan sonra seçin **Yayımla** teklifiniz müşteriler için kullanılabilir hale getirir işlemini başlatmak üzere.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Hizmet Kataloğu yönetilen uygulama yayımlama hakkında daha fazla bilgi için bkz: [oluşturma ve bir hizmet Kataloğu yönetilen uygulamayı yayımlayın](publish-service-catalog-app.md).