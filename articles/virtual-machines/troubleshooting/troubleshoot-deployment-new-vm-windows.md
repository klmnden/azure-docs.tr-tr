---
title: Azure'da Windows VM dağıtımı sorunlarını giderme | Microsoft Docs
description: Azure'da yeni bir Windows sanal makine oluşturduğunuzda, Resource Manager dağıtım sorunlarını giderme
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: jeconnoc
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8f18fbabe52c9170cde70900933ce96a3a6400c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62123865"
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Azure'da yeni bir Windows sanal makine oluştururken dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>En sık karşılaşılan sorunlar
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Diğer VM Dağıtım sorunlar ve sorular için bkz. [dağıtma azure'da Windows sanal makine sorunlarını giderme](troubleshoot-deploy-vm-windows.md).

## <a name="collect-activity-logs"></a>Toplama etkinlik günlükleri
Sorun gidermeye başlamak için sorunla ilişkili hatanın tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, izlenecek işlem hakkında ayrıntılı bilgi içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** İşletim sistemi Windows genelleştirilmiş olduğundan ve karşıya yüklenen ve genelleştirilmiş ayarıyla yakalanan, ardından olmayacaktır hataları. Benzer şekilde, işletim sistemi Windows özelleştirilmiş ve karşıya yüklendi ve özel ayarlarla yakalanan sonra olmayacaktır hataları.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** İşletim sistemi Windows genelleştirilmiş olarak karşıya ise özelleştirilmiş, OOBE ekranında takılı VM ile bir sağlama zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** İşletim sistemi Windows özelleştirilmiş ve genelleştirilmiş olarak yüklenmiş ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için OOBE ekranında takılı VM ile bir sağlama hatası alırsınız.

**Çözümleme**

Hem bu hataları gidermek için [özgün VHD yüklemek için Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd), mevcut şirket içi, işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile. Genelleştirilmiş olarak yüklemek için önce sysprep çalıştırmayı unutmayın.

**Hataları yakalamaya:**

**N<sup>3</sup>:** İşletim sistemi Windows genelleştirilmiş olarak yakalanır ise özgün VM genelleştirilmiş olarak işaretlenir kullanılabilir olmadığı için özelleştirilmiş, sağlama bir zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** Windows özelleştirilmiş ve genelleştirilmiş olarak yakalanan işletim sistemi ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama hatası alırsınız. İşaretli olduğu için ayrıca orijinal VM kullanılamaz olarak özelleştirilmiş.

**Çözümleme**

Hem bu hataları gidermek için geçerli görüntünün portaldan silin ve [geçerli Vhd'lerden elde](../windows/create-vm-specialized.md) işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Sorun: Galeri/özel/Market görüntüsü; ayırma hatası
Yeni VM istek, istenen VM boyutu destekleyemiyorsa veya isteği gerçekleştirmek için kullanılabilir boş alan yok bir kümeye sabitlenmiş durumlarda bu hata oluşur.

**1. neden:** Küme, istenen VM boyutu destekleyemez.

**1. çözüm:**

* Daha küçük bir VM boyutu isteği yeniden deneyin.
* İstenen VM boyutu değiştirilemiyorsa:
  * Kullanılabilirlik kümesindeki tüm sanal makineler durdurun.
    Tıklayın **kaynak grupları** > *kaynak grubunuzun* > **kaynakları** >  *, kullanılabilirlik kümesi*  >  **Sanal makineler** > *sanal makinenizi* > **Durdur**.
  * Tüm sanal makineleri durdurduktan sonra istenen boyutu yeni bir VM oluşturun.
  * İlk olarak, yeni VM'yi başlatın ve ardından her biri durdurulmuş sanal makineler seçin ve tıklayın **Başlat**.

**2. neden:** Kümenin boş kaynak yok.

**2. çözüm:**

* İsteği daha sonra yeniden deneyin.
* Yeni VM'yi farklı bir kullanılabilirlik kümesinin bir parçası olarak
  * (Aynı bölgede) farklı bir kullanılabilirlik yeni bir VM oluşturun.
  * Yeni VM, aynı sanal ağa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Windows VM'yi durdurduğunuzda ya da azure'daki mevcut bir Windows VM'yi yeniden boyutlandırma sırasında sorunlarla karşılaşırsanız bkz [başlatma veya varolan bir Windows sanal makinesini azure'da yeniden boyutlandırmayla ilgili sorunları giderme Resource Manager dağıtım sorunlarını](restart-resize-error-troubleshooting.md).


