---
title: Portalı kullanarak Linux için paylaşılan bir Azure sanal makine görüntüleri oluşturma | Microsoft Docs
description: Oluşturmak ve sanal makine görüntüleri paylaşmak için Azure portal'ı kullanmayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/27/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: ea0a5961aff5935efea3eb98fd53904ce864ec2c
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708904"
---
# <a name="create-a-shared-image-gallery-using-the-azure-portal"></a>Azure portalını kullanarak bir paylaşılan görüntü Galerisi oluşturma

A [paylaşılan görüntü Galerisi](shared-image-galleries.md) kuruluşunuz genelinde paylaşımı özel görüntü basitleştirir. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi dağıtım görevlerini bootstrap için kullanılabilir. 

Paylaşılan görüntü Galerisi özel VM görüntülerinizi başkalarıyla kuruluşunuzdaki içinde veya bölgeler, bir AAD kiracısı arasında paylaşmanıza olanak tanır. Paylaşmak istediğiniz hangi görüntüleri seçin, bunları bulunan ve kendileriyle paylaşmak istediğiniz yapmak istediğiniz hangi bölgeleri. Paylaşılan görüntülerini mantıksal olarak gruplayabilirsiniz, böylece birden fazla galeri oluşturabilirsiniz. 

Galeri tam rol tabanlı erişim denetimi (RBAC) sağlayan bir üst düzey bir kaynaktır. Görüntüleri tutulan olabilir ve farklı bir Azure bölgesi kümesi için her görüntü sürümü çoğaltmak seçebilirsiniz. Galeri, yalnızca yönetilen görüntüleri ile çalışır.

Paylaşılan görüntü Galerisi özelliği, birden çok kaynak türü vardır. Biz kullanarak veya kaldırılacak bunlar bu makaledeki oluşturma:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen bir görüntü** | Bu, tek başına kullanılan veya oluşturmak için kullanılan bir temel görüntü, bir **görüntü sürümü** bir görüntü galerisinde. Yönetilen bir görüntü genelleştirilmiş sanal makinelerinden oluşturulur. Yönetilen bir görüntü, birden çok sanal makine sağlamak için kullanılabilir ve artık paylaşılan görüntü sürümlerini oluşturmak için kullanılan VHD özel türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** yönetmek ve görüntüler, ancak kimlerin erişebildiğini siz denetlersiniz paylaşımı için bir depodur. |
| **Görüntü tanımı** | Görüntüleri bir galeri içindeki tanımlanır ve görüntü ve dahili olarak kullanma gereksinimleri hakkında bilgi Yürüt. Bu, görüntünün Windows veya Linux, sürüm notları ve minimum ve maksimum bellek gereksinimleri olup olmadığını içerir. Görüntü türü bir tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** bir galeri kullanırken bir VM oluşturmak için kullanın. Görüntünün birden çok sürümü, ortamınız için gerektiği şekilde olabilir. Yönetilen bir görüntü kullanırken gibi bir **görüntü sürümü** bir VM oluşturmak için görüntü sürümü sanal makine için yeni bir disk oluşturmak için kullanılır. Yansıma sürümü birden çok kez kullanılabilir. |


## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede örneği tamamlamak için var olan yönetilen bir görüntü olması gerekir. İzleyebileceğiniz [Öğreticisi: Azure PowerShell ile Azure VM'deki özel görüntüsünü oluşturma](tutorial-custom-images.md) gerekirse oluşturmak için. Yönetilen bir görüntü veri diski varsa, veri disk boyutu 1 TB'den fazla olamaz.

Bu makalede, üzerinden geçmeden değiştirdiğinizde kaynak grubu ve VM adlarını gerektiğinde.

 
[!INCLUDE [virtual-machines-common-shared-images-portal](../../../includes/virtual-machines-common-shared-images-portal.md)]

## <a name="create-vms-from-an-image"></a>Bir görüntüden VM oluşturma

Görüntü sürümü tamamlandıktan sonra bir veya daha fazla yeni VM'ler oluşturabilirsiniz. 

> [!IMPORTANT]
> Portal, başka bir azure kiracısı bir görüntüden bir VM dağıtmak için kullanamazsınız. Kiracılar arasında paylaşılan bir görüntüden VM oluşturma için kullanmanız gerekir [Azure CLI](shared-images.md#create-a-vm) veya [Powershell](../windows/shared-images.md#create-vms-from-an-image).


Bu örnek adlı bir VM oluşturur *Myımage*, *myResourceGroup* içinde *Doğu ABD* veri merkezi.

1. Görüntü sürümünüzün sayfasında **VM Oluştur** sayfanın üst kısmındaki menüden.
1. İçin **kaynak grubu**seçin **Yeni Oluştur** ve türü *myResourceGroup* adı.
1. İçinde **sanal makine adı**, türü *myVM*.
1. İçin **bölge**seçin *Doğu ABD*.
1. İçin **kullanılabilirlik seçeneklerini**, varsayılan değerini bırakın *gerekli altyapı artıklık*.
1. Değeri **görüntü** görüntü sürümü sayfasından başlattıysanız otomatik olarak doldurulması gerekir.
1. İçin **boyutu**kullanılabilir boyutları listesinden bir VM boyutu seçin ve "Seçin"'ye tıklayın.
1. Altında **yönetici hesabı**seçin **parola** veya **SSH ortak anahtarı**, ardından bilgilerinizi girin.
1. VM için uzaktan erişim altında izin vermek istiyorsanız **ortak gelen bağlantı noktası**, seçin **Seçili bağlantı noktalarına izin** seçip **SSH (22)** açılır listeden. VM için uzaktan erişime izin vermek istemiyorsanız bırakın **hiçbiri** için seçilen **ortak gelen bağlantı noktası**.
1. İşlemi tamamladığınızda, seçin **gözden geçir + Oluştur** sayfanın alt kısmındaki düğmesi.
1. VM doğrulama denetimini geçtikten seçin **Oluştur** dağıtımı başlatmak için sayfanın alt kısmındaki.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu ve **Sil**’i seçip silinecek kaynak grubunun adını onaylayın.

Tek tek kaynakları silmek isterseniz, bunları ters sırada silmeniz gerekir. Örneğin, bir görüntü tanımını silmek için tüm o görüntüden oluşturulan görüntü sürümlerini silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan görüntü Galerisi kaynak şablonlarını kullanarak da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan bir görüntü Galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan bir görüntü galerisinde bir görüntü tanımı oluşturun](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan bir görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resmi sürümden bir VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [genel bakış](shared-image-galleries.md). Sorun yaşarsanız bkz [sorun giderme paylaşılan resim galerileri](troubleshooting-shared-images.md).

