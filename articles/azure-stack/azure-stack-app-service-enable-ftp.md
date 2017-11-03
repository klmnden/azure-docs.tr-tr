---
title: "FTP Azure yığın uygulama hizmeti etkinleştirme | Microsoft Docs"
description: "FTP Azure yığın uygulama hizmeti etkinleştirmeyi tamamlamak için adımlar"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: erikje
ms.openlocfilehash: 9cadc57831ac7f7e5d32b10a4a87dab3fac02958
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-ftp-in-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service’te FTP’yi etkinleştirme

Böylece kiracılarınız içerik ve uygulama dosyalarını karşıya yükleyebilir, FTP yayımlamayı etkinleştirmek istiyorsanız Azure yığın uygulama hizmeti başarıyla dağıttıktan sonra tamamlanması gereken bazı ek adımlar vardır.  Gelecek sürümlerde adımları otomatik olarak yapılır.

> [!NOTE]
> Hizmet veya kuruluş yöneticileri Azure yığın kaynak sağlayıcısı üzerinde bir uygulama hizmeti yapılandırma adımları içindir.

## <a name="enable-ftp"></a>FTP etkinleştir

1.  Azure yığın portalına Hizmet Yöneticisi olarak oturum açın.
2.  Gözat **ağ arabirimleri** seçip **FTP NIC** altında **kaynak grubu** - **AppService yerel**. ![Azure yığın ağ arabirimleri][1]
3.  Not **genel IP adresi** , **FTP NIC**. 
![Azure yığın ağ arabirimi ayrıntıları][2]
4.  Sonraki göz atın **sanal makineleri** seçip **FTP0 VM**. ![Azure yığın sanal makineler][3]
5.  VM Olanağını kullanarak bir Uzak Masaüstü Oturum açma **Bağlan** düğmesi ve uygulama hizmet dağıtımı sırasında ayarladığınız yönetici kimlik bilgilerini kullanarak oturum oturum açın.  
![Azure yığın sanal makine ayrıntıları][4]
6.  Açık **Internet Information Service (IIS) Yöneticisi** FTP VM'de (FTP0-VM).
7.  Altında **siteleri** seçin **barındıran FTP sitesinin**.
8.  Açık **FTP güvenlik duvarı desteği**. ![Uygulama hizmeti FTP0-VM IIS Yöneticisi][5]
9.  FTP NIC'ı tıklatın ve genel IP adresini girin **Uygula** ![IIS Yöneticisi'ni FTP güvenlik duvarı desteği][6]

## <a name="validate-the-enabling-of-ftp"></a>Etkinleştirme FTP doğrula

1.  Azure yığın portalına Kiracı veya Hizmet Yöneticisi olarak oturum açın.
2.  Gözat **uygulama hizmetleri** Web, mobil veya API uygulaması oluşturdunuz ve seçin. ![Uygulama Hizmetleri][7]
3.  Uygulama Ayrıntıları notta **FTP ana bilgisayar adı** ve **FTP/dağıtım kullanıcı adı**. ![Uygulama hizmeti uygulama ayrıntıları][8]
> [!NOTE]
> Bir giriş altında görmüyorsanız **FTP/dağıtım kullanıcı adı**, dağıtım kimlik bilgilerini ilk kullanarak ayarlamak gereken **dağıtım kimlik bilgileri** dikey.

4.  Windows Gezgini'ni açın, örneğin, ftp://ftp.appservice.azurestack.local çubuğu dosya adresi içine FTP ana bilgisayar adı girin
5.  İstendiğinde girin **dağıtım kimlik bilgileri** ettiğiniz adım 3 ' özelliği etkinleştirilmişse app service uygulama içeriği dizin listesini görürsünüz. ![FTP listeleme dosyası][9]
<!--Image references-->
[1]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interfaces.png
[2]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interface-details.png
[3]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines.png
[4]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines-FTP0-VM.png
[5]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager.png
[6]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager-FTP-Firewall-Support.png
[7]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-services.png
[8]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-service-app-detail.png
[9]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-ftp-file-listing.png
