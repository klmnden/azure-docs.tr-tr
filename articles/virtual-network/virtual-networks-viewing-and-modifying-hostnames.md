---
title: "Görüntüleme ve ana bilgisayar adları değiştirme | Microsoft Docs"
description: "Nasıl görüntülemek ve Azure sanal makineleri için ana bilgisayar adlarını değiştirmek, web ve çalışan rolleri ad çözümlemesi için"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 9a3a1e1b58dcb828e2d2d09c18f1aab6d46051aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="viewing-and-modifying-hostnames"></a>Görüntüleme ve ana bilgisayar adları değiştirme
Ana bilgisayar adına göre başvurulacak rolü örneklerinizi izin vermek için her bir rol hizmeti yapılandırma dosyasında ana bilgisayar adı değeri ayarlamanız gerekir. İstenen konak adına ekleyerek bunu **vmName** özniteliği **rol** öğesi. Değeri **vmName** özniteliği her rol örneği ana bilgisayar adı için temel olarak kullanılır. Örneğin, varsa **vmName** olan *webrole* ve bu rol üç örneği vardır, ana bilgisayar adlarını örneklerinin olacaktır *webrole0*, *webrole1*, ve *webrole2*. Sanal makine adına dayalı bir sanal makine için konak adı doldurulmuş için sanal makineler için bir konak adı yapılandırma dosyasında belirtmek gerekmez. Bir Microsoft Azure hizmet yapılandırma hakkında daha fazla bilgi için bkz: [Azure hizmet yapılandırma şeması (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Ana bilgisayar adları görüntüleme
Sanal makineler ve rol örnekleri ana bilgisayar adlarını aşağıdaki araçlardan birini kullanarak bir bulut hizmetinde görüntüleyebilirsiniz.

### <a name="azure-portal"></a>Azure Portal
Kullanabileceğiniz [Azure portal](http://portal.azure.com) genel bakış dikey penceresinde bir sanal makine için sanal makineleri ana bilgisayar adlarını görüntülemek için. Dikey için bir değer gösterir göz önünde bulundurmanız **adı** ve **ana bilgisayar adı**. Bunlar başlangıçta aynı olsa da, ana bilgisayar adının değiştirilmesi sanal makine veya rol örneği adı değişmez.

Rol örneklerine de Azure portalında görüntülenebilir, ancak bir bulut hizmeti örnekleri listelediğinizde, ana bilgisayar adı görüntülenmez. Her örneği için bir ad görürsünüz, ancak bu ad ana bilgisayar adını temsil etmiyor.

### <a name="service-configuration-file"></a>Hizmet yapılandırma dosyası
Dağıtılan bir hizmet için hizmet yapılandırma dosyası indirebilirsiniz **yapılandırma** Azure portalında hizmetin dikey. Ardından arayabileceğiniz **vmName** için öznitelik **rol adı** ana bilgisayar adı görmek için öğesi. Bu ana bilgisayar adı, her rol örneği ana bilgisayar adı için temel olarak kullanıldığını aklınızda bulundurun. Örneğin, varsa **vmName** olan *webrole* ve bu rol üç örneği vardır, ana bilgisayar adlarını örneklerinin olacaktır *webrole0*, *webrole1*, ve *webrole2*.

### <a name="remote-desktop"></a>Uzak Masaüstü
Uzak Masaüstü'nü (Windows), Windows PowerShell uzaktan iletişimini (Windows) ya da sanal makineleri veya rol örneklerini (Linux ve Windows) SSH bağlantısını etkinleştirdikten sonra etkin bir Uzak Masaüstü bağlantı ana bilgisayar adından çeşitli şekillerde görüntüleyebilirsiniz:

* Komut istemi veya SSH terminal ana bilgisayar adını yazın.
* (Yalnızca Windows) tüm komut satırında/ipconfig yazın.
* Bilgisayar adı, sistem ayarları (yalnızca Windows) görüntüleyin.

### <a name="azure-service-management-rest-api"></a>Azure Hizmet Yönetimi REST API'si
Bir REST istemciden aşağıdaki yönergeleri izleyin:

1. Azure Portalı'na bağlanmak için bir istemci sertifikası olduğundan emin olun. Bir istemci sertifikası edinmek için sunulan adımları izleyin [nasıl yapılır: indirme ve yayımlama ayarları içeri aktarma ve abonelik bilgileri](https://msdn.microsoft.com/library/dn385850.aspx). 
2. X-ms-version değeri 2013-11-01 adlı bir üstbilgi girişi ayarlayın.
3. Aşağıdaki biçimde bir istek gönderin: https://management.core.windows.net/\<subscrition kimliği\>/services/hostedservices/\<hizmet adı\>? katıştırmak ayrıntı = true
4. Ara **ana bilgisayar adı** öğesini her **RoleInstance** öğesi.

> [!WARNING]
> Ayrıca iç etki alanı soneki bulut hizmetinizden REST çağrısı yanıt için denetleyerek görüntüleyebilirsiniz **InternalDnsSuffix** öğesi veya ipconfig çalıştırarak/tüm bir komut isteminden kullanarak veya bir Uzak Masaüstü oturumunda (Windows) cat /etc/resolv.conf bir SSH terminal (Linux) çalışıyor.
> 
> 

## <a name="modifying-a-hostname"></a>Bir ana bilgisayar adını değiştirme
Değiştirilen hizmet yapılandırma dosyası karşıya ya da Uzak Masaüstü oturumu bilgisayardan yeniden adlandırma herhangi bir sanal makine veya rol örneği için ana bilgisayar adını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Ad çözümlemesi (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure hizmet yapılandırma şeması (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network yapılandırma şeması](http://go.microsoft.com/fwlink/?LinkId=248093)

[Ağ yapılandırma dosyalarını kullanarak DNS ayarlarını belirtin](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

