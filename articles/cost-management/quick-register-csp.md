---
title: "Azure maliyeti yönetimiyle CSP iş ortağı bilgileri kullanarak kaydolun | Microsoft Docs"
description: "Azure maliyeti Yönetimi Cloudyn tarafından ile kaydetmek için CSP iş ortağı bilgileri kullanın."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/13/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 84f2fec61f791d4fc9264eaa01e24180696da853
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="register-with-the-csp-partner-program-and-view-cost-data"></a>Görünüm veri maliyet ve CSP iş ortağı programı ile kaydetme

CSP iş ortağı olarak, Azure maliyeti Yönetimi Cloudyn tarafından ile kaydedebilirsiniz. Kaydınızı Cloudyn portalına erişim sağlar. Bu hızlı başlangıç ayrıntıları kayıt işlemini Cloudyn deneme aboneliği oluşturmak ve Cloudyn portalında oturum açmak için gerekli. Ayrıca, maliyet verileri hemen görüntüleme başlatmak nasıl gösterir.


>[!NOTE]

>Yalnızca CSP doğrudan iş ortakları ve CSP dolaylı sağlayıcıları Cloudyn kayıt tamamlayabilirsiniz.
>
>İş ortağı merkezi API yapılandırma, kimlik doğrulama ve veri erişimi için gereklidir. Bir iş ortağı merkezi genel yönetici hesabını API erişimini sağlamak için gereklidir.
Daha fazla bilgi için bkz: [iş ortağı merkezi API Bağlan](https://msdn.microsoft.com/library/partnercenter/mt709136.aspx).
>
>CSP dolaylı sağlayıcılarına Cloudyn ile kaydolduktan sonra Cloudyn erişimi CSP dolaylı yetkili satıcılar için kullanılabilir hale getirilebilir. CSP dolaylı satıcılar, ardından Azure müşteriler ve abonelikleri Cloudyn erişim sağlayabilir.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

- http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-trial-registration"></a>Bir deneme kaydı oluşturun

1. Azure portalında tıklatın **Yönetimi maliyeti + faturalama** Hizmetler listesinde.
2. Altında **genel bakış**, tıklatın **Yönetimi maliyeti**  
    ![Yönetim sayfasında maliyet](./media/quick-register-csp/cost-mgt-billing-service.png)
3. Üzerinde **Yönetimi maliyeti** sayfasında, **Yönetimi maliyeti Git** Cloudyn kayıt sayfasına yeni bir pencerede açmak için.
4. Cloudyn portal deneme kaydı sayfasında, şirketinizin adını yazın, seçin **Microsoft CSP iş ortağı programı yönetici**ve ardından **sonraki**.  
5. Girin bir **uygulama kimliği**, **Commerce Kimliğini**, **uygulama gizli anahtar**seçip **varsayılan fiyatlandırma planı**. Bilgilerin elinizin altında yoksa, oturum açtığınızda iş ortağı merkezi portalında [https://partnercenter.microsoft.com](https://partnercenter.microsoft.com) birincil yöneticinizle birlikte hesap ve aşağıdaki adımları uygulayın:
  1. Git **Pano** > **hesap ayarları** > **Uygulama Yönetimi**.
  2. Daha önce bir Web uygulaması oluşturduysanız, bu adımı atlayın. Aksi takdirde tıklatın **Ekle Yeni web uygulaması** içinde **Web uygulaması** bölümü.
  3. Kopya **uygulama kimliği** web uygulamanızdan GUID.
  4. Kopya **Commerce Kimliğini** web uygulamanızdan GUID.
  5. Anahtar geçerlilik süresi, gerektiğinde bir veya iki yıl seçin. Seçin **Ekle anahtarı** kopyalayabilir ve gizli anahtar değer kaydedin.  
    ![CSP iş ortağı Merkezi](./media/quick-register-csp/csp-partner-center.png)
  6. Kayıt sayfasına geri dönün ve bilgileri yapıştırın.  
      ![CSP hesabı kimlik bilgileri](./media/quick-register-csp/csp-reg.png)
6. Kullanım hükümlerini kabul etmeniz sonra bilgilerinizi doğrulayın. Tıklatın **sonraki** Cloudyn Azure kaynak veri toplamak üzere yetkilendirmek için. Toplanan verileri kullanım, performans, faturalama ve aboneliklerinizi etiket verilerini içerir.  
7. Altında **diğer Paydaşlar davet**, kullanıcıların e-posta adreslerini yazarak ekleyebilirsiniz. Tamamlandığında, tıklayın **sonraki**. Cloudyn için eklenen tüm faturalama verileriniz için yaklaşık iki saat sürer.
8. Tıklatın **Cloudyn için Git** Cloudyn Portalı'nı açmak için ve daha sonra **bulut hesap yönetimi** sayfasında, kayıtlı CSP hesap bilgilerinizi görmeniz gerekir.

## <a name="configure-indirect-csp-access-in-cloudyn"></a>İçinde Cloudyn dolaylı CSP erişimi yapılandırma

Varsayılan olarak, iş ortağı merkezi API yalnızca CSP'ler doğrudan erişilebilir. Ancak, doğrudan bir CSP sağlayıcı erişim dolaylı CSP müşterileri veya ortakları Cloudyn içinde varlık gruplarını kullanarak yapılandırabilirsiniz.

Dolaylı CSP müşterileri ve ortakları için erişimi etkinleştirmek için adımları [bir deneme kaydı oluşturmak](#create-a-trial-registration) deneme kaydı oluşturma için. Ardından, Cloudyn varlık gruplarını kullanarak segment dolaylı CSP veri için aşağıdaki adımları tamamlayın. Ardından, uygun kullanıcı izinlerini varlık gruplarını atayın.

1. Bilgilerine sahip bir varlık grubu oluşturmak [varlıkları oluşturma](tutorial-user-access.md#create-entities).
2. Bölümündeki adımları izleyin [abonelikleri maliyet varlıklara atama](https://support.cloudyn.com/hc/en-us/articles/115005139425-Video-Assigning-subscriptions-to-Cost-Entities). Dolaylı CSP müşterinin hesabına ve bunların Azure abonelikleri daha önce oluşturduğunuz bir varlığa ilişkilendirin.
3. Bölümündeki adımları izleyin [yönetici erişimi olan bir kullanıcı oluşturma](tutorial-user-access.md#create-a-user-with-admin-access) yönetici erişimi bir kullanıcı hesabı oluşturmak için. Ardından, kullanıcı hesabı dolaylı hesabı için daha önce oluşturduğunuz belirli varlıklar için yönetici erişimi olduğundan emin olun.

Bunlar için oluşturulan hesapları kullanarak Cloudyn portalı dolaylı CSP ortakları oturum açın.


[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç, CSP bilgilerinizi maliyet yönetimi ile kaydetmek için kullanılır. Ayrıca Cloudyn Portalı'na imzalanmış ve maliyet verileri görüntüleme başlatıldı. Azure maliyeti Yönetimi Cloudyn tarafından hakkında daha fazla bilgi için maliyet yönetimi için öğreticisi için devam edin.

> [!div class="nextstepaction"]
> [Gözden geçirme kullanım ve maliyetler](./tutorial-review-usage.md)
