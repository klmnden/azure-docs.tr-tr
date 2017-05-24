---
title: "Microsoft Azure portalına genel bakış"
description: "Microsoft Azure portalını kullanmayı öğrenin."
services: 
documentationcenter: 
author: davidwrede
manager: erikre
editor: jimbe
ms.assetid: 53cb9df1-c96a-4f4e-b022-18336cd3d697
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/16/2015
ms.author: dwrede
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: e4c1c2b956b2cae673e60223ce777ba1096dce33
ms.contentlocale: tr-tr
ms.lasthandoff: 05/12/2017


---
# <a name="microsoft-azure-portal-overview"></a>Microsoft Azure portalına genel bakış
Microsoft Azure portalı, Azure kaynaklarınızı sağlayabileceğiniz ve yönetebileceğiniz merkezi bir yerdir.  Bu öğretici, portal hakkında bilgi edinmenizi sağlar ve şu temel özellikleri nasıl kullanacağınızı gösterir:

* **Kapsamlı bir market**, Microsoft ve diğer sağlayıcılar tarafından sunulan satın alabileceğiniz ve/veya sağlayabileceğiniz binlerce öğeye göz atmanızı sağlar.
* **Birleştirilmiş ve ölçeklenebilir gezinme deneyimi** önem verdiğiniz kaynakları bulmanızı ve çeşitli yönetim işlemlerini gerçekleştirmenizi kolaylaştırır.
* **Tutarlı yönetim sayfaları** (veya dikey pencereler) ayarları, eylemleri, fatura bilgilerini, durum izleme ve kullanım verilerini ve çok daha fazlasını tutarlı bir yolla kullanıma sunarak Azure'un çok çeşitli hizmetlerini yönetmenize olanak tanır.
* **Kişisel bir deneyim** oturum açtığınızda görmek istediğiniz bilgileri gösteren özelleştirilmiş bir başlangıç ekranı oluşturmanızı sağlar.  Kutucuk içeren tüm yönetim dikey pencerelerini özelleştirebilirsiniz.
  
  ![Azure Portal UI Yönlendirme][UIOrientation]

## <a name="before-you-get-started"></a>Başlamadan önce
Bu öğreticiyi incelemek için geçerli bir Azure aboneliğinizin olması gerekir.  Bir aboneliğiniz yoksa, hemen şimdi [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial/).  Abonelik aldıktan sonra, <https://portal.azure.com> adresinden portala erişebilirsiniz.

## <a name="how-to-create-a-resource"></a>Kaynak oluşturma
Azure’da, tek bir yerden oluşturabileceğiniz binlerce öğe içeren bir market bulunur.  Yeni bir Windows Server 2012 VM oluşturmak istediğinizi düşünelim.  +YENİ hub’ı, markette öne çıkan bir dizi seçkin kategoriye giriş noktanızdır.  Her kategoride bir dizi öne çıkan öğenin yanı sıra tüm kategorileri ve arama işlevini gösteren tüm markete bir bağlantı bulunur. Yeni bir Windows Server 2012 VM oluşturmak için aşağıdaki eylemleri gerçekleştirin:  

1. Windows Server 2012 öne çıkan bir öğedir, bu nedenle İşlem kategorisinden seçilebilir.  
2. Forma bazı temel bilgileri girin.
3. ‘Oluştur’a tıkladığınızda sanal makineniz anında sağlama işlemine başlar.

Bildirim hub'ı kaynağınız oluşturulduğunda ve yönetim dikey penceresi açılacağı zaman sizi uyarır (isterseniz daha sonra kaynaklara göz atabilirsiniz).

![Portal Kategorileri][PortalCategories]

## <a name="how-to-find-your-resources"></a>Kaynaklarınızın bulma
Sık erişilen kaynaklar başlangıç panonuza sabitleyebilirsiniz, ancak sık erişmediğiniz bir kaynağı bulmak için gezinmeniz gerekebilir.  Aşağıda gösterilen gözatma hub’ı tüm kaynaklarınızı almanın bir yoludur.  Aboneliğe göre filtre uygulayabilir, sütunları seçebilir/yeniden boyutlandırabilir ve tek tek öğelere tıklayarak yönetim dikey pencerelerine gidebilirsiniz.

![Gözatma Hub’ı][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Kaynağa erişimi yönetme ve erişimi devretme
Bu dikey pencereden, uzak masaüstünü kullanarak sanal makineye bağlanabilir, temel performans ölçümlerini izleyebilir, rol tabanlı erişim (RBAC) kullanarak bu sanal makineye erişimi denetleyebilir, sanal makineyi yapılandırabilir veya diğer önemli yönetim görevleri gerçekleştirebilirsiniz.  Rol tabanlı olarak erişim devretme büyük ölçekli yönetim görevleri için kritik önem taşır.  Daha fazla bilgi edinmek için [buraya](active-directory/role-based-access-control-configure.md) tıklayın. Bir kaynağa erişim devretmek için aşağıdaki eylemleri gerçekleştirin:

1. Kaynağınızı bulun.
2. Temel Parçalar bölümünde ‘Tüm Ayarlar'a tıklayın.
3. Ayarlar listesinde ‘Kullanıcılar’a tıklayın.
4. Komut çubuğunda ‘Ekle’ye tıklayın.
5. Bir kullanıcı ve rol seçin.

![Kaynak Yönetme][ManageResource]

## <a name="how-to-get-help"></a>Yardım alma
Bir sorununuz varsa, size yardımcı olmaya hazırız.  Portalda sağ tarafta görebileceğiniz üzere bir yardım ve destek sayfası vardır.  Ayrıca, [destek planınıza](https://azure.microsoft.com/support/plans/) bağlı olarak doğrudan portal üzerinden destek bileti oluşturabilirsiniz.  Bir destek bileti oluşturduktan sonra portal üzerinden bu biletin yaşam döngüsünü yönetebilirsiniz. Gözat -> Yardım + destek bölümüne giderek yardım ve destek sayfasına ulaşabilirsiniz.  

![Yardım ve destek][HelpSupport]

## <a name="summary"></a>Özet
Şimdi bu öğreticide öğrendiklerinizi gözden geçirelim:

* Kaydolmayı, abonelik almayı ve portalda gezinmeyi öğrendiniz
* UI portalı hakkında bilgi edindiniz ve kaynak oluşturmayı ve kaynaklara göz atmayı öğrendiniz
* Kaynak oluşturmayı ve kaynaklara göz atmayı öğrendiniz
* Dikey pencerelerin yapısı veya yönetimi ve farklı türdeki kaynakların tutarlı şekilde yönetilmesi hakkında bilgi edindiniz
* Sizin için önemli bilgileri ön plana çıkarmak üzere portalı nasıl özelleştireceğinizi öğrendiniz
* Rol tabanlı erişim (RBAC) kullanarak kaynaklarına erişimi denetlemeyi öğrendiniz
* Yardım ve destek alma yollarını öğrendiniz

Microsoft Azure portalını uygulamalarınızı bulutta oluşturma ve yönetme sürecini önemli ölçüde basitleştirir.  [Geri bildirimlerinizi önemsiyor](https://feedback.azure.com/forums/223579-azure-preview-portal/) ve buna uygun geliştirmeler yapıyoruz. En son gelişmelerden haberdar olmak için [yönetim bloguna](https://azure.microsoft.com/blog/topics/management/) göz atın.   Tüm Azure güncelleştirmelerini görebileceğiniz için bir diğer önemli yer ise [ScottGu’nun blogudur](http://weblogs.asp.net/scottgu).

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png

