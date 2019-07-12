---
title: Bir Azure Resource Manager şablonu - Azure ile bir Windows sanal masaüstü Önizleme konak havuzu oluşturma
description: Bir konak havuzu bir Azure Resource Manager şablonu ile Windows sanal masaüstü önizlemede oluşturmak nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: cdc61aede6e650bce62768b7a97f8640affd594f
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620479"
---
# <a name="create-a-host-pool-with-an-azure-resource-manager-template"></a>Azure Resource Manager şablonuyla ana bilgisayar havuzu oluşturma

Ana bilgisayar havuzları, Windows sanal masaüstü Önizleme Kiracı ortamlar içinde bir veya daha fazla aynı sanal makinelerden oluşan bir koleksiyondur. Her konak havuzu, fiziksel masaüstünde yaptıkları gibi kullanıcı etkileşim kurabilir bir uygulama grubu içerebilir.

Microsoft tarafından sağlanan bir Azure Resource Manager şablonu ile Windows sanal masaüstü Kiracı için bir konak havuzu oluşturmak için bu bölümün yönergeleri izleyin. Bu makalede, Windows sanal masaüstü konak havuz oluşturma, bir kaynak grubu ile Vm'leri bir Azure aboneliği oluşturun, bu sanal makineler için AD etki alanına ve Vm'leri ile Windows sanal masaüstü kaydetme hakkında size bildirir.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Azure Resource Manager şablonu çalıştırmak için ihtiyacınız olanlar

Azure Resource Manager şablonu çalıştırmadan önce şunları bildiğinizden emin olun:

- Burada kullanmak istediğiniz görüntüyü kaynağıdır. Azure Galerisi'nden olduğundan veya özel?
- Etki alanına katılma kimlik.
- Windows sanal masaüstü kimlik bilgilerinizi.

Azure Resource Manager şablonuyla bir Windows sanal masaüstü konak havuzu oluşturduğunuzda, Azure Galerisi, yönetilen bir görüntü veya yönetilmeyen görüntü bir sanal makine oluşturabilirsiniz. VM görüntüleri oluşturma hakkında daha fazla bilgi için bkz. [Windows VHD veya VHDX yüklemek için hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) ve [Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).

## <a name="run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool"></a>Yeni bir ana makine havuzu sağlamak için Azure Resource Manager şablonu çalıştırın

Başlamak için Git [bu GitHub URL'sini](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool).

### <a name="deploy-the-template-to-azure"></a>Şablonu Azure'a dağıtma

Bir kurumsal aboneliğinde dağıtıyorsanız, aşağıya kaydırın ve **azure'a Dağıt**, ardından Atla tamamlanan parametreleri doldurun temel görüntü kaynağınızı.

Bir bulut çözümü sağlayıcısı abonelikte dağıtıyorsanız, Azure'a dağıtmak için aşağıdaki adımları izleyin:

1. Ekranı aşağı kaydırın ve sağ **azure'a Dağıt**, ardından **kopya bağlantı konumu**.
2. Not Defteri gibi bir metin düzenleyicisinde açın ve bağlantı var. yapıştırın.
3. Hemen sonra "https://portal.azure.com/" girin (#) diyez etiketi önce bir at işareti (@) ve Kiracı etki alanı adından. İşte bir örnek kullanmanız gerektiğini biçimi: https://portal.azure.com/@Contoso.onmicrosoft.com#create/.
4. Azure portalında bir bulut çözümü sağlayıcısı aboneliğe yönetici/katkıda bulunan izinlerine sahip bir kullanıcı olarak oturum açın.
5. Adres çubuğuna metin düzenleyiciye kopyaladığınız bağlantı yapıştırın.

Windows sanal masaüstü hangi parametreleri hakkında senaryonuz için girmeniz gereken yönergeler için bkz [Benioku dosyası](https://github.com/Azure/RDS-Templates/blob/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool/README.md). Benioku dosyasını her zaman en son değişikliklerle güncelleştirilir.

## <a name="assign-users-to-the-desktop-application-group"></a>Masaüstü uygulama grubuna kullanıcı atama

GitHub Azure Resource Manager şablonu tamamlandıktan sonra sanal makinelerinizde tam oturum Masaüstü test başlamadan önce kullanıcı erişimi atayın.

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Masaüstü uygulama grubuna kullanıcıları atamak için bir PowerShell penceresi açın ve Windows sanal masaüstü ortama oturum açmak için bu cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bundan sonra kullanıcılar bu cmdlet ile masaüstü uygulama grubuna ekleyin:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Kullanıcının UPN kullanıcının kimliğini Azure Active Directory'de eşleşmesi gerekir (örneğin, user1@contoso.com). Birden çok kullanıcı eklemek istiyorsanız, her kullanıcı için bu cmdlet'i çalıştırmanız gerekir.

Bu adımları tamamladıktan sonra Masaüstü uygulama grubuna eklenen kullanıcılar Windows sanal masaüstüne desteklenen Uzak Masaüstü istemcileri ile oturum açın ve bir kaynağı için bir oturum Masaüstü bakın.

>[!IMPORTANT]
>Güvenliğini sağlamaya yardımcı olmak için azure'da Windows sanal masaüstü ortamınızı Vm'lerinizde gelen bağlantı noktası 3389 açmayın öneririz. Windows sanal masaüstü açık bir konak havuzun Vm'leri erişmek kullanıcılar için 3389 numaralı gelen bağlantı noktası gerektirmez. Sorun giderme amacıyla 3389 numaralı bağlantı noktası açmanız gerekiyorsa, kullanmanızı öneririz [tam zamanında VM erişimi](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).