---
title: Azure veri kutusu ağır ayarlamak için öğretici | Microsoft Docs
description: Kablo ve, Azure veri kutusu ağır bağlanma hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 3e6bfe4a93ab8c97bcffb84bda08977f8d811fa8
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592364"
---
# <a name="tutorial-cable-and-connect-to-your-azure-data-box-heavy"></a>Öğretici: Kablo ve, Azure veri kutusu ağır için bağlanın


Bu öğreticide, kablo, bağlama ve kapatma, Azure veri kutusu yoğun açıklar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Veri kutusu ağır kablolarını bağlama
> * İçin veri kutusu ağır bağlanma

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladınız [Öğreticisi: Azure veri kutusu ağır sipariş](data-box-heavy-deploy-ordered.md).
2. Aldığınız, veri kutusu yoğun ve sipariş durumu Portalı'nda **teslim edildi**.
3. Gözden geçirdim [veri kutusu ağır güvenlik yönergeleri](data-box-safety.md).
4. Yakınlık bu ayak izine sahip bir cihaz barındırabilecek bir kullanılabilir ağ bağlantısına sahip bir veri merkezinde düz bir siteye erişimi olmalıdır. Bu cihaz, bir rafa monte olamaz.
5. Depolama Cihazınızı kullanmak için dört üzerindeki olan bağlılığımızı temel güç kablosu aldınız.
6. Veri merkezi ağına bağlı bir konak bilgisayarınız olmalıdır. Veri kutusu ağır verileri bu bilgisayardan kopyalayın. Ana bilgisayarınızda çalıştırmalısınız bir [desteklenen işletim sistemi](data-box-heavy-system-requirements.md).
7. Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 
8. Cihaz için yerel kullanıcı Arabirimi bağlanın ve RJ-45 kablosu ile dizüstü bilgisayar olması gerekir. Dizüstü bilgisayar, her cihazın düğümü kez yapılandırmak için kullanın.
9. Bir 40 GB/sn kablo veya cihaz düğüm başına 10 GB/sn kablo gerekir.
    - İle uyumlu kablolar seçin [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) ağ arabirimi.
    - 40 GB/sn kablosunun, kablo cihazı sonuna QSFP + olması gerekir.
    - 10 Gbps kablosunun, içine bir QSFP aygıta takılan + uç SFP + bağdaştırıcısı (veya QSA bağdaştırıcısı) ile bir son 10 GB/sn anahtarda takılan SFP + kablo gerekir.

## <a name="cable-your-device-for-power"></a>Cihazınızın güç bağlantısını yapın

Cihazınızın kablolarını bağlama için aşağıdaki adımları uygulayın.

1. Üzerinde oynanıp oynanmadığını veya başka herhangi bir hasarı olup olmadığını anlamak için cihazı inceleyin. Cihaz kurcalanmış ya da ciddi hasar görmüşse devam etmeyin. [Microsoft Support başvurun](data-box-disk-contact-microsoft-support.md) hemen, cihazın iyi çalışma sırayla olup olmadığını ve yenisini sevk gerekip gerekmediğini değerlendirmenize yardımcı olacak.
2. Cihaz yükleme siteye taşıyın.

    ![Veri kutusu ağır cihaz yükleme sitesi](media/data-box-heavy-deploy-set-up/data-box-heavy-install-site.png)

3. Aşağıda gösterildiği gibi arka tekerlekler cihazda kilitleyin.

    ![Veri kutusu ağır cihaz tekerlekler kilitli](media/data-box-heavy-deploy-set-up/data-box-heavy-casters-locked.png)

4. Ön ve arka kapı cihazın kilidini düğmelerini bulun. Kilidini açmak ve cihaz tarafında temizleme olana kadar ön kapısı taşıyın. Bu da arka kapı ile yineleyin.
    Cihaz üzerinden cihazı için en iyi önden arkaya hava akışına izin vermek için işletimsel olduğunda hem kapılarını açık kalmalıdır.

    ![Veri kutusu ağır kapılarını açın](media/data-box-heavy-deploy-set-up/data-box-heavy-doors-open.png)

5. Cihazın arkasına Tepsisi dört güç kabloları olması gerekir. Kabloların tepsisinden Kaldır ve kenara koyun.

    ![Veri kutusu yoğun güç kablosu tepsisinde](media/data-box-heavy-deploy-set-up/data-box-heavy-power-cords-tray.png)

6. Sonraki adım, cihazın arkasına çeşitli bağlantı noktaları belirlemektir. İki cihaz düğümler **Düğüm1** ve **Düğüm2**. Dört ağ arabirimleri, her düğüme sahip **MGMT**, **Veri1**, **veri2**, **DATA3**. **MGMT** cihazın ilk yapılandırma sırasında yönetimini yapılandırmak için kullanılır. **Veri1**-**DATA3** veri bağlantı noktalarıdır. **MGMT** ve **DATA3** ise 1 GB/sn, bağlantı noktalarının **Veri1**, **veri2** 40 GB/sn bağlantı noktaları veya 10 GB/sn bağlantı noktaları çalışabilir. İki cihaz düğümlerinin alt kısmında iki cihaz düğümler arasında paylaşılan dört güç kaynağı (PSUs) birimleridir. Bu cihaz, yüz tanıma gibi **PSUs** olan **PSU1**, **PSU2**, **PSU3**, ve **PSU4** soldan sağa doğru.

    ![Veri kutusu ağır bağlantı noktaları](media/data-box-heavy-deploy-set-up/data-box-heavy-ports.png)

7. Aygıt güç kaynakları tüm dört power kabloları bağlayın. Yeşil LED'lerini açın ve yanıp sönme.
8. Güç düğmelerini ön düzlemi cihaz düğümler üzerinde etkinleştirmek için kullanın. Güç düğmesi mavi ışıkları gelin kadar birkaç saniye basılı tutun. Artık cihaz arkasındaki güç kaynakları için tüm yeşil LED'lerini katı olmalıdır. Cihazın işletim Panosu hata LED'lerini de içerir. LED aydınlatma bir hata, hatalı PSU veya fan veya sürücü ile ilgili bir sorun gösterir.  

    ![Veri kutusu ağır ön ops panelindeki](media/data-box-heavy-deploy-set-up/data-box-heavy-front-ops-panel.png)

## <a name="cable-first-node-for-network"></a>Ağ kablosu ilk düğümü

Cihazın düğümlerinden biri üzerinde kablosu ağ için aşağıdaki adımları uygulayın.

1. Ana bilgisayar 1 GB/sn yönetim bağlantı noktasına bağlanmak için bir CAT 6 RJ-45 ağ kablosu (mavi kablo resim) kullanın.
2. Veriler için en az bir (1 GB/sn'ye tercih edilir) 40-GB/sn ağ arabirimini bağlamak için bir QSFP + kablosu (fiber veya Bakır) kullanın. 10 Gbps anahtarı kullanıyorsanız, bir SFP + kablosu ile bir QSFP + SFP + bağdaştırıcısına (QSA bağdaştırıcısı) veri için 40 GB/sn ağ arabirimini bağlamak için kullanın.

    ![Kablolu veri kutusu ağır bağlantı noktaları](media/data-box-heavy-deploy-set-up/data-box-heavy-ports-cabled.png)

    > [!IMPORTANT]
    > Veri 1 ve veri2 ayarlı ve yerel web kullanıcı Arabiriminde görüntülenen eşleşmiyor.
    > 40 GB/sn kablo bağdaştırıcısı yolu aşağıda gösterildiği gibi eklendiğinde bağlanır.

    ![Veri kutusu ağır 40 GB/sn kablo bağdaştırıcısı](media/data-box-heavy-deploy-set-up/data-box-heavy-cable-adaptor.png)

## <a name="configure-first-node"></a>İlk düğüm yapılandırma

Yerel yapılandırma ve Azure portalını kullanarak Cihazınızı ayarlamak için aşağıdaki adımları uygulayın.

1. Portaldan cihaz kimlik bilgilerini indirin. **Genel > Cihaz ayrıntıları**’na gidin. **Cihaz parolası**’nı kopyalayın. Bu parolalar, portaldaki belirli bir sırada bağlıdır. Veri kutusu ağır iki düğüm karşılık gelen, iki cihaz seri numaraları görürsünüz. Cihaz Yöneticisi parolasını her iki düğümü aynıdır.

    ![Veri kutusu ağır cihaz kimlik bilgileri](media/data-box-heavy-deploy-set-up/data-box-heavy-device-credentials.png)

2. İstemci iş istasyonunuzu CAT6 RJ-45 ağ kablosu aracılığıyla cihazı bağlayın.
3. Cihaz bir statik IP adresi ile bağlanmak için kullanmakta olduğunuz bilgisayardaki Ethernet bağdaştırıcısını yapılandırın `192.168.100.5` ve alt ağ `255.255.255.0`.

    ![Veri kutusu ağır yerel web kullanıcı Arabirimine bağlanır](media/data-box-heavy-deploy-set-up/data-box-heavy-connect-local-web-ui.png)

4. Yerel web kullanıcı Arabirimi cihazın şu URL'de bağlanın: `http://192.168.100.10`. Tıklayın **Gelişmiş** ve ardından **devam 192.168.100.10 (güvenli)** .
5. Yerel web kullanıcı arabiriminin **Oturum açma** sayfasını görürsünüz.
    
    - Bu sayfadaki düğüm seri numaralarını birini portal kullanıcı arabirimini hem de yerel web kullanıcı Arabirimi eşleşir. Seri numarası eşleme düğüm numarasını not edin. İki düğümü ve iki cihaz seri numaraları portalında vardır. Bu eşleme hangi düğümün hangi seri numarasına karşılık gelen anlamanıza yardımcı olur.
    - Bu noktada cihaz kilitlidir.
    - Cihazda oturum için önceki adımda elde ettiğiniz cihaz Yöneticisi parolasını sağlayın. **Oturum aç**’a tıklayın.

    ![Veri kutusu ağır yerel web kullanıcı Arabiriminde oturum açın](media/data-box-heavy-deploy-set-up/data-box-heavy-unlock-device.png)

5. Panoda, ağ arabirimleri yapılandırıldığından emin olun. Cihaz düğümünüzü dört ağ arabirimi vardır iki 1 GB/sn ve iki 40 GB/sn. Bir yönetim arabirimi 1 GB/sn arabirim biridir ve bu nedenle değil kullanıcı tarafından yapılandırılabilir. Kalan üç ağ arabirimleri için veri ayrılmış ve kullanıcı tarafından yapılandırılabilir.

- Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır.
- DHCP etkin değilse, ağ arabirimleri adresine gidin ve gerekirse statik IP atayın.

    ![Veri kutusu ağır Pano düğümü 1](media/data-box-heavy-deploy-set-up/data-box-heavy-dashboard-1.png)

## <a name="configure-second-node"></a>İkinci düğüm yapılandırma

Ayrıntılı adımların [ilk düğüm yapılandırma](#configure-first-node) cihazın İkinci düğüm için.

![Veri kutusu ağır Pano düğümü 2](media/data-box-heavy-deploy-set-up/data-box-heavy-dashboard-2.png)

Cihaz kurulumu tamamlandıktan sonra cihaz paylaşımlarına bağlanabilir ve verileri bilgisayarınızdan cihaza kopyalayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure veri kutusu ağır konuları hakkında gibi öğrendiniz:

> [!div class="checklist"]
> * Veri kutusu ağır kablolarını bağlama
> * İçin veri kutusu ağır bağlanma

Veri kutusu ağır üzerinde veri kopyalama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box'a verilerinizi kopyalama](./data-box-heavy-deploy-copy-data.md)
