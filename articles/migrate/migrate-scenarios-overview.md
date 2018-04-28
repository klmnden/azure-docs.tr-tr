---
title: Şirket içi veri merkezinizi Azure'a geçirme | Microsoft Docs
description: Şirket içi veri merkezlerinizi Azure'a geçirme hakkında bir genel bakış teknik incelemesini okuyun.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: be322596da0c3e5ba18aa64285c437cdb823fc4b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="migrating-your-on-premises-workloads-to-azure"></a>Şirket içi iş yüklerinizi Azure’a geçirme


Microsoft Azure, geliştiriciler ve BT uzmanları olarak küresel bir veri merkezi ağı aracılığıyla bir dizi araç ve çerçeve üzerinde uygulama oluşturmak, uygulamaları dağıtmak ve yönetmek için kullanabileceğiniz kapsamlı bir bulut hizmetleri kümesine erişim sağlar. İşiniz dijital değişimle ilişkili zorluklarla karşılaştığında Azure, kaynakları ve işlemleri en iyi duruma nasıl getireceğinizi, müşteriler ve çalışanlar ile nasıl etkileşim kuracağınızı ve ürünlerinizi nasıl dönüştüreceğinizi belirlemenize yardımcı olur.

Ancak Azure, bulutun hız ve esneklik, düşük maliyetler, performans ve güvenilirlik açısından sağladığı tüm avantajlarla bile çoğu kuruluşun bir süre daha şirket içi veri merkezlerini çalıştırması gerekeceğinin bilincindedir. Bulutu benimsemenin önündeki engellere karşı Azure, şirket içi veri merkezleriniz ile Azure genel bulutu arasında köprü işlevi görecek bir hibrit bulut stratejisi sunar. Örneğin, şirket içi kaynakları korumak için yedekleme gibi Azure bulut kaynaklarını kullanma veya şirket içi iş yüklerine ilişkin içgörü elde etmek için Azure Analytics’i kullanma. 

Hibrit bulut stratejimizin bir parçası olarak Azure, şirket içi uygulamaların ve iş yüklerinin buluta geçirilmesi konusunda, sayısı her geçen gün artan çözümler sunar. Basit adımları uygulayarak, şirket içi kaynaklarınızın Azure bulutunda nasıl çalışacağını belirlemek için bunları kapsamlı bir şekilde değerlendirebilirsiniz. Daha sonra, elde ettiğiniz kapsamlı değerlendirme ile kaynakları Azure’a güvenle geçirebilirsiniz. Kaynaklar Azure’da hazır ve çalışır duruma geldiğinde erişimi, esnekliği, güvenliği ve güvenilirliği sürdürüp geliştirmek için bu kaynakları iyi duruma getirebilirsiniz.

Bu geçiş makaleleri dizisi, şirketiniz için geçiş stratejisini nasıl planlayıp oluşturabileceğinizi gösterir. Makaleler artan karmaşıklığa ilişkin olarak, zaman içinde daha fazlasını ekleyeceğimiz çeşitli senaryolar gösterir. Bu senaryolar aşağıdaki tabloda özetlenmiştir. Her senaryo için bir genel bakış ve geçiş mimarisi sağlamanın yanı sıra geçiş işleminde yer alan adımları da gösteriyoruz. 

**Senaryo** | **Çözüm** | **Hizmetler** | **Makale** 
--- | --- | --- | ---
[1. Senaryo: Bulma ve değerlendirme](migrate-scenarios-assessment.md) | Azure’a geçiş için şirket içi uygulamaları bulun ve değerlendirin | Data Migration Yardımcısı, Azure Geçişi hizmeti  | Kullanıma sunuldu
**2. Senaryo: Lift-and-shift ile geçiş** | Azure’da iç uygulamaları yeniden barındırın. Geçişten sonra Azure'da iyileştirme yapın. | Azure Site Recovery, Azure Veritabanı Geçiş Hizmeti, Azure SQL Yönetilen Örneği | Kullanıma sunuldu
**3. Senaryo: Yeniden düzenleme ve geçirme** | Azure’a geçiş sırasında şirket içi müşteri uygulamalarını modernleştirin ve yeniden düzenleyin. | Planlanıyor | Planlandı
**4. Senaryo: Yeniden oluşturma ve geçirme** | Azure’a geçiş sırasında müşteriye ait işlemsel web sitelerini yeniden oluşturun ve geçirin. | Planlanıyor | Planlandı
**5. Senaryo: Yeniden derleme** |Müşteri uygulamasını ve verilerini yeniden derleyin ve Azure'a geçirin | Planlanıyor | Planlandı




