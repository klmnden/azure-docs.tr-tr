---
title: Azure Lab Services paylaşılan görüntü galerisinde kullanabilecek | Microsoft Docs
description: Bir kullanıcı başka bir görüntü paylaşın ve başka bir kullanıcı, laboratuvarda bir VM şablonu oluşturmak için görüntü kullanabilirsiniz, paylaşılan görüntü Galerisi kullanmak için bir laboratuvar hesabı yapılandırmayı öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 8d8b6fffe197d4180b091518dcd1615d0e0b9d19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412847"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Lab Services paylaşılan görüntü galerisinde kullanın
Bu makalede, Öğretmenler/Laboratuvar Yöneticisi şablonu sanal makine görüntüsü de başkaları tarafından kullanılabilmeleri nasıl tasarruf edebilecekleri gösterilmektedir. Bu görüntüleri bir Azure kaydedilir [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md). İlk adım, Laboratuvar Yöneticisi var olan bir paylaşılan görüntü Galerisine Laboratuvar hesabına ekler. Paylaşılan görüntü Galerisi bağlandıktan sonra labs Laboratuvar hesabında oluşturulan görüntüleri paylaşılan görüntü Galerisine kaydedebilirsiniz. Diğer Öğretmenler, bu görüntü, kendi sınıfları için bir şablon oluşturmak için paylaşılan bir görüntü galerisinden seçebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
- Kullanarak bir paylaşılan görüntü Galerisi oluşturma [Azure PowerShell](../../virtual-machines/windows/shared-images.md) veya [Azure CLI](../../virtual-machines/linux/shared-images.md).
- Paylaşılan görüntü Galerisi Laboratuvar hesabına eklediğiniz. Adım adım yönergeler için bkz: [görüntü Galerisine ekleme veya ayırma nasıl paylaşılan](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü Galerisine bir görüntüyü kaydedin
Paylaşılan görüntü Galerisi bağlandıktan sonra bir Öğretmen kaydedebilir veya diğer Öğretmen tarafından yeniden kullanılabilir, böylece paylaşılan görüntü Galerisine bir görüntü yükleyin. Paylaşılan görüntü Galerisine bir görüntü yüklemek için yönergeler için bkz: [paylaşılan görüntü Galerisi genel bakış](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Sınıf Laboratuvarlarını kullanıcı arabirimi (UI) curently, paylaşılan bir görüntü Galerisine bir laboratuvar görüntüsü kaydediliyor desteklemiyor. 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanma
Öğretmen/Profesör yeni Laboratuvar oluşturma sırasında şablonu için paylaşılan görüntü galerisinde kullanılabilir özel bir görüntü seçebilirsiniz.

![Galeriden sanal makine görüntüsünü kullanma](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md).
