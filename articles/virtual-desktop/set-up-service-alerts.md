---
title: Windows sanal masaüstü için - Azure hizmet uyarıları ayarlama
description: Nasıl Windows sanal masaüstü için hizmet bildirimleri almak için Azure hizmet durumu ayarlanır.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: v-chjenk
ms.openlocfilehash: cae75f16da2cad453c74b7e6e9fb62dd789fe5c7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081374"
---
# <a name="tutorial-set-up-service-alerts"></a>Öğretici: Hizmet uyarıları ayarlama

Azure hizmet durumu, hizmet sorunlarını ve sistem durumu danışmanları sanal Windows Masaüstü için izlemek için kullanabilirsiniz. Azure hizmet durumu, uyarı (örneğin, e-posta veya SMS) farklı türlerini size yardımcı bildirebilir bir sorun ve anlamanızda güncel tutmanız etkisini anlayın. Azure hizmet durumu kapalı kalma süresini azaltın ve planlı Bakım ve kaynaklarınızın kullanılabilirliğini etkileyebilecek değişiklikler için hazırlık da yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Oluşturun ve hizmet uyarılarını yapılandırın.

Azure hizmet durumu hakkında daha fazla bilgi için bkz. [Azure durumu belgeleri](https://docs.microsoft.com/azure/service-health/).

## <a name="prerequisites"></a>Önkoşullar

- [Öğretici: Windows sanal masaüstü önizlemesinde bir kiracı oluşturma](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory)
- [Öğretici: PowerShell ile hizmet sorumluları ve rol atamalarını oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-service-principal-role-powershell)
- [Öğretici: Azure Marketi ile konak havuz oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace)

## <a name="create-service-alerts"></a>Hizmet uyarılarını oluşturma

Bu bölümde, Azure hizmet durumu yapılandırma ve Azure Portal'da erişebileceğiniz bildirimleri ayarlama işlemini gösterir. Farklı uyarı türleri ayarlayabilir ve bunları zamanında bildirmek üzere zamanlayabilirsiniz.

### <a name="recommended-service-alerts"></a>Önerilen hizmet uyarıları

Hizmet uyarılarını aşağıdaki sistem durumu olay türü için oluşturduğunuz öneririz:

- **Hizmet sorunu:** Kullanıcılarınızın, hizmet veya sanal Windows Masaüstü kiracınız yönetme olanağı ile bağlantı etkileyen önemli konularda bildirimleri alın.
- **Sistem durumu Danışmanı:** Dikkat etmeniz gereken bildirimleri alın. Bu tür bir bildirim bazı örnekleri şunlardır:
    - Açık bağlantı noktası 3389 olarak olmayan güvenli bir şekilde yapılandırılmış sanal makineleri (VM'ler)
    - İşlevlerin kullanımdan kaldırma

### <a name="configure-service-alerts"></a>Hizmet uyarılarını yapılandırma

Hizmet uyarılarını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Seçin **hizmet durumu.**
3. Yönergeleri kullanın [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) , uyarılar ve bildirimler ayarlamak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ayarlama ve hizmet sorunları ve sistem durumu danışmanları sanal Windows Masaüstü için izlemek için Azure hizmet durumu kullanma öğrendiniz. Windows sanal masaüstü oturum açma hakkında bilgi edinmek için Windows sanal masaüstü bilgi belgeleri Bağlan geçin.

> [!div class="nextstepaction"]
> [Uzak Masaüstü İstemcisi Windows 7 ve Windows 10 bağlanma](./connect-windows-7-and-10.md)
