---
title: Power BI uygulaması teklifi yayımlama | Azure Market
description: Bir Power BI uygulaması teklif Microsoft AppSource Market'te yayımlayın.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: pabutler
ms.openlocfilehash: aae23feaf1cc5887de061414af985ef16070546b
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943190"
---
# <a name="publish-a-power-bi-app-offer"></a>Power BI uygulaması teklifi yayımlama

Teklif bulut iş ortağı Portalı'nda tanımlandığı ve ilgili teknik varlıkları oluşturulan sonra son adım, teklif yayımlama için gönderme sağlamaktır. Bu işlem,'ın sol bölmesinde başlatmak için **yeni teklif** penceresinde **Yayımla**. Daha fazla bilgi için [yayımlama Azure Market ve AppSource teklifler](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Yayımlama adımları

Yayımlama işlemi temel adımları şunlardır:

![Power BI uygulaması için yayımlama işlemi adımlarını sunar.](./media/publishing-process-steps.png)

Bu tablo, her bir adımın açıklar ve kendi tahmini tamamlanma süresi sağlar:

|   Yayımlama Adım            |   Zaman     |   Açıklama                                                                  |
| --------------------         |------------| ----------------                                                               |
| Önkoşulları doğrulama       | 15 dakika     | Bilgi sunan ve ayarlar doğrulanır sunar.                            |
| Sertifika                | 1-7 gün   | Power BI sertifika ekibi, teklifinizi analiz eder. Takım, Power BI uygulamanız aracılığıyla sağlanan yükleme URL'si uygulamayı yükleyerek el ile doğrulama testi çalıştırır. Birincil doğrulama (Bu belgenin sonraki bölümlerinde açıklanmıştır) uygulama sertifika işlemini bir parçası olarak gerçekleştirilir.         |
| Paketleme                    | \< 1 saat  | Teklife ilişkin teknik varlıkları, müşteri kullanımı için hazırlanmıştır.                        |
| Müşteri adayı oluşturma kayıt | \< 1 saat  | Müşteri adayı sistemleri yapılandırılıp dağıtılan.                                      |
| Yayımcı oturumu kapatma            | \-         | Teklif Canlı geçmeden önce son gözden geçirme ve onaylama tamamlayın. Teklifinizin önizlemesi için bir bağlantı da gerekir. Önizleme nasıl göründüğünü ile odaklanabiliyoruz sonra seçin **Go Live** üzerinde **durumu** sekmesi. Bu, uygulamanızın appsource'ta listelemek için onboarding ekibi için bir istek gönderir.    |
| Canlı                         | \< 3 saat | Teklifinizi artık genel ("canlı") Appsource'ta listelenir ve müşterilerin uygulamanızı görüntülemek ve Power BI aboneliklerini içinde dağıtın. Ayrıca, bir onay e-posta alacaksınız. Sağ sütununda **tüm teklifleri** sekmesinde, tüm tekliflerin durumunu görebilirsiniz. Üzerinde **durumu** sekmesi, teklifiniz için ayrıntılı yayımlama akış durumunu görebilirsiniz. |
|   |   |

Bu işlemin tamamlanması için en fazla sekiz gün izin verir. Bu yayımlama adımları olduktan sonra Power BI uygulaması teklifinizin listelenir [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) Power BI uygulamaları bölümü.


### <a name="app-certification-process"></a>Uygulama sertifika işlemini

Microsoft ekleme ekibi, Power BI uygulaması teklif gönderiminiz doğrulamak için bu işlemi kullanır:

1. Yasal belgeler ve Yardım bağlantıları gözden geçirin.
2. Destek iletişim bilgileri doğrulayın.
3. Yükleyici URL doğru yüklemesini doğrulamak için kullanın.
4. Uygulamayı kötü amaçlı yazılım ve diğer kötü amaçlı içerik taraması.
5. Görüntülenen içerik, uygulamanın açıklama eşleştiğini doğrulayın.
6. Uygulama ile ilgili işlemler Power BI'da beklendiği gibi çalıştığını doğrulayın. Takım, raporlar ve panolar, örnek verilerle açılır, özel veri kaynaklarına bağlanır, veri ve benzeri yeniler.

Bunlar herhangi bir sorunla karşılaşmanız halinde sertifika ekibi geri bildirim sağlar.  Power BI uygulama gereksinimleri hakkında daha fazla bilgi için bkz. [Power BI uygulaması belgeleri](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızda düzenli olarak izleyin öneririz [AppSource Market](https://appsource.microsoft.com).  Ayrıca kullanmalısınız [satıcı Insights](../../cloud-partner-portal-orig/si-getting-started.md) özelliği [bulut iş ortağı portalı](https://cloudpartner.azure.com/#insights) Market müşterileri ve uygulama kullanımı hakkında Öngörüler almak için. Son olarak, [teklifinizi güncelleştirme](./cpp-update-existing-offer.md).
