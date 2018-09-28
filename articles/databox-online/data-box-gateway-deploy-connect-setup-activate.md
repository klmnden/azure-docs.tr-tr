---
title: Yapılandırmak için bağlama ve Azure portalında Azure veri kutusu ağ geçidi etkinleştirme | Microsoft Docs
description: Veri kutusu ağ geçidini dağıtmak için üçüncü öğretici bağlanmak, ayarlanmış yönlendiren ve sanal Cihazınızı etkinleştirin.
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
ms.date: 09/27/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 77ccc6ebbb48ab3d7ce22d6c593bd62aa8dec3ac
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419497"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway-preview"></a>Öğretici: Bağlanın, ayarlama, Azure veri kutusu ağ geçidi (Önizleme) etkinleştirme 

## <a name="introduction"></a>Giriş

Bu öğreticide, ayarlamak ve veri kutusu ağ geçidi cihazınızın yerel web kullanıcı arabirimini kullanarak etkinleştirmek için bağlanacağını açıklar. 

Kurulum ve etkinleştirme işlemini tamamlamak için yaklaşık 10 dakika sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal cihaza bağlanma
> * Ayarlama ve sanal cihaz etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


> [!IMPORTANT]
> - Veri kutusu ağ geçidi Önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 


## <a name="prerequisites"></a>Önkoşullar

Yapılandırma ve veri kutusu ağ geçidini ayarlamak için önce emin olun:

* Sanal cihazı hazırladıktan ve ona bağlı bir URL içinde ayrıntılı olarak elde edilen [veri kutusu ağ geçidi Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md) veya [veri kutusu ağ geçidi vmware'de sağlama](data-box-gateway-deploy-provision-vmware.md).
* Veri kutusu ağ geçidi cihazları yönetmek için oluşturduğunuz veri kutusu ağ geçidi hizmetini etkinleştirme anahtarı var. Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).

<!--* If this is the second or subsequent virtual device that you are registering with an existing StorSimple Device Manager service, you should have the service data encryption key. This key was generated when the first device was successfully registered with this service. If you have lost this key, see [Get the service data encryption key](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) for your Data Box Gateway.-->

## <a name="connect-to-the-local-web-ui-setup"></a>Yerel web kullanıcı Arabirimi kurulum için bağlantı 

1. Bir tarayıcı penceresi açın ve yerel web kullanıcı Arabirimine bağlanın. Şunu yazın:
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Önceki öğreticide belirtilen bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata görürsünüz. Tıklayın **bu Web sayfasına devam**. (Bu adımları kullanılan tarayıcıya dayalı farklı olabilir.)
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

2. Web kullanıcı Arabirimine sanal cihazınızın oturum açın. Varsayılan parola *Password1*. 
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

3. Cihaz Yöneticisi parolasını değiştirmeniz istenir. 8 ile 16 arasında karakter içeren yeni bir parola yazın. Parola şunlardan 3 tanesini içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.

    ![](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

En sunuldu **Pano** cihazınızın.

## <a name="set-up-and-activate-the-virtual-device"></a>Ayarlama ve sanal cihaz etkinleştirme
 
1. Panodan, çeşitli ayarları yapılandırmak ve veri kutusu ağ geçidi hizmeti ile sanal cihaz kaydetmek için gerekli gidebilirsiniz. **Ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **cihaz adı** ve **Cloud ayarları**.
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

2. İçinde **cihaz adı** sayfasında, cihazınız için bir kolay ad yapılandırın. Kolay adı 1 ila 15 karakter uzunluğunda olabilir ve harf, rakam ve kısa çizgi içerebilir.

    ![](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

3. (İsteğe bağlı olarak) yapılandırın, **ağ ayarları**. En az 1 ağ arabirimi ve kaç temel alınan sanal makinede yapılandırdığınıza bağlı olarak daha fazlasını görürsünüz. **Ağ ayarları** etkinleştirilmiş bir ağ arabirimine sahip sanal bir cihaz için aşağıda gösterildiği gibi sayfa.
    
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Ağ ayarlarını yapılandırırken aşağıdakileri göz önünde bulundurun:

    - Ağ arabirimleri, DHCP, ortamınızda etkin olduğunda otomatik olarak yapılandırılır. Bu nedenle, bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
    - DHCP etkin değilse, gerekirse statik IP atayabilirsiniz.
    - Ağ Arabiriminizin IPv4 yapılandırabilirsiniz.
   
4. (İsteğe bağlı olarak), web Ara sunucusunu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca, burada yapılandırabilirsiniz olduğunu unutmayın.
   
   ![](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   İçinde **Web proxy** sayfası:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: *http://&lt;konak IP adresi veya FDQN&gt;: bağlantı noktası numarası*. HTTPS URL'leri desteklenmediğine dikkat edin.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulama kullanılıyorsa, ayrıca sağlamanız gerekir bir **kullanıcıadı** ve **parola**.
   4. **Uygula**'ya tıklayın. Doğrulama ve yapılandırılmış web proxy ayarlarını uygulayın.

5. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
    
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    İçinde **saat ayarlarını** sayfası:
    
    1. Açılır listeden seçin **saat dilimi** içinde cihaz dağıtıldığı coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
    2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
    3. İsteğe bağlı olarak belirtin bir **ikincil NTP sunucusu** cihazınız için.
    4. **Uygula**'ya tıklayın. Bu, doğrulamak ve yapılandırılan süre ayarlar uygulanır.

6. İçinde **Cloud ayarları** sayfasında, Azure portalında veri kutusu ağ geçidi hizmeti ile Cihazınızı etkinleştirin.
    
    1. Girin **etkinleştirme anahtarı** aldığınız [etkinleştirme anahtarı alma](data-box-gateway-deploy-prep.md#get-the-activation-key) veri kutusu ağ geçidi için.

    2. Tıklayın **etkinleştirme**. 
       
         ![](./media/data-box-gateway-deploy-connect-setup-activate/image10.png)
    
    3. Cihaz başarıyla etkinleştirilmeden önce bir dakika beklemeniz gerekebilir. Etkinleştirme işleminden sonra cihaz başarıyla etkinleştirildiğini belirten sayfayı güncelleştirir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, veri kutusu ağ geçidi ile ilgili konular gibi öğrendiniz:

> [!div class="checklist"]
> * Sanal cihaza bağlanma
> * Ayarlama ve sanal cihaz etkinleştirme


Veri kutusu ağ geçidi ile veri aktarma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Veri kutusu ağ geçidi ile veri aktarımı](./data-box-gateway-deploy-add-shares.md).
