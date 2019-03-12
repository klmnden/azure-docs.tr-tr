---
title: Yapılandırmak için bağlama ve Azure portalında Azure veri kutusu ağ geçidi etkinleştirme | Microsoft Docs
description: Veri kutusu ağ geçidini dağıtmak için üçüncü öğretici bağlanmak, ayarlanmış yönlendiren ve sanal Cihazınızı etkinleştirin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 84eb458c68c7accf1b638b8e21907516328cb892
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57545099"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway-preview"></a>Öğretici: Bağlanma, ayarlama, Azure veri kutusu ağ geçidi (Önizleme) etkinleştirme 

## <a name="introduction"></a>Giriş

Bu öğreticide, ayarlamak ve veri kutusu ağ geçidi cihazınızın yerel web kullanıcı arabirimini kullanarak etkinleştirmek için bağlanacağını açıklar. 

Kurulum ve etkinleştirme işlemini tamamlamak için yaklaşık 10 dakika sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal cihaza bağlanma
> * Ayarlama ve sanal cihaz etkinleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


> [!IMPORTANT]
> - Data Box Gateway önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 


## <a name="prerequisites"></a>Önkoşullar

Yapılandırma ve veri kutusu ağ geçidini ayarlamak için önce emin olun:

* Sanal cihazı hazırladıktan ve ona bağlı bir URL içinde ayrıntılı olarak elde edilen [veri kutusu ağ geçidi Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md) veya [veri kutusu ağ geçidi vmware'de sağlama](data-box-gateway-deploy-provision-vmware.md).
* Veri kutusu ağ geçidi cihazları yönetmek için oluşturduğunuz veri kutusu ağ geçidi hizmetini etkinleştirme anahtarı var. Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Yerel web kullanıcı Arabirimi kurulum için bağlantı 

1. Bir tarayıcı penceresi açın ve yerel web kullanıcı Arabirimine bağlanın. Şunu yazın:
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Önceki öğreticide belirtilen bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata görürsünüz. Tıklayın **bu Web sayfasına devam**. (Bu adımları kullanılan tarayıcıya dayalı farklı olabilir.)
   
    ![Bağlantı sırasında hata oluştu](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

2. Web kullanıcı Arabirimine sanal cihazınızın oturum açın. Varsayılan parola *Password1*. 
   
    ![Yerel web kullanıcı Arabiriminde oturum açın](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

3. Cihaz Yöneticisi parolasını değiştirmeniz istenir. 8 ile 16 arasında karakter içeren yeni bir parola yazın. Parola şunlardan 3 tanesini içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.

    ![Cihaz Yöneticisi parolasını değiştirme](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

En sunuldu **Pano** cihazınızın.

## <a name="set-up-and-activate-the-virtual-device"></a>Ayarlama ve sanal cihaz etkinleştirme
 
1. Panodan, çeşitli ayarları yapılandırmak ve veri kutusu ağ geçidi hizmeti ile sanal cihaz kaydetmek için gerekli gidebilirsiniz. **Ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **cihaz adı** ve **Cloud ayarları**.
   
    ![Yerel web kullanıcı Arabirimi "Pano" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

2. İçinde **cihaz adı** sayfasında, cihazınız için bir kolay ad yapılandırın. Kolay adı 1 ila 15 karakter uzunluğunda olabilir ve harf, rakam ve kısa çizgi içerebilir.

    ![Yerel web kullanıcı Arabirimi "Cihaz adı" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

3. (İsteğe bağlı olarak) yapılandırın, **ağ ayarları**. En az 1 ağ arabirimi ve kaç temel alınan sanal makinede yapılandırdığınıza bağlı olarak daha fazlasını görürsünüz. **Ağ ayarları** etkinleştirilmiş bir ağ arabirimine sahip sanal bir cihaz için aşağıda gösterildiği gibi sayfa.
    
    ![Yerel web kullanıcı Arabirimi "Ağ ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Ağ ayarlarını yapılandırırken aşağıdakileri göz önünde bulundurun:

    - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bu nedenle, bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
    - DHCP etkin değilse, gerekirse statik IP atayabilirsiniz.
    - Ağ Arabiriminizin IPv4 yapılandırabilirsiniz.

    >[!NOTE] 
    > Cihaza bağlanmak için başka bir IP adresi yoksa ağ arabiriminden statik DHCP, yerel IP adresini geçme öneririz. Birini kullanıyorsanız, ağ arabirimi ve geçiş için DHCP, DHCP adresini belirlemek mümkün olacaktır. Bir DHCP adresi için değiştirmek istiyorsanız, cihazın hizmete kaydolduktan sonra kadar bekleyin ve sonra değiştirebilirsiniz. Bulunan tüm bağdaştırıcıları IP'ler daha sonra görüntüleyebileceğiniz **cihaz özelliklerini** hizmetiniz için Azure portalında.

4. (İsteğe bağlı olarak), web Ara sunucusunu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca, burada yapılandırabilirsiniz olduğunu unutmayın.
   
   ![Yerel web kullanıcı Arabirimi "Web proxy ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   İçinde **Web proxy** sayfası:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: *http://&lt;konak IP adresi veya FQDN&gt;: bağlantı noktası numarası*. HTTPS URL'leri desteklenmediğine dikkat edin.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulama kullanılıyorsa, ayrıca sağlamanız gerekir bir **kullanıcıadı** ve **parola**.
   4. **Uygula**'ya tıklayın. Doğrulama ve yapılandırılmış web proxy ayarlarını uygulayın.

5. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
    
    ![Yerel web kullanıcı Arabirimi "Saat ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    İçinde **saat ayarlarını** sayfası:
    
    1. Açılır listeden seçin **saat dilimi** içinde cihaz dağıtıldığı coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
    2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
    3. İsteğe bağlı olarak belirtin bir **ikincil NTP sunucusu** cihazınız için.
    4. **Uygula**'ya tıklayın. Bu, doğrulamak ve yapılandırılan süre ayarlar uygulanır.

6. İçinde **Cloud ayarları** sayfasında, Azure portalında veri kutusu ağ geçidi hizmeti ile Cihazınızı etkinleştirin.
    
    1. Girin **etkinleştirme anahtarı** aldığınız [etkinleştirme anahtarı alma](data-box-gateway-deploy-prep.md#get-the-activation-key) veri kutusu ağ geçidi için.

    2. Tıklayın **etkinleştirme**. 
       
         ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image10a.png)
    
    3. İlk cihaz etkinleştirilir. Cihaz ardından varsa kritik güncelleştirmelerini taranır ve varsa, güncelleştirmeleri otomatik olarak uygulanır. Belirten bir bildirim görürsünüz. 

        İletişim kutusu, kopyalayın ve güvenli bir konuma kaydedin, bir kurtarma anahtarı da vardır. Bu anahtar, cihaz önyükleme yapılamıyor durumunda verilerinizi kurtarmak için kullanılır.

        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image12.png)    

    4. Güncelleştirme başarıyla tamamlandıktan sonra birkaç dakika beklemeniz gerekebilir. Cihaz başarıyla etkinleştirildiğini belirten sayfayı güncelleştirir.

        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası güncelleştirildi](./media/data-box-gateway-deploy-connect-setup-activate/image13.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki Data Box Gateway konularını öğrendiniz:

> [!div class="checklist"]
> * Sanal cihaza bağlanma
> * Ayarlama ve sanal cihaz etkinleştirme


Veri kutusu ağ geçidi ile veri aktarma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Veri kutusu ağ geçidi ile veri aktarımı](./data-box-gateway-deploy-add-shares.md).
