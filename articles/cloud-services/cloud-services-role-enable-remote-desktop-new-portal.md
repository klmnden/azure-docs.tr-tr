---
title: Azure bulut Hizmetleri rolünde için Uzak Masaüstü Bağlantısı etkinleştirme | Microsoft Docs
description: Azure bulut hizmeti uygulamanız Uzak Masaüstü bağlantılarına izin verecek şekilde yapılandırma
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
ms.author: mmccrory
ms.openlocfilehash: 2169fd95f51b468770a2e1e4c185d493babf220f
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
ms.locfileid: "29877373"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Azure bulut Hizmetleri rolünde için Uzak Masaüstü Bağlantısı etkinleştir

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan rolü Masaüstü erişmenize olanak tanır. Uzak Masaüstü Bağlantısı çalışırken, uygulamanızın sorunları tanılamak ve gidermek için kullanabilirsiniz.

Uzak Masaüstü Uzak Masaüstü uzantısı aracılığıyla etkinleştirmeyi seçebilirsiniz veya hizmet tanımında Uzak Masaüstü modüller ekleyerek geliştirme sırasında rolünüze Uzak Masaüstü Bağlantısı etkinleştirebilirsiniz. Tercih edilen yaklaşım uygulamanızı yeniden dağıtmak zorunda kalmadan uygulama bile dağıtıldıktan sonra Uzak Masaüstü'nü etkinleştirme gibi Uzak Masaüstü uzantısı kullanmaktır.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>Uzak Masaüstü Azure portalından yapılandırın

Azure portalı Uzak Masaüstü uzantısı yaklaşımı kullanır, bu nedenle bile uygulama dağıtıldıktan sonra Uzak Masaüstü etkinleştirebilirsiniz. **Uzak Masaüstü** bulut hizmetiniz için ayarları sağlar, Uzak Masaüstü'nü etkinleştirmek için sanal makinelere bağlanmak için kullanılan yerel yönetici hesabını değiştirmek, sertifika kimlik doğrulamasında kullanılan ve sona erme ayarlayın tarih.

1. Tıklatın **bulut Hizmetleri**, bulut hizmeti adını seçin ve ardından **Uzak Masaüstü**.

    ![Bulut Hizmetleri Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Tek bir rol için veya tüm roller için Uzak Masaüstü'nü etkinleştirin, ardından değiştirici değerini değiştirmek isteyip istemediğinizi **etkin**.

3. Kullanıcı adı, parola, süre sonu ve sertifika için gerekli alanları doldurun.

    ![Bulut Hizmetleri Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > İlk olarak Uzak Masaüstü'nü etkinleştirin ve seçin, tüm rol örneklerini yeniden **Tamam** (onay işareti). Yeniden başlatma önlemek için parolayı şifrelemek için kullanılan sertifikanın rolünün yüklenmesi gerekir. Bir yeniden başlatmayı engellemek için [bulut hizmeti için bir sertifika karşıya](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) ve bu iletişim kutusuna dönün.

4. İçinde **rolleri**, güncelleştirme veya seçmek istediğiniz rolü seçin **tüm** tüm rolleri için.

5. Tamamladığınızda, yapılandırmayı güncelleştirmelerinizi seçin **kaydetmek**. Rolü örneklerinizi bağlantıları almaya hazır önce birkaç dakika sürebilir.

## <a name="remote-into-role-instances"></a>Uzaktan rol örnekleri içine

Uzak Masaüstü roller üzerinde etkinleştirildikten sonra Azure portalından doğrudan bir bağlantı başlatabilirsiniz:

1. Tıklatın **örnekleri** açmak için **örnekleri** ayarlar.
2. Yapılandırılmış Uzak Masaüstü sahip rol örneği seçin.
3. Tıklatın **Bağlan** rol örneği için bir RDP dosyası indirilemedi.

    ![Bulut Hizmetleri Uzak Masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Tıklatın **açık** ve ardından **Bağlan** Uzak Masaüstü bağlantısı başlatmak için.

>[!NOTE]
> Bulut hizmetiniz bir NSG durduğunu değilse, bağlantı noktalarında trafiğine izin veren kuralları oluşturmanız gerekebilir **3389** ve **20000**.  Uzak Masaüstü bağlantı noktasını kullanır **3389**.  Bulut hizmeti örnekleri yükü dengelenmiş, olduğundan, doğrudan bağlanmak için hangi örneğinin denetleyemezsiniz.  *RemoteForwarder* ve *RemoteAccess* aracıları RDP trafiği yönetmek ve istemcisinin bir RDP tanımlama bilgisi göndermek ve bağlanmak için tek bir örnek belirtin.  *RemoteForwarder* ve *RemoteAccess* aracıları gerektiren bu bağlantı noktasını **20000*** bir NSG varsa, engellenmiş olabilir açık değil.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)
