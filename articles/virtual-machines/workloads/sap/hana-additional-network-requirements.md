---
title: (Büyük örnekler) Azure üzerinde SAP HANA için ek donanım gereksinimleri | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da ek ağ gereksinimleri.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ee89553846309bd96f3cad3648e337488cf07d3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164764"
---
# <a name="additional-network-requirements"></a>Ek ağ gereksinimleri

Bazen HANA büyük örnekleri dağıtımının bir parçası ek ağ gereksinimlerine sahip olabilir. Bu makalede, aşağıdaki ağ gereksinimlerini gösterir:
- [Daha fazla IP adresleri veya alt ağ ekleme ](#adding-more-ip-addresses-or-subnets)
- [Sanal vetworks ekleme](#adding-vnets)
- [Express route bağlantı hattı bant genişliğini artırma](#increasing-expressroute-circuit-bandwidth)
- [Bir ek express route bağlantı hattı ekleme](#adding-an-additional-expressroute-circuit)
- [Bir alt ağ siliniyor](#deleting-a-subnet)
- [Bir sanal ağ siliniyor](#deleting-a-vnet)
- [Bir express yönlendirici bağlantı hattı siliniyor](#deleting-an-expressroute-circuit)


## <a name="adding-more-ip-addresses-or-subnets"></a>Daha fazla IP adresleri veya alt ekleme

Azure portalı, PowerShell veya CLI ekleyerek daha fazla IP adresleri veya alt ağlar'ı kullanın.

Bu durumda, yeni IP adresi aralığı yeni aralığı olarak toplanmış olan yeni bir aralık oluşturmak yerine VNet adres alanı eklemek için kullanılması önerilir. Her iki durumda da, bu değişikliği istemcinizde, HANA büyük örneği birimleri için yeni IP adresi aralığı dışına bağlantı izin vermek için Microsoft'a göndermeniz gerekir. Eklenen yeni bir VNet adres alanı almak için bir Azure destek isteği açabilirsiniz. Onay aldıktan sonra sonraki adımları gerçekleştirin.

Azure portalından ek bir alt ağ oluşturmak için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network)ve PowerShell'de oluşturmak için bkz: [PowerShell kullanarak sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="adding-vnets"></a>Sanal ağlar ekleme

Bir veya daha fazla Azure sanal ağları ilk kez bağladıktan sonra SAP HANA (büyük örnekler) azure'da erişim bulunmakla eklemek isteyebilirsiniz. İlk olarak, bir Azure destek isteği gönderin, bu istekte her ikisi de belirli Azure dağıtım ve IP adresi alanı aralıklarının Azure VNet adres alanının tanımlanması belirli bilgiler içerir. Azure hizmet yönetimi üzerinde SAP HANA sonra ek sanal ağlar ve ExpressRoute bağlanmak için gereken bilgileri sağlar. Her sanal ağ için HANA büyük örnekler için ExpressRoute bağlantı hattına bağlamak için benzersiz bir yetkilendirme anahtarı gerekir.

Yeni bir Azure sanal ağı eklemek için adımları:

1. Ekleme işlemi, ilk adımda tamamına bakın _oluşturarak Azure sanal ağı_ bölümü.
2. Ekleme işlemi, ikinci adımda tamamına bakın _ağ geçidi alt ağı oluşturma_ bölümü.
3. HANA büyük örneği ExpressRoute işlem hattına ek sanal ağlarınıza bağlanmak için yeni sanal ağ hakkında bilgi içeren bir Azure destek isteği açın ve yeni yetkilendirme anahtar isteyin.
4. Bildirim yetkilendirme tamamlandıktan sonra üçüncü tamamlamak için Microsoft tarafından sağlanan yetkilendirme bilgileri adım ekleme işlemi konusunda bkz _sanal ağları bağlama_ bölümü.

## <a name="increasing-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini artırma

Azure hizmet yönetimi üzerinde SAP HANA başvurun. SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı bant artırmak için önerilir, Azure destek isteği oluşturun. (Bir tek bağlantı hattı bant genişliğini en çok 10 GB/sn için bir artışı isteyebilirsiniz.) İşlem tamamlandıktan sonra bildirim sonra alırsınız; Bu daha yüksek hız azure'da etkinleştirmek için gereken başka bir işlem.

## <a name="adding-an-additional-expressroute-circuit"></a>Ek bir ExpressRoute bağlantı hattı ekleme

Ek bir ExpressRoute bağlantı hattı, bir Azure destek isteği yeni bir ExpressRoute bağlantı hattı oluşturmak için (ve buna bağlanmak için yetkilendirme bilgileri almak için) yapmak gerektiğini kopyaladınız Azure hizmet yönetimi üzerinde SAP HANA ile danışın. Adres alanını kontrol edecek kullanılabilir sanal ağlar, yetkilendirme sağlamak için Azure hizmet yönetimi üzerinde SAP HANA için sırada isteği yapmadan önce tanımlanmalıdır.

Yeni devreyi oluşturulur ve Azure Hizmet Yönetimi yapılandırma üzerinde SAP HANA tamamlandıktan sonra devam etmeniz için gereken bildirim bilgileri alma oluşturacaksınız. Oluşturma ve bu ek bağlantı hattına yeni Vnet'in bağlanmak için yukarıda verilen adımları izleyin. Bunlar zaten başka bir SAP HANA (büyük örnek) Azure ExpressRoute bağlantı hattı aynı Azure bölgesindeki bağlıysa bu ek bağlantı hattına Azure Vnet'leri bağlamak mümkün değildir.

## <a name="deleting-a-subnet"></a>Bir alt ağ siliniyor

Bir sanal ağ alt ağı kaldırmak için Azure portalı, PowerShell veya CLI kullanılabilir. Azure sanal ağ IP adresi aralığı/Azure VNet adres alanı toplu bir aralığı olan durumunda, Microsoft ile sizin için hiçbir izleme yoktur. VNet hala adres alanları BGP rota yayma dışında silinen bir alt ağ içerir. Azure sanal ağ IP adresi aralığı/Azure VNet adres alanının hangi biri silinmiş alt ağınız için atanmış birden çok IP adresi aralıklarını olarak tanımlanmışsa, VNet adres alanınızı dışında silmeli ve daha sonra Azure hizmet yönetimi için üzerinde SAP HANA bildirin SAP HANA (büyük örnekler) azure'da ile iletişim kurmak için izin verilen aralıklar kaldırın.

Bir alt ağını silmek için bkz: [bir alt ağını silmek](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet) alt ağ oluşturma hakkında daha fazla bilgi.

## <a name="deleting-a-vnet"></a>Bir sanal ağ siliniyor

Bir sanal ağı silmek için bkz: [bir sanal ağı silme](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network). Azure hizmet yönetimi üzerinde SAP HANA, SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı üzerinde mevcut yetkilendirmeleri kaldırır ve Azure sanal ağ IP adresi aralığı/Azure HANA büyük örnekleri ile iletişim için VNet adres alanı kaldırın.

VNet kaldırıldıktan sonra kaldırılacak IP adresi alanı aralıklarının sağlamak için Azure destek isteği açın.

Her şeyi kaldırıldığından emin olmak için aşağıdaki öğeleri silin:

- ExpressRoute bağlantısı, sanal ağ geçidi, sanal ağ geçidi genel IP ve sanal ağ

## <a name="deleting-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattı siliniyor

Ek bir SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı üzerinde kaldırmak için Azure hizmet yönetimi üzerinde SAP HANA ile bir Azure destek isteği açın ve istek bağlantı hattını silinmesi gerekir. Azure aboneliğinde, silebilir veya gerektiği şekilde VNet tutun. Ancak, HANA büyük örnekleri ExpressRoute bağlantı hattına bağlı sanal ağ geçidi arasındaki bağlantı silmeniz gerekir.

Ayrıca bir VNet kaldırmak istiyorsanız, yukarıdaki bölümde VNet silmeye ilişkin yönergeleri izleyin.

**Sonraki adımlar**

- Başvuru [yüklemek ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırmak nasıl](hana-installation.md).
