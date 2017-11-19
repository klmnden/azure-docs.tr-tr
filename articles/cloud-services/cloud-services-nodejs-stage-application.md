---
title: "Bir bulut hizmeti dağıtımı (Node.js). Aşama | Microsoft Docs"
description: "Azure uygulamanızı hazırlama ortamınıza dağıtın ve ardından sanal IP (VIP) takas kullanarak bir üretim ortamına dağıtma hakkında bilgi edinin."
services: cloud-services
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: 307741d0792b34332d98bfa4f2d62c9fd1cf07a1
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="staging-an-application-in-azure"></a>Bir Azure uygulamasında hazırlama
Paketlenmiş bir uygulama, uygulama Internet üzerinden erişilebilir üretim ortamına geçmeden önce sınanacak Azure hazırlama ortamında dağıtılabilir. Yalnızca Azure tarafından oluşturulan karıştırılmış bir URL ile aşamalı uygulama erişimi hazırlama ortamında tam olarak üretim ortamı gibi olmasıdır. Uygulamanızın düzgün çalıştığını doğruladıktan sonra üretim ortamına bir sanal IP (VIP) değiştirme gerçekleştirerek dağıtılabilir.

> [!NOTE]
> Bu makaledeki adımları yalnızca bir Azure bulut hizmeti olarak barındırılan düğümü uygulamalar için de geçerlidir.
> 
> 

## <a name="step-1-stage-an-application"></a>1. adım: uygulamayı aşamalandırma
Bu görev, bir uygulama kullanarak hazırlamak alınmaktadır **Microsoft Azure PowerShell**.

1. Hizmet yayımlama olduğunda, yalnızca geçir **-yuvası** parametresi **Publish-AzureServiceProject** cmdlet'i.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Oturum [Klasik Azure portalı] seçip **bulut Hizmetleri**. Sonra bulut hizmeti oluşturulur ve **hazırlama** sütun durum güncelleştirilmiştir **çalıştıran**, hizmet adına tıklayın.
   
   ![portal çalışan bir hizmeti görüntüleme][cloud-service]
3. Seçin **Pano**ve ardından **hazırlama**.
   
   ![Bulut hizmeti Panosu][cloud-service-dashboard]
4. Değeri Not **Site URL'si** sağındaki girişi. DNS adı Azure oluşturulan karıştırılmış bir iç kimliğidir.
   
    ![site URL'si][cloud-service-staging-url]

Şimdi uygulamayı doğru ve hazırlık ortamında hazırlama sitesi URL'yi kullanarak çalıştığını doğrulayabilirsiniz.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>2. adım: uygulama üretim yuvasıyla takas ediliyor VIP'ler tarafından yükseltme
Hazırlama ortamında bir uygulama yükseltilmiş sürümünü doğruladıktan sonra hızlı bir şekilde, üretim hazırlama ve üretim ortamlarını sanal IP (VIP) takas tarafından yapabilirsiniz.

> [!NOTE]
> Bu adım zaten üretim uygulamaya dağıtılan ve uygulamanın yükseltilmiş sürümünü hazırlanmış olduğunu varsayar.
> 
> 

1. İçine oturum [Klasik Azure portalı], tıklatın **bulut Hizmetleri** ve hizmet adını seçin.
2. Gelen **Pano**seçin **hazırlama**ve ardından **takas** sayfanın sonundaki. Bu VIP takas iletişim kutusunu açar.
   
   ![VIP takas iletişim][vip-swap-dialog]
3. Bilgileri gözden geçirin ve ardından **Tamam**. Bu iki dağıtımı üretime hazırlama dağıtım geçer ve Üretim dağıtımı için hazırlama anahtarları güncelleştirme başlar.

Başarıyla bir dağıtıma hazırlanan ve hazırlama dağıtımda olan VIP'ler takas Üretim dağıtımı yükseltildi.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure'da VIP'ler takas tarafından hizmet yükseltmesi üretime dağıtma]

[Klasik Azure portalı]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Azure'da VIP'ler takas tarafından hizmet yükseltmesi üretime dağıtma]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
