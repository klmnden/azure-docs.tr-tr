---
title: Görüntüleme ve ana bilgisayar adlarını değiştirme | Microsoft Docs
description: Nasıl görüntülemek ve Azure sanal makineler için konak adlarını değiştirmek için web ve çalışan rolleri için ad çözümlemesi
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 3fdb0f566789382a1606b19e4fac179f9ecf40cd
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122966"
---
# <a name="viewing-and-modifying-hostnames"></a>Görüntüleme ve ana bilgisayar adlarını değiştirme
Ana bilgisayar adına göre başvurulmak üzere rol örneklerinizin izin vermek için hizmet yapılandırma dosyasında her rol için konak adı için bir değer ayarlamanız gerekir. İstenen konak adına ekleyerek bunu **vmName** özniteliği **rol** öğesi. Değerini **vmName** özniteliği, her bir rol örneğinin ana bilgisayar adı için bir temel olarak kullanılır. Örneğin, varsa **vmName** olduğu *webrole* ve bu rolü üç örnekleri vardır, örneklerin ana bilgisayar adları olacaktır *webrole0*, *webrole1*, ve *webrole2*. Sanal makine adına dayalı bir sanal makine için konak adı doldurulduğundan yapılandırma dosyasında sanal makineler için bir ana bilgisayar adı belirtmeniz gerekmez. Bir Microsoft Azure hizmetini yapılandırma hakkında daha fazla bilgi için bkz. [Azure hizmet yapılandırma şemasına (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Ana bilgisayar adlarını görüntüleme
Aşağıdaki araçlardan herhangi birini kullanarak bir bulut hizmetinde sanal makineler ve rol örnekleri ana bilgisayar adlarını görüntüleyebilirsiniz.

### <a name="service-configuration-file"></a>Hizmet yapılandırma dosyası
Dağıtılan bir hizmet için hizmet yapılandırma dosyasını indirebilirsiniz **yapılandırma** Azure portalında hizmet dikey penceresi. Ardından arayabileceğiniz **vmName** özniteliğini **rol adı** ana bilgisayar adı görmek için öğesi. Bu ana bilgisayar adı için her bir rol örneği ana bilgisayar adını bir temel olarak kullanılan aklınızda bulundurun. Örneğin, varsa **vmName** olduğu *webrole* ve bu rolü üç örnekleri vardır, örneklerin ana bilgisayar adları olacaktır *webrole0*, *webrole1*, ve *webrole2*.

### <a name="remote-desktop"></a>Uzak Masaüstü
Uzak Masaüstü'nü (Windows), Windows PowerShell uzaktan iletişimini (Windows) veya sanal makine veya rol örnekleri için (Linux ve Windows) bir SSH bağlantısı etkinleştirdikten sonra ana bilgisayar adı etkin bir Uzak Masaüstü bağlantısından çeşitli şekillerde görüntüleyebilirsiniz:

* Komut istemi veya terminal SSH ana bilgisayar adı yazın.
* İpconfig yazın / (yalnızca Windows) tüm komut satırında.
* Bilgisayar adı, sistem ayarları (yalnızca Windows) görüntüleyin.

### <a name="azure-service-management-rest-api"></a>Azure Hizmet Yönetimi REST API'si
Bir REST istemcisinden bu yönergeleri izleyin:

1. Azure portalına bağlanmak için bir istemci sertifikası olduğundan emin olun. Bir istemci sertifikası almak için bölümünde verilen adımları izleyin. [nasıl yapılır: İndirme ve içeri aktarma yayımlama ayarları ve abonelik bilgilerini](https://msdn.microsoft.com/library/dn385850.aspx). 
2. X-ms-version değeri 2013-11-01 adlı bir üst bilgi girişi ayarlayın.
3. Şu biçimde bir istek gönderin: https:\//management.core.windows.net/\<subscrition kimliği\>/services/hostedservices/\<hizmet-adı\>? ekleme-detail = true
4. Aranan **HostName** her öğe **Roleınstance** öğesi.

> [!WARNING]
> Ayrıca iç etki alanı soneki için REST çağrısı yanıt bulut hizmetinizden denetleyerek görüntüleyebilirsiniz **InternalDnsSuffix** öğesi veya çalıştırarak ipconfig/tüm bir komut isteminden tarafından veya bir Uzak Masaüstü oturumunda (Windows) cat /etc/resolv.conf SSH terminal (Linux) çalışıyor.
> 
> 

## <a name="modifying-a-hostname"></a>Bir ana bilgisayar adı değiştirme
Değiştirilen hizmet yapılandırma dosyasını karşıya yükleme ya da bir Uzak Masaüstü oturumundan bilgisayarın yeniden adlandırılması, herhangi bir sanal makine veya rol örneği için konak adı değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Ad çözümleme (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure hizmet yapılandırma şeması (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network yapılandırma şeması](https://go.microsoft.com/fwlink/?LinkId=248093)

[Ağ yapılandırma dosyalarını kullanarak DNS ayarlarını belirleme](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

