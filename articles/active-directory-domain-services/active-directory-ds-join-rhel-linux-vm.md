---
title: 'Azure Active Directory etki alanı Hizmetleri: RHEL VM için yönetilen etki alanına Katıl | Microsoft Docs'
description: Red Hat Enterprise Linux sanal makinesini Azure AD Domain Services için katılın
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 1102a8c80b0040c7044d64c674d956d339238623
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60417359"
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Red Hat Enterprise Linux 7 sanal makinesini yönetilen bir etki alanına ekleme
Bu makalede, bir Red Hat Enterprise Linux (RHEL) 7 sanal makinesini bir Azure AD Domain Services yönetilen etki alanına ekleme işlemini göstermektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:  
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, bölümünde açıklanan tüm görevleri izleyin [Başlarken kılavuzunda](active-directory-ds-getting-started.md).
4. Sanal ağın DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. Tamamlamak için gereken adımlar [Azure AD Domain Services yönetilen etki alanınıza parolalarını eşitleyin](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux sanal makinesi sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'da bir RHEL 7 sanal makinesini sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineye dağıtma **aynı sanal ağda Azure AD Domain Services, etkinleştirdiğiniz**.
> * Çekme bir **farklı bir alt** Azure AD Domain Services, etkinleştirdiğiniz bir.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Yeni sağlanan Linux sanal makineye uzaktan bağlanın
RHEL 7.2 sanal makine Azure'da sağlandı. VM sağlama sırasında oluşturulan yerel yönetici hesabını kullanarak sanal makineye uzaktan bağlanmak için sonraki görevdir bakın.

Makaledeki yönergeleri [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine konak dosyasını yapılandırma
SSH terminalinizi/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```
Burada, 'contoso100.com' yönetilen etki alanınızın DNS etki alanı adı ' dir. 'contoso-rhel' yönetilen etki alanına katıldığınız RHEL sanal makinenin adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Linux sanal makinesinde gerekli paketleri yükleyin
Ardından, sanal makinede etki alanına katılım için gerekli paketleri yükleyin. SSH terminalinizde, gerekli paketleri yüklemek için aşağıdaki komutu yazın:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen etki alanına
Gerekli paketleri, Linux sanal makinesinde yüklü olan, sonraki görev sanal makinenin yönetilen etki alanına sağlamaktır.

1. AAD Domain Services yönetilen etki alanında bulur. SSH terminalinizde şu komutu yazın:

    ```
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE]
   > **Sorun giderme:** Varsa *bölge bulma* yönetilen etki alanınıza bulamıyor:
   >   * Etki alanı (try ping) sanal makineden erişilebilir olduğundan emin olun.
   >   * Sanal makinenin yönetilen etki alanında kullanılabilir olduğu aynı sanal ağa gerçekten dağıtılmış olduğunu kontrol edin.
   >   * Sanal ağın DNS sunucusu ayarlarını yönetilen etki alanının etki alanı denetleyicilerine işaret edecek şekilde güncelleştirdiyseniz denetleyin.

2. Kerberos başlatın. SSH terminalinizde şu komutu yazın:

    > [!TIP]
    > * 'AAD DC Administrators' grubuna ait bir kullanıcıyla belirttiğinizden emin olun.
    > * Büyük harf etki alanı adı belirtin, başka kinit başarısız olur.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. Makine etki alanına ekleyin. SSH terminalinizde şu komutu yazın:

    > [!TIP]
    > Önceki adımda ('kinit') belirtilen aynı kullanıcı hesabı kullanın.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

("Başarıyla kaydoldu makine bölgedeki") bir ileti almalısınız zaman makine başarıyla yönetilen etki alanına katılmış olmalıdır.


## <a name="verify-domain-join"></a>Etki alanına katılım doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Bağlanma RHEL VM farklı bir SSH bağlantısı kullanarak etki alanına katıldı. Bir etki alanı kullanıcı hesabı kullanın ve kullanıcı hesabının doğru şekilde çözülmüş olup olmadığını denetleyin.

1. Terminal, SSH içinde etki alanına bağlanmak için aşağıdaki komutu yazın, SSH kullanarak sanal makinede RHEL katıldı. Yönetilen etki alanına ait bir etki alanı hesabını kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
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
Başvurmak [sorun giderme etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) makalesi.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makinesi bir Azure AD Domain Services yönetilen etki alanına ekleyin](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Kerberos yükleme](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows tümleştirmesi Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
