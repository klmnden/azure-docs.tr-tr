---
title: Azure Marketi için VHD oluşturma (SSS) sırasında sık karşılaşılan sorunlar | Microsoft Docs
description: Sık sorulan VHD oluşturma ve ilişkili sorunları hakkında sorular.
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: Patrick.Butler
editor: ''
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 10/02/2018
ms.author: hascipio; v-divte; v-miclar
ms.openlocfilehash: 381f88c4641417bceca0f988d4b1a187aedaa642
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60744202"
---
# <a name="common-issues-during-vhd-creation-faq"></a>VHD oluşturma (SSS) sırasında sık karşılaşılan sorunlar

Aşağıdaki sık sorulan sorular (SSS) teklifleri için sanal makine (VM) oluşturma sırasında sanal sabit disk (VHD) ve sanal makine kapak sık karşılaşılan sorunları karşılaşıldı. 

## <a name="how-do-you-create-a-vm-from-the-azure-portal-using-the-vhd-that-is-uploaded-to-premium-storage"></a>Premium depolama alanına yüklenir VHD kullanılarak Azure portalından bir VM nasıl oluşturulur?

Azure Market, VM Teklifler oluşturma yönetilen depolama alanında bulunan görüntülerinden ya da Azure Premium depolama şu anda desteklemiyor.  Bu depolama seçenekleri hakkında daha fazla bilgi için bkz. [Azure yönetilen disklere genel bakış](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).


## <a name="can-you-use-generation-2-vms-for-offers"></a>2. kuşak Vm'lerde teklifleri için kullanabilir miyim?

Hayır, yalnızca nesil 1 VHD'ler desteklenir.  Ancak, şu anda 2. kuşak VM'ler için destek araştırmak için Microsoft Azure platformu ekibiyle birlikte çalışıyoruz.  Farklar hakkında daha fazla bilgi için bkz. [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)


## <a name="how-do-you-change-the-name-of-the-host"></a>Nasıl ana bilgisayar adı değişir mi?

Şunları yapamazsınız.  VM oluşturulduktan sonra kullanıcılar (sahipleri dahil olmak üzere) ana bilgisayar adı güncelleştirilemiyor.


## <a name="how-do-you-reset-the-remote-desktop-service-or-its-sign-in-password"></a>Uzak Masaüstü hizmetini veya oturum açma parolası nasıl sıfırlansın mı?

Aşağıdaki makaleler, Windows ve Linux tabanlı VM'ler için RDS ayarlarına sıfırlamanıza açıklanmaktadır:   

- [Windows VM’de Uzak Masaüstü hizmetini veya oturum açma parolasını sıfırlama](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
- [Bir Linux VM parolayı veya SSH anahtarı sıfırlama, SSH yapılandırmasını düzeltin ve VMAccess uzantısını kullanarak disk tutarlılık denetimi hakkında](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)


## <a name="how-do-you-generate-new-ssh-certificates"></a>Yeni SSH sertifikaları nasıl oluşturduğunuz?

Sertifikaların oluşturulmasını makalesinde açıklanan [Get VM görüntünüz için paylaşılan erişim imzası URI'si](./cpp-get-sas-uri.md) sonraki bölümdeki [teknik varlıkları için bir VM teklifi oluşturma](./cpp-create-technical-assets.md).


## <a name="how-do-you-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Bir sanal özel ağ (VPN) Vm'lerimi ile çalışmaya nasıl yapılandırırım?

Ardından Azure Resource Manager dağıtım modelini kullanıyorsanız, VPN ayarlama, genel üç seçeneğiniz vardır:
- [Azure portalını kullanarak rota temelli VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-portal)
- [PowerShell kullanarak rota temelli VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-powershell)
- [CLI kullanarak rota temelli VPN ağ geçidi oluşturma](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-cli)


## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Azure tabanlı VM'ler üzerinde Microsoft sunucu yazılımları çalıştırmak için Microsoft destek ilkeleri nelerdir?

Bu destek ilkeleri makalesinde ayrıntılı [Microsoft Azure sanal makineler için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>Sanal makineler ilişkili benzersiz tanımlayıcıları mı?

Evet, Azure üzerinde barındırılıyorsa.  Azure, Azure sanal makine benzersiz kimliği oluşturulan her yeni VM kaynağı benzersiz bir tanımlayıcı atar.  Daha fazla bilgi için blog gönderisini okuyun [Azure sanal makine benzersiz kimliği](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/).  Bu tanımlayıcı üzerinden programlı olarak da edinebilirsiniz [listesi API'sini](https://docs.microsoft.com/rest/api/compute/virtualmachines/list).


## <a name="in-a-vm-how-do-you-manage-the-custom-script-extension-in-the-startup-task"></a>Bir VM'de, nasıl özel betik uzantısı'nda başlangıç görevinin yönetebilirsiniz?

Aşağıdaki makalede Azure PowerShell modülü, Azure Resource Manager şablonları ve sorun giderme adımları Windows sistemlerinde ayrıntıları kullanarak özel betik uzantısı kullanma işlemi açıklanmaktadır: [Windows için özel betik uzantısı](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)


## <a name="are-32-bit-applications-or-services-supported-in-the-azure-marketplace"></a>32-bit uygulamalar veya hizmetler Azure Marketi'nde desteklenen misiniz?

Genellikle yapamazsınız.  Azure sanal makineler için standart Hizmetleri ve desteklenen işletim sistemlerinin 64 bit.  Ancak, teknik açıdan, uygulamaların geriye dönük uyumluluk için 32 bit sürümlerini çalıştıran en çok 64-bit işletim sistemlerini destekler.  VM çözümünüzün bir parçası desteklenmez ve bu nedenle 32-bit uygulamaları ancak kullanın *yüksek oranda önerilmez*.  Bunun yerine, uygulamanızın bir 64-bit proje olarak yeniden derleyin.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [32-bit uygulamaları çalıştırma](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications)
- [Azure sanal makineler'de 32-bit işletim sistemleri için destek](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)


## <a name="every-time-i-try-to-create-an-image-from-my-vhds-i-get-the-error-vhd-is-already-registered-with-image-repository-as-the-resource-in-powershell-i-did-not-create-any-image-before-nor-did-i-find-any-image-with-this-name-in-azure-how-do-i-resolve-this-issue"></a>My Vhd'lerden bir görüntü oluşturmak çalıştığım her zaman şu hatayı alır `.VHD is already registered with image repository as the resource` PowerShell'de. Önce herhangi bir görüntü oluşturmadı ya da bu ada sahip herhangi bir görüntü Azure'da buldunuz. Bu sorunu nasıl giderebilirim?

Kullanıcı bir VM üzerinde bir kilit sahip bir VHD'den sağladıysa Bu sorun genellikle oluşur.  Bu VHD'den ayrılan hiçbir VM olduğundan emin olun ve sonra işlemi yeniden deneyin.  Bu sorun devam ederse bir destek bileti açıklandığı şekilde açın [bulut iş ortağı portalı için desteği](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-support-for-cloud-partner-portal). 

