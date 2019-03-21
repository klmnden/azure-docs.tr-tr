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
ms.openlocfilehash: baee8c8e931a18c1d72b52c6141c29cba98f9c53
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096410"
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
| 2. |Yönetici |BizTalk hizmeti sağlayın veya oluşturun. |[BizTalk Hizmeti oluşturma](https://msdn.microsoft.com/library/azure/dn232347.aspx) |
| 3. |Yönetici |Sizin veya şirketinizin BizTalk Hizmetleri dağıtımı kaydetme |[Kaydetme ve BizTalk Hizmetleri portalında bir BizTalk hizmeti dağıtımını güncelleştirme](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| 4. |Yönetici |Uygulama, bir şirket içi iş kolu (LOB) sistemine bağlamak için BizTalk bağdaştırıcı hizmetini kullanıyorsa geçerlidir veya bir kuyruk veya konuda hedef kullanır.  Azure Service Bus Namespace oluşturun. Bu ad alanı, Service Bus verenin adı ve hizmet veri yolu yayıncı değerleri geliştiriciye verirsiniz. |[Nasıl yapılır: Oluşturma veya değiştirme bir Service Bus hizmeti Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) ve [alma verenin adı ve verenin anahtarı değerleri](biztalk-issuer-name-issuer-key.md) |
| 5. |Geliştirici |SDK'sını yükleyin ve Visual Studio'daki BizTalk hizmeti projesini oluşturun. |[Azure BizTalk Hizmetleri SDK'sını yükleme](https://msdn.microsoft.com/library/azure/hh689760.aspx) ve [Azure'da zengin Mesajlaşma uç noktaları oluşturma](https://msdn.microsoft.com/library/azure/hh689766.aspx) |
| 6. |Geliştirici |Proje BizTalk hizmetiniz için Azure üzerinde barındırılan, BizTalk hizmetinize dağıtın. |[Dağıtmak ve BizTalk Hizmetleri Proje yenileniyor](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| 7. |Yönetici |EDI kullanıyorsanız geçerlidir.  İş ortakları ekleme ve Microsoft Azure BizTalk Hizmetleri portalında sözleşmeler oluşturun. Bir sözleşme oluşturduğunuzda Köprüsü ve/veya anlaşma ayarlarına geliştirici tarafından oluşturulan dönüşümler ekleyebilirsiniz. |[EDI, AS2 ve EDIFACT BizTalk Hizmetleri portalında yapılandırma](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| 8. |Yönetici |Kullanarak [REST](https://msdn.microsoft.com/library/azure/dn232347.aspx), performans ölçümleri dahil olmak üzere, BizTalk hizmeti durumunu izleyin. |[BizTalk Services: Pano, İzleyici ve ölçek sekmeleri](https://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Yönetici |Microsoft Azure BizTalk Hizmetleri portalını kullanarak, köprü dosyaları işlendikçe BizTalk Hizmetleri ve izleme iletileri tarafından kullanılan yapılar yönetin. |[BizTalk Hizmetleri portalını kullanma](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| 10. |Yönetici |BizTalk hizmetini yedeklemek için yedekleme planı oluşturma. |[İş sürekliliği ve olağanüstü durum kurtarma BizTalk Services](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a>Sonraki Adımlar
[Öğreticiler ve örnekler](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Visual Studio'da proje oluşturma](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Azure BizTalk Hizmetleri SDK'sını yükleme](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Kavramlar
[Visual Studio'da proje oluşturma](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI, AS2 ve EDIFACT iletileri (işletmeler arası)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a>Diğer Kaynaklar
[Kaynak ve hedef köprüsü Mesajlaşma son noktalarını ekleyin](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[İleti eşlemeleri ve dönüştürmeler oluşturmak ve bilgi edinin](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[BizTalk bağdaştırıcı hizmeti (BAS) kullanma](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk Hizmetleri](https://go.microsoft.com/fwlink/p/?LinkID=303664)

