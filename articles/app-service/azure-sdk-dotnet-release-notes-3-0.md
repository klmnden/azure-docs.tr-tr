---
title: .NET 3.0 sürüm notları için Azure SDK'sı | Microsoft Docs
description: .NET 3.0 için Azure SDK sürüm notları
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: 207aa137b25e44baf73e7f481ebc8b6362dfa245
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-sdk-for-net-30-release-notes"></a>.NET 3.0 için Azure SDK sürüm notları

Bu konu, .NET için Azure SDK'ın 3.0 sürümü için sürüm notları içerir.

## <a name="azure-sdk-for-net-30-release-summary"></a>.NET 3.0 sürüm özeti için Azure SDK'sı

Yayın Tarihi: 07/03/2017
 
Bu sürümde hiçbir önemli değişiklikler Azure SDK 3.0 tanıtılmıştır. Bu SDK mevcut bulut hizmeti projeleri ile yararlanmak için gereken yükseltme hiçbir işlem yok. Bir yükseltme işlemi gerektirmeden Azure SDK 3.0 kullanılmasına izin vermek için Azure SDK 2.9 aynı dizine Azure SDK 3.0 yükler. En bileşenleri ana sürüm 2.9 değiştirilmemesi ancak bunun yerine yalnızca yapı numarası güncelleştirildi.

## <a name="visual-studio-2017-rtw"></a>Visual Studio 2017 RTW

- Visual Studio 2017 ' bu sürümü .NET için Azure SDK'sı, Azure iş yükü için yerleşik olarak bulunur. Azure geliştirme yapmak için ihtiyacınız olan araçları, Visual Studio ileride 2017 bir parçası olur. Visual Studio 2015 için SDK Webpı kullanılabilir olmaya devam eder. Visual Studio 2017 yayımlanan göre Biz Visual Studio 2013 için .NET sürümleri için Azure SDK'sı sonlandırdıktan.

### <a name="azure-diagnostics"></a>Azure Tanılama

- Yalnızca bulut Hizmetleri tanılama depolama bağlantı dizesi için bir belirteç değiştirilmiştir anahtarla bir kısmi bağlantı dizesi depolamak için kullanılacak davranışı değişti. Kendi erişim denetlenebilir için gerçek depolama anahtarı artık kullanıcı profili klasöründe depolanır. Visual Studio yerel hata ayıklama ve yayımlama işlemi için kullanıcı profili klasöründen depolama anahtarı okur. 
- Kullanıcıların Azure'da sürekli tümleştirme ve dağıtım için yayımlarken tanılama uzantısını ayarlamak için depolama anahtarı belirtebilirsiniz şekilde yukarıda açıklanan değişikliğe yanıt olarak, Visual Studio Online ekibi Azure Cloud Services Dağıtımı görev şablonu geliştirilmiştir.
- Bu güvenli bağlantı dizesini ve simgeleştirme için Azure tanılama (arasında environements yapılandırmasındaki sorunları gidermenize yardımcı olacak WAD), depolanmasını mümkün yaptık.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 sanal makineler

- Visual Studio artık işletim sistemi ailesi 5 (Windows Server 2016) sanal makineler için bulut Hizmetleri dağıtma destekler. Mevcut bulut hizmetlerini yeni işletim sistemi ailesi hedeflemek için ayarlarınızı değiştirebilirsiniz. .Net 4.6 ya da daha yüksek kullanarak hizmet oluşturmayı seçerseniz yeni bulut Hizmetleri, oluştururken, varsayılan olarak işletim sistemi ailesi 5 kullanmak için hizmet alır.  Daha fazla bilgi için gözden geçirebilirsiniz [konuk işletim sistemi ailesi destek tablo](../cloud-services/cloud-services-guestos-update-matrix.md).

### <a name="known-issues"></a>Bilinen sorunlar

- Azure .NET SDK 3.0 bir sorun, Visual Studio 2017 Visual Studio 2015 ile yan yana yapılandırmasında kaldırırken kullanıma sunuldu.  Visual Studio 2015 için Azure SDK yüklü değilse Microsoft Azure Storage öykünücüsü ve Microsoft Azure işlem öykünücüsü, Visual Studio 2017'i kaldırırsanız kaldırılacak.  Bu oluşturma ve Visual Studio 2015 yeni bulut Hizmetleri projelerinde hata ayıklama sırasında bir hata oluşturur. Bu sorunu çözmek için Web Platformu yükleyicisi Azure SDK'sını yeniden yükleyin.  Sorun çözümlenir gelecekteki bir Visual Studio 2017 güncelleştirme.  .

 
### <a name="azure-in-role-cache"></a>Azure rol içi önbellek 

- Azure rol içi önbelleği için destek, 30 Kasım 2016 tarihinde sona erdi. Daha fazla ayrıntı için tıklatın [burada](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).




