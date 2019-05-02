---
title: Windows sanal masaüstü genel bakış, geri bildirim ve Destek - Azure sorunlarını giderme
description: Bir Windows sanal masaüstü Kiracı ortamını ayarlama sırasında sorunları gidermek için bir genel bakış.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 8e344d6908ba19f8e2294c7777b9c1016eafaf52
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927650"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Genel bakış, geri bildirim ve destek sorunlarını giderme

Bu makalede, bir Windows sanal masaüstü Kiracı ortamını ayarlama karşılaşabilirsiniz ve bu sorunları çözümlemeye yönelik yollar sağlar sorunlar genel bakış sağlar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir. Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine Windows sanal masaüstü hizmetiyle tartışmak için.

## <a name="escalation-tracks"></a>Yükseltme izler

Belirlemek ve uzak masaüstü istemcisini kullanarak bir kiracı ortamını ayarlarken, karşılaşabileceğiniz sorunları çözmek için aşağıdaki tabloyu kullanın.

>[!NOTE]
>Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir. Biz Windows sanal masaüstü Destek birimine başvurduğunuzda teknoloji topluluğuna forumumuzda şimdilik gidin. Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine sorunları tartışmak için. Bir destek sorunu çözmeniz gerekiyorsa, sorunun oluştuğu için yaklaşık zaman çerçevesini ve etkinlik kimliği içerir.

| **Sorunu**                                                            | **Önerilen çözüm**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Kiracı oluşturma                                                    | Bir Azure kesintisi varsa, kişi [Azure Destek](https://azure.microsoft.com/support/options/); Aksi takdirde kişi **Uzak Masaüstü Hizmetleri/Windows sanal masaüstü desteği**.|
| Azure portalında Market şablonları erişme       | Bir Azure kesintisi varsa, kişi [Azure Destek](https://azure.microsoft.com/support/options/). <br> <br> Azure Market'te Windows sanal masaüstü şablonları serbestçe kullanılabilir.|
| Github'dan Azure Resource Manager şablonları erişme                                  | "Oluşturma Windows Sanal Masaüstü oturumu konağı VM'ler" bölümüne bakın [Kiracı ve konak havuz oluşturma](troubleshoot-set-up-issues.md). Hala çözülmemiş bir sorun ise, kişi [GitHub Destek ekibine](https://github.com/contact). <br> <br> GitHub şablon eriştikten sonra hata ortaya çıkarsa bağlantı [Azure Destek](https://azure.microsoft.com/support/options/).|
| Azure sanal ağ (VNET) ve Express Route oturum ana bilgisayarı havuzu ayarları               | İlgili kişi **Azure desteği (ağ)**. |
| Oturum Ana havuzuna Azure Resource Manager şablonları ile Windows sanal masaüstü sağlanan kullanılmadığında, sanal makine (VM) oluşturma | İlgili kişi **Azure desteği (işlem)**. <br> <br> Windows sanal masaüstü ile sağlanan Azure Resource Manager şablonları ile sorunları görmek için Windows sanal masaüstü oluşturarak Kiracı bölümünü [Kiracı ve konak havuz oluşturma](troubleshoot-set-up-issues.md). |
| Azure portalında Windows Sanal Masaüstü Oturumu Ana bilgisayar ortamını yönetme    | İlgili kişi **Azure Destek**. <br> <br> Uzak Masaüstü Hizmetleri/Windows sanal masaüstü PowerShell kullanırken yönetimi sorunları için bkz: [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md) veya başvurun **Uzak Masaüstü Hizmetleri/Windows Sanal Masaüstü destek ekibi** . |
| Windows sanal masaüstü yapılandırmasına bağlı ana bilgisayar havuzları ve uygulama gruplarına (uygulama grupları)      | Bkz: [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md), veya başvurun **Uzak Masaüstü Hizmetleri/Windows sanal masaüstü Destek ekibine**. <br> <br> Sorunları örnek grafik kullanıcı arabirimi (GUI) bağlıdır, Yammer topluluğuna ulaşın.|
| Başlangıç menüsünde arızası Uzak Masaüstü istemcileri                                                 | Bkz: [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md) ve bu sorunu çözmezse, kişi **Uzak Masaüstü Hizmetleri/Windows sanal masaüstü Destek ekibine**.  <br> <br> Bir ağ sorunu ise, kullanıcılarınız kendi ağ yöneticisine başvurmanız gerekir. |
| Akış bağlı                                                                 | Kullanmayla ilgili sorun giderme "kullanıcı bağlanır ancak hiçbir şey görüntülenmez (akış)" bölümünü [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md). <br> <br> İçin bir uygulama grubu için kullanıcılarınızın atanmışsa İlerlet **Uzak Masaüstü Hizmetleri/Windows sanal masaüstü Destek ekibine**. |
| Bulma sorunları nedeniyle ağ akışı                                            | Kullanıcılarınızın kendi ağ yöneticisine başvurmanız gerekir. |
| Bağlanan istemciler                                                                    | Bkz: [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md) ve bu sorununuzu çözmezse bkz [oturumu ana bilgisayar Sanal Makine Yapılandırması](troubleshoot-vm-configuration.md). |
| Uzak uygulama veya Masaüstü yanıt hızı                                      | Belirli bir uygulama ya da ürün sorunları bağlı ise, bir üründen sorumlu ekibiyle iletişim kurun. |
| Lisans iletileri ya da hataları                                                          | Belirli bir uygulama ya da ürün sorunları bağlı ise, bir üründen sorumlu ekibiyle iletişim kurun. |

## <a name="next-steps"></a>Sonraki adımlar

- Bir Windows sanal masaüstü ortamında bir kiracı ve konak havuzu oluşturulurken sorunlarını gidermek için bkz: [Kiracı ve konak havuz oluşturma](troubleshoot-set-up-issues.md).
- Bir sanal makine (VM) Windows sanal masaüstü yapılandırılırken sorunlarını gidermek için bkz: [oturumu ana bilgisayar Sanal Makine Yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantıları ile ilgili sorunları gidermek için bkz: [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md).
- PowerShell ile Windows sanal masaüstü kullanırken sorunlarını gidermek için bkz: [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Önizleme hizmeti hakkında daha fazla bilgi için bkz: [Windows Desktop Önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Bir sorun giderme öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablonu dağıtımlardaki sorunları çözmek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Eylemler denetleme hakkında bilgi edinmek için [Resource Manager denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).