---
title: "Azure Portalı'nı kullanarak bir sanal makine ölçek kümesi oluşturma | Microsoft Docs"
description: "Azure portalını kullanarak ölçek kümeleri dağıtın."
keywords: "Sanal makine ölçekleme kümeleri"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc52368a1a14ad094e7ab9180b90a9aa4ccdb6d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Azure portal ile bir sanal makine ölçek kümesi oluşturma
Bu öğretici bir sanal makine ölçek kümesi yalnızca birkaç dakika içinde Azure portalını kullanarak oluşturmak için ne kadar kolay olduğunu gösterir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Marketten VM görüntüsünü seçin
Portaldan bir ölçeği CentOS, CoreOS, Debian, Ubuntu Server, diğer Linux görüntüler ve Windows Server görüntülerini ile Ayarla kolayca dağıtabilirsiniz.

İlk olarak, gitmek [Azure portal](https://portal.azure.com) bir web tarayıcısında. Tıklatın **yeni**, arama **ölçek kümesini**ve ardından **sanal makine ölçek kümesi** girişi:

![portal arama Azure sanal makine ölçek kümesi](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>Ölçek kümesini oluşturma
Şimdi varsayılan ayarları kullanmak ve hızla ölçek kümesi oluşturun.

* Ölçek kümesi için bir ad girin.  
Bu ad ölçek kümesini önünde yük dengeleyici FQDN'sini tabanı olur, nedenle adının tüm Azure arasında benzersiz olduğundan emin olun.

* İstenen işletim sistemi türünüzü seçin.

* İstenen kullanıcı adınızı girin ve tercih ettiğiniz hangi kimlik doğrulama türünü seçin.  
Bir parola seçerseniz, en az 12 karakter uzunluğunda ve üç dışında dört aşağıdaki karmaşıklık gereksinimlerini karşılayacak olmalıdır: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. [Kullanıcı adı ve parola gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) hakkında daha fazla bilgi edinin. Seçerseniz **SSH ortak anahtarını**, yalnızca emin Yapıştır ortak anahtarınız *değil* özel anahtarınızı:

* Seçin **Evet** veya **Hayır** için **100 örnekleri ölçeklendirmeyi etkinleştirmek**.  
Evet, Ölçek ayarlarsanız, birden çok yerleştirme grupları üzerinden yayılabilir. Daha fazla bilgi için bkz: [bu belgeleri](./virtual-machine-scale-sets-placement-groups.md).

* Uygun bir seçtiğinizden emin olun **örnek boyutu**.  
Sanal makine boyutları hakkında daha fazla bilgi için bkz: [Windows VM boyutları](..\virtual-machines\windows\sizes.md) veya [Linux VM boyutları](..\virtual-machines\linux\sizes.md).

* İstenen kaynak grubu adı ve konumu girin.  
Varsa bölgeniz ve **örnek boyutu** kullanılabilirlik bölgeleri destekler **kullanılabilirlik bölgeleri** alan etkindir. Bu kullanılabilirlik bölgeler hakkında daha fazla bilgi için bkz [genel bakış](../availability-zones/az-overview.md) makalesi.

* İstenen etki alanı adı etiketi (temel ölçek kümesini önünde yük dengeleyici için FQDN) girin.  
Bu etiketi tüm Azure arasında benzersiz olması gerekir.

* İstenen işletim sistemi disk görüntüsü, örnek sayısı ve makine boyutu seçin.

* İstenen disk türünü seçin: yönetilen veya yönetilmeyen.  
Daha fazla bilgi için bkz: [bu belgeleri](./virtual-machine-scale-sets-managed-disks.md). Ölçeği Ayarla seçtiyseniz, birden çok yerleştirme grupları span, yönetilen disk yerleştirme grupları kapsanacak ölçek kümeleri için gerekli olmadığından bu seçenek kullanılamaz.

* Etkinleştirin veya otomatik ölçeklendirme devre dışı bırakın ve etkin olmadığını yapılandırın.

![Azure sanal makine ölçek kümesi portal oluşturma komut istemi](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>VM ölçek kümesi ile bağlanma
Bir tek yerleştirme gruba ayarlamak, Ölçek sınırlamak seçerseniz, sonra ölçek kümesi ile kolayca ayarlamanıza ölçek bağlanmanıza olanak (Aksi halde, bir jumpbox aynı oluşturmak, büyük olasılıkla gerek ölçek kümesindeki sanal makinelere bağlanmak için yapılandırılmış NAT kuralları dağıtılır  sanal ağ ölçek kümesi olarak). Bunları görmek için gidin `Inbound NAT Rules` ölçek kümesi için yük dengeleyici sekmesinde:

![Azure sanal makine ölçek kümesi portal nat kuralları](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

Bu NAT kuralları kullanılarak ayarlanan ölçeğinde her bir VM bağlanabilir. Gelen bağlantı noktasını 50000, NAT kuralı ise örneği için bir Windows ölçek kümesi için RDP aracılığıyla bu makine üzerinde bağlanamadı `<load-balancer-ip-address>:50000`. Linux ölçek kümesi için komutu kullanarak bağlan `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Sonraki adımlar
CLI üzerinden belgeleri nasıl ölçek dağıtılacağı ayarlar için bkz: [bu belgeleri](virtual-machine-scale-sets-cli-quick-create.md).

Ölçek dağıtma belgelerine Powershell'den ayarlar için bkz: [bu belgeleri](virtual-machine-scale-sets-windows-create.md).

Visual Studio'dan belgeleri nasıl ölçek dağıtılacağı ayarlar için bkz: [bu belgeleri](virtual-machine-scale-sets-vs-create.md).

Genel belgeler için kullanıma [belgelerine genel bakış sayfasında ölçek kümeleri için](virtual-machine-scale-sets-overview.md).

Genel bilgi için kullanıma [ölçek kümeleri için ana giriş sayfasının](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

