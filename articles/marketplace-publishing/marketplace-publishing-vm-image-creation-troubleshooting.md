---
title: VHD oluşturma işlemi sırasında sık karşılaşılan sorunları giderme | Microsoft Docs
description: Genel sorun giderme soruları ve sorunları VHD oluşturma sırasında sorularınızın yanıtları.
services: Azure Marketplace
documentationcenter: ''
author: v-miclar
manager: ''
editor: ''
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: e58a5f8901b2ab53d47095e1d5fd232783e3b1ba
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079145"
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>VHD oluşturma sırasında karşılaşılan yaygın sorunları giderme
Bu makalede, Azure Market yayımcısı ve/veya bir sorununuz sahip olabilir veya sık sorulan sorular yayımlama veya kendi sanal makine çözümleri yönetme Abonelikteki yardımcı olmak için sağlanmıştır.

1. Ana bilgisayar adı nasıl değiştirebilirim?
   
    VM oluşturulduktan sonra kullanıcılar, ana bilgisayar adı güncelleştirilemiyor.
2. Uzak Masaüstü hizmetini veya oturum açma parolasını sıfırlama nasıl?
   
   * [Windows VM için başvuru](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Linux VM için başvuru](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Yeni ssh sertifikalarını oluşturmak nasıl?
   
   Bağlantı bakın: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Açık bir VPN sertifikası yapılandırmak nasıl?
   
   Bağlantı bakın: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Microsoft sunucu yazılımı Microsoft Azure sanal makine ortamında (hizmet olarak altyapı-a-) çalıştırmaya yönelik destek ilkesi nedir?
   
   Bağlantı bakın: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Herhangi bir benzersiz tanımlayıcı sanal makineler var mı?
   
   Azure, Azure VM benzersiz kimliği, her VM'deki kodlar. Bu blog ve belgeleri ayrıntılara bakın.
7. Bir VM ile özel betik uzantısı'nda başlangıç görevinin nasıl yönetebilirim?
   
   Bağlantı bakın: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Premium depolama alanına yüklenir VHD kullanılarak Azure portalından bir VM oluşturmak nasıl?
   
   Bu özellik henüz desteklemiyoruz.
9. 32 bitlik bir uygulamayı Azure Marketi'nde desteklenir?
   
   Destek İlkesi hakkında daha fazla ayrıntı için bağlantıya bakın: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. My Vhd'lerden bir görüntü oluşturmak getirmeye çalışıyorum her zaman şu hatayı alır ". VHD zaten ile görüntü deposuna kaynak olarak PowerShell'de kayıtlı". Önce herhangi bir görüntü oluşturmadı ya da bu ada sahip herhangi bir görüntü Azure'da buldunuz. Bu sorunu nasıl giderebilirim?
    
    Bu sorun genellikle kullanıcı bu VHD bir VM'den sağlandı ve bu VHD'de kilit ise oluşur. Bu VHD'den ayrılan hiçbir VM olup olmadığını denetleyin. Hata devam ederse, ardından olursa bu bağlantıyı kullanarak bir destek bileti veya yayımlama portalı (11 soru yanıt ayrıntıları verilmiştir).