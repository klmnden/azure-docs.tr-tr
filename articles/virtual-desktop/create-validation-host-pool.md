---
title: Hizmet güncelleştirmeleri - Azure doğrulamak için bir Windows sanal masaüstü Önizleme konak havuzu oluşturma
description: Hizmet güncelleştirmeleri üretim için güncelleştirmeleri çalışırken önce izlemek için bir doğrulama konak havuzu oluşturma
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: v-chjenk
ms.openlocfilehash: b654c177accb92d5b9a861f14874e795f4525a43
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65761215"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Öğretici: Hizmet güncelleştirmeleri doğrulamak için bir konak havuzu oluşturma

Ana bilgisayar havuzları, Windows sanal masaüstü Önizleme Kiracı ortamlar içinde bir veya daha fazla aynı sanal makinelerden oluşan bir koleksiyondur. Ana bilgisayar havuzları üretim ortamınıza dağıtmadan önce doğrulama konak havuz oluşturma önemle öneririz. Güncelleştirmelerin ilk doğrulama konak havuzları, hizmet güncelleştirmeleri üretim ortamınıza sunulmadan önce izleyin belirtmenize izin vererek uygulanır. Doğrulama konak havuzu, üretim ortamınızdaki kullanıcılar kapalı kalma süresine neden hatalara neden değişiklikleri keşfedebilir değil.

En son güncelleştirmeleri içeren uygulamalar iş emin olmak için doğrulama konak havuzu konak havuzlarına üretim ortamınızda mümkün olduğunca benzer olmalıdır. Üretim ana havuzuna yapmasını kullanıcıların sık doğrulama ana havuzuna bağlanmanız gerekir. Konak havuzunuz test otomatik, otomatikleştirilmiş test doğrulama konak havuzunda içermelidir.

Sorunları tanılama hizmetiyle doğrulama konak havuzundaki hata ayıklaması yapabilirsiniz veya [sorun giderme makaleleri Windows Sanal Masaüstü](https://docs.microsoft.com/Azure/virtual-desktop/troubleshoot-set-up-overview).

>[!NOTE]
> Doğrulama konak havuzunun gelecekteki tüm güncelleştirmeler test yerinde bırakın öneririz.

Başlamadan önce [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), henüz yapmadıysanız.

## <a name="create-your-host-pool"></a>Konak havuzunuz oluşturma

Bir konak havuzu şu makalelerden herhangi birine yönergeleri izleyerek oluşturabilirsiniz:
- [Öğretici: Azure Marketi ile konak havuz oluşturma](create-host-pools-azure-marketplace.md)
- [Bir Azure Resource Manager şablonu ile bir ana makine havuzu oluşturma](create-host-pools-arm-template.md)
- [PowerShell ile bir ana makine havuzu oluşturma](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Konak havuzunuz doğrulama konak havuz tanımlayın

Yeni konak havuzu doğrulama konak havuzu tanımlamak için aşağıdaki PowerShell cmdlet'lerini çalıştırın. Tırnak işaretleri değerleri oturumunuza uygun değerlerle değiştirin:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Doğrulama özelliği ayarlanmış olduğunu onaylamak için aşağıdaki PowerShell cmdlet'ini çalıştırın. Tırnak işaretleri değerleri oturumunuza uygun değerlerle değiştirin.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Cmdlet'i sonuçları bu çıktıya benzer görünmelidir:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Zamanlamayı güncelleştir

Önizleme'de, hizmet güncelleştirmeleri yaklaşık aylık bir tempoyla oluşur. Önemli bir sorun varsa, kritik güncelleştirmeler daha sık bir tempoyla sağlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Doğrulama konak havuzunu oluşturduğunuza göre isteğe bağlı olarak doğrulama konak havuzunuz RemoteApps ile doldurabilirsiniz. Windows sanal masaüstü uygulamalarında yönetme hakkında daha fazla bilgi için Yönet uygulama grupları öğretici bakın.

> [!div class="nextstepaction"]
> [Uygulama grupları öğretici yönetme](./manage-app-groups.md)
