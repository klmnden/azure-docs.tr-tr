---
title: Yapılandırmak için bağlama ve Azure portalında bir Azure veri kutusu Edge cihazı etkinleştirme | Microsoft Docs
description: Veri kutusu Edge dağıtmak için üçüncü öğretici bağlanmak, ayarlanmış yönlendiren ve fiziksel Cihazınızı etkinleştirin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 01/09/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: e5f2ecd2cdff0ae5f3f5f086bde0741f7f6d2dbb
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121626"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge-preview"></a>Öğretici: Bağlanma, ayarlamak ve Azure veri kutusu Edge (Önizleme) etkinleştirme 

Bu öğreticide, ayarlamak ve Azure veri kutusu Edge cihazınızın yerel web kullanıcı arabirimini kullanarak etkinleştirmek için nasıl bağlanabilirsiniz açıklanır. 

Kurulum ve etkinleştirme işlemini tamamlamak için yaklaşık 20 dakika sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir fiziksel cihaza bağlanma
> * Ayarlama ve fiziksel cihaz etkinleştirme

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş ve bu çözümü dağıtın önce gözden [Azure Önizleme için hizmet koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 


## <a name="prerequisites"></a>Önkoşullar

Yapılandırma ve veri kutusu Edge Cihazınızı ayarlamak için önce emin olun:

* Fiziksel cihaz içinde ayrıntılı olarak yüklediğiniz [veri kutusu Edge yükleme](data-box-edge-deploy-install.md).
* Veri kutusu Edge cihazı yönetmek için oluşturduğunuz veri kutusu Edge hizmetinden etkinleştirme anahtarı var. Daha fazla bilgi için Git [Azure veri kutusu Edge dağıtmaya hazırlanma](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Yerel web kullanıcı Arabirimi kurulum için bağlantı 

1. Sınır cihazı bir statik IP adresi 192.168.100.5 ve 255.255.255.0 alt ağı ile bağlanmak için bilgisayarınızda Ethernet bağdaştırıcısını yapılandırın.

1. Bilgisayar bağlantı noktası 1 Cihazınızda bağlanın. 

1. Bir tarayıcı penceresi erişim yerel web kullanıcı Arabirimi adresinde açın ve https://192.168.100.10.  
    Bu eylem, cihazda ayarladıktan sonra birkaç dakika sürebilir. 

    Bir hata veya Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir uyarı görürsünüz. 
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

1. Seçin **bu Web sayfasına devam**.  
    Bu adımlar, kullandığınız tarayıcıya bağlı olarak değişebilir.

1. Web kullanıcı Arabirimine cihazınızın oturum açın. Varsayılan parola *Password1*. 
   
    ![Veri kutusu Edge cihaz oturum açma sayfası](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

1. İstemde, cihaz Yöneticisi parolasını değiştirin.  
    Yeni parola 8 ila 16 karakter içermelidir. Şu karakterlerden üçünü içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.

Artık cihazınızın Panosu'nda demektir.

## <a name="set-up-and-activate-the-physical-device"></a>Ayarlama ve fiziksel cihaz etkinleştirme
 
Panonuzu yapılandırmak ve fiziksel cihaz ile veri kutusu Edge hizmetine kaydetmek için gereken çeşitli ayarlarını görüntüler. **Cihaz adı**, **ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **Cloud ayarları**.
   
![Veri kutusu Edge cihazı Panosu](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. Sol bölmede seçin **cihaz adı**, cihazınız için kolay bir ad girin.  
    Kolay adı 1 ila 15 karakter içeren ve harf, rakam ve kısa çizgi içermesi gerekir.

    !["Cihaz adı" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

1. (İsteğe bağlı) Sol bölmede seçin **ağ ayarları** ve ardından ayarlarını yapılandırın.  
    Fiziksel Cihazınızda altı ağ arabirimi var. Bağlantı noktası 1 ve 2 bağlantı noktası 1 GB/sn ağ arabirimi var. Bağlantı noktası 3, 4 bağlantı noktası, bağlantı noktası 5 ve 6 bağlantı noktası 25 GB/sn ağ arabirimlerinin var. Bağlantı noktası 1 yalnızca yönetim bağlantı noktası olarak otomatik olarak yapılandırılır ve bağlantı noktası 2 bağlantı noktası 6 olan tüm veri bağlantı noktaları. **Ağ ayarları** sayfası aşağıda gösterildiği gibi.
    
    !["Ağ ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Ağ ayarlarını yapılandırın, göz önünde bulundurun:

    - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
    - DHCP etkin değilse, gerekirse statik IP atayabilirsiniz.
    - Ağ Arabiriminizin IPv4 yapılandırabilirsiniz.

    >[!NOTE] 
    > Cihaza bağlanmak için başka bir IP adresi yoksa yerel IP adresi DHCP, ağını arabirimin statik'den geçiş yapabilirim öneririz. Birini kullanıyorsanız, ağ arabirimi ve geçiş için DHCP, DHCP adresini belirlemek mümkün olacaktır. Bir DHCP adresi için değiştirmek istiyorsanız, cihazın hizmete kaydolduktan sonra kadar bekleyin ve sonra değiştirebilirsiniz. Ardından tüm adpaters IP'ler görüntüleyebilirsiniz **cihaz özelliklerini** hizmetiniz için Azure portalında.

1. (İsteğe bağlı) Sol bölmede seçin **Web proxy ayarları**ve ardından, web Ara sunucusunu yapılandırın. Web proxy yapılandırması bir web proxy kullanıyorsanız, isteğe bağlı olsa, yalnızca bu sayfada yapılandırabilirsiniz.
   
   !["Web proxy ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Üzerinde **Web proxy ayarları** sayfasında, aşağıdakileri yapın:
   
   a. İçinde **Web proxy URL'si** kutusunda, URL'yi şu biçimde girin: `http://host-IP address or FDQN:Port number`. HTTPS URL'leri desteklenmez.

   b. Altında **kimlik doğrulaması**seçin **hiçbiri** veya **NTLM**.

   c. Kimlik doğrulaması kullanıyorsanız, bir kullanıcı adı ve parola girin.

   d. Doğrulama ve yapılandırılmış bir web proxy ayarlarını uygulamak için **ayarları uygulamak**.

1. (İsteğe bağlı) Sol bölmede seçin **saat ayarlarını**ve ardından saat dilimini ve cihazınız için birincil ve ikincil NTP sunucuları yapılandırın.  
    Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
    
    !["Zaman ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    Üzerinde **saat ayarlarını** sayfasında, aşağıdakileri yapın:
    
    a. İçinde **saat dilimi** aşağı açılan listesinde, hangi cihaz dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.  
        Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    b. İçinde **birincil NTP sunucusu** kutusunda cihazınız için birincil sunucunun girin veya time.windows.com varsayılan değerini kabul edin.  
        Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.

    c. İsteğe bağlı olarak **ikincil NTP sunucusu** kutusuna, cihazınız için ikincil sunucu girin.

    d. Doğrulama ve yapılandırılmış zaman ayarları uygulamak için **Uygula**.

6. Sol bölmede seçin **Cloud ayarları**ve ardından Azure portalında veri kutusu Edge hizmetiyle Cihazınızı etkinleştirin.
    
    a. İçinde **etkinleştirme anahtarı** kutusuna, bölümünde edindiğiniz etkinleştirme anahtarı girin [etkinleştirme anahtarı alma](data-box-edge-deploy-prep.md#get-the-activation-key) veri kutusu Edge için.

    b. **Uygula**’yı seçin. 
       
    !["Bulut ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    Cihaz başarıyla etkinleştirildikten sonra bağlantı modu seçenek sunulur. Cihaz kısmen bağlantısı kesilmiş veya bağlantısı kesik modda çalışmanız gerekiyorsa, bu ayarları yapılandırılır. 

    !["Bulut ayarları" Etkinleştirme Onayı](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

Cihaz Kurulumu tamamlanır. Artık Cihazınızda paylaşımlarını ekleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir fiziksel cihaza bağlanma
> * Ayarlama ve fiziksel cihaz etkinleştirme


Veri kutusu Edge cihazınıza ile veri aktarmayı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Veri kutusu Edge ile veri aktarımı](./data-box-edge-deploy-add-shares.md).
