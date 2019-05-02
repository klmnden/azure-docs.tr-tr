---
title: Azure uygulama teklif Market sekmesi
description: Bir Azure uygulaması teklif için pazarlama varlıkları tanımlamak için Market sekmesini kullanın.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pabutler
ms.openlocfilehash: 7ea6e6be0597a114b02fad8c41e37d21ce1f6028
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64942961"
---
# <a name="azure-application-marketplace-tab"></a>Azure uygulama Market sekmesi

Azure, uygulamayı tanımlayan ve Pazarlama Varlıkları sağlamak için Market sekmesini kullanın. Bu sekme aşağıdaki biçimlerden içerir: Genel bakış, pazarlama Yapıtlar, müşteri adayı yönetim ve yasal.

## <a name="overview-form"></a>Genel Bakış formu

Genel Bakış formu, sonraki ekran görüntüsünde gösterilen gerekli ve isteğe bağlı alanları içerir. Gerekli alanlar yıldız (*) indicted.

![Genel Bakış formu](./media/azureapp-marketplace-overview.png)

Teklif için bir mağaza oluşturmak için kullanılacak ayarları aşağıdaki tabloda açıklanmaktadır.   Bir yıldız işareti ile eklenmiş alanlar gereklidir.

|      Alan         |    Açıklama    |
|  ---------------   |  ---------------  |
| **Başlık\***        | Teklif başlığı. Market'te dikkat çekecek şekilde gösterilir. En fazla 50 karakterdir. |
| **Özeti\***      | Teklifin kısa özeti. En fazla 100 karakterdir.           |
| **Uzun özeti\*** | Artık özeti teklif (Özet ile aynı olabilir ancak). En fazla uzunluk 256 karakterdir.           |
| **Açıklaması\***  | Teklif açıklaması. En fazla 3000 karakterdir. Basit HTML biçimlendirmeyi izin verilir, da dahil olmak üzere &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; ve üst bilgi etiketler.  |
| **Pazarlama tanımlayıcısı\*** | Bu teklif için ilişkilendirmek için benzersiz bir URL, kuruluşunuz ve çözüm adı, en fazla uzunluğu 50 karakterden genellikle içerir. Hizmetiniz için bir kısa ve kolay pazarlama tanımlayıcı seçin. Bu Market URL'lerinde Bu teklif için kullanılır. Örneğin, yayımcı Kimliğiniz "contoso" ve "örnek uygulaması" Pazarlama tanımlayıcınızı ise, teklifinizi Azure Marketi'nde URL'si olacaktır https://azuremarketplace.microsoft.com/en-us/marketplace/apps/contoso.sampleApp  
| **Önizleme abonelik kimlikleri\*** | Bir abonelik tanımlayıcıları nizleyicileri 100 ila ekleyin. Canlı geçmeden önce yayımlandıktan sonra Önizleme sürümünde olduğu sürece bu teknik listelenen abonelikler teklifinizi erişebilir.          |
| **Faydalı bağlantılar**    | İsteğe bağlı olarak, kullanıcıların teklifinizin desteği, belgeleri, Forum, vs. gibi çeşitli kaynak bağlantıları sağlayabilir.  En az bir bağlantı belgelerinize ekleme önerilir.            |
| **Önerilen kategorileri (en fazla 5)\*** | Bir ile beş kategorilerini seçin. Seçili kategorilerdeki, teklifinizin Azure Market'te ve Azure Portalı'nda ürün kategorilerini eşleştirmek için kullanılır. Bunlar Gözat sayfalarında ve, Ürün Ayrıntıları sayfası gösterilir. |
|  |  |


## <a name="marketing-artifacts"></a>Pazarlama Yapıtları

Pazarlama Yapıtları formun sonraki ekran görüntüsünde gösterilen gerekli ve isteğe bağlı alanları içerir. Gerekli alanlar yıldız (*) indicted.

![Pazarlama yapıtları formu](./media/azureapp-marketplace-artifacts.png)

Aşağıdaki tabloda, pazarlama yapıları açıklar.

|      Alan         |    Açıklama    |
|  ---------------   |  ---------------  |
| **Küçük\***        | Küçük logo: PNG biçiminde 40 x 40 piksel     |
| **Orta\***       | Orta logosu: PNG biçiminde 90 x 90 piksel    |
| **Büyük\***        | Geniş logo: PNG biçiminde 115 x 115 piksel   |
| **Geniş\***         | Geniş logo: PNG biçiminde 255 x 115 piksel    |
| **Hero**           | İsteğe bağlı bir hero logosu: PNG biçiminde 815 x 290 piksel. **Not:** Karşıya yüklendikten sonra hero simge silinemiyor. |
| **Ekran görüntüleri (en fazla 5)** |        Ekran görüntüleri, Ürün Ayrıntıları sayfasında görüntülenir. Bunlar görsel olarak uygulamanıza yaptığı ve nasıl çalıştığı iletişim kurmak için iyi bir yoldur. Örneğin, mimari diyagramlar gösterebilir veya büyük/küçük harf çizimler kullanın. Ekran görüntüleri isteğe bağlıdır ve SKU başına 5 sınırlıdır. Bir ekran eklemek için:<ul><li>Seçin **+ Ekle ekran görüntüsü** ekran açmak için</li><li>**Ad** -adı/başlığı (en fazla uzunluğu en fazla 100 karakter.) girin</li><li>**Karşıya yükleme** -görüntüyü karşıya yükleme. PNG biçiminde olması gerekir ve boyutu 533 x 324 pikseldir.</li></ul>           |
| **Video ekleme**      | İsteğe bağlı, videoları, Ürün Ayrıntıları sayfasında görüntülenir. Bunlar görsel olarak uygulamanızın yaptığı ve nasıl çalıştığı iletişim kurmak için iyi bir yoldur. Bir video eklemek için: <ul><li>Seçin **+ Ekle video** Video penceresini açmak için</li><li>**Ad** -adı/başlığı (en fazla uzunluğu en fazla 100 karakter.) girin</li><li>**Bağlantı** – videonun (YouTube veya Vimeo) barındıran site için URL girin</li><li>**Küçük resim** – küçük resmi karşıya yükleyin. PNG biçiminde olması gerekir ve boyutu 533 x 324 pikseldir.</li></ul>          |
|  |  |


### <a name="artifact-examples-in-azure-marketplace"></a>Azure Marketi'nde yapıt örnekleri

Sonraki ekran görüntüsü yakalamayı Market arama sonucu örneği gösterilmektedir.

![Market teklifi arama sonucu](./media/azureapp-marketplace-example-browse.png)

Aşağıdaki görüntüde, arama sonucunda teklifin kutucuğunda bir müşteri tıkladıktan sonra teklifi Market'te nasıl görüntüleneceğini gösterir.

![Market teklifi arama sonucu ayrıntıları](./media/azureapp-marketplace-example-details.png)


### <a name="artifact-examples-in-azure-portal"></a>Azure Portalı'nda yapıt örnekleri

Aşağıdaki ekranda bir teklif Azure Portalı'nda nasıl görüntüleneceğini show yakalar. Bu örnekte uygulama teklif göz atarak bulundu **Market > her şey > Geliştirme + Test > Jenkins**. Jenkins teklif logosu, başlık ve yayımcı görünen adı gösterir.

![Azure Portal'da Gözat sunar](./media/azureapp-portalbrowse-artifacts-jenkins.png)

Bir kullanıcının Jenkins seçtiğinde sonraki ekran görüntüsü yakalamayı uygulamayla ilgili ayrıntılı bilgileri gösterir.

![Azure portalında teklifi ayrıntıları](./media/azureapp-portal-artifacts-jenkins-details.png)


### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut iş ortağı portalına karşıya logo yönergeleri izlemelidir:

- Azure tasarımının basit bir renk paleti vardır. Logonuzu düşük tutmak için ikincil renk ve birincil sayısı.
- Azure Portal'ın Tema renkleri beyaz ve siyah. Bu renkler, logolar için arka plan rengi olarak kullanmaktan kaçının. Logo, Azure portalında belirgin hale getirecek bir renk kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız, logolar/metin beyaz, olmayan emin siyah veya mavi.
- Arka plan gradyan logonuzu kullanmayın.
- Metin, hatta şirket veya marka adı logosunu yerleştirmekten kaçının. Logonuzu Görünüm ve yapısını "Düz" olmalıdır ve gradyanları kaçınmalıdır.
- Logo genişletme.


#### <a name="hero-logo"></a>Kahraman logosu

Hero logosu isteğe bağlıdır.

>[!IMPORTANT]
>Karşıya yüklendikten sonra Hero logosu nelze odstranit.

Hero logosu için aşağıdaki yönergeleri kullanın:

- Siyah, beyaz ve saydam arka planlar izin verilmez.
- Açık bir renk arka planı olarak logosu kullanmaktan kaçının. Yayımcı görünen adı, planı başlık ve uzun Özet teklif beyaz renkte görüntülenir ve arka plan karşı göze gerekir.
- Logo tasarlarken çoğu metin kullanmaktan kaçının. Teklifin listelenen Yayımcı adı, planı başlık, uzun Özet teklif ve Oluştur düğmesine içinde logosu programlı olarak katıştırılır.
- Sağ taraftaki bir kullanılmayan Dikdörtgen alanı hero logonuzun içerir. Bu 415 x 100 piksel ile 370 piksel sol uzaklığı boşluktur.


## <a name="lead-management"></a>Tedarik Yönetimi

Müşteri adayı yönetimini yapılandırmak için isteğe bağlı bir alan sağlama yönetim formundadır. Müşteri adayı yönetimini yapılandırmak için müşteri adayı hedef açılır listeden seçin. Sonraki ekran görüntüsü yakalamayı kullanılabilir hedefler gösterir.

![Select sağlama yönetim hedef](./media/azureapp-marketplace-leadmgmt.png)

>[!TIP]
>Bu iletiyi görmeye bilgi simgesini seçin: "Müşteri adaylarınızı depolanacağı sistem seçin. CRM sisteminize bağlamayı öğrenin [burada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) . "

Daha fazla bilgi için [yapılandırma müşteri adayları](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads).


## <a name="legal"></a>Yasal Bilgiler

Her teklif için gerekli yasal belgeler sağlamak için yasal formu kullanın.

Şu bilgileri belirtin:

- **Gizlilik İlkesi URL'si\***  – uygulamanızın gizlilik ilkesi için bir bağlantı girin.
- **Kullanım koşullarını\***  – uygulamanız için kullanım koşullarını girin. Müşterilerin uygulamanızı deneyebilmeniz için önce bu koşulları kabul etmek için gereklidir.

![Yasal formu](./media/azureapp-marketplace-legal.png)


## <a name="next-steps"></a>Sonraki adımlar

[Destek sekmesi](./cpp-support-tab.md)
