---
title: "BizTalk Services listesinde görev yönetim ve geliştirme | Microsoft Docs"
description: "Planlama ve iş Azure BizTalk Services dağıtmak için yardımcı."
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: 9edd7261ca62f505ffb4854e3132fae916768f67
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Yönetim ve geliştirme görev listesinde BizTalk Hizmetleri

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="getting-started"></a>Başlarken
Microsoft Azure BizTalk Services ile çalışırken, çeşitli şirket içi ve bulut tabanlı bileşenler dikkate alınması gereken vardır. Başlamak için aşağıdaki işlem akışı göz önünde bulundurun:  

| Adım | Kimin sorumlu olduğunu | Görev | İlgili Bağlantılar |
| --- | --- | --- | --- |
| 1. |Yönetici |Bir Microsoft hesabı veya kurumsal bir hesap kullanarak Microsoft Azure aboneliği oluşturma |[Azure portal](https://portal.azure.com) |
| 2. |Yönetici |Oluşturun veya bir BizTalk hizmeti sağlanamadı. |[BizTalk hizmeti oluşturma](https://msdn.microsoft.com/library/azure/dn232347.aspx) |
| 3. |Yönetici |Siz veya şirketinizin BizTalk Services Dağıtımı kaydetme |[Kaydetme ve BizTalk Services portalı üzerinde bir BizTalk hizmeti dağıtımı güncelleştiriliyor](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| 4. |Yönetici |Bir kuyruk veya konu hedef kullanır veya uygulama bir şirket içi iş kolu (LOB) sistemine bağlamak için BizTalk bağdaştırıcı hizmeti kullanıyorsa uygular.  Azure hizmet veri yolu Namespace oluşturun. Bu ad alanı, hizmet veri yolu verenin adı ve hizmet veri yolu verenin anahtarı değerlerini geliştiriciye verirsiniz. |[Nasıl yapılır: oluşturmak veya bir hizmet veri yolu hizmet Namespace değiştirmek](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) ve [alma verenin adı ve verenin anahtarı değerleri](biztalk-issuer-name-issuer-key.md) |
| 5. |Geliştirici |SDK'sını yükleyin ve Visual Studio içindeki BizTalk hizmeti projesi oluşturun. |[Azure BizTalk Services SDK'sı yükleme](https://msdn.microsoft.com/library/azure/hh689760.aspx) ve [Azure üzerinde zengin Mesajlaşma uç noktaları oluşturma](https://msdn.microsoft.com/library/azure/hh689766.aspx) |
| 6. |Geliştirici |BizTalk hizmeti projesine Azure üzerinde barındırılan, BizTalk hizmetinize dağıtın. |[Dağıtma ve BizTalk Services projesi yenileme](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| 7. |Yönetici |EDI kullanıyorsanız geçerlidir.  İş ortakları ekleyin ve Microsoft Azure BizTalk Services Portalı'ndaki sözleşmeler oluşturun. Bir sözleşme oluşturduğunuzda Köprüsü ve/veya anlaşma ayarlarına geliştirici tarafından oluşturulan dönüşümler ekleyebilirsiniz. |[BizTalk Hizmetleri portalında EDI, AS2 ve EDIFACT yapılandırma](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| 8. |Yönetici |Kullanarak [REST](https://msdn.microsoft.com/library/azure/dn232347.aspx), performans ölçümleri de dahil olmak üzere, BizTalk hizmeti durumunu izleyin. |[BizTalk Services: Pano, İzleyici ve Ölçek sekmeleri](http://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Yönetici |Microsoft Azure BizTalk Services Portalı'nı kullanarak, köprüsü dosyaları tarafından işlenen olarak BizTalk Services ve izleme iletilerini tarafından kullanılan yapılar yönetin. |[BizTalk Services Portalı'nı kullanarak](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| 10. |Yönetici |BizTalk hizmeti yedeklemek için bir yedekleme planı oluşturun. |[İş devamlılığı ve olağanüstü durum kurtarma BizTalk Hizmetleri](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a>Sonraki Adımlar
[Öğreticiler ve örnekler](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Visual Studio projesi oluşturma](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Azure BizTalk Services SDK'sını yükleyin](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Kavramlar
[Visual Studio projesi oluşturma](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI, AS2 ve EDIFACT Mesajlaşma (iş için iş)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a>Diğer Kaynaklar
[Kaynak ve hedef köprüsü Mesajlaşma uç noktaları ekleme](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[İleti eşlemeleri ve dönüşümler oluşturmak ve bilgi edinin](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[BizTalk bağdaştırıcı hizmeti (BAS) kullanma](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk Hizmetleri](http://go.microsoft.com/fwlink/p/?LinkID=303664)

