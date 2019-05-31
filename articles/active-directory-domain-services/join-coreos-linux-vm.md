---
title: 'Azure Active Directory etki alanı Hizmetleri: CoreOS Linux VM için yönetilen etki alanına Katıl | Microsoft Docs'
description: CoreOS Linux sanal makinesini Azure AD Domain Services için katılın
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: 133ab04b79d1c0ba021c55b9de0860d31cc79bd7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246067"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>CoreOS Linux sanal makinesi için yönetilen etki alanına Katıl
Bu makalede, bir CoreOS Linux sanal makinesini Azure'da bir Azure AD Domain Services yönetilen etki alanına katılın işlemini göstermektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](create-instance.md).
4. Sanal ağın DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. Tamamlamak için gereken adımlar [Azure AD Domain Services yönetilen etki alanınıza parolalarını eşitleyin](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-coreos-linux-virtual-machine"></a>Bir CoreOS Linux sanal makinesi sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'daki bir CoreOS sanal makine sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Bu makalede **CoreOS Linux (Stable)** azure'da sanal makine görüntüsü.

> [!IMPORTANT]
> * Sanal makineye dağıtma **aynı sanal ağda Azure AD Domain Services, etkinleştirdiğiniz**.
> * Çekme bir **farklı bir alt** Azure AD Domain Services, etkinleştirdiğiniz bir.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Yeni sağlanan Linux sanal makineye uzaktan bağlanın
Azure'da CoreOS VM sağlandı. VM sağlama sırasında oluşturulan yerel yönetici hesabını kullanarak sanal makineye uzaktan bağlanmak için sonraki görevdir bakın.

Makaledeki yönergeleri [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine konak dosyasını yapılandırma
SSH terminalinizi/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```
Burada, 'contoso100.com' yönetilen etki alanınızın DNS etki alanı adı ' dir. 'contoso-coreos' yönetilen etki alanına katıldığınız CoreOS sanal makinenin adıdır.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Linux sanal makinesinde SSSD hizmetini yapılandırma
Ardından, SSSD yapılandırma dosyanızı güncelleştirin ('/ etc/sssd/sssd.conf') aşağıdaki örnekle eşleşecek şekilde:

 ```
 [sssd]
 config_file_version = 2
 services = nss, pam
 domains = CONTOSO100.COM

 [domain/CONTOSO100.COM]
 id_provider = ad
 auth_provider = ad
 chpass_provider = ad

 ldap_uri = ldap://contoso100.com
 ldap_search_base = dc=contoso100,dc=com
 ldap_schema = rfc2307bis
 ldap_sasl_mech = GSSAPI
 ldap_user_object_class = user
 ldap_group_object_class = group
 ldap_user_home_directory = unixHomeDirectory
 ldap_user_principal = userPrincipalName
 ldap_account_expire_policy = ad
 ldap_force_upper_case_realm = true
 fallback_homedir = /home/%d/%u

 krb5_server = contoso100.com
 krb5_realm = CONTOSO100.COM
 ```
Değiştir ' CONTOSO100. COM' yönetilen etki alanınızın DNS etki alanı adına sahip. Sermaye durumda conf dosyasındaki etki alanı adını belirttiğinizden emin olun.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen etki alanına
Gerekli paketleri, Linux sanal makinesinde yüklü olan, sonraki görev sanal makinenin yönetilen etki alanına sağlamaktır.

```
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
```


> [!NOTE]
> **Sorun giderme:** Varsa *adcli* yönetilen etki alanınıza bulamıyor:
>   * Etki alanı (try ping) sanal makineden erişilebilir olduğundan emin olun.
>   * Sanal makinenin yönetilen etki alanında kullanılabilir olduğu aynı sanal ağa gerçekten dağıtılmış olduğunu kontrol edin.
>   * Sanal ağın DNS sunucusu ayarlarını yönetilen etki alanının etki alanı denetleyicilerine işaret edecek şekilde güncelleştirdiyseniz denetleyin.

SSSD hizmetini başlatın. SSH terminalinizde şu komutu yazın:
  ```
  sudo systemctl start sssd.service
  ```


## <a name="verify-domain-join"></a>Etki alanına katılım doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Bağlanmak için farklı bir SSH bağlantısı kullanılarak CoreOS VM etki alanına katıldı. Bir etki alanı kullanıcı hesabı kullanın ve kullanıcı hesabının doğru şekilde çözülmüş olup olmadığını denetleyin.

1. Terminal, SSH içinde etki alanına bağlanmak için aşağıdaki komutu yazın, SSH kullanarak sanal makinede CoreOS katıldı. Yönetilen etki alanına ait bir etki alanı hesabını kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
    ```

2. SSH terminalinizde giriş dizini doğru başlatılmadı görmek için aşağıdaki komutu yazın.
    ```
    pwd
    ```

3. SSH terminalinizde grup üyeliklerini doğru şekilde çözümlenmesine olmadığını görmek için aşağıdaki komutu yazın.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Etki alanına katılım sorunlarını giderme
Başvurmak [sorun giderme etki alanına katılma](join-windows-vm.md#troubleshoot-joining-a-domain) makalesi.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](create-instance.md)
* [Bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleyin](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
