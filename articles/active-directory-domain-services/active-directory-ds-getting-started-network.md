---
title: "Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs"
description: "Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 680ffc41ab96d69153ef7039698bf9285ed6ce16
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir


## <a name="before-you-begin"></a>Başlamadan önce
[Azure Active Directory Domain Services için ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md) bölümüne bakın.


## <a name="task-2-configure-network-settings"></a>Görev 2: ağ ayarlarını yapılandırma
Sonraki yapılandırma görevi, bir Azure sanal ağı ve ayrılmış bir alt ağ içindeki oluşturmaktır. Sanal ağınızdaki bu alt ağda Azure Active Directory Domain Services’ı etkinleştirin. Ayrıca varolan bir sanal ağı seçin ve ayrılmış bir alt ağ içindeki oluşturma.

1. Tıklatın **sanal ağ** bir sanal ağ seçin.
    > [!NOTE]
    > **Klasik sanal ağlar yeni dağıtımlar için desteklenmez.** Klasik sanal ağlar yeni dağıtımlar için desteklenmez. Klasik sanal ağlarda dağıtılan var olan yönetilen etki alanları desteklenmeye devam edilir. Biz yakın gelecekte bir Resource Manager sanal ağa var olan bir yönetilen etki Klasik sanal ağdan geçirme özelliğini sağlar.
    >

2. Üzerinde **Seç sanal ağ** sayfası, var olan tüm sanal ağları bakın. Yalnızca seçili kaynak grubu ve Azure konumuna ait sanal ağları bkz **Temelleri** sihirbaz sayfası.
3. Azure AD etki alanı Hizmetleri etkinleştirilmelidir sanal ağ seçin. Varolan bir sanal ağı seçin veya yeni bir tane oluşturun.

  > [!TIP]
  > **Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra yönetilen etki alanınızı farklı bir sanal ağa taşıyamazsınız.** Yönetilen etki alanınızı etkinleştirmek için doğru sanal ağı seçin. Yönetilen bir etki alanına oluşturduktan sonra yönetilen etki alanı silmeden farklı bir sanal ağa taşıyamazsınız. Gözden geçirme öneririz [konuları Azure Active Directory etki alanı Hizmetleri için ağ](active-directory-ds-networking.md) devam etmeden önce.  
  >

4. **Sanal ağ oluştur:** tıklatın **Yeni Oluştur** yeni bir sanal ağ oluşturmak için. Ayrılmış bir alt ağ için Azure AD Etki Alanı Hizmetleri'ni kullanarak öneririz. Örneğin, 'alt ağ içinde dağıtılan anlamak diğer yöneticilere kolaylaşır DomainServices' adında bir alt ağ oluşturun. Tıklatın **Tamam** tamamladığınızda.

    ![Sanal ağ seçin](./media/getting-started/domain-services-blade-network-pick-vnet.png)

5. **Varolan bir sanal ağı:** mevcut bir sanal ağ, çekme planlıyorsanız [sanal ağlar uzantısını kullanarak ayrılmış bir alt ağ oluşturmak](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)ve bu alt ağ seçin. Tıklatın **sanal ağ** varolan bir sanal ağı seçin. Tıklatın **alt** yeni yönetilen etki alanınızı etkinleştirileceği içinde varolan sanal ağınızdaki ayrılmış bir alt ağ seçmek için. Tıklatın **Tamam** tamamladığınızda.

    ![Sanal ağ içindeki alt ağ seçin](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Bir alt seçme yönergeleri**
  > 1. Ayrılmış bir alt ağ için Azure AD Etki Alanı Hizmetleri'ni kullanın. Herhangi bir sanal makine bu alt ağına dağıtmayın. Bu yapılandırma, ağ güvenlik grupları (Nsg'ler) yapılandırmak, yönetilen etki alanınız kesintiye uğratmadan iş yükleri/sanal makineleriniz için etkinleştirir. Ayrıntılar için bkz [konuları Azure Active Directory etki alanı Hizmetleri için ağ](active-directory-ds-networking.md).
  2. Desteklenen bir yapılandırma olduğundan Azure AD etki alanı Hizmetleri dağıtmak için ağ geçidi alt ağı seçmeyin.
  3. Seçtiğiniz alt ağı yeterli kullanılabilir adres alanı - en az 3-5 kullanılabilir IP adreslerine sahip olduğundan emin olun.
  >

6. İşiniz bittiğinde tıklatın **Tamam** geçmek için **yönetici grubuna** Sihirbazı sayfası.


## <a name="next-step"></a>Sonraki adım
[Görev 3: yönetim grubunu yapılandırın ve Azure AD Etki Alanı Hizmetleri'ni etkinleştirme](active-directory-ds-getting-started-admingroup.md)
