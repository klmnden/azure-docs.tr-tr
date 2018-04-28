---
title: Microsoft Power BI Embedded kullanmaya başlama | Microsoft Docs
description: İş zekası uygulamanıza Power BI Embedded
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/11/2018
ms.author: maghan
ms.openlocfilehash: 7b604f9a26fc4c9a2c76a28ca01d066fe1640718
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Microsoft Power BI Embedded kullanmaya başlama

**Power BI Embedded**, bağımsız yazılım satıcılarına (ISV) ve geliştiricilere kapasite tabanlı, saatlik ölçüm modeli aracılığıyla uygulamalarına etkileyici görseller, raporlar ve panolar ekleme imkanı sunar.

![Ekleme akış şeması](media/get-started/introduction.png)

Power BI Embedded; ISV'lere, geliştiricilerine ve müşterilerine avantajlar sunar. Örneğin bir ISV, Power BI Desktop kullanarak ücretsiz görsel oluşturmaya başlayabilir. ISV'ler görsel analitik geliştirme çabalarını en aza indirerek pazarlama süresini hızlandırabilir ve farklı veri deneyimleri sunarak rakiplerinden sıyrılabilir. ISV'ler ayrıca eklenmiş analitiklerle oluşturulan katma değer için ek ücret tahsil edebilir.

Geliştiriciler zamanlarını görselleri ve analitikleri geliştirmeye harcamak yerine uygulamalarının temel özelliklerini geliştirmeye ayırabilir. Geliştiriciler ayrıntılı belgelere sahip API'leri ve SDK'ları kullanarak müşteri raporlarının ve panoların taleplerini hızlı bir şekilde karşılayabilir. Son olarak ISV'ler uygulamalarında gezinmesi kolay veri bağlamını kullanarak müşterilerin tüm cihazlardan bağlam dahilinde ve rahat veri kaynaklı kararlar almasını sağlayabilir.

## <a name="register-an-application-within-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'ye kaydetme

Özel uygulamaya ekleme yapmak için Azure Active Directory'ye (AAD) kayıtlı bir uygulama kullanılması gerekir. Kayıtlı uygulama için kiracınız bir Power BI kiracısı olmalıdır. Power BI kiracısı, kuruşta Power BI kaydı oluşturmuş olan en az bir kullanıcı olduğu anlamına gelir. Bir kullanıcının Power BI kaydı oluşturması, Power BI API'lerinin kayıtlı uygulama içinde gösterilmesini sağlar.

Bir uygulamayı AAD'ye kaydetme hakkında daha fazla bilgi için bkz. [Power BI içeriğini eklemek için bir Azure AD uygulamasını kaydetme](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

## <a name="embed-content-in-your-application"></a>Uygulamanıza içerik ekleme

Uygulamanızı AAD'ye kaydettikten sonra Power BI içeriğini uygulamaya ekleyebilirsiniz. İçerik eklemek için REST API ile JavaScript API'lerini kullanın.

Başlamanıza yardımcı olmak size birkaç örnek sunuyoruz. Örnek bir kılavuz için [Uygulamanızla bir pano, kutucuk veya rapor tümleştirme](https://powerbi.microsoft.com/documentation/powerbi-developer-embed-sample-app-owns-data/).

## <a name="get-capacity-and-move-to-production"></a>Kapasite oluşturma ve üretim aşamasına geçme

Uygulamanıza üretim aşamasına geçirmek için Microsoft Azure'da Power BI Embedded kapasitesi oluşturun. Kapasite oluşturma hakkında bilgi almak için bkz. [Azure portalında Power BI Embedded kapasitesi oluşturma](create-capacity.md).

> [!IMPORTANT]
> Ekleme belirteçleri yalnızca geliştirme testlerine yönelik olduğu için, bir Power BI ana hesabının oluşturabileceği ekleme belirteçlerinin sayısı sınırlıdır. Üretim ekleme senaryoları için [kapasite satın alınmalıdır](https://docs.microsoft.com/power-bi/developer/embedded-faq#technical). Bir kapasite satın alındığında ekleme belirteci oluşturma ile ilgili herhangi bir sınır yoktur.

Kapasitenizi Power BI yönetim portalından yönetin. Uygulama çalışma alanlarınız hakkında destek almak için çalışma alanı atama özelliğini kullanın. Daha fazla bilgi için bkz. [Power BI Premium ve Power BI Embedded kapasitelerini yönetme](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/).

## <a name="next-steps"></a>Sonraki adımlar

Power BI Embedded kapasitesi oluşturmaya hazırsanız bkz. [Azure portalında Power BI Embedded kapasitesi oluşturma](create-capacity.md).

Örnek bir kılavuz arıyorsanız bkz. [Uygulamanızla bir pano, kutucuk veya rapor tümleştirme](https://powerbi.microsoft.com/documentation/powerbi-developer-embed-sample-app-owns-data/).

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)
