---
title: Power BI uygulaması teklifi - Azure Marketi'nde yayımlama | Microsoft Docs
description: Bir Power BI uygulaması teklif Microsoft AppSource Marketplace üzerinde yayımlayın.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 2b3783060cf5502076ce3bc98cf07f005366a9e2
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666719"
---
# <a name="publish-power-bi-app-offer"></a>Power BI uygulaması teklifi yayımlama

Teklif Portalı'nda tanımlandığı ve ilgili teknik varlıkları oluşturulan sonra son adım, teklif yayımlama için gönderme sağlamaktır.  Bu işlemi başlatmak için tıklatın **Yayımla** Dikey Menüde düğmesinde **yeni teklif** penceresi.  Daha fazla bilgi için [yayımlama Azure Market ve AppSource teklifler](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Yayımlama adımları

Aşağıdaki diyagram, "Canlı Git" yayımlama işlemi ana adımları gösterir.

![Power BI uygulaması için yayımlama işlemi adımları](./media/publishing-process-steps.png)

Aşağıdaki tabloda, adımları açıklar ve bunların tamamlanmasını en uzun süreyi tahmin sağlar:

|   Yayımlama Adım            |   Zaman     |   Açıklama                                                                  |
| --------------------         |------------| ----------------                                                               |
| Önkoşulları doğrulama       | 15 dakika     | Bilgi sunan ve ayarlar doğrulanır sunar.                            |
| Sertifika                | 1-7 gün   | Power BI sertifika ekibi teklifinizi olmaktadır. Power BI uygulamanız el ile doğrulama testi sağlanan yükleme URL'si aracılığıyla uygulamayı yükleyerek çalıştırıyoruz. Ana doğrulamaları uygulama sertifika işlemini bir parçası olarak gerçekleştirilir; aşağıya bakın.         |
| Paketleme                    | \< 1 saat  | Teklife ilişkin teknik varlıkları, müşteri kullanımı için hazırlanmıştır.                        |
| Müşteri adayı oluşturma kayıt | \< 1 saat  | Müşteri adayı sistemleri yapılandırılıp dağıtılan.                                      |
| Yayımcı oturumu kapatma            | \-         | Son yayımcı gözden geçirme ve teklif Canlı geçmeden önce onay. Teklifinizin önizlemesi için bir bağlantı da gerekir. Önizleme nasıl göründüğünü ile memnun olduğunuzda tıklayın **Go Live** düğmesine **durumu** sekmesi. Bu eylem, uygulamanızı appsource'ta listelemek için onboarding ekibi için bir istek gönderir.    |
| Canlı                         | \< 3 saat | Teklifinizi artık genel ("canlı") Appsource'ta listelenir ve müşterilerin görüntüleyebilir ve Power BI aboneliklerini uygulamanızda dağıtmak olacaktır. Ayrıca, bir onay e-posta alırsınız. Herhangi bir noktada tıklayabilirsiniz **tüm teklifleri** sekmesi ve tüm Teklifleriniz için durum sağ sütunda listelenen bakın. Tıklayabilirsiniz **durumu** teklifiniz için ayrıntılı yayımlama akış durumunu görmek için sekmesinde. |
|   |   |

Bu işlemin tamamlanması için en fazla sekiz gün bekleyin. Bu yayımlama adımları olduktan sonra Power BI uygulaması teklifinizin listelenir [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) Power BI uygulamaları bölümü.


### <a name="app-certification-process"></a>Uygulama sertifika işlemini

Microsoft ekleme ekibi, Power BI teklif gönderiminiz doğrulamak için aşağıdaki süreci kullanır:

1. Yasal belgeler ve Yardım bağlantıları gözden geçirilir.
2. Destek iletişim bilgileri doğrulanır.
3. Yükleyici URL'si uygun yüklemesini doğrulamak için kullanılır. 
4. Uygulama, kötü amaçlı yazılım ve diğer kötü amaçlı içerik için taranır. 
5. Doğrulama işlemi gerçekleştirildiğinde görüntülenen içeriğin uygulamanın açıklama eşleşir.
6. Uygulamayla ilgili işlemleri, Power BI'da beklendiği gibi çalışır: örnek veriler içeren raporlar ve panolar açın, özel veri kaynakları, yenileme, vb. için bağlanın.

Bunlar herhangi bir sorunla karşılaşmanız halinde sertifika ekibi geri bildirim sağlar.  Power BI uygulama gereksinimleri hakkında daha fazla bilgi için bkz. [Power BI uygulaması belgeleri](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızda düzenli olarak izleyin öneririz [AppSource Market](https://appsource.microsoft.com).  Ayrıca, kullanmanız gereken [satıcı Insights](../../cloud-partner-portal-orig/si-getting-started.md) özelliği [bulut iş ortağı portalı](https://cloudpartner.azure.com/#insights) Market müşterileri ve kullanımı hakkında bilgiler sağlamak için.  Ayrıca belirli gerçekleştirebilirsiniz [teklifinizi güncelleştirmeleri](./cpp-update-existing-offer.md).
