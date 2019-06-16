---
title: Öğretici yapılandırmak için bağlanmak için Azure portalında Azure veri kutusu Edge cihazı etkinleştirme | Microsoft Docs
description: Veri kutusu Edge dağıtmak için öğretici bağlanmak, ayarlanmış yönlendiren ve fiziksel Cihazınızı etkinleştirin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: cf2aa9bc1234f8bc92829b107d1a788b75d56a6b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075079"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge"></a>Öğretici: Bağlanma, ayarlamak ve Azure veri kutusu Edge etkinleştirme 

Bu öğreticide, ayarlamak ve Azure veri kutusu Edge cihazınızın yerel web kullanıcı arabirimini kullanarak etkinleştirmek için nasıl bağlanabilirsiniz açıklanır.

Kurulum ve etkinleştirme işlemini tamamlamak için yaklaşık 20 dakika sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir fiziksel cihaza bağlanma
> * Ayarlama ve fiziksel cihaz etkinleştirme

## <a name="prerequisites"></a>Önkoşullar

Yapılandırma ve veri kutusu Edge Cihazınızı ayarlamak için önce emin olun:

* Fiziksel cihaz içinde ayrıntılı olarak yüklediğiniz [veri kutusu Edge yükleme](data-box-edge-deploy-install.md).
* Veri kutusu Edge cihazı yönetmek için oluşturduğunuz veri kutusu Edge hizmetinden etkinleştirme anahtarı var. Daha fazla bilgi için Git [Azure veri kutusu Edge dağıtmaya hazırlanma](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Yerel web kullanıcı Arabirimi kurulum için bağlantı 

1. Veri kutusu sınır cihazı bir statik IP adresi 192.168.100.5 ve 255.255.255.0 alt ağı ile bağlanmak için bilgisayarınızda Ethernet bağdaştırıcısını yapılandırın.

2. Bilgisayar bağlantı noktası 1 Cihazınızda bağlanın. Aşağıdaki çizim, bağlantı noktası 1 Cihazınızda tanımlamak için kullanın.

    ![Kabloları takılmış bir cihazın arka yüzü](./media/data-box-edge-deploy-install/backplane-cabled.png)


3. Bir tarayıcı penceresi erişim yerel web kullanıcı Arabirimi adresinde açın ve `https://192.168.100.10`.  
    Bu eylem, cihazda ayarladıktan sonra birkaç dakika sürebilir. 

    Bir hata veya Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir uyarı görürsünüz. 
   
    ![Web sitesi güvenlik sertifikası hata iletisi](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

4. Seçin **bu Web sayfasına devam**.  
    Bu adımlar, kullandığınız tarayıcıya bağlı olarak değişebilir.

5. Web kullanıcı Arabirimine cihazınızın oturum açın. Varsayılan parola *Password1*. 
   
    ![Veri kutusu Edge cihaz oturum açma sayfası](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

6. İstemde, cihaz Yöneticisi parolasını değiştirin.  
    Yeni parola 8 ile 16 karakter içermesi gerekir. Şu karakterlerden üçünü içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter.

Artık cihazınızın Panosu'nda demektir.

## <a name="set-up-and-activate-the-physical-device"></a>Ayarlama ve fiziksel cihaz etkinleştirme
 
Panonuzu yapılandırmak ve fiziksel cihaz ile veri kutusu Edge hizmetine kaydetmek için gereken çeşitli ayarlarını görüntüler. **Cihaz adı**, **ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **Cloud ayarları**.
   
![Yerel web kullanıcı Arabirimi "Pano" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. Sol bölmede seçin **cihaz adı**, cihazınız için kolay bir ad girin.  
    Kolay adı 1 ila 15 karakter içeren ve harf, rakam ve kısa olması gerekir.

    ![Yerel web kullanıcı Arabirimi "Cihaz adı" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

2. (İsteğe bağlı) Sol bölmede seçin **ağ ayarları** ve ardından ayarlarını yapılandırın.  
    Fiziksel cihazınızdan altı ağ arabirimi vardır. Bağlantı noktası 1 ve 2 bağlantı noktası 1 GB/sn ağ arabirimi var. Bağlantı noktası 3, 4 bağlantı noktası, bağlantı noktası 5 ve 6 bağlantı noktası olan 10 GB/sn ağ arabirimi olarak hizmet verebilen ayrıca tüm 25 GB/sn ağ arabirimleri. Bağlantı noktası 1 yalnızca yönetim bağlantı noktası olarak otomatik olarak yapılandırılır ve bağlantı noktası 2 bağlantı noktası 6 olan tüm veri bağlantı noktaları. **Ağ ayarları** sayfasıdır aşağıda gösterildiği gibi.
    
    ![Yerel web kullanıcı Arabirimi "Ağ ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Ağ ayarlarını yapılandırın, göz önünde bulundurun:

   - Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır. Bir IP adresi, alt ağ, ağ geçidi ve DNS otomatik olarak atanır.
   - DHCP etkin değilse, gerekirse statik IP atayabilirsiniz.
   - Ağ Arabiriminizin IPv4 yapılandırabilirsiniz.

     >[!NOTE] 
     > Cihaza bağlanmak için başka bir IP adresi yoksa yerel DCHP, ağ arabirimine gelen statik IP adresi geçme öneririz. Birini kullanıyorsanız, ağ arabirimi ve geçiş için DHCP, DHCP adresini belirlemek mümkün olacaktır. Bir DHCP adresi için değiştirmek istiyorsanız, cihazın hizmete kaydolduktan sonra kadar bekleyin ve sonra değiştirebilirsiniz. Bulunan tüm bağdaştırıcıları IP'ler daha sonra görüntüleyebileceğiniz **cihaz özelliklerini** hizmetiniz için Azure portalında.

3. (İsteğe bağlı) Sol bölmede seçin **Web proxy ayarları**ve ardından, web Ara sunucusunu yapılandırın. Web proxy yapılandırması bir web proxy kullanıyorsanız, isteğe bağlı olsa, yalnızca bu sayfada yapılandırabilirsiniz.
   
   ![Yerel web kullanıcı Arabirimi "Web proxy ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Üzerinde **Web proxy ayarları** sayfasında, aşağıdakileri yapın:
   
   a. İçinde **Web proxy URL'si** kutusunda, URL'yi şu biçimde girin: `http://host-IP address or FQDN:Port number`. HTTPS URL'leri desteklenmez.

   b. Altında **kimlik doğrulaması**seçin **hiçbiri** veya **NTLM**.

   c. Kimlik doğrulaması kullanıyorsanız, bir kullanıcı adı ve parola girin.

   d. Doğrulama ve yapılandırılmış bir web proxy ayarlarını uygulamak için **ayarları uygulamak**.

4. (İsteğe bağlı) Sol bölmede seçin **saat ayarlarını**ve ardından saat dilimini ve cihazınız için birincil ve ikincil NTP sunucuları yapılandırın.  
    Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
       
    Üzerinde **saat ayarlarını** sayfasında, aşağıdakileri yapın:
    
    1. İçinde **saat dilimi** aşağı açılan listesinde, hangi cihaz dağıtıldığı coğrafi konuma karşılık gelen saat dilimini seçin.
        Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.

    2. İçinde **birincil NTP sunucusu** kutusunda cihazınız için birincil sunucunun girin veya time.windows.com varsayılan değerini kabul edin.  
        Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.

    3. İsteğe bağlı olarak **ikincil NTP sunucusu** kutusuna, cihazınız için ikincil sunucu girin.

    4. Doğrulama ve yapılandırılmış zaman ayarları uygulamak için **ayarları uygulamak**.

        ![Yerel web kullanıcı Arabirimi "Saat ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)

5. (İsteğe bağlı) Sol bölmede seçin **depolama ayarlarını** Cihazınızda depolama dayanıklılığını yapılandırma. Bu özellik şu anda önizleme sürümündedir. Varsayılan olarak, cihazdaki depolama alanına dayanıklı değildir ve cihazda bir veri diski başarısız olursa veri kaybı olan. Dayanıklı seçeneğini etkinleştirdiğinizde, cihazdaki depolama alanına yapılandırılacak ve veri kaybı olmadan bir veri diski başarısız cihaza dayanabilir. Depolama olarak dayanıklı yapılandırma cihazınızın kullanılabilir kapasiteyi azaltır.

    > [!IMPORTANT] 
    > Dayanıklılık, yalnızca cihaz etkinleştirmeden önce yapılandırılabilir. 

    ![Yerel web kullanıcı Arabirimi "Depolama ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/storage-settings.png)

6. Sol bölmede seçin **Cloud ayarları**ve ardından Azure portalında veri kutusu Edge hizmetiyle Cihazınızı etkinleştirin.
    
    1. İçinde **etkinleştirme anahtarı** kutusuna, bölümünde edindiğiniz etkinleştirme anahtarı girin [etkinleştirme anahtarı alma](data-box-edge-deploy-prep.md#get-the-activation-key) veri kutusu Edge için.
    2. **Uygula**’yı seçin.
       
        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)

    3. İlk cihaz etkinleştirilir. Cihaz ardından varsa kritik güncelleştirmelerini taranır ve varsa, güncelleştirmeleri otomatik olarak uygulanır. Belirten bir bildirim görürsünüz.

        İletişim kutusu, kopyalayın ve güvenli bir konuma kaydedin, bir kurtarma anahtarı da vardır. Bu anahtar, cihaz önyükleme yapılamıyor durumunda verilerinizi kurtarmak için kullanılır.

        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası güncelleştirildi](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)

    4. Güncelleştirme başarıyla tamamlandıktan sonra birkaç dakika beklemeniz gerekebilir. Cihaz başarıyla etkinleştirildiğini belirten sayfayı güncelleştirir.

        ![Yerel web kullanıcı Arabirimi "Bulut ayarları" sayfası güncelleştirildi](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-8.png)

Cihaz Kurulumu tamamlanır. Artık Cihazınızda paylaşımlarını ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir fiziksel cihaza bağlanma
> * Ayarlama ve fiziksel cihaz etkinleştirme

Veri kutusu Edge cihazınıza ile veri aktarmayı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [Veri kutusu Edge ile veri aktarımı](./data-box-edge-deploy-add-shares.md).
