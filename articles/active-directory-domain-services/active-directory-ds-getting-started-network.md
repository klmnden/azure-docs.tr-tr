---
title: 'Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs'
description: Azure Active Directory etki alanı Azure portalını kullanarak Services'i etkinleştirme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: ergreenl
ms.openlocfilehash: 3020d7b29f19ec2ab578acbebac8db8ea320a844
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103589"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Services'i etkinleştirme


## <a name="before-you-begin"></a>Başlamadan önce
[Azure Active Directory Domain Services için ağ ile ilgili dikkat edilmesi gerekenler](active-directory-ds-networking.md) bölümüne bakın.


## <a name="task-2-configure-network-settings"></a>2. Görev: Ağ ayarlarını yapılandırma
Bir Azure sanal ağı ve adanmış bir alt ağ, sonraki yapılandırma görevi oluşturmaktır. Sanal ağınızdaki bu alt ağda Azure Active Directory Domain Services’ı etkinleştirin. Ayrıca, var olan bir sanal ağ seçin ve bunun içinde ayrılmış bir alt ağ oluşturmak.

1. Tıklayın **sanal ağ** bir sanal ağ seçin.
    > [!NOTE]
    > **Yeni dağıtımlar için Klasik sanal ağlar desteklenmez.** Yeni dağıtımlar için Klasik sanal ağlar desteklenmez. Klasik sanal ağlarda dağıtılan mevcut yönetilen etki alanlarını desteklemeye devam eder. Microsoft, bir Resource Manager sanal ağı klasik bir sanal ağdan mevcut bir yönetilen etki alanına yakın gelecekte geçirmenize olanak tanıyacak.
    >

2. Üzerinde **sanal ağ Seç** sayfasında, var olan tüm sanal ağları görürsünüz. Yalnızca üzerinde seçtiğiniz kaynak grubu ve Azure konumuna ait sanal ağlar gördüğünüz **Temelleri** sihirbaz sayfası.
3. Azure AD Domain Services etkinleştirilmelidir sanal ağı seçin. Mevcut bir sanal ağ seçin veya yeni bir tane oluşturun.

   > [!TIP]
   > **Azure AD Domain Services etkinleştirildikten sonra yönetilen etki alanınıza farklı bir sanal ağa taşıyamazsınız.** Yönetilen etki alanınıza etkinleştirmek için doğru sanal ağı seçin. Yönetilen bir etki alanı oluşturduktan sonra yönetilen etki alanı silmeden farklı bir sanal ağa taşıyamazsınız. Gözden geçirme öneririz [konuları Azure Active Directory Domain Services için ağ](active-directory-ds-networking.md) devam etmeden önce.  
   >

4. **Sanal ağ oluşturun:** Tıklayın **Yeni Oluştur** yeni bir sanal ağ oluşturmak için. Azure AD Domain Services için ayrılmış bir alt ağ kullanın. Örneğin, alt ağ içinde dağıtılan anlamak diğer yöneticiler için kolaylaştıran DomainServices', ada sahip bir alt ağ oluşturun. Tıklayın **Tamam** bitirdiğinizde.

    ![Sanal ağ seçin](./media/getting-started/domain-services-blade-network-pick-vnet.png)

   > [!WARNING]
   > Özel IP adres alanı içinde bir adres alanı seçtiğinizden emin olun. Genel Adres alanında olan olmadığınız IP adresleri, Azure AD Domain Services içinde hataya neden olur.

5. **Var olan sanal ağı:** Mevcut bir sanal ağ seçin planlıyorsanız [sanal ağlar uzantısı kullanılarak ayrılmış bir alt ağ oluşturma](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet)ve ardından bu alt ağ seçin. Tıklayın **sanal ağ** var olan sanal ağı seçin. Tıklayın **alt** içinde yeni bir yönetilen etki alanınıza etkinleştirmek, var olan sanal ağınızda ayrılmış alt ağında seçmek için. Tıklayın **Tamam** bitirdiğinizde.

    ![Sanal ağ içindeki alt ağ seçin](./media/getting-started/domain-services-blade-network-pick-subnet.png)

   > [!NOTE]
   > **Bir alt seçme yönergeleri**
   > 1. Azure AD Domain Services için ayrılmış bir alt ağ kullanın. Diğer tüm sanal makineleri, bu alt ağına dağıtmayın. Bu yapılandırma, ağ güvenlik grupları (Nsg'ler) yapılandırmak iş yüklerini/sanal makineleriniz için yönetilen etki alanınıza kesintiye uğratmadan sağlar. Ayrıntılar için bkz [konuları Azure Active Directory Domain Services için ağ](active-directory-ds-networking.md).
   > 2. Azure AD Domain Services'ı dağıtmak için ağ geçidi alt ağı, desteklenen bir yapılandırma olmadığından seçmeyin.
   > 3. Seçtiğiniz alt ağ, adres alanında en az 3-5 kullanılabilir IP adresleri olması gerekir.

6. İşiniz bittiğinde tıklayın **Tamam** geçmek için **yönetici grubuna** Sihirbazı sayfası.


## <a name="next-step"></a>Sonraki adım
[3. Görev: yönetici grubunu yapılandırma ve Azure AD Domain Services'ı etkinleştir](active-directory-ds-getting-started-admingroup.md)
