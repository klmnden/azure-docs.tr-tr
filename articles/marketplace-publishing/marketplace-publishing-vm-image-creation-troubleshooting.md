---
title: "VHD oluşturma sırasında sık karşılaşılan sorunları giderme | Microsoft Docs"
description: "Genel sorun giderme sorulara yanıtlar ve VHD oluşturma sırasında verir."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>VHD oluşturma sırasında karşılaşılan yaygın sorunları giderme
Bu makalede, Azure Marketi yayımcı ve/veya bir sorunla sahip olabilir veya sık sorulan sorular yayımlama veya kendi sanal makine solution(s) yönetme ortak yönetici yardımcı olmak için sağlanmıştır.

1. Ana bilgisayar adı nasıl değişiyor?
   
    VM oluşturulduktan sonra kullanıcılar ana bilgisayar adı güncelleştirilemiyor.
2. Uzak Masaüstü hizmetini veya oturum açma parolasını sıfırlamak nasıl?
   
   * [Windows VM için başvurusu](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Linux VM için başvurusu](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Yeni ssh sertifikalarını oluşturmak nasıl?
   
   Lütfen bağlantısına bakın: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Açık bir VPN sertifika ilkesi nasıl yapılandırılır?
   
   Lütfen bağlantısına bakın: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Microsoft Azure sanal makine ortamında (altyapı-bir hizmet olarak) Microsoft sunucusu yazılımını çalıştırmak için destek ilkesi nedir?
   
   Lütfen bağlantısına bakın: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Sanal makineler herhangi bir benzersiz tanımlayıcı var mı?
   
   Azure Azure VM benzersiz kimliği her VM kodlar. Ayrıntılar bu blog ve belgelerine bakın.
7. VM ile başlangıç görevi özel betik uzantısının nasıl yönetebilirim?
   
   Lütfen bağlantısına bakın: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Premium depolama alanına karşıya VHD kullanılarak Azure portalından bir VM oluşturmak nasıl?
   
   Bu özellik henüz desteklemiyoruz.
9. 32 bit bir uygulama içinde Azure Marketi destekleniyor mu?
   
   Bağlantı destek ilkesi hakkında ayrıntılar için lütfen: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. My VHD'leri bir görüntü oluşturmak çalışıyorum her zaman hata alma ". VHD zaten görüntü deposuyla kaynak olarak PowerShell'de kayıtlı". Ya da Azure üzerinde bu ada sahip herhangi bir görüntü buldunuz önce herhangi bir görüntü oluşturmadı. Bu nasıl giderebilirim?
    
    Bu genellikle durum bu VHD VM'den kullanıcı sağlanan ve bu VHD kilit ise. Lütfen bu VHD'den ayrılan hiçbir VM olup olmadığını denetleyin. Hata devam ederse, bu bağlantıyı kullanarak bir destek bileti Lütfen yükseltmek veya yayımlama portal ile ilgili bu (soru 11 yanıtında Ayrıntılar verilmiştir).