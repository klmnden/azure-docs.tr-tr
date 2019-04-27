---
title: Yapılandırmak için bağlama ve Azure portalında Azure veri kutusu ağ geçidi etkinleştirme | Microsoft Docs
description: Veri kutusu ağ geçidini dağıtmak için üçüncü öğretici bağlanmak, ayarlanmış yönlendiren ve sanal Cihazınızı etkinleştirin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: alkohli
ms.openlocfilehash: 898cb63f8868ce2abaee8784214322edf9a56997
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60756520"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway"></a>Öğretici: Bağlanma, ayarlama, Azure veri kutusu ağ geçidini etkinleştir

## <a name="introduction"></a>Giriş

Bu öğreticide, bağlanmak için ayarlama ve veri kutusu ağ geçidi cihazınızın yerel web kullanıcı arabirimini kullanarak etkinleştirmek açıklar. 

Kurulum ve etkinleştirme işlemini tamamlamak için yaklaşık 10 dakika sürebilir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir sanal cihaza bağlanma
> * Ayarlama ve sanal cihaz etkinleştirme

## <a name="prerequisites"></a>Önkoşullar

Yapılandırma ve veri kutusu ağ geçidini ayarlamak için önce emin olun:

* Sağlanan sanal cihazı ve ona bağlı bir URL içinde ayrıntılı olarak elde edilen [veri kutusu ağ geçidi Hyper-V'de sağlama](data-box-gateway-deploy-provision-hyperv.md) veya [veri kutusu ağ geçidi vmware'de sağlama](data-box-gateway-deploy-provision-vmware.md).
* Veri kutusu ağ geçidi cihazları yönetmek için oluşturduğunuz veri kutusu ağ geçidi hizmetini etkinleştirme anahtarı var. Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Yerel web kullanıcı Arabirimi kurulum için bağlantı 

1. Yerel web kullanıcı Arabirimi adresinde bir tarayıcı penceresi ve erişim'i açın:
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Önceki öğreticide belirtilen bağlantı URL'sini kullanın. Bir hata veya Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir uyarı görürsünüz.

2. Seçin **bu Web sayfasına devam**. Bu adımlar, kullandığınız tarayıcıya bağlı olarak değişebilir.
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

3. Web kullanıcı Arabirimine sanal cihazınızın oturum açın. Varsayılan parola *Password1*. 
   
    ![Yerel web kullanıcı Arabiriminde oturum açın](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

4. İstemde, cihaz parolasını değiştirin. Yeni parola 8 ile 16 karakter içermesi gerekir. Şunlardan 3 tanesini içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.

    ![Cihaz parolasını değiştirme](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

Şimdi, işiniz **Pano** cihazınızın.

## <a name="set-up-and-activate-the-virtual-device"></a>Ayarlama ve sanal cihaz etkinleştirme
 
Panonuzu yapılandırmak ve veri kutusu ağ geçidi hizmeti ile sanal cihaz kaydetmek için gereken çeşitli ayarlarını görüntüler. **Cihaz adı**, **ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **Cloud ayarları**.
   
![Yerel web kullanıcı Arabirimi "Pano" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

1. Sol bölmede seçin **cihaz adı**, cihazınız için kolay bir ad girin. Kolay adı 1 ila 15 karakter uzunluğunda içerir ve harf, rakam ve kısa olması gerekir.

    ![Yerel web kullanıcı Arabirimi "Cihaz adı" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

2. (İsteğe bağlı) Sol bölmede seçin **ağ ayarları** ve ardından ayarlarını yapılandırın. Sanal Cihazınızda en az bir ağ arabirimi ve kaç temel alınan sanal makinede yapılandırdığınıza bağlı olarak daha fazlasını görürsünüz. **Ağ ayarları** etkinleştirilmiş bir ağ arabirimine sahip sanal bir cihaz için aşağıda gösterildiği gibi sayfa.
    
    ![Yerel web kullanıcı Arabirimi "Ağ ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Ağ ayarlarını yapılandırma, göz önünde bulundurun:

    - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
    - DHCP etkin değilse, gerekirse statik IP atayabilirsiniz.
    - Ağ Arabiriminizin IPv4 yapılandırabilirsiniz.

     >[!NOTE] 
     > Cihaza bağlanmak için başka bir IP adresi yoksa ağ arabiriminden statik DHCP, yerel IP adresini geçme öneririz. Birini kullanıyorsanız, ağ arabirimi ve geçiş için DHCP, DHCP adresini belirlemek mümkün olacaktır. Bir DHCP adresi için değiştirmek istiyorsanız, cihazın hizmete kaydolduktan sonra kadar bekleyin ve sonra değiştirebilirsiniz. Bulunan tüm bağdaştırıcıları IP'ler daha sonra görüntüleyebileceğiniz **cihaz özelliklerini** hizmetiniz için Azure portalında.

3. (İsteğe bağlı), web ara sunucusunu yapılandırın. Web proxy yapılandırması bir web proxy kullanıyorsanız, isteğe bağlı olsa, yalnızca bu sayfada yapılandırabilirsiniz.
   
   ![Yerel web kullanıcı Arabirimi "Web proxy ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   Üzerinde **Web proxy** sayfasında, aşağıdakileri yapın:
   
   1. İçinde **Web proxy URL'si** kutusunda, URL'yi şu biçimde girin: `http://&lt;host-IP address or FQDN&gt;:Port number`. HTTPS URL'leri desteklenmez.
   2. Altında **kimlik doğrulaması**seçin **hiçbiri** veya **NTLM**.
   3. Kimlik doğrulaması kullanıyorsanız, girin bir **kullanıcıadı** ve **parola**.
   4. Doğrulama ve yapılandırılmış bir web proxy ayarlarını uygulamak için **Uygula**.

4. (İsteğe bağlı) Sol bölmede seçin **saat ayarlarını**ve ardından saat dilimini ve cihazınız için birincil ve ikincil NTP sunucuları yapılandırın. 

    Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
    
    ![Yerel web kullanıcı Arabirimi "Saat ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    İçinde **saat ayarlarını** sayfasında, aşağıdakileri yapın:
    
    1. İçinde **saat dilimi** aşağı açılan listesinde, cihazın dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.
        Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya varsayılan değerini kabul edin `time.windows.com`.   
        Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.

    3. İsteğe bağlı olarak **ikincil NTP sunucusu** kutusuna, cihazınız için ikincil sunucu girin.

    4. Doğrulama ve yapılandırılmış zaman ayarları uygulamak için **Uygula**.

6. Sol bölmede seçin **Cloud ayarları**ve ardından Azure portalında veri kutusu ağ geçidi hizmeti ile Cihazınızı etkinleştirin.
    
    1. İçinde **etkinleştirme anahtarı** kutusuna **etkinleştirme anahtarı** aldığınız [etkinleştirme anahtarı alma](data-box-gateway-deploy-prep.md#get-the-activation-key) veri kutusu ağ geçidi için.

    2. Seçin **etkinleştirme**.
       
         ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image10a.png)
    
    3. Cihaz etkinleştirilir ve kritik güncelleştirmeler, otomatik olarak kullanılabilir olması durumunda uygulanır. Belirten bir bildirim görürsünüz. Azure portal aracılığıyla güncelleştirme ilerleme durumunu izleyin.

        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası](./media/data-box-gateway-deploy-connect-setup-activate/image12.png)
        
        **İletişim kutusu, bir kurtarma anahtarı kopyalayın ve güvenli bir konuma kaydetmeniz gerekir ayrıca sahiptir. Bu anahtar, cihaz önyükleme yapılamıyor durumunda verilerinizi kurtarmak için kullanılır.**


    4. Güncelleştirme başarıyla tamamlanması için birkaç dakika beklemeniz gerekebilir. Güncelleştirme sonrasında olduğundan tamamlanması, cihaz için oturum açın. **Cloud ayarları** sayfasında cihaz başarıyla etkinleştirilir gösterecek biçimde güncelleştirilir.

        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası güncelleştirildi](./media/data-box-gateway-deploy-connect-setup-activate/image13.png)

Cihaz Kurulumu tamamlanır. Artık Cihazınızda paylaşımlarını ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir sanal cihaza bağlanma
> * Ayarlama ve sanal cihaz etkinleştirme

Veri kutusu ağ geçidi ile veri aktarmayı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Veri kutusu ağ geçidi ile veri aktarımı](./data-box-gateway-deploy-add-shares.md).
