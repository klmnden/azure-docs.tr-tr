---
title: 'Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs'
description: Azure Active Directory etki alanı Azure portalını kullanarak Services'i etkinleştirme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: b6651c038a2b3abd15b8b0587e6a0e95832401b1
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502320"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Services'i etkinleştirme
Bu makalede, Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri (Azure AD DS) etkinleştirmek gösterilmektedir.


## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri tamamlamak için gerekir:

* Geçerli bir **Azure aboneliği**.
* Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
* **Azure aboneliği Azure AD dizini ile ilişkili olmalıdır**.
* Gereksinim duyduğunuz **genel yönetici** ayrıcalıkları Azure AD dizininizde Azure AD Domain Services'ı etkinleştirmek için.


## <a name="enable-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri'ni etkinleştirme

Başlatmak için **etkinleştirin, Azure AD Domain Services** Sihirbazı, aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com) gidin.
2. Sol bölmede **Kaynak oluştur**’a tıklayın.
3. İçinde **yeni** sayfasında **etki alanı Hizmetleri** Arama çubuğuna.

    ![Etki Alanı Hizmetleri'ni arayın](./media/getting-started/search-domain-services.png)

4. Seçmek için tıklatın **Azure AD Domain Services** arama önerilerini listesinden. Üzerinde **Azure AD Domain Services** sayfasında **Oluştur** düğmesi.

    ![Etki alanı hizmetlerini görüntüle](./media/getting-started/domain-services-blade.png)

5. **Etkinleştirin, Azure AD Domain Services** Sihirbazı başlatılır.


## <a name="task-1-configure-basic-settings"></a>Görev 1: temel ayarları yapılandırma
İçinde **Temelleri** sayfasında sihirbazın, yönetilen etki alanı için DNS etki alanı adını belirtin. Ayrıca, kaynak grubunu ve yönetilen etki alanında dağıtılmalıdır Azure konum da seçebilirsiniz.

![Yapılandırma temelleri](./media/getting-started/domain-services-blade-basics.png)

1. Seçin **DNS etki alanı adı** yönetilen etki alanınız için.

   > [!NOTE]
   > **Bir DNS etki alanı adı seçmek için yönergeleri**
   > * **Yerleşik etki alanı adı:** varsayılan olarak, sihirbaz dizinin varsayılan/dahili-içeren etki alanı adını belirtir. (ile bir **. onmicrosoft.com** soneki) sizin için. İnternet üzerinden güvenli LDAP erişimi için yönetilen etki alanı etkinleştirmeyi seçerseniz, bu etki alanı adı için bir ortak CA güvenli LDAP sertifika edinme veya bir genel DNS kaydı oluşturma sorunları bekler. Microsoft sahibi *. onmicrosoft.com* etki alanı ve CA'lar değil sertifika bu etki alanı için özgün olduğunu belgelemekten.
   * **Özel etki alanı adları:** bir özel etki alanı adı da yazabilirsiniz. Bu örnekte özel etki alanı adı *contoso100.com* şeklindedir.
   * **Yönlendirilebilir olmayan etki alanı sonekleri:** genellikle yönlendirilemeyen etki alanı adı soneki önleme öneririz. Örneğin, bir etki alanı DNS etki alanı adı 'contoso.local' ile oluşturmaktan kaçınmak iyidir. '.Local' DNS soneki yönlendirilemez ve DNS çözümlemesi ile sorunlara neden olabilir.
   * **Etki alanı ön eki kısıtlamaları:** belirtilen etki alanı adınızla önek (örneğin, *contoso100* içinde *contoso100.com* etki alanı adı) 15 veya daha az karakter içermelidir. Yönetilen bir etki alanı ön eki 15 karakterden uzunsa oluşturulamıyor.
   * **Ağ adı çakışıyor:** yönetilen etki alanı için seçtiğiniz DNS etki alanı adı sanal ağ içinde zaten mevcut olduğundan emin olun. Özellikle, kontrol olmadığını:
       * Sanal ağda aynı DNS etki alanı adıyla bir Active Directory etki alanı zaten var.
       * Yönetilen etki alanını etkinleştirmek planladığınız sanal ağ ile şirket içi ağınıza bir VPN bağlantısı vardır. Bu senaryoda, şirket içi ağınızda aynı DNS etki alanı adına sahip bir etki alanına sahip olun.
       * Sanal ağda bu ada sahip bir bulut hizmetiniz zaten varsa.
    >

2. Azure'ı seçin **abonelik** içinde yönetilen etki alanı oluşturmak istiyorsunuz.

3. Seçin **kaynak grubu** için yönetilen etki alanına ait olmalıdır. Seçin ya da **Yeni Oluştur** veya **var olanı kullan** seçenekleri kaynak grubunu seçin.

4. Azure'u seçin **konumu** yönetilen etki alanı oluşturulması. Üzerinde **ağ** Sayfası Sihirbazı'nın seçtiğiniz bir konuma ait yalnızca sanal ağlar görürsünüz.

5. Tıklayın **Tamam** üzerinde taşımayı **ağ** Sihirbazı sayfası.


## <a name="next-step"></a>Sonraki adım
[2. Görev: Ağ ayarlarını yapılandırma](active-directory-ds-getting-started-network.md)
