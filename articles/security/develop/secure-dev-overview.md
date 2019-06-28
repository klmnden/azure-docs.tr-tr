---
title: Microsoft Azure ile ilgili en iyi geliştirme güvenliğini sağlama
description: Daha güvenli kod geliştirin ve bulutta daha güvenli bir uygulama dağıtmanıza yardımcı olması için en iyi yöntemler.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: ee5c043038fbb747e90bca9530cbe2e94c8a95c2
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144470"
---
# <a name="secure-development-best-practices-on-azure"></a>Azure ile ilgili en iyi geliştirme güvenliğini sağlama
Bu makale serisi, güvenlik etkinliklerini ve bulut için uygulamalar geliştirirken dikkate alınması gereken denetimleri gösterir. Microsoft Security Development Lifecycle (SDL) ve güvenlik sorularını ve yaşam döngüsünün her aşamasında göz önünde kavramları aşamalarını ele alınmaktadır. Etkinlikleri ve yaşam döngüsünün her aşamasında tasarlamak, geliştirmek ve daha güvenli bir uygulamayı dağıtmak için kullanabileceğiniz Azure Hizmetleri tanımlamanıza yardımcı olmaktır.

Öneriler makalelerinde deneyimimizi ile Azure güvenlik ve müşterilerimizin deneyimleri gelirler. Belirli bir geliştirme projenizi aşaması sırasında dikkat etmeniz gereken için bu makaleler başvuru olarak kullanabilirsiniz, ancak aynı zamanda tüm baştan sona en az bir kez makaleleri okumanızı öneririz. Tüm makaleyi okumanızı, projenizin önceki aşamada atlamış kavramları tanıtır. Ürününüzü yayımlamadan önce bu kavramları, güvenli yazılımlar oluşturur, güvenlik uyumluluk gereksinimlerini karşılamak ve geliştirme maliyetleri düşürmenize yardımcı olabilir.

Bu makaleler için yazılım tasarımcılarının, geliştiricilerin kaynak amaçlara yönelik olduğunu ve test ediciler tüm düzeylerdeki kimin oluşturun ve güvenli Azure uygulamaları dağıtın.

## <a name="overview"></a>Genel Bakış

Güvenlik herhangi bir uygulamanın en önemli yönlerinden birisidir ve doğru almak için basit bir şey değildir. Neyse ki, Azure, yardımcı olabilecek birçok hizmet uygulamanızın bulut güvenliğini sağlar. Bu makaleler, etkinlikler ve Azure Hizmetleri, güvenli kod geliştirin ve bulutta daha güvenli bir uygulama dağıtmanıza yardımcı olması için yazılım geliştirme yaşam döngüsünün her aşamasında uygulayabileceğiniz adresi.

## <a name="security-development-lifecycle"></a>Güvenlik geliştirme yaşam döngüsü

Güvenli bir yazılım geliştirme proje metodolojiyi ne olursa olsun, bakım için gereksinim analiz gelen yazılım geliştirme yaşam döngüsünün her aşamasına güvenlik tümleştirme gerektirir. en iyi uygulamaları takip ([şelale](https://en.wikipedia.org/wiki/Waterfall_model), [Çevik](https://en.wikipedia.org/wiki/Agile_software_development), veya [DevOps](https://en.wikipedia.org/wiki/DevOps)). Yüksek profilli veri ihlallerinin Uyandırma ve işletimsel güvenlik açıkları yararlanılması, geliştiricilerin güvenlik geliştirme süreci boyunca ele alınması gerektiğini anlamak.

Daha sonra bir sorunu gidermek olacak düzeltin. daha fazla geliştirme yaşam döngünüzü maliyet. Güvenlik, hiçbir özel durum sorunlardır. Yazılım geliştirmenizin erken aşamalarında güvenlik konularını göz ardı, izleyen her aşama önceki aşamanın güvenlik açıklarını devralabilir. Birden çok güvenlik sorunlarını ve ihlal olasılığını son ürününüzü birikti. Güvenlik geliştirme yaşam döngüsünün her aşamasında oluşturma, sorunları erkenden yakalayabilir yardımcı olur ve geliştirme maliyetlerinizi düşürmenize yardımcı olur.

Biz izlenmesi Microsoft [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) etkinlikleri ve güvenli bir yazılım geliştirme yöntemleri yaşam döngüsünün her aşamasında karşılamak için kullanabileceğiniz Azure Hizmetleri.

SDL aşamaları şunlardır:

  - Eğitim
  - Gereksinimler
  - Tasarım
  - Uygulama
  - Doğrulama
  - Yayınla
  - Yanıt

![Güvenlik geliştirme yaşam döngüsü](./media/secure-dev-overview/01-sdl-phase.png)

Biz SDL aşamalar tasarım Grup Bu makaleler, geliştirin ve dağıtın.

## <a name="engage-your-organizations-security-team"></a>Kuruluşunuzun güvenlik ekibi etkileşim kurun

Kuruluşunuzun güvenlik etkinliklerle baştan sona geliştirme yaşam döngüsü boyunca yönetmenize yardımcı olan bir biçimsel uygulama güvenlik programı olabilir. Kuruluşunuzun güvenlik ve uyumluluk takımı varsa, uygulamanızı geliştirmeye başlamadan önce bunları etkileşim kurmak amacıyla emin olun. SDL her aşamada, kaçırılan herhangi bir görev olup olmadığını isteyin.

Birçok okuyucu etkileşim kurmak için güvenlik ve uyumluluk takım olmayabilir biliyoruz. Bu makaleler, güvenlik sorularını ve SDL her aşamada dikkate almanız gereken kararların yönlendirmeye yardımcı olabilirsiniz.

## <a name="resources"></a>Kaynaklar

Güvenli uygulamalar geliştirme hakkında daha fazla bilgi edinmek ve uygulamalarınızı azure'da güvenli hale getirmek için aşağıdaki kaynakları kullanın:

[Microsoft Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) – SDL, geliştiricilerin daha güvenli yazılımlar derlemesine yardımcı olan Microsoft gelen bir yazılım geliştirme işlemdir. Yardımcı geliştirme maliyetlerini düşürürken güvenlik uyumluluk gereksinimlerini karşılayın.

[Açık Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page) – OWASP, ücretsiz makaleler, yöntemleri, belgeler, araçları ve teknolojileri alanında web uygulaması güvenlik üreten bir çevrimiçi bir topluluktur.

[Sol gibi bir Patronunuzdan gönderme](https://code.likeagirl.io/pushing-left-like-a-boss-part-1-80f1f007da95?WT.mc_id=docs-blog-tajanca) – farklı geliştiriciler daha güvenli kod oluşturma için tamamlaması uygulama güvenlik etkinlik türleri özetlenmektedir çevrimiçi makaleleri bir dizi.

[Microsoft kimlik platformu](https://docs.microsoft.com/azure/active-directory/develop/) – Microsoft kimlik platformu olan Azure AD kimlik hizmeti ve geliştirici platformu bir halidir. Bu kimlik doğrulama hizmeti, açık kaynak kitaplıkları, uygulama kaydı ve yapılandırması, tam Geliştirici belgeleri, kod örnekleri ve diğer Geliştirici içeriği oluşan bir tam özellikli platformudur. Microsoft kimlik platformu, OAuth 2.0 ve Openıd Connect gibi endüstri standardı protokolleri destekler.

[Azure çözümleri için önerilen güvenlik uygulamaları](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions/) – tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinde yönetin sırasında kullanılacak en iyi güvenlik yöntemleri topluluğu. Bu yazıda, BT uzmanları için kaynak edilmeye yöneliktir. Bu, tasarımcılar, mimarları, geliştiriciler ve oluşturan güvenli Azure çözümlerini dağıtmak ve test ediciler içerebilir.

[Güvenlik ve uyumluluk şemaları azure'da](https://servicetrust.microsoft.com/ViewPage/BlueprintOverview) – Azure güvenlik ve uyumluluk şemaları yardımcı olabilecek kaynakları oluşturmak ve katı düzenlemeleri ve standartları ile uyumlu bulut destekli uygulamalarını verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenlik denetimleri öneririz ve yardımcı olabilecek etkinlikleri tasarlamak, geliştirmek ve güvenli uygulamalar dağıtın.

- [Güvenli uygulamalar tasarlama](secure-design.md)
- [Güvenli uygulamalar geliştirin](secure-develop.md)
- [Güvenli uygulamalar dağıtma](secure-deploy.md)
