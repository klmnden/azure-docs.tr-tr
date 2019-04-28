---
title: Mantıksal uygulamayı Test Sürüşü | Microsoft Docs
description: Bir Dynamics AX/CRM örneğiyle veya yalnızca Azure dışında herhangi bir kaynağa bağlandığında, Test Sürüşü oluşturulacağını açıklar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 4fd946b53956509844ad0a9396575f1ee2450414
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61070461"
---
<a name="logic-app-test-drive"></a>Mantıksal uygulamayı Test Sürüşü
====================

Bu makalede, Appsource'ta teklifini sahip ve Dynamics AX/CRM örneğinde veya yalnızca Azure dışında herhangi bir kaynağa bağlandığında, Test Sürüşü oluşturmak isteyen yayımcılar içindir.

<a name="how-to-build-a-logic-app-test-drive"></a>Mantıksal uygulamayı Test Sürüşü oluşturma
-----------------------------------

Mantıksal uygulamayı Test Sürüşleri şu an için GitHub üzerindeki hala için sürücü belgeleri test [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app)gidin daha fazla bilgi için vardır.

<a name="how-to-publish-a-test-drive"></a>Bir Test sürüşüne yayımlama
---------------------------

Oluşturulan Test Sürüşünüz olduğuna göre bu bölümde, başarılı bir şekilde Test Sürüşünüz yayımlamak gerekli alanların her biri açıklanmaktadır.

![Test Sürüşü özelliği etkinleştirin](./media/azure-resource-manager-test-drive/howtopub1.png)

İlk ve en önemli alan Test formun tüm gerekli alanları doldurun, sunulur isteyip istemediğinizi geçiş yapmak için kullanılır. Seçtiğinizde, **Hayır** form devre dışı kalır ve devre dışı Test Sürüşü ile yeniden yayımlarsanız, Test Sürüşünüz üretimden kaldırılır.

*Not*: Etkin kullanıcılar tarafından kullanılan tüm Test Sürüşlerine yoktur, bu Test Sürüşleri oturumun süresi dolana kadar çalışmaya devam eder.

### <a name="details"></a>Ayrıntılar

Doldurmak için sonraki bölüme, Teklif Ayrıntıları, Test Sürüşü hakkında ' dir.

![Test sürücü ayrıntıları](./media/azure-resource-manager-test-drive/howtopub2.png)

**Açıklama -** *[gerekli alan]* budur burada Test Sürüşünüz nedir hakkında temel bir açıklama yazın. Müşteri, hangi senaryolarını Test Sürüşünüz ürününüzü hakkında kapsayan okumak için buraya gelir. 

**Kullanıcı el ile -** *[gerekli alan]* Test Sürüşü deneyiminizin ayrıntılı izlenecek yolu budur. Müşteri, bu açılır ve tam olarak kendi Test Sürüşü yapmasını istediğiniz aracılığıyla size yol. Bu içeriği kolayca izleyin ve anlamak önemlidir! (.Pdf dosyası olması gerekir)

**Test sürücü tanıtım videosu -** \[önerilen\] benzer kullanıcı el ile Test Sürüşü deneyiminizin videosu dahil en iyisidir. Müşteri, bu önceki ya da kendi Test Sürüşü sırasında izleyecek ve tam olarak kendi Test Sürüşü yapmasını istediğiniz aracılığıyla size yol. Bu içeriği kolayca izleyin ve anlamak önemlidir!

- **Ad** -videonuzu başlığı
- **Bağlantı** -YouTube veya Vimeo katıştırılmış bir URL olmalıdır. Katıştırılmış URL'sini alma hakkında bir örnek aşağıda verilmiştir:
- **Küçük resim** -yüksek kaliteli görüntü (533 x 324) piksel olmalıdır. Test Sürüşü deneyiminizi kısmı görüntüsü burada olması önerilir.

Nasıl bu alanlar, müşteri için Test Sürüşü deneyimlerini sırasında görünmesini aşağıdadır.

![Test sürücü alanları Görünüm](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Teknik yapılandırma

Burada Test sürücü mantıksal Uygulamanızı yapılandırmak ve nasıl özellikle tanımlamak doldurmak için sonraki bölüme olan Test Sürüşünüz iş örnekler.

![Test sürücü teknik yapılandırması](./media/azure-resource-manager-test-drive/howtopub5_logicapp.png)

- **Bölge** - *[Field gerekli]* seçtiğiniz Burada, sürücü mantıksal uygulamayı test etme kaynaklarınızın dağıtıldığı çekme bölgedir.

    *Not:* Mantıksal uygulamanızı bir bölgede depolanır tüm özel kaynaklar varsa, bu bölgeye burada seçili olduğundan emin olun. Bunu yapmak için en iyi yolu **tamamen mantıksal uygulamanızı yerel olarak Şirket portalı, Azure aboneliğinize dağıtmak ve çalışır durumda olduğunu doğrulayın** burada yazmadan önce.

- **En fazla eş zamanlı Test Sürüşleri** - *[Field gerekli]* dağıtılan ve bekleniyor zaten olan sayı, Test Sürüşü yapmasını örneklere erişmek seçili bölge başına. Müşteriler, bir dağıtım için beklemek zorunda yerine bu Test Sürüşleri anında erişebilirsiniz.

    *Not:* Tüm, N Öğrenci sayısını içeren bir Test sürüşü için istediğiniz bir Web Semineri/class çalıştırıyorsanız, yayımlama önerilir N sık erişimli örnekleri ve sonra bir kez ile sınıfı üzerinde yeniden yayımlamanız için sık erişimli örnekleri normal sayısı geri sayısıdır.

- **Test sürücü süresi (saat) -** *[Field gerekli]* ne kadar Test Sürüşü içinde etkin kalacak süre \# saat. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer.

- **Azure kaynak grubu adı -** *[gerekli alan]* yazma, mantıksal uygulamayı Test Sürüşleri kaydedildiği kaynak grubu adı.

- **Mantıksal uygulama adı - Ata** *[gerekli alan]* yazma, müşteri alır önce bir Test Sürüşü kullanıcı atamak için kullanılan uygulama mantığındaki burada mantıksal uygulama adını yazın. Bu dosya, yukarıdaki kaynak grubunda kaydedildiğinden emin olun.

- **Mantıksal uygulama adı - sağlamasını kaldırma** *[gerekli alan]* Test Sürüşü içinde oluşturulan tüm kaynakları sağlamasını için mantıksal uygulama adını yazın. Bu dosya, yukarıdaki kaynak grubunda kaydedildiğinden emin olun.

- **Erişim bilgileri -** *[gerekli alan]* bir müşteri, Test Sürüşü aldıktan sonra erişim bilgileri kullanıcılara sunulur. Bu yönergeler, Test sürücü Resource Manager şablonunuzu yararlı çıkış parametrelerini paylaşmak için yöneliktir. Çıktı parametreleri eklemek için çift kaşlı ayraçlar kullanın (örneğin, **{{outputname}}**), ve konumda doğru eklenir. (HTML biçimlendirme dizesi burada ön uç işleme için önerilir).

### <a name="test-drive-deployment-subscription-details"></a>Test Sürüşü dağıtım Abonelik Ayrıntıları

Doldurmak için son bölümü, Test sürücüleri otomatik olarak Azure aboneliğinizi ve Azure Active Directory (AD) bağlanarak dağıtabilmesini sağlamaktır.

![Test sürücü dağıtım Abonelik Ayrıntıları](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure abonelik kimliği** *[Field gerekli]* bu Azure hizmetlerini ve Azure portalına erişim verir. Burada kullanım raporlama ve Hizmetleri faturalandırılır aboneliktir. Zaten yoksa bir **ayrı** Azure aboneliği için Test Sürüşleri yalnızca, Lütfen bir tane biri olun. Azure abonelik kimlikleri, Azure portalında oturum açıyorsanız ve sol taraftaki menüyü Aboneliklerde giderek bulabilirsiniz.
(Örnek: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Azure Abonelikleri](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD Kiracı kimliği** *[Field gerekli]* bir kiracı bulabilirsiniz altındaki özellikler - kimliği zaten mevcut varsa\> dizin kimliği

![Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails3.png)

Aksi takdirde, Azure Active Directory'de yeni bir kiracı oluşturun.

![Azure Active Directory özellikleri ekran](./media/azure-resource-manager-test-drive/subdetails4.png)

! Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails5.png)

![Azure Active Directory kiracıları](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD uygulama kimliği** *[Field gerekli]* oluşturmak ve yeni bir uygulamayı kaydetmek için bir sonraki adım olacaktır. Test Sürüşü örneğinizin işlemleri gerçekleştirmek için bu uygulamayı kullanacağız.

1. Yeni oluşturduğunuz dizine gidin veya dizin zaten var ve Azure Active directory içinde Filtre bölmesini seçin.
2. "Uygulama kayıtları" ve "Ekle" ye tıklayın.
3. Bir uygulama adı sağlayın.
4. Türü olarak seçin "Web uygulaması / API'si"
5. Oturum açma URL'si herhangi bir değer girin, biz de kazandık\'t bu alanı kullanıyor.
6. Oluştur'a tıklayın.
7. Uygulama oluşturulduktan sonra - özelliklerine gidin\> çok kiracılı uygulama ayarlayın ve Kaydet'e basın.

Kaydet’e tıklayın. Son adım, bu kayıtlı uygulama için uygulama Kimliğini alın ve burada Test Sürüşü alana yapıştırın sağlamaktır.

![Azure Active Directory Uygulama Kimliği](./media/azure-resource-manager-test-drive/subdetails7.png)

Verilen kullanıyoruz uygulamayı aboneliğinize dağıtmak için biz uygulamanın abonelik üzerinde katkıda bulunan olarak eklemeniz gerekir. Bu yönergeleri olarak olan aşağıda:

1. Abonelikler dikey penceresine gidin ve yalnızca Test Sürüşü için kullanmakta olduğunuz uygun aboneliği seçin.
1. Tıklayın **erişim denetimi (IAM)**.
1. Tıklayın **rol atamaları** sekmesi.  ![Yeni bir erişim denetimi sorumlusu ekleme Azure Active Directory](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. Tıklayın **rol ataması Ekle**.
1. Rol olarak ayarla **katkıda bulunan**.
1. Azure AD uygulama adını yazın ve rol atamak için uygulamayı seçin.
    ![Azure Active Directory izinleri](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. **Kaydet**’e tıklayın.

**Azure AD uygulama anahtarı -** *[gerekli alan]* bir kimlik doğrulama anahtarını oluşturmak için son alandır. Anahtarı altında anahtarı bir açıklama ekleyin, ardından süresiz olarak süresini Kaydet'i belirleyin. Bu **önemli** süresi dolmuş zorunda kalmamak için anahtar, hangi test sürüşünüz üretimde çalışmamasına neden olur. Bu değeri kopyalayın ve gerekli Test Sürüşü alanına yapıştırın.

![Azure Active Directory anahtarları bölümüne](./media/azure-resource-manager-test-drive/subdetails8.png)

> [!CAUTION]
> Bir base64 kodlu anahtarı şu anda oluşturmaz, çünkü Azure uygulama kayıt önizlemesi kullanamazsınız.


<a name="next-steps"></a>Sonraki adımlar
----------

Test Sürüşü alanlarınızı doldurulan sahip olduğunuza göre üzerinden geçmek ve **yeniden yayımlamanız** teklifinizi. Test Sürüşünüz ve sertifika alma süreci geçtikten sonra gitmesi gereken bir müşteri deneyimini Java'da test **Önizleme** teklifinizin. Test Sürüşü kullanıcı Arabiriminde başlatın ve Test Sürüşleri tam olarak doğru şekilde dağıtıldığını doğrulayın.

Bir müşteri ile tamamlandıktan sonra Test Sürüşü hizmeti otomatik olarak bu kaynak gruplarını temizler, böylece müşterileriniz için hazırlanan gibi herhangi bir bölümünü Test Sürüşü silmeyin dikkat edin önemlidir.

Şimdi Önizleme teklifinizle birlikte hissedene sonra durumuna gelir **yayınlayın**! Teklif çift yayımlanan onay uçtan uca deneyiminin tamamı silindikten sonra Microsoft son gözden geçirme işleminden yoktur. Herhangi bir nedenden dolayı teklifini reddetti, teklifinizi ne düzeltilecek gerekenleri mühendislik birimi ilgili kişisi için size bildirim göndereceğiz.

Lütfen Git başka sorularım varsa, sorun giderme önerilerine aradığınız veya Test Sürüşünüz daha da başarılı hale getirmek istediğiniz [SSS, sorun giderme ve en iyi](./marketing-and-best-practices.md).
