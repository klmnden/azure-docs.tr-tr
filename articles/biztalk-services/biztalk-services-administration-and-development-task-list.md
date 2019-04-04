---
title: BizTalk hizmetleri listesinde görev yönetim ve geliştirme | Microsoft Docs
description: Proje planlama ve Azure BizTalk Services'ı dağıtmak için yardımcı.
services: biztalk-services
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 6b8e0fea73dc24f7e680482be1f06fba6d702804
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916249"
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Yönetim ve geliştirme görev listesinde BizTalk Hizmetleri

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="getting-started"></a>Başlarken
Microsoft Azure BizTalk Services ile çalışırken, çeşitli şirket içi ve bulut tabanlı bileşenler dikkate alınması gereken vardır. Başlamak için aşağıdaki işlem akışını göz önünde bulundurun:  

| Adım | Kimin sorumlu olduğunu | Görev | İlgili Bağlantılar |
| --- | --- | --- | --- |
| 1. |Yönetici |Bir Microsoft hesabı veya Kurumsal hesap kullanarak Microsoft Azure aboneliği oluşturun |[Azure portal](https://portal.azure.com) |
| 2. |Yönetici |BizTalk hizmeti sağlayın veya oluşturun. |[BizTalk Hizmeti oluşturma](/previous-versions/azure/reference/dn232347(v=azure.100)) |
| 3. |Yönetici |Sizin veya şirketinizin BizTalk Hizmetleri dağıtımı kaydetme |[Kaydetme ve BizTalk Hizmetleri portalında bir BizTalk hizmeti dağıtımını güncelleştirme](/previous-versions/azure/hh689837(v=azure.100)) |
| 4. |Yönetici |Uygulama, bir şirket içi iş kolu (LOB) sistemine bağlamak için BizTalk bağdaştırıcı hizmetini kullanıyorsa geçerlidir veya bir kuyruk veya konuda hedef kullanır.  Azure Service Bus Namespace oluşturun. Bu ad alanı, Service Bus verenin adı ve hizmet veri yolu yayıncı değerleri geliştiriciye verirsiniz. |[Nasıl yapılır: Oluşturma veya değiştirme bir Service Bus hizmeti Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) ve [alma verenin adı ve verenin anahtarı değerleri](biztalk-issuer-name-issuer-key.md) |
| 5. |Geliştirici |SDK'sını yükleyin ve Visual Studio'daki BizTalk hizmeti projesini oluşturun. |[Azure BizTalk Hizmetleri SDK'sını yükleme](/previous-versions/azure/hh689760(v=azure.100)) ve [Azure'da zengin Mesajlaşma uç noktaları oluşturma](/previous-versions/azure/hh689766(v=azure.100)) |
| 6. |Geliştirici |Proje BizTalk hizmetiniz için Azure üzerinde barındırılan, BizTalk hizmetinize dağıtın. |[Dağıtmak ve BizTalk Hizmetleri Proje yenileniyor](/previous-versions/azure/hh689881(v=azure.100)) |
| 7. |Yönetici |EDI kullanıyorsanız geçerlidir.  İş ortakları ekleme ve Microsoft Azure BizTalk Hizmetleri portalında sözleşmeler oluşturun. Bir sözleşme oluşturduğunuzda Köprüsü ve/veya anlaşma ayarlarına geliştirici tarafından oluşturulan dönüşümler ekleyebilirsiniz. |[EDI, AS2 ve EDIFACT BizTalk Hizmetleri portalında yapılandırma](/previous-versions/azure/hh689853(v=azure.100)) |
| 8. |Yönetici |Kullanarak [REST](/previous-versions/azure/reference/dn232347(v=azure.100)), performans ölçümleri dahil olmak üzere, BizTalk hizmeti durumunu izleyin. |[BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](https://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Yönetici |Microsoft Azure BizTalk Hizmetleri portalını kullanarak, köprü dosyaları işlendikçe BizTalk Hizmetleri ve izleme iletileri tarafından kullanılan yapılar yönetin. |[BizTalk Hizmetleri Portalını Kullanma](/previous-versions/azure/dn874043(v=azure.100)) |
| 10. |Yönetici |BizTalk hizmetini yedeklemek için yedekleme planı oluşturma. |[İş sürekliliği ve olağanüstü durum kurtarma BizTalk Services](/previous-versions/azure/dn509557(v=azure.100)) |

## <a name="next-steps"></a>Sonraki Adımlar
[Öğreticiler ve Örnekler](/previous-versions/azure/hh689895(v=azure.100))

[Projeyi Visual Studio'da oluşturma](/previous-versions/azure/hh689811(v=azure.100))

[Azure BizTalk Hizmetleri SDK'sını yükleme](/previous-versions/azure/hh689760(v=azure.100))

## <a name="concepts"></a>Kavramlar
[Projeyi Visual Studio'da oluşturma](/previous-versions/azure/hh689811(v=azure.100))  
[EDI, AS2 ve EDIFACT iletileri (işletmeler arası)](/previous-versions/azure/hh689898(v=azure.100))  

## <a name="other-resources"></a>Diğer Kaynaklar
[Kaynak ve hedef köprüsü Mesajlaşma son noktalarını ekleyin](/previous-versions/azure/hh689877(v=azure.100))  
[İleti eşlemeleri ve dönüştürmeler oluşturmak ve bilgi edinin](/previous-versions/azure/hh689905(v=azure.100))  
[BizTalk bağdaştırıcı hizmeti (BAS) kullanma](/previous-versions/azure/hh689889(v=azure.100))  
[Azure BizTalk Hizmetleri](https://go.microsoft.com/fwlink/p/?LinkID=303664)

