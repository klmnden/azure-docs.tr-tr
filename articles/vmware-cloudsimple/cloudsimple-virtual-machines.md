---
title: VMware çözümüyle CloudSimple - Azure sanal makinelerine genel bakış
description: CloudSimple sanal makineler ve avantajları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 59f5438bbedea2ff7793a5df95f1d3df58b9bba6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576993"
---
# <a name="cloudsimple-virtual-machines-overview"></a>CloudSimple sanal makinelere genel bakış

CloudSimple VMware Vm'lerini Azure portalından yönetmenize olanak sağlar.  Bir küme veya bir kaynak havuzu vSphere kümenizden Azure aboneliğinize eşleme tarafından yönetilir.  CloudSimple sanal makine Self Servis Yönetimi VMware vm'lerinin Azure portalından getirir.  

Azure'dan CloudSimple VM oluşturmak için bir VM şablonu, özel bulut vCenter mevcut olması gerekir.  Şablon, uygulamalar ve işletim sistemini özelleştirmek için kullanılır.  VM şablonu, Kurumsal güvenlik ilkeleri karşılamak için sağlamlaştırılmış.  Sanal makineler oluşturmak ve bunları bir Self Servis modeli kullanılarak Azure portalından kullanmak için bir şablon kullanabilirsiniz.

## <a name="benefits"></a>Avantajlar

Azure portalından CloudSimple sanal makineler oluşturmak ve VMware sanal makinelerini yönetmek kullanıcılar için Self Servis bir mekanizma sağlar.

* Özel bulut vCenter CloudSimple VM oluşturma
* VM özelliklerini yönetme
  * Diskleri ekleme/kaldırma
  * NIC Ekle/Kaldır
* Sanal makinenizin CloudSimple Power işlemleri
  * Açma ve kapatma
  * VM'yi Sıfırla
* VM silme

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* Bilgi nasıl [Azure aboneliğinize eşleme](https://docs.azure.cloudsimple.com/azuresubscriptionmapping/)