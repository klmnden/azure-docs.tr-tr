---
title: "Azure RemoteApp şablon görüntülerinde neler var? | Microsoft Belgeleri"
description: "Azure RemoteApp ile birlikte gelen şablon görüntüleri hakkında bilgi edinin."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 348306795fd3c8275b21e4ec6dceae408916bf72
ms.lasthandoff: 03/31/2017


---
# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Azure RemoteApp şablon görüntülerinde neler var?
> [!IMPORTANT]
> Azure RemoteApp 31 Ağustos 2017’de kullanımdan kaldırılacaktır. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Azure RemoteApp aboneliğiniz üç şablon görüntüsü içerir:

* Windows Server 2012
* Microsoft Office 365 ProPlus (Office 365 aboneliği gereklidir)
* Microsoft Office 2013 Professional Plus (yalnızca deneme)

> [!IMPORTANT]
> Azure RemoteApp aboneliğiniz, ayrı bir abonelik gerektiren Office 365 ProPlus ve üretimde kullanılamayan Office 2013 dışında görüntülerdeki yazılımlara erişim sağlar. Bu, şablon görüntülerindeki program veya uygulamaları kullanıcılarınızla paylaşabileceğiniz anlamına gelir. Örneğin, Windows Server 2012 R2 görüntüsünü kullanan bir koleksiyon oluşturursanız, kullanıcıların RemoteApp üzerinden erişmesi için System Center Endpoint Protection’ı yayımlayabilirsiniz.
> 
> Daha fazla bilgi için [RemoteApp lisanslama ayrıntılarına](remoteapp-licensing.md) bakın. Ayrıca Office lisanslama bilgileri için [Azure RemoteApp ile Office’i kullanma](remoteapp-o365.md) bölümüne bakın.
> 
> 

Her görüntünün içerdikleri ile ilgili ayrıntılar için okumaya devam edin.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("temel alınan görüntü")
Bu görüntü Microsoft Windows Server 2012 R2 Datacenter işletim sistemini temel alır ve Azure RemoteApp şablon görüntüleri için gereksinimleri karşılamak üzere aşağıdaki roller ve özellikleri içerir:

* .NET Framework 4.5, 3.5.1, 3.5
* Masaüstü Deneyimi
* Mürekkep ve El Yazısı Hizmetleri
* Medya Altyapısı
* Uzak Masaüstü Oturumu Konağı
* Windows PowerShell 4.0
* Windows PowerShell ISE
* WoW64 Desteği

Bu görüntüde ayrıca şu uygulamalar yüklüdür:

* Adobe Flash Player
* Microsoft Silverlight
* Microsoft System Center 2012 Endpoint Protection
* Microsoft Windows Media Player

## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (abonelik gereklidir)
Office 365 en çok istenen uygulama olduğundan, çalışmanız için "özel" bir görüntü oluşturduk.

Bu görüntü, temel alınan görüntünün bir uzantısıdır ve Windows Server 2012 R2 görüntüsünde açıklanan bileşenlere ek olarak aşağıdaki Microsoft Office 365 ProPlus bileşenlerini içerir:

* Access
* Excel
* Lync
* OneNote
* OneDrive İş (eşitleme aracısının Azure RemoteApp ile kullanılması desteklenmez)
* Outlook
* PowerPoint
* Word
* Microsoft Office Yazım Denetleme Araçları

Görüntü ayrıca Visio Pro ve Project Pro’yu da içerir.

Aşağıdaki uygulamalar da dahildir:

* SQL Native istemcisi
* ODBC Sürücüsü
* SQL Server Data Mining istemcisi
* MasterDataServices istemcisi
* Microsoft Publisher
* PowerQuery
* PowerMap

Office 365 ProPlus uygulamalarının tüm işlevleri yalnızca bir Office 365 ProPlus planı olan kullanıcılar tarafından kullanılabilir. Office 365 abonelik planları hakkında daha fazla bilgi için bkz. [Office 365 hizmet planları](http://technet.microsoft.com/library/office-365-plan-options.aspx). Hala sorularınız mı var? [Office 365 + RemoteApp](remoteapp-o365.md) hakkındaki bilgilere göz atın. Ayrıca [Azure RemoteApp ile Office 365 aboneliğinizi kullanma](remoteapp-officesubscription.md) başlıklı yeni makaleye de göz atın.

Office 365 ProPlus, Visio Pro ve Project Pro’nun kendi lisanslarına sahip olduğuna ve ayrı ayrı lisanslanmaları gerektiğine dikkat edin.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (yalnızca deneme)
Ücretsiz deneme süresi boyunca, hizmeti Office 2013 görüntüsüyle test edebilirsiniz.

Bu görüntü, temel alınan görüntünün bir uzantısıdır ve Windows Server 2012 R2 görüntüsünde açıklanan bileşenlere ek olarak aşağıdaki Microsoft Office 2013 Professional Plus bileşenlerini içerir:

* Access
* Excel
* Lync
* OneNote
* OneDrive İş (eşitleme aracısının Azure RemoteApp ile kullanılması desteklenmez)
* Outlook
* PowerPoint
* Project
* Visio
* Word
* Microsoft Office Yazım Denetleme Araçları

> [!IMPORTANT]
> **Yasal bilgiler:** Bu görüntü bir Microsoft Office lisansı içermez ve *üretim için kullanılamaz*. Office 2013 Professional Plus görüntüsü yalnızca deneme kullanımı için tasarlanmıştır. Office uygulamalarını Azure RemoteApp içinde üretim için kullanmak istiyorsanız, Office 365 ProPlus görüntüsünü kullanmanız gerekir. Office’i lisanslama hakkında daha fazla bilgi için bkz. [Azure RemoteApp ile Office 365’i kullanma](remoteapp-o365.md)
> 
> 


