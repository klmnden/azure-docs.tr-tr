---
title: PowerShell - Azure ile RDP özelliklerini özelleştirme
description: PowerShell cmdlet'leri ile RDP özellikleri için Windows sanal masaüstü özelleştirme yapma.
services: virtual-desktop
author: v-hevem
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: v-hevem
ms.openlocfilehash: ce14f990272fa1e70d07c0f4a1f18025b536eccc
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618874"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Bir ana makine havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme

Çoklu monitör deneyimi ve ses yönlendirmesi gibi bir konak havuzun Uzak Masaüstü Protokolü (RDP) özelliklerini özelleştirme, ihtiyaçlarını temel alarak kullanıcılarınız için en iyi deneyim sunmanıza olanak sağlar. Windows Sanal Masaüstü kullanarak RDP özelliklerini özelleştirebilirsiniz **- CustomRdpProperty** parametresinde **kümesi RdsHostPool** cmdlet'i.

Bkz: [Uzak Masaüstü RDP dosyası ayarları](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files) için desteklenen özellikler ve varsayılan değerlerine tam bir listesi.

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Ekleme veya tek bir özel RDP özelliği Düzenle

Ekleme veya tek bir özel RDP özelliği düzenlemek için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```
![PowerShell cmdlet Get-RDSRemoteApp adı vurgulanmış FriendlyName ile ekran görüntüsü.](media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Ekle veya birden çok özel RDP özelliği Düzenle

Noktalı virgülle ayrılmış bir dize olarak özel RDP özellikleri sağlayarak eklemek veya birden çok özel RDP özelliklerini düzenlemek için aşağıdaki PowerShell cmdlet'lerini çalıştırın:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```
![PowerShell cmdlet Get-RDSRemoteApp adı vurgulanmış FriendlyName ile ekran görüntüsü.](media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Tüm özel RDP özellikler Sıfırla

' Ndaki yönergeleri takip ederek özel RDP özellikler varsayılan değerlerine sıfırlayabilirsiniz [ekleme veya düzenleme tek bir özel RDP özelliği](#add-or-edit-a-single-custom-rdp-property), veya aşağıdakini çalıştırarak, bir ana makine havuzu için tüm özel RDP özellikler sıfırlayabilirsiniz PowerShell cmdlet:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```
![PowerShell cmdlet Get-RDSRemoteApp adı vurgulanmış FriendlyName ile ekran görüntüsü.](media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Sonraki adımlar

Belirtilen konak havuzu RDP özelliklerini özelleştirdiğinize göre, bir Windows sanal masaüstü istemcisi için bir kullanıcı oturumunun parçası olarak test etmek için oturum açabilir. Bunu yapmak için Windows sanal masaüstü bilgi belgeleri Bağlan geçin:

- [Windows 10 ve Windows 7 ' bağlanma](connect-windows-7-and-10.md)
- [Bir web tarayıcısından bağlanma](connect-web.md)
