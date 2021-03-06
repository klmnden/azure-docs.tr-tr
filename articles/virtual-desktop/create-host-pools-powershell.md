---
title: -Azure PowerShell ile Windows sanal masaüstü Önizleme konak havuz oluşturma
description: Bir konak havuzu Windows sanal masaüstü Önizleme aşamasında PowerShell cmdlet'leri ile oluşturma
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: helohr
ms.openlocfilehash: 374d5a8f51e28b8a10595842cfc301db503b6bed
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67613323"
---
# <a name="create-a-host-pool-with-powershell"></a>PowerShell ile ana bilgisayar havuzu oluşturma

Ana bilgisayar havuzları, Windows sanal masaüstü Önizleme Kiracı ortamlar içinde bir veya daha fazla aynı sanal makinelerden oluşan bir koleksiyondur. Her konak havuzu, fiziksel masaüstünde yaptıkları gibi kullanıcı etkileşim kurabilir bir uygulama grubu içerebilir.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Bir konak havuzu oluşturmak için PowerShell istemcinizi kullanın

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Windows sanal masaüstü ortama oturum açmak için aşağıdaki cmdlet'i çalıştırın

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Ardından, Windows sanal masaüstü kiracınıza yeni bir ana makine havuzu oluşturmak için bu cmdlet'i çalıştırın:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Konak havuzu katılın ve yerel bilgisayarınızda yeni bir dosyaya kaydetmek için bir oturum ana bilgisayarı yetkilendirmek için bir kayıt belirtecinizi oluşturmak için İleri cmdlet'ini çalıştırın. -ExpirationHours parametresini kullanarak kayıt belirtecinizi ne kadar süreyle geçerli olacağını belirtebilirsiniz.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token > <PathToRegFile>
```

Bundan sonra varsayılan masaüstü uygulaması Grup ana makine havuzu için Azure Active Directory Kullanıcıları eklemek için bu cmdlet'i çalıştırın.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

**Ekle RdsAppGroupUser** cmdlet'i güvenlik grupları eklemeyi desteklemez ve yalnızca bir kullanıcı aynı anda uygulama grubuna ekler. Uygulama grubu için birden çok kullanıcı eklemek istiyorsanız, uygun kullanıcı asıl adlarından cmdlet'ini yeniden çalıştırın.

Daha sonra kullanacağınız bir değişken için kayıt belirteci vermek için aşağıdaki cmdlet'i çalıştırın [Windows sanal masaüstü ana havuzuna sanal makineleri kaydetmek](#register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Ana makine havuzu için sanal makine oluşturma

Artık Windows sanal masaüstü konak havuzunuza katılabilir bir Azure sanal makinesinde oluşturabilirsiniz.

Birden çok yolla bir sanal makine oluşturabilirsiniz:

- [Bir Azure Galerisi görüntüsünü bir sanal makine oluşturun](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Yönetilen bir görüntüden sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Yönetilmeyen bir görüntüden sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-preview-agent-installations"></a>Windows sanal masaüstü Önizleme aracı yüklemeleri için sanal makineleri hazırlama

Windows sanal masaüstü aracıları yüklemek ve Windows sanal masaüstü konak havuzunuz için sanal makineleri kaydetmek için önce sanal makinelerinizi hazırlamak için şunları yapmanız gerekir:

- Etki alanına katılım makine gerekir. Bu gelen Windows sanal masaüstü kullanıcılar kendi Azure Active Directory hesabı, Active Directory hesabına eşlenmesi ve başarılı bir şekilde sanal makineye erişim izni sağlar.
- Bir Windows Server işletim sistemi sanal makine çalışıyorsa Uzak Masaüstü oturumu ana bilgisayarı (RDSH) rolünü yüklemeniz gerekir. RDSH rolü düzgün şekilde yüklemek Windows sanal masaüstü aracıları sağlar.

Başarıyla etki alanına katılma için her sanal makinede aşağıdaki işlemleri yapın:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. Sanal makinede başlatın **Denetim Masası** seçip **sistem**.
3. Seçin **bilgisayar adı**seçin **ayarlarını değiştir**ve ardından **Değiştir...**
4. Seçin **etki alanı** ve ardından sanal ağ üzerinde Active Directory etki alanını girin.
5. Makine etki alanına katılım ayrıcalıkları olan bir etki alanı hesabıyla kimlik doğrulaması.

    >[!NOTE]
    > Bir Azure AD Domain Services ortama Vm'lerinize birleştirdiğimiz, etki alanı katılma kullanıcınızın üyesi olduğundan emin olun [AAD DC Administrators grubu](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-admingroup#task-3-configure-administrative-group).

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool"></a>Windows sanal masaüstü Önizleme ana havuzuna sanal makineleri kaydetmek

Bir Windows sanal masaüstü ana havuzuna sanal makineleri kaydetme, sanal masaüstü Windows aracılarını yükleme olarak kadar kolaydır.

Sanal Masaüstü Windows aracılarını kaydetmek için her sanal makinede aşağıdakileri yapın:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. İndirin ve Windows sanal masaüstü Aracısı'nı yükleyin.
   - İndirme [Windows sanal masaüstü aracı](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv).
   - İndirilen yükleyiciye sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır**, ardından **Tamam**. Bu yükleyici güven için sisteminizi olanak tanır.
   - Yükleyiciyi çalıştırın. Yükleyici için kayıt belirtecinizi sorduğunda, aradığınızı değeri girin **dışarı aktarma RdsRegistrationInfo** cmdlet'i.
3. İndirin ve sanal masaüstü Aracısı Windows Önyükleme Yükleyicisi'ni yükleyin.
   - İndirme [Windows sanal masaüstü aracı Şifresizdir](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH).
   - İndirilen yükleyiciye sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır**, ardından **Tamam**. Bu yükleyici güven için sisteminizi olanak tanır.
   - Yükleyiciyi çalıştırın.

>[!IMPORTANT]
>Güvenliğini sağlamaya yardımcı olmak için azure'da Windows sanal masaüstü ortamınızı Vm'lerinizde gelen bağlantı noktası 3389 açmayın öneririz. Windows sanal masaüstü açık bir konak havuzun Vm'leri erişmek kullanıcılar için 3389 numaralı gelen bağlantı noktası gerektirmez. Sorun giderme amacıyla 3389 numaralı bağlantı noktası açmanız gerekiyorsa, kullanmanızı öneririz [tam zamanında VM erişimi](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Sonraki adımlar

Bir konak havuzu yaptığınız, RemoteApps ile doldurabilirsiniz. Windows sanal masaüstü uygulamalarında yönetme hakkında daha fazla bilgi için Yönet uygulama grupları öğretici bakın.

> [!div class="nextstepaction"]
> [Uygulama grupları öğretici yönetme](./manage-app-groups.md)
