---
title: Bir Azure bulut hizmeti Visual Studio ve IntelliTrace ile bir yayımlanan hata ayıklama | Microsoft Docs
description: Bir bulut hizmeti Visual Studio ve IntelliTrace ile hata ayıklama öğrenin
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: mikejo
ms.openlocfilehash: 2ca15bd5ffa88d2e8053decf5b81c265b1d9c6e1
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
ms.locfileid: "30292566"
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Yayımlanan Azure bulut hizmeti Visual Studio ve IntelliTrace ile hata ayıklama
IntelliTrace ile Azure içinde çalıştığında bir rol örneği için kapsamlı hata ayıklama bilgileri oturum açabilir. Bir sorunun nedenini bulmak gerekiyorsa, Azure'da çalışıyormuş gibi kodunuzu Visual Studio'dan adım için IntelliTrace günlüklerini kullanabilirsiniz. Etkin, Azure uygulamanız Azure'daki bir bulut hizmeti olarak çalışan ve Visual Studio kaydedilen verileri yeniden yürütme olanak tanır, IntelliTrace kayıtları kodu yürütme ve ortam verilerini anahtarı. 

Visual Studio Enterprise varsa IntelliTrace ve Azure uygulamanızı hedeflerinizi .NET Framework 4 veya sonraki bir sürümünü kullanabilirsiniz. IntelliTrace Azure rollerinizi için bilgi toplar. Sanal makineler için her zaman 64-bit işletim sistemlerini çalıştıran bu rolleri.

Alternatif olarak, kullandığınız [uzaktan hata ayıklama](http://go.microsoft.com/fwlink/p/?LinkId=623041) doğrudan Azure'da çalışan bir bulut hizmetine eklemek için.

> [!IMPORTANT]
> IntelliTrace yalnızca hata ayıklama senaryoları için tasarlanmıştır ve Üretim dağıtımı için kullanılmamalıdır.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>IntelliTrace için bir Azure uygulaması yapılandırma
IntelliTrace için Azure uygulama etkinleştirmek için oluşturma ve Visual Studio Azure project uygulamayı yayımlama. Azure'da yayımlamadan önce Azure uygulamanız için IntelliTrace yapılandırmanız gerekir. IntelliTrace yapılandırmadan uygulamanızı yayımlarsanız, projeyi yeniden yayımlamanız gerekir. Daha fazla bilgi için bkz: [yayımlama bir Azure bulut hizmeti Visual Studio kullanarak projeleri](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Proje derleme hedeflerinizi ayarlanır Azure uygulamanızı dağıtmak hazır olduğunuzda doğrulayın **hata ayıklama**.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **Yayımla**.
   
1. İçinde **Azure uygulamasını Yayımla** iletişim kutusu, Azure aboneliği seçin ve Seç **sonraki**.

1. İçinde **ayarları** sayfasında, **Gelişmiş ayarları** sekmesi.

1. Aç **IntelliTrace'i etkinleştirin** bulutta yayımlandığında, uygulamanız için IntelliTrace günlükleri toplamak için seçeneği.
   
1. Temel IntelliTrace yapılandırmasını özelleştirmek için seçin **ayarları** yanına **IntelliTrace'i etkinleştirin**.

    ![IntelliTrace settings link](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. İçinde **IntelliTrace ayarları** iletişim kutusunda, günlük, mi çağrı bilgileri toplamak, hangi modülleri ve toplamak için işlemler günlüklerde ve kayıt ayırmak için ne kadar alan hangi olayların belirtebilirsiniz. IntelliTrace hakkında daha fazla bilgi için bkz: [IntelliTrace ile hata ayıklama](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![IntelliTrace ayarları](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

IntelliTrace günlüğünü (varsayılan boyutu 250 MB'dır) IntelliTrace ayarlarında belirtilen en büyük boyutunu döngüsel günlük dosyasıdır. IntelliTrace günlüklerini sanal makine dosya sistemindeki bir dosyaya toplanır. Günlükleri istediğinizde, bir anlık görüntü zamandaki o noktada alınır ve yerel bilgisayara indirilir.

Azure bulut hizmeti için Azure yayımlandıktan sonra IntelliTrace Azure düğümünden etkinleştirilmiş olup olmadığını belirleyebilirsiniz **Sunucu Gezgini**aşağıdaki görüntüde gösterildiği gibi:

![Sunucu Gezgini - IntelliTrace etkin](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>Rol örneği için IntelliTrace günlüklerini indirin
Visual Studio kullanarak, aşağıdaki adımları izleyerek bir rol örneği için IntelliTrace günlüklerini yükleyebilirsiniz:

1. İçinde **Sunucu Gezgini**, genişletin **bulut Hizmetleri** düğümünü ve günlükleri indirmek istediğiniz rol örneği bulun. 

1. Rol örneği sağ tıklatın ve s bağlam menüsünden seçin **IntelliTrace günlüklerini görüntüle**. 

    ![IntelliTrace günlüklerini menü seçeneği görüntüleyin](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. IntelliTrace günlüklerini, yerel bilgisayarınızda bir dizindeki bir dosya indirilir. IntelliTrace isteği her zaman günlükleri, yeni bir anlık görüntüsü oluşturulur. Günlükleri karşıdan yüklenirken Visual Studio işlemin ilerlemesini görüntüler **Azure etkinlik günlüğü** penceresi. Aşağıdaki çizimde gösterildiği gibi daha fazla ayrıntı işlem için satır öğeyi genişletebilirsiniz.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Visual Studio IntelliTrace günlüklerini karşıdan yüklenirken çalışmaya devam edebilirsiniz. Günlük yükleme tamamlandığında, Visual Studio açılır.

> [!NOTE]
> IntelliTrace günlüklerini framework oluşturur ve daha sonra işleyen özel durumları içerebilir. İç framework kod bunları güvenle yoksayabilirsiniz bir rolü, başlatma için normal bir parçası olarak bu özel durumları oluşturur.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure bulut hizmetlerinde hata ayıklama seçenekleri](vs-azure-tools-debugging-cloud-services-overview.md)
- [Visual Studio kullanarak bir Azure bulut hizmetinde yayımlama](vs-azure-tools-publishing-a-cloud-service.md)