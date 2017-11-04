---
title: "Azure Active Directory etki alanı Hizmetleri için Windows Server VM katılma | Microsoft Docs"
description: "Bir Windows Server sanal makine Azure Resource Manager şablonları kullanarak bir yönetilen etki alanına ekleyin."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 4eabfd8e-5509-4acd-86b5-1318147fddb5
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 8cfa13ee028b5e367158de42ceab0a1cd99d45c2
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-a-resource-manager-template"></a>Bir Windows Server sanal makine bir Resource Manager şablonu kullanarak bir yönetilen etki alanına ekleyin
Bu makalede Resource Manager şablonları kullanarak bir Azure AD etki alanı Hizmetleri yönetilen etki alanına Windows Server sanal makine nasıl gösterir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. Sanal ağ için DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz: [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. İçin gerekli adımları [Azure AD etki alanı Hizmetleri yönetilen etki alanınız için parola eşitleme](active-directory-ds-getting-started-password-sync.md).


## <a name="install-and-configure-required-tools"></a>Yükleme ve gerekli araçları yapılandırma
Bu belgede özetlenen adımları gerçekleştirmek için aşağıdaki seçeneklerden birini kullanabilirsiniz:
* **Azure PowerShell**: [yükleyin ve yapılandırın](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
* **Azure platformlar arası komut satırı arabirimi**: [yükleyin ve yapılandırın](https://azure.microsoft.com/documentation/articles/xplat-cli-install/)


## <a name="option-1-provision-a-new-windows-server-vm-and-join-it-to-a-managed-domain"></a>Seçenek 1: yeni bir Windows Server VM sağlamak ve yönetilen bir etki alanına katılma
**Hızlı Başlangıç şablonu adı**: [201 vm etki alanına katılma](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)

Windows Server sanal makine dağıtma ve yönetilen bir etki alanına katılmak için aşağıdaki adımları gerçekleştirin:
1. Gidin [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/201-vm-domain-join/).
2. **Azure’a dağıt**’a tıklayın.
3. İçinde **özel dağıtım** sayfasında, sanal makine sağlamak için gerekli bilgileri sağlayın.
4. Seçin **Azure aboneliği** , sanal makine sağlamak. Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz aynı Azure aboneliğini seçin.
5. Var olan seçin **kaynak grubu** veya yeni bir tane oluşturun.
6. Çekme bir **konumu** , yeni bir sanal makine dağıtmak.
7. İçinde **varolan bir sanal ağ adı**, Azure AD etki alanı Hizmetleri yönetilen etki alanı, dağıttığınız sanal ağ belirtin.
8. İçinde **var olan bir alt ağ adı**, olduğu gibi bu sanal makineyi dağıtmak için sanal ağ içindeki alt ağı belirtin. Ağ geçidi alt ağı sanal ağında seçmeyin. Ayrıca, yönetilen etki alanınızı dağıtıldığı ayrılmış bir alt ağ seçmeyin.
9. İçinde **DNS etiket öneki**, sağlanacak sanal makine için konak adı belirtin. Örneğin, 'contoso-win'.
10. Uygun seçin **VM boyutu** sanal makine için.
11. İçinde **için etki alanına katılma**, yönetilen etki alanınızın DNS etki alanı adını belirtin.
12. İçinde **etki alanı kullanıcı adı**, VM yönetilen etki alanına katmak için kullanılması gereken, yönetilen etki alanı kullanıcı hesabı adı belirtin.
13. İçinde **etki alanı parolası**, 'domainUsername' parametresi tarafından başvurulan etki alanı kullanıcı hesabı parolasını belirtin.
14. İsteğe bağlı: Belirttiğiniz bir **OU yolu** , sanal makine eklemek özel bir OU. Bu parametre için bir değer belirtmezseniz, varsayılan sanal makine eklenir **AAD DC bilgisayarlar** yönetilen etki alanı OU.
15. İçinde **VM yönetici kullanıcı adı** alanında, sanal makine için bir yerel yönetici hesabı adı belirtin.
16. İçinde **VM yönetici parolası** alanında, sanal makine için bir yerel yönetici parolasını belirtin. Sanal makinenin parolası yanılma saldırılarına karşı korumak güçlü yerel yönetici parolasını belirtin.
17. Tıklatın **hüküm ve koşulları yukarıda belirtildiği ediyorum**.
18. Tıklatın **satın alma** sanal makine sağlamak için.

> [!WARNING]
> **Parolalar dikkatle işleyin.**
> Şablon parametresi dosyası, etki alanı hesapları için parolaları ve aynı zamanda sanal makine için yerel yönetici parolalarını içerir. Bu dosya duran geçici dosya paylaşımlarında veya diğer paylaşılan konumu bırakmadığından emin olun. Öneririz, dispose bu dosyanın tamamladıktan sonra sanal makinelerinizi dağıtma.
>

Dağıtım başarıyla tamamlandıktan sonra yeni sağlanan Windows sanal makinenizi yönetilen etki alanına katıldı.


## <a name="option-2-join-an-existing-windows-server-vm-to-a-managed-domain"></a>Seçenek 2: varolan bir Windows Server VM yönetilen bir etki alanına katılın.
**Hızlı Başlangıç şablonu**: [201 vm-etki-birleştirme-var](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)

Varolan bir Windows Server sanal makinesini yönetilen bir etki alanına katılmak için aşağıdaki adımları gerçekleştirin:
1. Gidin [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
2. **Azure’a dağıt**’a tıklayın.
3. İçinde **özel dağıtım** sayfasında, sanal makine sağlamak için gerekli bilgileri sağlayın.
4. Seçin **Azure aboneliği** , sanal makine sağlamak. Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz aynı Azure aboneliğini seçin.
5. Var olan seçin **kaynak grubu** veya yeni bir tane oluşturun.
6. Çekme bir **konumu** , yeni bir sanal makine dağıtmak.
7. İçinde **VM listesi** alan, yönetilen etki alanına katılması için var olan sanal makinelerin adlarını belirtin. Tek tek VM adları ayırmak için virgül kullanın. Örneğin, **contoso web, contoso-API**.
8. İçinde **etki alanına katılım kullanıcı adını**, VM yönetilen etki alanına katmak için kullanılması gereken, yönetilen etki alanı kullanıcı hesabı adı belirtin.
9. İçinde **etki alanı katılma kullanıcı parolası**, 'domainUsername' parametresi tarafından başvurulan etki alanı kullanıcı hesabı parolasını belirtin.
10. İçinde **etki alanı FQDN'si**, yönetilen etki alanınızın DNS etki alanı adını belirtin.
11. İsteğe bağlı: Belirttiğiniz bir **OU yolu** , sanal makine eklemek özel bir OU. Bu parametre için bir değer belirtmezseniz, varsayılan sanal makine eklenir **AAD DC bilgisayarlar** yönetilen etki alanı OU.
12. Tıklatın **hüküm ve koşulları yukarıda belirtildiği ediyorum**.
13. Tıklatın **satın alma** sanal makine sağlamak için.

> [!WARNING]
> **Parolalar dikkatle işleyin.**
> Şablon parametresi dosyası, etki alanı hesapları için parolaları ve aynı zamanda sanal makine için yerel yönetici parolalarını içerir. Bu dosya duran geçici dosya paylaşımlarında veya diğer paylaşılan konumu bırakmadığından emin olun. Öneririz, dispose bu dosyanın tamamladıktan sonra sanal makinelerinizi dağıtma.
>

Dağıtımı başarıyla tamamlandıktan sonra belirtilen Windows sanal makineleri yönetilen etki alanına eklenir.


## <a name="related-content"></a>İlgili İçerik
* [Azure PowerShell genel bakış](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.4.0)
* [Azure Hızlı Başlangıç şablonu - etki alanına yeni bir VM](https://azure.microsoft.com/resources/templates/201-vm-domain-join/)
* [Azure Hızlı Başlangıç şablonu - VMs varolan etki alanına katılma](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
