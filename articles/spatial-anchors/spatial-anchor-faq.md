---
title: Azure uzamsal yer işaretleri hakkında sık sorulan sorular | Microsoft Docs
description: Azure uzamsal bağlayıcılarını cihazlar arası sağlayan yönetilen bir bulut hizmeti ve geliştirici platformu, HoloLens, iOS ve Android cihazlarda birden çok kullanıcı ve karışık gerçeklik karşılaşır. Bu SSS hizmeti hakkındaki bir teknik açıdan adresi.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 73979ec3bd1d667453a186ea1f20bbeddc12db8f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273305"
---
# <a name="frequently-asked-questions-about-azure-spatial-anchors"></a>Azure Spatial Anchors hakkında sık sorulan sorular

HoloLens, iOS ve Android cihazlarda, birden çok kullanıcı, mekan kullanan karma gerçeklik sağlayan geliştirici platformu deneyimleri ve Azure uzamsal bağlayıcılarını yönetilen bir bulut hizmetidir.

Daha fazla bilgi için [Azure uzamsal yer işaretleri genel bakış](overview.md).

## <a name="azure-spatial-anchors-product-faqs"></a>Azure uzamsal bağlayıcılarını ürün ile ilgili SSS

**S: Hangi cihazların Azure uzamsal bağlayıcılarını destekliyor mu?**

**C:** ARKit desteğiyle iOS cihazlarında HoloLens uygulamaları oluşturmak üzere geliştiricilere Azure uzamsal bağlayıcıları etkinleştirir ve Android'de ARCore cihazlarla destekler; iOS ve Android için hem telefon ve tabletlerden içerir.

**S: Azure uzamsal bağlayıcılarını kullanmak için buluta bağlı olması gerekiyor mu?**

**C:** Azure uzamsal yer işaretleri şu anda İnternet'e bir ağ bağlantısı gerektirir. Üzerinde yorumlarınız bizim sunduğumuz [geri bildirim sitesinde](https://feedback.azure.com/forums/919252-azure-spatial-anchors).

**S: Uzamsal Azure bağlantıları için bağlantı gereksinimleri nelerdir?**

**C:** Azure uzamsal yer işaretleri, Wi-Fi ve mobil geniş bant bağlantı ile çalışır.

**S: Nasıl doğru bir şekilde Azure uzamsal yer işaretleri, yer işaretleri bulabilirsiniz?**

**C:** Pek çok etken bağlayıcılarını--Aydınlatmanın, ortamda ve hatta bağlantı yerleştirildiği surface nesneleri bulma doğruluğunu etkiler. Doğruluk gereksinimlerinizi karşılayıp karşılamadığını belirlemek için burada bunları kullanmayı planlıyorsanız, ortamları temsilcisi tutturucular deneyin. Ortamlar nerede doğruluk olmadığından toplantı gereksinimlerinizi karşılaşırsanız bkz [günlüğe kaydetme ve Tanılama'da Azure uzamsal bağlayıcılarını](./concepts/logging-diagnostics.md).

**S: Ne oluşturmak ve bağlantıları bulmak için sürer?**

**C:** Oluşturma ve bağlantıları bulmak için gereken süreyi birçok faktöre--ağ bağlantısı, cihazın işleme ve yük ve ortamınıza bağlıdır. Birçok sektörlerdeki üretim, perakende ve hizmetin kendi senaryoları için harika bir deneyim sağlayan oyun belirten gibi uygulamalar oluşturan müşteriler sahibiz.

## <a name="privacy-faq"></a>Gizlilik hakkında SSS

**S: Uygulamamı bir yerde bir uzamsal tutturucusu yerleştirir, tüm uygulamalar ona erişim izniniz?**

**C:** Yer işaretleri, Azure hesabına göre yalıtılır. Hesabınıza erişim izni için yalnızca uygulama bağlayıcılarını hesabındaki erişmek mümkün olacaktır.

**S: Bir ortam hakkında bilgiler aktarılan ve Azure uzamsal Çıpalarını kullandığına hizmette depolanan? Ortamın resimleri aktarılan ve depolanan?**

**A**: Oluşturma veya bağlayıcılarını bulma, resimleri ortamın türetilmiş bir biçime cihazda işlenir. Bu türetilmiş biçimi için gönderilen ve hizmette depolanan.

Saydamlık sağlamak için bir ortam ve türetilmiş seyrek noktası bulut görüntüsü aşağıda verilmiştir. Noktası bulut aktarılan ve hizmette depolanan ortamın geometrik temsilini gösterir. Her bir noktası seyrek noktası bulutta iletmek ve o noktasının görsel özelliklerini karmasını depolar. Karma türetilir ancak içermiyor, herhangi bir piksel veri.

Azure uzamsal bağlayıcılarını aynılarını için [Azure hizmet sözleşmesi hüküm](https://go.microsoft.com/fwLink/?LinkID=522330&amp;amp;clcid=0x9)ve [Microsoft gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=521839&amp;clcid=0x409).

![Bir ortam ve onun türetilmiş seyrek noktası bulut](./media/sparse-point-cloud.png)
*Şekil 1: Bir ortam ve onun türetilmiş seyrek noktası bulut*


**S: Microsoft'a tanılama bilgileri gönderebilir bir yolu var mı?**

**A**: Evet. Azure uzamsal yer işaretleri, geliştiricilerin Azure uzamsal bağlayıcılarını API aracılığıyla Katılmayı tercih edebilirsiniz bir tanı Modu'nu sahiptir. Bir ortam oluşturun ve tahmin edilebilir bir biçimde bağlayıcılarını bulma nerede karşılaşırsanız bu örneğin, kullanışlıdır. Hata ayıklama bize yardımcı olacak bilgileri içeren bir tanılama raporu gönder sorun. Daha fazla bilgi için [günlüğe kaydetme ve Tanılama'da Azure uzamsal bağlayıcılarını](./concepts/logging-diagnostics.md).

## <a name="availability-and-pricing-faqs"></a>Kullanılabilirlik ve fiyatlandırma ile ilgili SSS

**S: SLA sağlıyor mu?**

**C:** Azure Hizmetleri için standart olduğundan, % 99,9'den büyük bir kullanılabilirlik hedef. Azure uzamsal yer işaretleri şu anda Önizleme aşamasındadır ve bu nedenle olduğuna dikkat edin [Önizleme ek koşullarında](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) uygulayın.

**S: Uygulama mağazalarının Azure uzamsal bağlayıcılarını kullanarak uygulamalarım yayımlayabilirim? Görev açısından kritik üretim senaryoları için Azure uzamsal bağlayıcılarını kullanabilir miyim?**

**C:** Azure uzamsal yer işaretleri şu anda Önizleme aşamasındadır ve bu süre boyunca uygulamaları geliştirmek için davet ediyoruz [görüş](https://feedback.azure.com/forums/919252-azure-spatial-anchors) ürün ve planı, üretim dağıtımları için hakkında.

Genel kullanılabilirlik (GA) tarihleri yakında duyurulacaktır.

**S: Yerinde herhangi bir azaltma sınırları var mı?**

**A**: Evet, azaltma sınırları vardır.  Normal uygulama geliştirme ve test için bunları ulaşırsınız bekliyoruz yok. Üretim dağıtımları için müşterilerin yüksek ölçekli gereksinimlerini desteklemek hazırız. [Bizimle iletişime geçin](mailto:azuremrs@microsoft.com) tartışmak için. Bu önizleme aşaması sırasında henüz bizim katmanlama ve fiyatlandırma yapısı yayımladık değil, ancak yakında bunun bekliyoruz.

**S: Azure uzamsal bağlayıcılarını hangi bölgelerde kullanılabilir?**

**C:** Bugün Azure uzamsal bağlayıcılarını hesap Azure Doğu ABD 2 bölgesinde oluşturabilirsiniz. Ne bu hem de işlem ve bu hizmeti destekleyen depolama bu bölgede olduğu anlamına gelir. İstemcilerinize bulunduğu yere hiçbir kısıtlama yoktur söylenir. Gelecekte, biz birincil tüm Azure bölgelerine bölgesel hizmet kullanılabilirliğini genişletilir.

**S: Azure Spatial Anchors için ücret alıyor musunuz? Şimdiye kadar ücretlendirilecektir?**

**C:** Önizleme sırasında fiyatlandırması hakkında ayrıntılı bilgi bulabilirsiniz bizim [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/spatial-anchors/).

## <a name="technical-faqs"></a>Teknik SSS

**S: Azure uzamsal bağlayıcıları nasıl çalışır?**

**C:** Azure uzamsal bağlayıcılarını üzerinde karma gerçeklik bağlıdır / genişletilmiş gerçeklik izleyiciler. Bu izleyicileri kameralar ortamıyla algılar ve alanı arasında hareket ettikçe 6--ın-serbestlik (6DoF) aygıtı takip edebilirsiniz.

Bir 6DoF İzleyicisi bir yapı taşı olarak göz önünde bulundurulduğunda, Azure uzamsal yer işaretleri, gerçek ortamınızda ilgilendiğiniz belirli noktaları "bağlantı" noktalarını belirlemek sağlar. Örneğin, gerçek hayatta kullanılan belirli bir yerde içeriğini işlemek için bir bağlantı kullanabilirsiniz.

Bir yer işareti oluşturduğunuzda, istemci SDK'sı bu nokta etrafında ortam bilgilerini yakalar ve hizmetine iletir. Başka bir cihaz, aynı alanda bağlayıcı için görünüyorsa, benzer veri hizmetine iletir. Bu verileri daha önce depolanan ortam verilerini karşı eşleştirilir. Yer işaretine göre cihaz konumu, daha sonra uygulamada kullanılmak üzere geri gönderilir.

**S: Nasıl Azure uzamsal bağlayıcılarını ARKit ve ARCore ile iOS ve Android üzerinde tümleşik çalışıyor mu?**

**C:** Azure uzamsal yer işaretleri, ARKit ve ARCore yerel izleme özelliklerinden yararlanır. Ayrıca, Larımız iOS ve Android hizmete bağlayarak bu bağlantıları yeniden bulmak için bir yönetilen bir bulut hizmetinde yer işaretleri kalıcı yapma ve uygulamalarınızı izin verme gibi özellikler sunar.

**S: Azure uzamsal yer işaretleri ile HoloLens nasıl tümleştirilir?**

**C:** Azure uzamsal yer işaretleri, HoloLens, yerel izleme özelliklerinden yararlanır. HoloLens üzerinde uygulama oluşturmaya yönelik bir Azure uzamsal bağlayıcılarını SDK sunuyoruz. SDK, yerel HoloLens özellikleriyle tümleşir ve ek özellikler sağlar. Bu özellikler, yönetilen bir bulut hizmetinde yer işaretleri kalıcı hale getirmek uygulama geliştiricilerin ve hizmete bağlayarak bu bağlantıları yeniden bulmak uygulamalarınızı vererek içerir.

**S: Hangi platformlar ve diller Azure uzamsal bağlayıcılarını destekliyor mu?**

**C:** Geliştiriciler, Azure uzamsal Çıpasıyla cihazlarındaki alışık olduğunuz araçları ve çerçeveleri kullanarak uygulamalar oluşturabilirsiniz:

- Unity HoloLens, iOS ve Android
- Swift veya iOS üzerinde Objective-C
- Java veya android'de Android NDK
- C++/WinRT on HoloLens

Kullanmaya başlama [geliştirme burada](index.yml).

**S: Unreal ile işe yarar?**

**C:** Unreal desteği yakında olmasını bekliyoruz.

**S: Xamarin ile çalışır?**

**C:** Evet. Bir Xamarin SDK'sı de verilmese, geliştiricilerin Azure uzamsal bağlayıcılarını Xamarin uygulamalarında Azure uzamsal bağlayıcılarını API ile tümleştirerek kullanabileceği bekliyoruz.

**S: Hangi bağlantı noktalarını ve protokolleri Azure uzamsal bağlayıcılarını kullanıyor?**

**C:** Azure uzamsal yer işaretleri, şifrelenmiş bir protokolü kullanarak 443 numaralı TCP bağlantı noktası üzerinden iletişim kurar. Kimlik doğrulaması için kullandığı [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/), bağlantı noktası 443 üzerinden HTTPS kullanarak iletişim kurar.
