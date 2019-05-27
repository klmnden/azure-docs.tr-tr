---
title: Azure Cloud Services'ta bir rol için Uzak Masaüstü Bağlantısı etkinleştirme | Microsoft Docs
description: Azure bulut hizmeti uygulamanızı Uzak Masaüstü bağlantılarına izin verecek şekilde yapılandırma
services: cloud-services
documentationcenter: ''
author: mmccrory
manager: timlt
editor: ''
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: memccror
ms.openlocfilehash: 0c36dc5fb6b2754fc93a02e29d8d8ae74df36da7
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963277"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Azure Cloud Services'ta bir rol için Uzak Masaüstü bağlantısını etkinleştirme

> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan rolünün erişmenizi sağlar. Sorun giderme ve çalışırken uygulamanızdaki sorunları tanılamak için Uzak Masaüstü Bağlantısı'nı kullanabilirsiniz.

Uzak Masaüstü modülleri, hizmet tanımında dahil ederek geliştirme sırasında rolde bir Uzak Masaüstü Bağlantısı etkinleştirebilirsiniz veya Uzak Masaüstü aracılığıyla Uzak Masaüstü uzantısı etkinleştirmeyi seçebilirsiniz. Tercih edilen yaklaşım uygulamanızı yeniden dağıtmaya gerek kalmadan uygulama bile dağıtıldıktan sonra Uzak Masaüstü'nü etkinleştirme gibi Uzak Masaüstü uzantısı kullanmaktır.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>Azure portalından Uzak Masaüstü'nü yapılandırma

Bile uygulama dağıtıldıktan sonra Uzak Masaüstü verebilmeniz için Azure portalında bir Uzak Masaüstü uzantısı yaklaşım kullanır. **Uzak Masaüstü** bulut hizmetiniz için ayarları sağlar, Uzak Masaüstü'nü etkinleştirme, sanal makinelere bağlanmak için kullanılan yerel yönetici hesabını değiştirmek, sertifika kimlik doğrulamasında kullanılan ve sona erme süresini ayarlayabilir tarih.

1. Tıklayın **Cloud Services**bulut hizmeti adını seçin ve ardından **Uzak Masaüstü**.

    ![Bulut Hizmetleri Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Tüm rolleri veya ayrı bir rol için Uzak masaüstünü etkinleştirme ve ardından için değiştirici değerini değiştirmek isteyip istemediğinizi **etkin**.

3. Kullanıcı adı, parola, süre sonu ve sertifika için gerekli alanları doldurun.

    ![Bulut Hizmetleri Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > İlk Uzak Masaüstü'nü etkinleştirme ve seçtiğiniz zaman tüm rol örneklerinin başlatılması **Tamam** (onay işareti). Yeniden başlatma önlemek için parolayı şifrelemek için kullanılan sertifikanın bu rolü yüklenmelidir. Yeniden başlatma önlemek için [bulut hizmeti için bir sertifika karşıya](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) ve ardından bu iletişim kutusuna dönün.

4. İçinde **rolleri**, güncelleştirme veya seçmek istediğiniz rolü seçin **tüm** tüm roller için.

5. Yapılandırma güncelleştirmelerinizi bitirdikten sonra seçin **Kaydet**. Rol örneklerinizin bağlantıları için hazır olmadan önce birkaç dakika sürer.

## <a name="remote-into-role-instances"></a>Uzaktan rol örneklerine bağlanma

Uzak Masaüstü roller üzerinde etkinleştirildikten sonra Azure portalından doğrudan bir bağlantısını başlatabilirsiniz:

1. Tıklayın **örnekleri** açmak için **örnekleri** ayarları.
2. Yapılandırılmış Uzak Masaüstü sahip rol örneği seçin.
3. Tıklayın **Connect** rol örneği için bir RDP dosyası indirilemedi.

    ![Bulut Hizmetleri Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Tıklayın **açık** ardından **Connect** Uzak Masaüstü Bağlantısı'nı başlatmak için.

>[!NOTE]
> Bulut hizmetinize bir NSG yer, bağlantı noktalarında trafiğe izin veren kuralları oluşturmanız gerekebilir **3389** ve **20000**.  Uzak Masaüstü bağlantı noktasını kullanır **3389**.  Bulut hizmeti örneklerinin, yük dengeli olduğundan doğrudan bağlanmak için hangi örneğinin denetleyemezsiniz.  *RemoteForwarder* ve *RemoteAccess* aracılar, RDP trafiği yönetmek ve bir RDP tanımlama bilgisi göndermek ve bağlanmak için tek bir örneğini belirtmek istemcinin sağlamak.  *RemoteForwarder* ve *RemoteAccess* aracıları gerektiren bu bağlantı noktasını **20000*** olan açık bir NSG varsa, engellenmiş olabilir.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)
