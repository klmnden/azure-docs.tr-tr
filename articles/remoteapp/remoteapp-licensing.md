---
title: Azure RemoteApp lisanslama | Microsoft Belgeleri
description: "Azure RemoteApp’te lisanslamanın nasıl çalıştığını öğrenin."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: ff8ebd20-61a1-4f10-87a6-234a170534c9
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 43d0dbb905b2f2b9d98fb3bf8c073ba1c4b6f4c4
ms.lasthandoff: 03/31/2017


---
# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Azure RemoteApp’te lisanslama nasıl çalışır?
> [!IMPORTANT]
> Azure RemoteApp 31 Ağustos 2017’de kullanımdan kaldırılacaktır. Ayrıntılı bilgi için [duyuruyu](https://go.microsoft.com/fwlink/?linkid=821148) okuyun.
> 
> 

Azure RemoteApp hizmetinizi ayarladınız, şablonlarınızı oluşturdunuz ve kullanıcılarınıza uygulama yayımlamaya hazırsınız. Ancak hala çözülmesi gereken son bir konu var: lisanslama. RemoteApp ve RemoteApp aracılığıyla paylaştığınız uygulamalar için lisanslama nasıl çalışır?

RemoteApp herhangi bir Windows lisansı veya Uzak Masaüstü CAL’si gerektirmez. Aboneliğiniz RemoteApp tarafındaki lisans sorunlarını çözer. (Ayrıntıları [fiyatlandırma planlarında](https://azure.microsoft.com/pricing/details/remoteapp) kontrol edebilirsiniz.)

Aboneliğinize dahil edilen görüntülerden birini kullanırsanız, bu görüntüdeki herhangi bir uygulamayı ayrı bir lisans olmadan paylaşabilirsiniz. Örneğin, koleksiyonunuzu oluşturmak için Windows Server 2012 R2 şablon görüntüsünü kullanıyorsanız, System Center Endpoint Protection’ı kullanıcılarınızla paylaşabilirsiniz. Bu kuralın tek özel durumu, ayrı bir abonelik gerektiren Office 365 ProPlus ve bir üretim koleksiyonunda paylaşılamayan Office 2013'tür.

RemoteApp ile birlikte gelen Office 365 şablon görüntüsünü kullanmak istiyorsanız, *mevcut* bir Office 365 ProPlus planınızın olması gerekir. Özel bir şablon kullanarak yayımladığınız herhangi bir Office 365 uygulaması için de aynı durum geçerlidir. Uygulamaları kendi aboneliğinizle etkinleştirmeniz gerekir. Bu deneme abonelikleri ve ücretli abonelikler için geçerlidir. Deneme süresi boyunca Office 365 şablon görüntüsünü kullanmak istiyorsanız, *ve zaten bir aboneliğiniz yoksa*, Office 365 sayfasına giderek bir deneme aboneliği için [kaydolun](https://go.microsoft.com/fwlink/p/?LinkID=403802). Daha fazla bilgi için bkz. [RemoteApp ve Office nasıl birlikte çalışır?](remoteapp-o365.md)

Deneme süresi boyunca, bir Office 365 deneme aboneliği istemiyorsanız, RemoteApp ile birlikte gelen Office 2013 Professional Plus şablon görüntüsünü kullanın. Bu şablon görüntüsü yalnızca 30 gün boyunca kullanılabilir ve ücretli bir koleksiyona dönüştürülemez.

Diğer uygulamalar için, uygulamayı paylaşmak için lisansa sahip olduğunuzdan emin olmanız gerekir.

Kulağa mantıklı geliyor, değil mi? Yasal olarak paylaşma hakkınız olan herhangi bir uygulamayı yayımlayabilirsiniz. Programlarınızı paylaşmak için gerçekten yetkili olduğunuzdan emin olmanız gerekir.

Lütfen bir bulut koleksiyonunda CAL veya Toplu Lisans sözleşmesi kullanamayacağınızı unutmayın. Karma koleksiyonunuzda uygulamaları etkinleştirmek için bir Toplu Lisans sözleşmesi *kullanabilirsiniz* (Office hariç). Bunları Toplu Lisans medyasından şablon görüntünüze yüklemeniz yeterlidir. Lisansları bir Uzak Masaüstü ortamında yüklemek için uygulama satıcısından aldığınız bilgiler doğrultusunda hareket edin.


