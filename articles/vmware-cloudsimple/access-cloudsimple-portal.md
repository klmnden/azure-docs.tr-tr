---
title: Erişim Azure VMware çözümüyle CloudSimple - Portal
description: VMware çözümü CloudSimple portalından Azure portalına erişin açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8c7bb080b350742d0722cdb4e07b82a6881ba05b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073663"
---
# <a name="accessing-the-vmware-solution-by-cloudsimple-portal-from-azure-portal"></a>Azure portalından CloudSimple portal tarafından VMware çözümü erişme

Çoklu oturum açma CloudSimple portalına erişim için desteklenir. Azure portalında oturum açtıktan sonra yeniden imzalama olmadan CloudSimple portala erişebilirsiniz. İlk kez size erişim yetkisi vermek için istenirse CloudSimple portalı [CloudSimple hizmet yetkilendirme](#consent-to-cloudsimple-service-authorization-application) uygulama.  Yetkilendirme tek seferlik bir işlemdir.

## <a name="before-you-begin"></a>Başlamadan önce

Yalnızca yerleşik olan kullanıcılar **sahibi** ve **katkıda bulunan** rolleri CloudSimple portalına erişebilir.  Rolleri, abonelik üzerinde yapılandırılmalıdır.  Rolünüz denetimi ile ilgili daha fazla bilgi için bkz: [rol atamalarını görüntüleyin](https://docs.microsoft.com/azure/role-based-access-control/check-access) makalesi.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

1. **Tüm Hizmetler**’i seçin.

2. Arama **CloudSimple Hizmetleri**.

3. Özel bulut oluşturmak istediğiniz CloudSimple hizmeti seçin.

4. Üzerinde **genel bakış** sayfasında **CloudSimple Portal'a**.  CloudSimple portalı Azure portalından ilk kez erişiyorsanız yetkilendirmek için istenir [CloudSimple hizmet yetkilendirme](#consent-to-cloudsimple-service-authorization-application) uygulama. 

    ![CloudSimple portalını başlatma](media/launch-cloudsimple-portal.png)

> [!TIP]
> Doğrudan Azure portalından bir özel bulut işlemi (örneğin, oluşturma veya özel Bulutu genişletme) seçerseniz, belirtilen sayfaya CloudSimple portalı açılır.

CloudSimple portalında **giriş** yan menüsünde özel Bulutlarınızın ilgili özet bilgiler görüntüler. Kaynaklarını ve kapasiteyi, özel bulut, uyarılar ve dikkat gerektiren görevler ile birlikte gösterilir. Ortak görevler için sayfanın üstündeki adlandırılmış simgeleri tıklatın.

![Giriş sayfası](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>Hizmet yetkilendirme CloudSimple uygulama onayı

CloudSimple portaldan Azure portalını ilk kez çalıştırdığınızda CloudSimple hizmet yetkilendirme uygulama için onay gerektirir.  Seçin **kabul** CloudSimple portala erişmek ve istenen izinler. 

![CloudSimple hizmet yetkilendirme - Yöneticiler onayı](media/cloudsimple-azure-consent.png)

Genel yönetici ayrıcalığı varsa, kuruluşunuz için onay verebilir.  Seçin **onay kuruluşunuz adına**.

![CloudSimple hizmet yetkilendirme - genel yönetici onayı](media/cloudsimple-azure-consent-global-admin.png)

İzinlerinizi CloudSimple portalına erişim izin vermiyorsa, gerekli izinleri vermek için kiracınızın genel yönetici ile iletişime geçin.  Kuruluşunuz adına bir genel yönetici onay verebilir.

![CloudSimple hizmeti yetkilendirme için onay - Yöneticiler gerektirir](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [özel bulut oluşturma](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Bilgi edinmek için nasıl [bir özel bulut ortamı yapılandırma](quickstart-create-private-cloud.md)