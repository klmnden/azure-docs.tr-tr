---
title: Azure Active Directory etki alanı Hizmetleri için bir Windows Server VM'sine katılma | Microsoft Docs
description: Bir Windows Server sanal makinesi, Azure Resource Manager şablonlarını kullanarak bir yönetilen etki alanına katılın.
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 4eabfd8e-5509-4acd-86b5-1318147fddb5
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: e4ca613059e10755056616b964cc500625fef187
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245962"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-a-resource-manager-template"></a>Bir Windows Server sanal makinesi bir Resource Manager şablonu kullanarak bir yönetilen etki alanına ekleme
Bu makalede Resource Manager şablonlarını kullanarak bir Azure AD Domain Services yönetilen etki alanına Windows Server sanal makinesinin nasıl gösterir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](create-instance.md).
4. Sanal ağın DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. Tamamlamak için gereken adımlar [Azure AD Domain Services yönetilen etki alanınıza parolalarını eşitleyin](active-directory-ds-getting-started-password-sync.md).


## <a name="install-and-configure-required-tools"></a>Yükleme ve gerekli araçları yapılandırma
Bu belgede özetlenen adımları gerçekleştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:
* **Azure PowerShell**: [Yükleme ve yapılandırma](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
* **Azure CLI**: [Yükleme ve yapılandırma](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)


## <a name="option-1-provision-a-new-windows-server-vm-and-join-it-to-a-managed-domain"></a>1. seçenek: Yeni bir Windows Server VM'si sağlama ve yönetilen bir etki alanına katılma
**Hızlı Başlangıç şablonu adı**: [201-vm-etki alanına katılma](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)

Bir Windows Server sanal makinesini dağıtmak ve yönetilen bir etki alanına katılmak için aşağıdaki adımları gerçekleştirin:
1. Gidin [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/201-vm-domain-join/).
2. **Azure’a dağıt**’a tıklayın.
3. İçinde **özel dağıtım** sayfasında, sanal makine sağlamak için gerekli bilgileri sağlayın.
4. Seçin **Azure aboneliği** , sanal makine sağlamak. Azure AD Domain Services'i etkinleştirdiğiniz aynı Azure aboneliği seçin.
5. Mevcut bir seçin **kaynak grubu** veya yeni bir tane oluşturun.
6. Çekme bir **konumu** yeni bir sanal makine dağıtacağınız.
7. İçinde **var olan sanal ağ adı**, Azure AD Domain Services yönetilen etki alanınız içinde dağıttığınız sanal ağ belirtin.
8. İçinde **var olan alt ağ adı**, istediğiniz bu sanal makineyi dağıtmak için sanal ağ içindeki alt ağ belirtin. Ağ geçidi alt ağı sanal ağ içinde seçmeyin. Ayrıca, yönetilen etki alanınıza dağıtıldığı ayrılmış alt ağında seçmeyin.
9. İçinde **etiket DNS ön eki**, sağlanma işlemi sanal makine için konak adı belirtin. Örneğin, 'contoso-win'.
10. Uygun seçin **VM boyutu** sanal makine için.
11. İçinde **etki alanına katılma**, yönetilen etki alanınızın DNS etki alanı adı belirtin.
12. İçinde **etkialanı kullanıcıadı**, sanal Makinenin yönetilen etki alanına eklemek için kullanılması gereken yönetilen etki alanınızdaki kullanıcı hesabı adını belirtin.
13. İçinde **etki alanı parolası**, 'domainUsername' parametresi tarafından başvurulan etki alanı kullanıcı hesabı parolasını belirtin.
14. İsteğe bağlı: Belirtebileceğiniz bir **OU yolu** , sanal makine eklemek özel bir kuruluş için. Bu parametre için bir değer belirtmezseniz varsayılan sanal makine eklenir **AAD DC bilgisayarlar** yönetilen etki alanındaki OU.
15. İçinde **VM yönetici kullanıcı adı** alanında, sanal makine için bir yerel yönetici hesabı adı belirtin.
16. İçinde **VM yönetici parolası** alanında, sanal makine için bir yerel yönetici parolasını belirtin. Parola deneme yanılma saldırılarına karşı korumak sanal makine için bir tanımlayıcı yerel yönetici parolasını belirtin.
17. Tıklayın **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**.
18. Tıklayın **satın alma** sanal makine sağlamak için.

> [!WARNING]
> **Uyarı parolalarıyla işleyin.**
> Şablon parametre dosyası, etki alanı hesapları için parola yanı sıra sanal makine için yerel yönetici parolaları içerir. Bu dosya duran geçici dosya paylaşımlarında veya diğer paylaşılan konumu bırakmadığından emin olun. Öneririz, dispose bu dosya tamamladıktan sonra sanal makinelerinizi dağıtma.
>

Dağıtım başarıyla tamamlandıktan sonra yeni sağlanan Windows sanal makinenizi yönetilen etki alanına katıldı.


## <a name="option-2-join-an-existing-windows-server-vm-to-a-managed-domain"></a>2. seçenek: Mevcut bir Windows Server sanal Makinesini yönetilen etki alanına katılın
**Hızlı Başlangıç şablonu**: [201-vm-etki-birleştirme-mevcut](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)

Varolan bir Windows Server sanal makinesini yönetilen etki alanına katılmak için aşağıdaki adımları gerçekleştirin:
1. Gidin [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
2. **Azure’a dağıt**’a tıklayın.
3. İçinde **özel dağıtım** sayfasında, sanal makine sağlamak için gerekli bilgileri sağlayın.
4. Seçin **Azure aboneliği** , sanal makine sağlamak. Azure AD Domain Services'i etkinleştirdiğiniz aynı Azure aboneliği seçin.
5. Mevcut bir seçin **kaynak grubu** veya yeni bir tane oluşturun.
6. Çekme bir **konumu** yeni bir sanal makine dağıtacağınız.
7. İçinde **VM List** alanında, yönetilen etki alanına katılması mevcut sanal makinelerin adlarını belirtin. Tek tek sanal makine adını ayırmak için virgül kullanın. Örneğin, **contoso web, contoso-API**.
8. İçinde **etki alanına katılım kullanıcı adını**, sanal Makinenin yönetilen etki alanına eklemek için kullanılması gereken yönetilen etki alanınızdaki kullanıcı hesabı adını belirtin.
9. İçinde **etki alanı kullanıcı parolası katılın**, 'domainUsername' parametresi tarafından başvurulan etki alanı kullanıcı hesabı parolasını belirtin.
10. İçinde **etki alanı FQDN'si**, yönetilen etki alanınızın DNS etki alanı adı belirtin.
11. İsteğe bağlı: Belirtebileceğiniz bir **OU yolu** , sanal makine eklemek özel bir kuruluş için. Bu parametre için bir değer belirtmezseniz varsayılan sanal makine eklenir **AAD DC bilgisayarlar** yönetilen etki alanındaki OU.
12. Tıklayın **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**.
13. Tıklayın **satın alma** sanal makine sağlamak için.

> [!WARNING]
> **Uyarı parolalarıyla işleyin.**
> Şablon parametre dosyası, etki alanı hesapları için parola yanı sıra sanal makine için yerel yönetici parolaları içerir. Bu dosya duran geçici dosya paylaşımlarında veya diğer paylaşılan konumu bırakmadığından emin olun. Öneririz, dispose bu dosya tamamladıktan sonra sanal makinelerinizi dağıtma.
>

Dağıtım başarıyla tamamlandıktan sonra belirtilen Windows sanal makineleri yönetilen etki alanına katılır.


## <a name="related-content"></a>İlgili İçerik
* [Azure PowerShell’e genel bakış](/powershell/azure/overview)
* [Azure Hızlı Başlangıç şablonu - etki alanına yeni bir VM](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)
* [Azure Hızlı Başlangıç şablonu - etki alanına var olan VM'ler](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
