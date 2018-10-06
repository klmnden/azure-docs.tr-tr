---
title: Yapılandırmak için bağlanmak ve Azure portalında Azure veri kutusu Edge etkinleştirme | Microsoft Docs
description: Veri kutusu Edge dağıtmak için üçüncü öğretici bağlanmak, ayarlanmış yönlendiren ve fiziksel Cihazınızı etkinleştirin.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 83bc1d81eaa930fc16c895f4e3b8b9bf1b1ad28c
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48832326"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-edge-preview"></a>Öğretici: Bağlanın, ayarlama, Azure veri kutusu Edge (Önizleme) etkinleştirme 

Bu öğreticide, ayarlamak ve yerel web UI aracılığıyla veri kutusu Edge cihazınıza etkinleştirmek için bağlanacağını açıklar. 

Kurulum ve etkinleştirme işlemini tamamlamak için yaklaşık 20 dakika sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Fiziksel cihaza bağlanma
> * Ayarlama ve fiziksel cihaz etkinleştirme

> [!IMPORTANT]
> Veri kutusu Edge Önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 


## <a name="prerequisites"></a>Önkoşullar

Yapılandırma ve veri kutusu Ucunuzdaki ayarlamak için önce emin olun:

* Fiziksel cihaz içinde ayrıntılı olarak yüklediğiniz [veri kutusu Edge yükleme](data-box-edge-deploy-install.md).
* Veri kutusu Edge cihazı yönetmek için oluşturduğunuz veri kutusu Edge hizmetinden etkinleştirme anahtarı var. Daha fazla bilgi için Git [Azure veri kutusu Edge dağıtmaya hazırlanma](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Yerel web kullanıcı Arabirimi kurulum için bağlantı 

1. Sınır cihazı bir statik IP adresi 192.168.100.5 ve 255.255.255.0 alt ağı ile bağlanmak için kullanmakta olduğunuz bilgisayardaki Ethernet bağdaştırıcısını yapılandırın.
2. Bilgisayar bağlantı noktası 1 Cihazınızda bağlanın. 
3. Bir tarayıcı penceresi erişim yerel web kullanıcı Arabirimi adresinde açın ve https://192.168.100.10. Bu eylem, cihazda açtıktan sonra birkaç dakika sürebilir. 
4. Bir hata veya Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir uyarı görürsünüz. Tıklayın **bu Web sayfasına devam**. (Bu adımları kullanılan tarayıcıya dayalı farklı olabilir.)
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

2. Web kullanıcı Arabirimine cihazınızın oturum açın. Varsayılan parola *Password1*. 
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

3. Cihaz Yöneticisi parolasını değiştirmeniz istenir. 8 ile 16 arasında karakter içeren yeni bir parola yazın. Parola 3 şu karakterleri içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.

Şimdi, işiniz **Pano** cihazınızın.

## <a name="set-up-and-activate-the-physical-device"></a>Ayarlama ve fiziksel cihaz etkinleştirme
 
1. Panodan, çeşitli ayarları yapılandırmak ve fiziksel cihaz ile veri kutusu Edge hizmetine kaydetmek için gerekli gidebilirsiniz. **Cihaz adı**, **ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **Cloud ayarları**.
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

2. İçinde **cihaz adı** sayfasında, cihazınız için bir kolay ad yapılandırın. Kolay adı 1 ila 15 karakter uzunluğunda olabilir ve harf, rakam ve kısa çizgi içerebilir.

    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

3. (İsteğe bağlı olarak) yapılandırın, **ağ ayarları**. Fiziksel cihazınızdan altı ağ arabirimi görürsünüz. Bağlantı noktası 1 ve 2 bağlantı noktası 1 GB/sn ağ arabirimi var. Bağlantı noktası 3, 4 bağlantı noktası, bağlantı noktası 5 ve 6 bağlantı noktası 25 GB/sn ağ arabirimlerinin var. Bağlantı Noktası 6 için bağlantı noktası 2 sırasında tüm veri bağlantı bağlantı noktası 1 yalnızca yönetim bağlantı noktası olarak otomatik olarak yapılandırılır. **Ağ ayarları** sayfası aşağıda gösterildiği gibi.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Ağ ayarlarını yapılandırırken göz önünde bulundurun:

    - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
    - DHCP etkin değilse, gerekirse statik IP atayabilirsiniz.
    - Ağ Arabiriminizin IPv4 yapılandırabilirsiniz.
   
4. (İsteğe bağlı olarak), web Ara sunucusunu yapılandırın. Web proxy yapılandırması bir web proxy kullanıyorsanız, isteğe bağlı olsa, yalnızca, burada yapılandırabilirsiniz.
   
   ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   İçinde **Web proxy** sayfası:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: `http://host-IP address or FDQN:Port number`. HTTPS URL'leri desteklenmez.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulaması kullanıyorsanız, aynı zamanda sağlamak ihtiyacınız bir **kullanıcıadı** ve **parola**.
   4. Tıklayın **Uygula** doğrulamak ve yapılandırılmış bir web proxy ayarlarını uygulamak için.

5. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    İçinde **saat ayarlarını** sayfası:
    
    1. Açılır listeden seçin **saat dilimi** içinde cihaz dağıtıldığı coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
    2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
    3. İsteğe bağlı olarak belirtin bir **ikincil NTP sunucusu** cihazınız için.
    4. Tıklayın **Uygula** doğrulamak ve yapılandırılmış zaman ayarlarını uygulamak için.

6. İçinde **Cloud ayarları** sayfasında, Azure portalında veri kutusu Edge hizmetiyle Cihazınızı etkinleştirin.
    
    1. Girin **etkinleştirme anahtarı** aldığınız [etkinleştirme anahtarı alma](data-box-edge-deploy-prep.md#get-the-activation-key) veri kutusu Edge için.

    2. **Uygula**'ya tıklayın. 
       
         ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    3. Cihaz başarıyla etkinleştirildikten sonra bağlantı modu seçenek sunulur. Cihaz kısmen bağlantısı kesilmiş veya bağlantısı kesik modda çalışmanız gerekiyorsa, bu ayarları yapılandırılır. 

        ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

Cihaz Kurulumu tamamlanır. Artık Cihazınızda paylaşımlarını ekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, veri kutusu Edge konuları hakkında gibi öğrendiniz:

> [!div class="checklist"]
> * Fiziksel cihaza bağlanma
> * Ayarlama ve fiziksel cihaz etkinleştirme


Veri kutusu Ucunuzdaki ile veri aktarma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Veri kutusu Edge ile veri aktarımı](./data-box-edge-deploy-add-shares.md).