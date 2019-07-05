---
title: Azure Güvenlik Merkezi'nde uygulama hizmetleri ile koruma | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi'nde, uygulama hizmetleri korumaya başlamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/27/2019
ms.author: v-mohabe
ms.openlocfilehash: 6a6b5b6e247bd7c105286b86257a6ae11c1d1c60
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551854"
---
# <a name="protect-app-service-with-azure-security-center"></a>App Service Azure Güvenlik Merkezi ile koruma
Bu makalede izleme ve App Service üzerinde çalışan uygulamalarınızı korumak için Azure Güvenlik Merkezi'ni kullanmanıza yardımcı olur.

App Service, yapı ve seçtiğiniz programlama dilinde web uygulamaları, altyapı yönetimine gerek kalmadan barındırmanıza olanak sağlar. App Service otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows ve Linux, hem de otomatik dağıtımlar GitHub, Azure DevOps ya da herhangi bir Git deposu desteği. 

Internet üzerinde neredeyse her kuruluş için bir ortak ve dinamik arabirimi sahip oldukları güvenlik açıklarını web uygulamalarında sık saldırganlar tarafından doğmasına. App Service üzerinde çalışan uygulamalar için istekleri her istek için karşılık gelen uygulama yönlendirme için sorumlu dünyanın dört bir yanındaki Azure veri merkezlerinde dağıtılmış birkaç ağ geçidine geçer. 

Azure Güvenlik Merkezi, değerlendirmesi ve öneriler, VM veya isteğe bağlı örnekleri korumalı alanlarda bulunan App Service'te çalışan uygulamalarınız üzerinde çalıştırabilirsiniz. Bulut sağlayıcısı olarak Azure olan görünürlüğü yararlanarak, Güvenlik Merkezi, birden fazla hedef arasında hangi sıklıkta çalıştırılacağını ortak web uygulaması saldırıları izlemek için App Service iç günlüklerini analiz eder.

Güvenlik Merkezi, App Service uygulamalarınızı saldırıları belirleyin ve saldırganlar keşif aşamasının birden çok Web siteleri, güvenlik açıklarını tanımlamak için tarama, Azure üzerinde barındırılan sırasında ortaya çıkan saldırılarından odaklanmak için bulutun ölçek yararlanır. Güvenlik Merkezi, tüm arabirimleri olmadığını HTTP üzerinden veya bir yönetim yöntemleri aracılığıyla kendi uygulamaları ile etkileşimde şekilde karşılamak için analiz ve makine öğrenimi modellerini kullanır. Ayrıca, azure'daki birinci taraf hizmet olarak Güvenlik Merkezi ayrıca temel alınan işlem düğümleri için bu PaaS ele alındığı ana bilgisayar tabanlı güvenlik analiz sunmak için benzersiz bir konumu, saldırılara karşı olan web uygulamalarını algılamak Güvenlik Merkezi etkinleştirildiğini zaten yararlanan.

## <a name="prerequisites"></a>Önkoşullar

App Service ortamınızı izlemek ve güvenliğini sağlamak için ayrılmış makinelerle ilişkilendirilen bir App Service planına sahip olmanız gerekir. Bu planlar şunlardır: Temel, Standart, Premium, Yalıtılmış veya Linux. Azure Güvenlik Merkezi Ücretsiz, Paylaşılan veya Tüketim planlarını desteklemez. Daha fazla bilgi için [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).

## <a name="security-center-protection"></a>Güvenlik Merkezi koruma

Azure Güvenlik Merkezi uygulama hizmetiniz çalışıyor VM örneği korur ve yönetim arabirimi. Ayrıca, istekleri ve yanıtları gönderilen ve App Service'te çalışan uygulamalarınızdan gelen izler.

Güvenlik Merkezi yerel olarak App Service, dağıtım ve ekleme gereksinimini ortadan ile tümleşik - tümleştirme tamamen saydamdır.



## <a name="enabling-monitoring-and-protection-of-app-service"></a>İzleme ve koruma App Service'ı etkinleştirme

1. Azure'da Güvenlik Merkezi'ni seçin.
2. Git **fiyatlandırma & ayarları** ve bir abonelik seçin.
3. Altında **fiyatlandırma katmanı**, **App Service'e** satır, geçiş planınıza **etkin**.

![App service Aç/Kapat](./media/security-center-app-services/app-services-toggle.png)

>[!NOTE]
> İçin kaynak miktarı listelenen örneklerinin sayısını, App Service fiyatlandırma katmanı dikey penceresi açıldığında şu anda etkin ilgili örnek sayısını temsil eder. Bu sayı ölçekleme seçeneklere göre değiştiği için ücretlendirilirsiniz örneklerinin uygun şekilde değiştirilecek.

Bu işlem ve iki durumlu izleme ve öneriler, App Service için devre dışı bırakmak için yineleyin, **App Service** planladığınız **devre dışı bırakılmış**.



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırmayı öğrenin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Uygulama Hizmetleri](security-center-virtual-machine-protection.md#app-services):  Bir listesini, App service ortamları ile sistem durumu özetlerini görüntüleyin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
