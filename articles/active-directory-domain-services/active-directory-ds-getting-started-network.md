---
title: 'Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs'
description: Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir
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
ms.topic: article
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: c6c5762a460fadb04f940742bed759ea17f74aad
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215744"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir


## <a name="before-you-begin"></a>Başlamadan önce
[Azure Active Directory Domain Services için ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md) bölümüne bakın.


## <a name="task-2-configure-network-settings"></a>Görev 2: ağ ayarlarını yapılandırma
Sonraki yapılandırma görevi, bir Azure sanal ağı ve ayrılmış bir alt ağ içindeki oluşturmaktır. Sanal ağınızdaki bu alt ağda Azure Active Directory Domain Services’ı etkinleştirin. Ayrıca varolan bir sanal ağı seçin ve ayrılmış bir alt ağ içindeki oluşturma.

1. Tıklatın **sanal ağ** bir sanal ağ seçin.
    > [!NOTE]
    > **Klasik sanal ağlar yeni dağıtımlar için desteklenmez.** Klasik sanal ağlar yeni dağıtımlar için desteklenmez. Klasik sanal ağlarda dağıtılan var olan yönetilen etki alanları desteklenmeye devam edilir. Microsoft, yakın gelecekte bir Resource Manager sanal ağa var olan bir yönetilen etki Klasik sanal ağdan geçirmenize olanak sağlar.
    >

2. Üzerinde **Seç sanal ağ** sayfası, var olan tüm sanal ağları bakın. Yalnızca seçili kaynak grubu ve Azure konumuna ait sanal ağları bkz **Temelleri** sihirbaz sayfası.
3. Azure AD etki alanı Hizmetleri etkinleştirilmelidir sanal ağ seçin. Varolan bir sanal ağı seçin veya yeni bir tane oluşturun.

  > [!TIP]
  > **Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra yönetilen etki alanınızı farklı bir sanal ağa taşıyamazsınız.** Yönetilen etki alanınızı etkinleştirmek için doğru sanal ağı seçin. Yönetilen bir etki alanına oluşturduktan sonra yönetilen etki alanı silmeden farklı bir sanal ağa taşıyamazsınız. Gözden geçirme öneririz [konuları Azure Active Directory etki alanı Hizmetleri için ağ](active-directory-ds-networking.md) devam etmeden önce.  
  >

4. **Sanal ağ oluştur:** tıklatın **Yeni Oluştur** yeni bir sanal ağ oluşturmak için. Ayrılmış bir alt ağ için Azure AD Etki Alanı Hizmetleri'ni kullanın. Örneğin, 'alt ağ içinde dağıtılan anlamak diğer yöneticilere kolaylaşır DomainServices' adında bir alt ağ oluşturun. Tıklatın **Tamam** tamamladığınızda.

    ![Sanal ağ seçin](./media/getting-started/domain-services-blade-network-pick-vnet.png)

  > [!WARNING]
  > Özel IP adres alanı içinde bir adres alanı seçtiğinizden emin olun. Ortak adres alanı olan ait olmayan IP adreslerini Azure AD etki alanı Hizmetleri içindeki hatalara neden olabilir.

5. **Varolan bir sanal ağı:** mevcut bir sanal ağ, çekme planlıyorsanız [sanal ağlar uzantısını kullanarak ayrılmış bir alt ağ oluşturmak](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet)ve bu alt ağ seçin. Tıklatın **sanal ağ** varolan bir sanal ağı seçin. Tıklatın **alt** yeni yönetilen etki alanınızı etkinleştirileceği içinde varolan sanal ağınızdaki ayrılmış bir alt ağ seçmek için. Tıklatın **Tamam** tamamladığınızda.

    ![Sanal ağ içindeki alt ağ seçin](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Bir alt seçme yönergeleri**
  > 1. Ayrılmış bir alt ağ için Azure AD Etki Alanı Hizmetleri'ni kullanın. Herhangi bir sanal makine bu alt ağına dağıtmayın. Bu yapılandırma, ağ güvenlik grupları (Nsg'ler) yapılandırmak, yönetilen etki alanınız kesintiye uğratmadan iş yükleri/sanal makineleriniz için etkinleştirir. Ayrıntılar için bkz [konuları Azure Active Directory etki alanı Hizmetleri için ağ](active-directory-ds-networking.md).
  2. Desteklenen bir yapılandırma olduğundan Azure AD etki alanı Hizmetleri dağıtmak için ağ geçidi alt ağı seçmeyin.
  3. Seçtiğiniz alt ağ, adres alanında en az 3-5 kullanılabilir IP adresi olması gerekir.
  >

6. İşiniz bittiğinde tıklatın **Tamam** geçmek için **yönetici grubuna** Sihirbazı sayfası.


## <a name="next-step"></a>Sonraki adım
[Görev 3: yönetim grubunu yapılandırın ve Azure AD Etki Alanı Hizmetleri'ni etkinleştirme](active-directory-ds-getting-started-admingroup.md)
