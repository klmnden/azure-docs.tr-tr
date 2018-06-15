---
title: Adlandırılmış kimlik doğrulama kimlik bilgilerini ayarlayın | Microsoft Docs
description: Visual Studio Azure uygulamaya Visual Studio'dan yayımlamak ya da var olan bir bulut hizmetini izlemeyi Azure, isteklerine kimlik doğrulaması için kullandığı kimlik bilgilerini sağlamanız öğrenin.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 61570907-42a1-40e8-bcd6-952b21a55786
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 3dce80f5c6611cb2a22293d1a574980db5ea8378
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791588"
---
# <a name="set-up-named-authentication-credentials"></a>Adlandırılmış kimlik doğrulama kimlik bilgilerini ayarlayın

Bir uygulamayı Azure'a yayımlama ya da var olan bir bulut hizmetini izlemek için Visual Studio isteklerine Azure, Azure abonelik Kimliğinizi ve en az 2048 bit anahtara sahip geçerli bir X.509 v3 sertifikası kimlik doğrulaması için kimlik bilgileri gerektirir. Aşağıdaki yöntemlerden birini kullanarak bu kimlik bilgileri sağlayın:

- Visual Studio seçin **Görünüm > Sunucu Gezgini**, sağ **Azure** düğümü, select **Microsoft Azure aboneliğine bağlanma**ve oturum açın.
- Bir abonelik dosyası oluşturun (`.publishsettings`), bir ortak anahtar sertifikası içerir. Abonelik dosyası, bu makalede anlatıldığı gibi birden fazla aboneliğiniz için kimlik bilgileri içerebilir.

Not: Bu kimlik bilgileri Azure storage Hizmetleri isteklerine kimlik doğrulaması için kullanılan kimlik bilgileri farklıdır.

## <a name="create-a-subscription-file"></a>Bir abonelik dosyası oluşturma

Sunucu Gezgini'nde sağ **Azure** düğümü ve select **yönetin ve filtre abonelikleri**. Ardından **sertifikaları** sekmesini ve ardından aşağıdaki işlemlerden birini yapın:

- Seçin **alma** açmak için **alma Microsoft Azure abonelikleri** iletişim kutusu. Seçin **indirme abonelik dosyası** bağlamak ve tarayıcıda indirilen dosya geçici bir konuma kaydedin. İletişim kutusu, geri döndüğünüzde indirme konumuna göz atın ve kimlik doğrulaması kullanmak için alma.
- Etkin bir aboneliğiniz seçip **Düzenle**, kimlik doğrulaması kullanmak için mevcut bir aboneliğe Düzenle iletişim kutusu açılır.
- Seçin **yeni** açmak için **yeni abonelik** iletişim kutusuna ve gerekli ayrıntıları sağlar. Bulut için sertifikayı karşıya yüklemek için hizmet belirtilmiştir iletişim kutusunda, Azure portalında oturum açın, bulut hizmetinize select gidin **ayarlar > Yönetim sertifikaları**seçin **karşıya**, sonra yolunu belirtin `.cer` dosya.

Bir sertifika kendiniz oluşturmak istiyorsanız,'ndaki yönergeleri başvurabilirsiniz [oluşturun ve bir yönetim sertifikası için Azure yükleyin](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) ve sertifikayı el ile karşıya [Azure portal](https://portal.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

- [Web uygulamaları genel bakış](https://docs.microsoft.com/azure/app-service/)
- [Uygulamanızı Azure App Service'e dağıtma](https://docs.microsoft.com/azure/app-service/app-service-deploy-local-git) 
- [Visual Studio kullanarak Web İşleri’ni dağıtma](https://docs.microsoft.com/azure/app-service/websites-dotnet-deploy-webjobs)
- [Oluşturma ve bir bulut hizmeti dağıtma](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)