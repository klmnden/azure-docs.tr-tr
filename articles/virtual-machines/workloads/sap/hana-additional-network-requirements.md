---
title: (Büyük örnekler) Azure üzerinde SAP HANA için ek donanım gereksinimleri | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da ek ağ gereksinimleri.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 953ee5d40a3a4c49d7cc01de804ae5c76ceedc7a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709790"
---
# <a name="additional-network-requirements-for-large-instances"></a>Büyük örnekler için ek donanım gereksinimleri

Azure'da SAP HANA büyük örnekleri dağıtımının bir parçası olarak ek ağ gereksinimleri olabilir.

## <a name="add-more-ip-addresses-or-subnets"></a>Daha fazla IP adresleri veya alt ağları Ekle

Daha fazla IP adresleri veya alt eklediğinizde, Azure portalı, PowerShell veya Azure CLI'yı kullanın.

Yeni IP adresi aralığı, yeni bir toplu aralığı oluşturmak yerine sanal ağ adres alanına yeni bir aralık olarak ekleyin. Bu değişiklik Microsoft'a gönderin. Bu, yeni IP adres aralığından HANA büyük örnek istemciniz birimleriyle bağlanmanıza olanak sağlar. Eklenen yeni sanal ağ adres alanı almak için bir Azure destek isteği açabilirsiniz. Onay aldıktan sonra sonraki adımları gerçekleştirin.

Azure portalından ek bir alt ağ oluşturmak için bkz [Azure portalını kullanarak bir sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network). Powershell'den oluşturmak için bkz: [PowerShell kullanarak sanal ağ oluşturma](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-virtual-network).

## <a name="add-virtual-networks"></a>Sanal ağları Ekle

Bir veya daha fazla Azure sanal ağları ilk kez bağladıktan sonra SAP HANA (büyük örnekler) azure'da erişim bulunmakla bağlanmak isteyebilirsiniz. İlk olarak, bir Azure destek isteği gönderin. Bu istekte belirli Azure dağıtım tanımlayan bilgiler içerir. Ayrıca, Azure sanal ağ adres alanının aralıkları ve IP adres alanı aralığı içerir. Microsoft hizmet yönetimi üzerinde SAP HANA sonra ek sanal ağlar ve Azure ExpressRoute bağlanmak için gereken bilgileri sağlar. Her sanal ağ için HANA büyük örnekler için ExpressRoute bağlantı hattına bağlamak için bir benzersiz yetkilendirme anahtarı gerekir.

## <a name="increase-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini artırın

Microsoft hizmet yönetimi üzerinde SAP HANA başvurun. SAP hana (büyük örnekler) Azure ExpressRoute bağlantı hattı üzerinde bant genişliğini artırmanız bildirmek, bir Azure destek isteği oluşturun. (Bir tek bağlantı hattı bant genişliğini en çok 10 GB/sn için bir artışı isteyebilirsiniz.) İşlem tamamlandıktan sonra bildirim sonra alırsınız; Bu daha yüksek hız azure'da etkinleştirmek için başka bir şey yapmanız gerekmez.

## <a name="add-an-additional-expressroute-circuit"></a>Ek bir ExpressRoute bağlantı hattı ekleyin

Microsoft hizmet yönetimi üzerinde SAP HANA başvurun. Ek bir ExpressRoute bağlantı hattı eklemek için öneri (yeni bağlantı hattına bağlamak için yetkilendirme bilgileri alma isteği dahil) bir Azure destek isteği oluşturun. İsteği yapmadan önce sanal ağlarda kullanılan adres alanını tanımlamalısınız. Microsoft hizmet yönetimi üzerinde SAP HANA ardından yetkilendirme sağlar.

Yeni devreyi oluşturulur ve Microsoft Hizmet Yönetimi yapılandırma üzerinde SAP HANA tamamlandıktan sonra devam etmeniz için gereken bilgileri içeren bir bildirimi alırsınız. Bunlar zaten başka bir SAP HANA (büyük örnek) Azure ExpressRoute bağlantı hattı aynı Azure bölgesinde bağlıysa bu ek bağlantı hattı için Azure sanal ağları bağlamak mümkün değildir.

## <a name="delete-a-subnet"></a>Bir alt ağı Sil

Bir sanal ağ alt ağı kaldırmak için Azure portalı, PowerShell veya Azure CLI'yı kullanabilirsiniz. Azure sanal ağı IP adres aralığı veya adres alanınızı toplu bir aralık varsa, Microsoft ile sizin için hiçbir izleme yoktur. (Ancak, sanal ağ hala silinmiş alt içerir BGP rota adres alanı yayma unutmayın.) Hangi biri silinmiş alt ağınız için atanmış birden çok IP adresi aralıklarını olarak, Azure sanal ağ adres aralığı veya adres alanı tanımlamış olabilirsiniz. Bu sanal ağ adresi alanınızdan silmek istediğinizden emin olun. Ardından, SAP HANA (büyük örnekler) Azure üzerinde SAP HANA ile iletişim kurmak için izin verilen aralıklar kaldırmak için Microsoft Hizmet Yönetimi bildirin.

Daha fazla bilgi için [bir alt ağını silmek](../../../virtual-network/virtual-network-manage-subnet.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-subnet).

## <a name="delete-a-virtual-network"></a>Bir sanal ağı silme

Bilgi için [bir sanal ağı silme](../../../virtual-network/manage-virtual-network.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#delete-a-virtual-network).

Microsoft hizmet yönetimi üzerinde SAP HANA, SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı üzerinde mevcut yetkilendirmeleri kaldırır. Ayrıca, Azure sanal ağı için IP adresi aralığı veya adres alanını HANA büyük örnekleri ile iletişimi kaldırır.

Sanal ağ kaldırdıktan sonra kaldırılacak aralıkları ve IP adres alanı aralığı sağlamak için Azure destek isteği açın.

Her şeyi kaldırmak emin olmak için ExpressRoute bağlantısı, sanal ağ geçidi, sanal ağ geçidi genel IP ve sanal ağ'ı silin.

## <a name="delete-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattını Sil

Ek bir SAP HANA (büyük örnekler) Azure ExpressRoute bağlantı hattı üzerinde kaldırmak için Microsoft hizmet yönetimi üzerinde SAP HANA ile bir Azure destek isteği açın. Devrenin silinmesini isteyin. Azure aboneliğinde, silebilir veya gerektiği şekilde sanal ağ tutun. Ancak, HANA büyük örnekler ExpressRoute bağlantı hattına bağlı sanal ağ geçidi arasındaki bağlantı silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [SAP HANA'yı (büyük örnekler) Azure'a yükleme ve yapılandırma](hana-installation.md)
