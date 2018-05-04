---
title: Şirket içi veri merkezinizi Azure'a geçirme | Microsoft Docs
description: Şirket içi veri merkezlerinizi Azure'a geçirme hakkında bir genel bakış teknik incelemesini okuyun.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/21/2018
ms.author: raynew
ms.openlocfilehash: 8ba490998ea5f20efca591327716a6e39e9c1ba8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="migrating-your-on-premises-workloads-to-azure"></a>Şirket içi iş yüklerinizi Azure’a geçirme


Microsoft Azure, geliştiriciler ve BT uzmanları olarak küresel bir veri merkezi ağı aracılığıyla bir dizi araç ve çerçeve üzerinde uygulama oluşturmak, uygulamaları dağıtmak ve yönetmek için kullanabileceğiniz kapsamlı bir bulut hizmetleri kümesine erişim sağlar. İşiniz dijital değişimle ilişkili zorluklarla karşılaştığında Azure, kaynakları ve işlemleri en iyi duruma nasıl getireceğinizi, müşteriler ve çalışanlar ile nasıl etkileşim kuracağınızı ve ürünlerinizi nasıl dönüştüreceğinizi belirlemenize yardımcı olur.

Ancak Azure, bulutun hız ve esneklik, düşük maliyetler, performans ve güvenilirlik açısından sağladığı tüm avantajlarla bile çoğu kuruluşun bir süre daha şirket içi veri merkezlerini çalıştırması gerekeceğinin bilincindedir. Bulutu benimsemenin önündeki engellere karşı Azure, şirket içi veri merkezleriniz ile Azure genel bulutu arasında köprü işlevi görecek bir hibrit bulut stratejisi sunar. Örneğin, şirket içi kaynakları korumak için yedekleme gibi Azure bulut kaynaklarını kullanma veya şirket içi iş yüklerine ilişkin içgörü elde etmek için Azure Analytics’i kullanma. 

Hibrit bulut stratejimizin bir parçası olarak Azure, şirket içi uygulamaların ve iş yüklerinin buluta geçirilmesi konusunda, sayısı her geçen gün artan çözümler sunar. Basit adımları uygulayarak, şirket içi kaynaklarınızın Azure bulutunda nasıl çalışacağını belirlemek için bunları kapsamlı bir şekilde değerlendirebilirsiniz. Daha sonra, elde ettiğiniz kapsamlı değerlendirme ile kaynakları Azure’a güvenle geçirebilirsiniz. Kaynaklar Azure’da hazır ve çalışır duruma geldiğinde erişimi, esnekliği, güvenliği ve güvenilirliği sürdürüp geliştirmek için bu kaynakları iyi duruma getirebilirsiniz.

Bu geçiş makaleleri dizisi, şirketiniz için geçiş stratejisini nasıl planlayıp oluşturabileceğinizi gösterir. Makaleler artan karmaşıklığa ilişkin olarak, zaman içinde daha fazlasını ekleyeceğimiz çeşitli senaryolar gösterir. Bu senaryolar aşağıdaki tabloda özetlenmiştir. Her senaryo için bir genel bakış ve geçiş mimarisi sağlamanın yanı sıra geçiş işleminde yer alan adımları da gösteriyoruz. 

**Senaryo** | **Çözüm** | **Hizmetler** | **Makale** 
--- | --- | --- | ---
[1. Senaryo: Bulma ve değerlendirme](migrate-scenarios-assessment.md) | Bul ve şirket içi uygulamalar, veriler ve geçiş için Azure altyapı değerlendirin | Data Migration Yardımcısı, Azure Geçişi hizmeti  | Kullanıma sunuldu
**[Senaryo 2: Rehost uygulama](migrate-scenarios-lift-and-shift.md)** | Yükseltme ve shift uygulamalar Azure. | Azure Site Recovery, Azure Veritabanı Geçiş Hizmeti, Azure SQL Yönetilen Örneği | Kullanıma sunuldu
**Senaryo 3: Düzenleme uygulama** | Uygulamaları Azure geçiş sırasında yeniden düzenleyin. | Planlanıyor | Planlandı
**Senaryo 4: Rearchitect uygulama** | Azure geçiş sırasında uygulamaları rearchitect. | Planlanıyor | Planlandı
**Senaryo 5: Yeniden uygulama** |Uygulamaları Azure geçiş sırasında yeniden oluşturma | Planlanıyor | Planlandı




