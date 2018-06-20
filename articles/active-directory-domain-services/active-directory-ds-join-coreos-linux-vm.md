---
title: 'Azure Active Directory etki alanı Hizmetleri: CoreOS Linux VM yönetilen bir etki alanına katılın. | Microsoft Docs'
description: Azure AD Etki Alanı Hizmetleri'ne CoreOS Linux sanal makine birleştirme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: maheshu
ms.openlocfilehash: 53127cbc663141a5080426072360237935f175db
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36217349"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>CoreOS Linux sanal makinesini yönetilen bir etki alanına katma
Bu makalede CoreOS Linux sanal makine Azure'da bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma kullanmayı gösterir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. Sanal ağ için DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz: [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. İçin gerekli adımları [Azure AD etki alanı Hizmetleri yönetilen etki alanınız için parola eşitleme](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-coreos-linux-virtual-machine"></a>CoreOS Linux sanal makine sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'da bir CoreOS sanal makine sağlama:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Bu makalede kullanan **CoreOS Linux (kararlı)** azure'da sanal makine görüntüsü.

> [!IMPORTANT]
> * Sanal makineye dağıtmak **Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz aynı sanal ağ**.
> * Çekme bir **farklı alt** Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz olandan.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Uzaktan yeni sağlandı Linux sanal makineye bağlanma
CoreOS sanal makine Azure'da sağlandı. Sonraki uzaktan VM sağlarken oluşturulmuş yerel yönetici hesabı kullanarak sanal makineye bağlanmak için bir görevdir.

Makalesindeki yönergeleri izleyin [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine üzerinde konak dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```
Burada, 'contoso100.com' DNS etki alanı, yönetilen etki alanı adıdır. 'contoso-coreos' CoreOS sanal makineyi yönetilen bir etki alanına katılma barındırıcı adıdır.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Linux sanal makinede SSSD hizmetini yapılandırma
Ardından, SSSD yapılandırma dosyanızda güncelleştirme ('/ etc/sssd/sssd.conf') aşağıdaki örnekle eşleşmesi için:

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
Değiştir ' CONTOSO100. COM', yönetilen etki alanınızın DNS etki alanı adına sahip. Büyük harf durumda conf dosyasındaki etki alanı adını belirttiğinizden emin olun.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen bir etki alanına katılma
Gerekli paketleri Linux sanal makinede yüklü olan, sonraki görev sanal makineyi yönetilen etki alanına belirlemektir.

```
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
```


> [!NOTE]
> **Sorun giderme:** varsa *adcli* yönetilen etki alanınızı bulamıyor:
  * Etki alanı sanal makine (try ping) erişilebilir olduğundan emin olun.
  * Sanal makine yönetilen etki alanı kullanılamıyor aynı sanal ağa gerçekten dağıtıldıktan denetleyin.
  * Sanal ağın DNS sunucusu ayarlarını yönetilen etki alanının etki alanı denetleyicilerine işaret edecek şekilde güncelleştirdiyseniz denetleyin.
>

SSSD hizmetini başlatın. SSH terminalinizde aşağıdaki komutu yazın:
  ```
  sudo systemctl start sssd.service
  ```


## <a name="verify-domain-join"></a>Etki alanına katılma doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Bağlanmak CoreOS VM farklı bir SSH bağlantısı kullanarak etki alanına katıldı. Bir etki alanı kullanıcı hesabı kullanmanızı ve kullanıcı hesabının doğru çözülmüş olup olmadığını denetleyin.

1. Terminal, SSH SSH kullanarak CoreOS sanal makine etki alanına bağlanmak için aşağıdaki komutu yazın katıldı. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
    ```

2. SSH terminalinizde giriş dizini doğru başlatılmadı görmek için aşağıdaki komutu yazın.
    ```
    pwd
    ```

3. SSH terminalinizde grup üyeliklerini doğru çözüldüğünü olmadığını görmek için aşağıdaki komutu yazın.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Sorun giderme etki alanına katılma
Başvurmak [sorun giderme etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) makalesi.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
