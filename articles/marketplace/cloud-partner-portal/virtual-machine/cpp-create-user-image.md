---
title: Kullanıcı VM görüntüsü oluşturmak için Azure Marketi | Microsoft Docs
description: Bir kullanıcı VM görüntüsü oluşturmak için gerekli başvuruları ve adımları listeler.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: bf87856dc28e83fb1308f20613338b9bbfd8f896
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744117"
---
# <a name="create-a-user-vm-image"></a>Kullanıcı VM görüntüsü oluşturma

Bu makalede, genelleştirilmiş bir VHD'den yönetilmeyen görüntü oluşturma için gereken iki genel adımlar açıklanmaktadır.  Başvurular, her adımda size yol göstermesi için sağlanır: görüntü yakalamak ve görüntüyü genelleştirin.


## <a name="capture-the-vm-image"></a>VM görüntüsü yakalama

Erişim yaklaşımınızı karşılık gelen VM yakalama üzerinde aşağıdaki makaledeki yönergeleri kullanın:

-  PowerShell: [Bir Azure VM'den yönetilmeyen bir VM görüntüsü oluşturma](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [Bir sanal makine veya VHD görüntüsü oluşturma](../../../virtual-machines/linux/capture-image.md)
-  API: [Sanal makineler - yakalama](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)


## <a name="generalize-the-vm-image"></a>VM görüntüyü Genelleştirme

Daha önce genelleştirilmiş bir VHD'den kullanıcı görüntüsü oluşturduğu için bu da genelleştirilmiş.  Yeniden erişim mekanizmanızı karşılık gelen makaleyi seçin.  (Bunu yakalandığında, zaten diskinizi genelleştirildiğinden.)

-  PowerShell: [VM'yi Genelleştirme](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [2. adım: VM görüntüsü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Sanal makineler - Generalize](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="next-steps"></a>Sonraki adımlar

Ardından şunları yapacaksınız [bir sertifika oluşturmak](cpp-create-key-vault-cert.md) ve yeni bir Azure Key Vault'ta depolar.  Bu sertifika, sanal makine için güvenli bir WinRM bağlantı kurmak için gereklidir.
