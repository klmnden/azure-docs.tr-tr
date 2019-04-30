---
title: Müşteri adayları yapılandırma | Microsoft Docs
description: Bulut iş ortağı portalında müşteri adayları yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 2a425e607ea7dac394ab90a3fed4d4026056bbc1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61069635"
---
<a name="get-customer-leads"></a>Müşteri adayları alma
==================

Bu makalede, bulut iş ortağı portalını kullanarak müşteri adayları oluşturmak açıklanmaktadır. Bu müşteri adayları, CRM sistemine bağlanmak ve bunları satış ardışık düzeninizi tümleştirme.

## <a name="leads"></a>Müşteri adayları

Müşteri adayları, ilgilendiğiniz veya, bir üründen dağıtma müşteriler olan [Azure Marketi](https://azuremarketplace.microsoft.com/) veya [AppSource](https://appsource.microsoft.com).

### <a name="azure-marketplace"></a>Azure Market

1.  Müşteri, bir "Test Sürüşü" teklifinizin alır. Test sürücüleri işletmenizi anında girişi herhangi bir tanımadan potansiyel müşterilerle paylaşmak hızlandırılmış bir fırsat sağlar. Tüm Test Sürüşlerine daha fazla bilgi için ürününüzün çalışırken ilgileniyor bir müşteri için bir müşteri adayı oluşturun. Test sürücülerine hakkında daha fazla bilgi [Azure Marketi'nde Test Sürüşü](https://azuremarketplace.azureedge.net/documents/azure-marketplace-test-drive-program.pdf).

    ![Market test sürücü örnekleri](./media/cloud-partner-portal-get-customer-leads/test-drive-offer.png)
 

<!-- -->

1. Müşteri "şimdi edinin" seçtikten sonra bilgi paylaşımı için toplanmasına onay verir. Bu müşteri adayı bir **ilk ilgi** lideri, ürününüzü alınırken ilgi ifade müşteri hakkındaki bilgiler paylaşacağı ediyoruz. Müşteri adayının üst edinme huninin bulunur.

   ![Şimdi seçeneği Al](./media/cloud-partner-portal-get-customer-leads/get-it-now-button.png)

1. Müşteri "Satın Al" seçer [Azure portalı](https://portal.azure.com/) ürününüzü almak için. Bu müşteri adayı bir **etkin** lideri, ürününüzü dağıtmaya başladı bir müşteri hakkındaki bilgiler paylaşacağı ediyoruz.

   ![Satın alma seçeneği](./media/cloud-partner-portal-get-customer-leads/purchase-button.png)


### <a name="appsource"></a>AppSource

1.  Müşteri, bir "Test Sürüşü" teklifiniz sürdü. Test sürücüleri işletmenizi anında girişi herhangi bir tanımadan potansiyel müşterilerle paylaşmak hızlandırılmış bir fırsat sağlar. Tüm Test Sürüşlerine, bir müşteri adayı, ilgileniyor bir müşterinin çalışırken daha fazla bilgi için ürün oluşturur. Test sürücülerine hakkında daha fazla bilgi [AppSource Test Sürüşü](https://appsource.microsoft.com/blogs/want-to-try-an-app-take-a-test-drive).

    ![Test sürücü örneği](./media/cloud-partner-portal-get-customer-leads/test-drive-offer-2.png)

2.  Müşteri "şimdi edinin" seçtikten sonra bilgi paylaşımı için toplanmasına onay verir. Bu müşteri adayı bir **ilk ilgi** lideri, ürününüzü alınırken ilgi ifade müşteri hakkındaki bilgiler paylaşacağı ediyoruz. Müşteri adayının üst edinme huninin bulunur.

      ![Şimdi seçeneği Al](./media/cloud-partner-portal-get-customer-leads/get-it-now-button-2.png)


3.  "Bana teklifinizi başvurun" Müşteri seçer. Bu müşteri adayı bir **etkin** lideri, biz ile izlenmesi gereken ürününüzü hakkında soran bir müşteri hakkındaki bilgiler paylaşacağı.

    ![Seçenek olarak benimle iletişim kurun](./media/cloud-partner-portal-get-customer-leads/contact-me-image.png)

<a name="lead-data"></a>Veri sağlama
---------

Müşteri edinme işlemi sırasında aldığınız her sağlama veri belirli alanlara sahiptir. Birden çok adımlardan müşteri adayı elde edecekleriniz olduğundan, müşteri adaylarını işlemek için en iyi XML'deki yinelenen ve izlemeler kişiselleştirmek için yoludur. Bu şekilde, uygun bir mesaj her müşteri doluyor ve benzersiz bir ilişki oluşturduğunuzu.

### <a name="lead-source"></a>Kaynak sağlama

Bir müşteri adayı kaynağı biçimi **kaynak**-**eylem** |  **sunar**

**Kaynakları**: "AzureMarketplace", "Raporundaki", "TestDrive" ve "AppSource (SPZA)"

**Eylemler**:
- "İşlemleri"--yükleme. Bir müşteri, ürün satın bu işlem Azure Market veya Appsource'ta andır.
- ". Sys"--temsil eden iş ortağı için deneme gerektiriyordu. İlgili kişi bana bir müşterinin kullandığı bu eylem Appsource'ta seçenektir.
- "DNC"--sizinle iletişime geçmez. Bu eylem, uygulama sayfada listelenen çapraz bir ortak irtibat kurulmasını istenen Appsource'ta andır. Biz bu müşteri Çapraz Yukarı uygulamanızda listelenen heads paylaşıyorsanız, ancak kurulması gerekmez.
- "Oluştur"--Bu eylem, yalnızca Azure portalı içinde ve bir müşteri hesabını teklifinizin satın aldığında oluşturulur.
- "StartTestDrive"--Bu eylem yalnızca Test sürücüler için içindir ve bir müşterinin kendi test sürüşü başlatıldığında oluşturulur.

**Sunar**

Aşağıdaki örnekler bir yayımcı ve belirli bir teklif atanan benzersiz tanımlayıcıları: checkpoint.check-noktası-r77-10sg-klg bitnami.openedxcypress ve docusign.3701c77e-1cfa - 4c 56 91e6 3ed0b622145a.


### <a name="customer-info"></a>Müşteri bilgileri

Alanları aşağıdaki örnekte, bir müşteri adayı bulunan müşteri bilgileri gösterir.
- Ad: John
- Soyadı: Smith
- E-posta: jsmith\@microsoft.com
- Telefon: 1234567890
- Ülke: ABD
- Şirket: Microsoft
- Başlık: CTO

>[!Note]
>Önceki örnekte tüm veriler her zaman her müşteri adayı için kullanılabilir.

Etkin bir şekilde müşteri adayları, geliştirme üzerinde çalışmakta olduğumuz bunu burada görmüyorsanız, ancak, lütfen olmasını istediğiniz bir veri alanı yoksa [görüşlerinizi bize bildirin](mailto:AzureMarketOnboard@microsoft.com).

<a name="how-to-connect-your-crm-system-with-the-cloud-partner-portal"></a>Bulut iş ortağı portalı ile CRM sisteminize bağlanma
------------------------------------------------------------

Müşteri adayları alma başlatmak için bizim sağlama Yönetimi Bağlayıcısı bulut iş ortağı portalında kolayca CRM bilgilerinizi takılabilir ve bağlantınızı bulunacağız oluşturduk. Artık bir dış sistemle tümleştirmek için önemli bir mühendislik çabası olmadan Market tarafından oluşturulan müşteri adayları kolayca yararlanabilirsiniz.

![Tedarik Yönetimi Bağlayıcısı](./media/cloud-partner-portal-get-customer-leads/lead-management-connector.png)

Ancak istediğiniz bir Azure depolama için doğrudan CRM sistemleri ve çeşitli adaylarını müşteri adaylarını yönetebileceğiniz tablo yazabiliriz. Aşağıdaki bağlantıların her birini olası müşteri adayı hedeflere bağlanma yönergeleri sağlar:

-   [Dynamics CRM Online](./cloud-partner-portal-lead-management-instructions-dynamics.md) Dynamics CRM Online müşteri adayları almak için nasıl yapılandırılacağı hakkında yönergeler alın.
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md) adayları alma Marketo sağlama yapılandırmasını ayarlama yönergeleri almak için.
-    [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md) adayları alma Salesforce örneğinizin ayarlamaya yönelik yönergeler alın.
-    [Azure tablo](./cloud-partner-portal-lead-management-instructions-azure-table.md) bir Azure tablosu müşteri adayları alma için Azure depolama hesabınızı ayarlamaya yönelik yönergeler alın.
-   [HTTPS uç noktası](./cloud-partner-portal-lead-management-instructions-https.md) müşteri adayları almak, Https uç noktası ayarlamaya yönelik yönergeler alın.

Müşteri adayı hedefiniz yapılandırmak ve teklifinizi yayımlayın sonra biz bağlantıyı doğrulamak ve bir test müşteri adayı gönderdiğiniz. Teklif kendiniz Önizleme ortamı elde etmeye çalışarak, kullanıma sunulmadan önce teklif görüntülerken, müşteri adayı bağlantınızı sınayabilirsiniz. Böylece tüm müşteri adaylarını kaybetmeyin güncel, müşteri adayı ayarları kalın bu nedenle bir şey sizin değiştirildiğinde bu bağlantıları güncelleştirme emin olun, emin olmak önemlidir.

<a name="what-next"></a>Şimdi ne yapacaksınız?
----------

Ayarlanan teknik yerleştirildikten sonra bu müşteri adayları geçerli satış ve pazarlama stratejisi ve çalışma süreçleri eklemeniz gerekir. Genel satış işleme ve yakından yüksek kaliteli müşteri adaylarını ve başarılı olmasını sağlamak için yeterli veri sağlamak sizinle birlikte çalışmak istediğiniz daha iyi anlamak çok ilgi duyuyoruz. Nasıl size en iyi duruma getirme ve bu müşterilerin başarılı olmasına yardımcı olmak için ek veri göndereceğiz müşteri adaylarını geliştirmek üzerinde bildirimleriniz bizim için değerli. İlginizi çeken bize bildirin [geri bildirim sağlama](mailto:AzureMarketOnboard@microsoft.com) ve Satış ekibinize Market müşteri adayları ile daha başarılı olması etkinleştirmek için öneriler.
