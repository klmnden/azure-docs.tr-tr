---
title: StorSimple Snapshot Manager ile cihazları yönetme | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşenini kullanarak bağlanmak ve StorSimple cihazları yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: SharS
manager: timlt
editor: ''
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 51632b8b68640814fc113a94925b6d6deaca4c5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60482581"
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Bağlanmak ve StorSimple cihazları yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın
## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager'da düğümlerini kullanabilirsiniz **kapsam** bölmesinde içeri aktarılan StorSimple cihaz verilerini doğrulamak ve bağlı depolama cihazları yenileyin. Ayrıca, tıkladığınızda **cihazları** düğümü, bağlı cihazlarınız ve ilgili durum bilgileri listesini görebilirsiniz **sonuçları** bölmesi.

![Bağlı cihazlar](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Şekil 1: StorSimple Snapshot Manager bağlı cihaz** 

Yapılandırmanıza bağlı olarak, **görünümü** seçimleri **sonuçları** bölmesinde her bir cihaz hakkında aşağıdaki bilgileri gösterir. (Bir görünüm yapılandırma hakkında daha fazla bilgi için Git [Görünüm menüsü](storsimple-use-snapshot-manager.md#view-menu).

| Sonuçları sütun | Açıklama |
|:--- |:--- |
| Ad |Klasik Azure portalında yapılandırıldığı şekilde cihaz adı |
| Model |Cihazın model numarası |
| Version |Cihazda yüklü yazılım sürümü |
| Durum |Cihazın kullanılabilir olup olmadığı |
| En son eşitleme yapıldı |Tarih ve cihazın en son ne zaman eşitlendiği zaman |
| Seri No |Cihazın seri numarası |

Sağ varsa **cihazları** düğümünde **kapsam** bölmesinde, aşağıdaki eylemlerden seçebilirsiniz:

* Ekleme veya bir cihazı Değiştir
* Bir cihazı bağlayın ve içeri aktarmalar doğrulayın
* Bağlı cihazlar Yenile

Tıklarsanız **cihazları** düğümünü ve ardından sağ cihaz ad içinde **sonuçları** bölmesinde, aşağıdaki eylemlerden seçebilirsiniz:

* Bir cihaz kimlik doğrulaması
* Cihaz ayrıntılarını görüntüleme
* Bir cihaz yenileme
* Cihaz yapılandırmasını silme
* Cihaz parolasını değiştirme

> [!NOTE]
> Tüm bu eylemler de kullanılabilir olan **eylemleri** bölmesi.


Bu öğreticide, bağlanma ve cihazları yönetmek ve aşağıdaki görevleri gerçekleştirmek için StorSimple Snapshot Manager kullanmayı açıklar:

* Ekleme veya bir cihazı Değiştir
* Bir cihazı bağlayın ve içeri aktarmalar doğrulayın
* Bağlı cihazlar Yenile
* Bir cihaz kimlik doğrulaması
* Cihaz ayrıntılarını görüntüleme
* Tek bir cihaza Yenile
* Cihaz yapılandırmasını silme
* Süresi dolan cihaz parolasını değiştirme
* Başarısız bir cihazı Değiştir

> [!NOTE]
> StorSimple Snapshot Manager arabirimi kullanma hakkında genel bilgi için Git [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Ekleme veya bir cihazı Değiştir
Eklemek veya StorSimple cihazını değiştirmek için aşağıdaki yordamı kullanın.

#### <a name="to-add-or-replace-a-device"></a>Eklemek veya bir cihazı değiştirmek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **cihazları** düğümünü ve ardından **bir cihaz yapılandırma**. **Bir cihaz yapılandırma** iletişim kutusu görüntülenir.
   
    ![Bir StorSimple cihazını Yapılandır](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. İçinde **cihaz** açılan kutusunda, cihaz veya sanal cihazın IP adresini seçin. 
4. İçinde **parola** metin kutusuna, Klasik Azure portalında cihaz için oluşturduğunuz StorSimple Snapshot Manager parolasını yazın. **Tamam** düğmesine tıklayın. Tanımladığınız cihaz için StorSimple Snapshot Manager arar. 
   
   * Kullanılabilir cihazsa, StorSimple Snapshot Manager bağlantı ekler.
   * Cihaz için herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. Tıklayın **Tamam** hata iletiyi kapatın ve ardından **iptal** kapatmak için **bir cihaz yapılandırma** iletişim kutusu.

## <a name="connect-a-device-and-verify-imports"></a>Bir cihazı bağlayın ve içeri aktarmalar doğrulayın
Bir StorSimple cihazı bağlayın ve yedeklemeleri ilişkili olan mevcut tüm birim gruplarını içeri aktarıldığından emin doğrulamak için aşağıdaki yordamı kullanın.

#### <a name="to-connect-a-device-and-verify-imports"></a>Bir cihazı bağlayın ve doğrulamak için içeri aktarır
1. Bir cihaz için StorSimple Snapshot Manager bağlamak Ekle'ndaki yönergeleri izleyin veya bir cihazı değiştirin. StorSimple Snapshot Manager şu şekilde bir aygıta bağlandığında, yanıt verir:
   
   * Cihaz için herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. 
   
   * Kullanılabilir cihazsa, StorSimple Snapshot Manager bağlantı ekler. Cihaz seçtiğinizde görünür **sonuçları** bölmesi ve Durum alanını cihaz olduğunu gösterir **kullanılabilir**. StorSimple Snapshot Manager birim gruplarını yedeklemeleri ilişkili koşuluyla, aygıtın yapılandırılmış herhangi bir birim gruplarını içeri aktarır. Yedekleme ilkelerini içeri aktarılmaz. İlişkili yedekleme olmayan birim gruplarını içeri aktarılmaz.
2. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
3. Sağ üst düğümü **kapsam** bölmesi ve ardından **içeri aktarmalar ekranını Değiştir**.
   
    ![İçeri aktarmalar ekranını Değiştir'i seçin](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. **İçeri aktarmalar ekranını Değiştir** iletişim kutusu görüntülenirse, yedeklemeleri ve içeri aktarılan birim gruplarını durumunu gösteren. **Tamam** düğmesine tıklayın.

Birim gruplarını ve Yedekleme başarıyla içeri aktarıldı olduktan sonra birim gruplarını ve oluşturduğunuz ve StorSimple Snapshot Manager ile yapılandırılmış yedeklemeler yalnızca yönetme gibi StorSimple Snapshot Manager bunları yönetmek için kullanabilirsiniz. 

## <a name="refresh-connected-devices"></a>Bağlı cihazlar Yenile
StorSimple Snapshot Manager ile bağlı StorSimple cihazları eşitlemek için aşağıdaki yordamı kullanın.

#### <a name="to-refresh-connected-devices"></a>Bağlı cihazlar yenilemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ tıklayın **cihazları**ve ardından **Yenile cihazları**. Birim gruplarını ve yedekleme, tüm son ekleri dahil olmak üzere görüntüleyebilmek bu StorSimple Snapshot Manager ile bağlı cihazlar eşitler. 
   
    ![StorSimple cihazlarını Yenile](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

**Yenile cihazları** eylem bağlı cihazlardan tüm yeni birim gruplarını ve ilişkili tüm yedeklemeler alır. Farklı **birimleri yeniden tara** eylemi için kullanılabilir **birimleri** düğümünün **Yenile cihazları** yedekleme kayıt defterini geri yüklemez.

## <a name="authenticate-a-device"></a>Bir cihaz kimlik doğrulaması
Bir StorSimple cihazı ile StorSimple Snapshot Manager kimliğini doğrulamak için aşağıdaki yordamı kullanın.

#### <a name="to-authenticate-a-device"></a>Bir cihazın kimliğini doğrulamak için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklayın **cihazları**.
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **doğrulaması**.
4. **Doğrulaması** iletişim kutusu görüntülenir. Cihaz parolasını yazın ve ardından **Tamam**.
   
    ![Kimlik doğrulaması iletişim kutusu](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Cihaz ayrıntılarını görüntüleme
StorSimple cihaz ayrıntılarını görüntüleyin ve gerekirse, cihaz StorSimple Snapshot Manager ile yeniden eşitlemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-and-resynchronize-device-details"></a>Ayrıntıları görüntülemek ve cihaz yeniden eşitlemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklayın **cihazları**.
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **ayrıntıları**.

4 **cihaz ayrıntıları** iletişim kutusu görüntülenir. Bu kutu gösterir adı, model, sürüm, seri numarası, durum, hedef iSCSI tam adı (IQN) ve son eşitleme tarih ve saat.

* Tıklayın **Resync** cihaza eşitlenecek.
* Tıklayın **Tamam** veya **iptal** iletişim kutusunu kapatın.
  
  ![Cihaz ayrıntıları](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Tek bir cihaza Yenile
Tek bir StorSimple cihazı ile StorSimple Snapshot Manager'ı yeniden eşitlemek için aşağıdaki yordamı kullanın.

#### <a name="to-refresh-a-device"></a>Bir cihaz yenilemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesinde tıklayın **cihazları**. 
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **Yenile cihaz**. Bu cihaz StorSimple Snapshot Manager ile eşitler.

## <a name="delete-a-device-configuration"></a>Cihaz yapılandırmasını silme
StorSimple anlık görüntü Yöneticisi'nden tek bir StorSimple cihaz yapılandırması silmek için aşağıdaki yordamı kullanın.

#### <a name="to-delete-a-device-configuration"></a>Cihaz yapılandırması silinemedi
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklayın **cihazları**. 
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **Sil**. 
4. Aşağıdaki ileti görüntülenir. Tıklayın **Evet** yapılandırmayı silmek veya **Hayır** silmeyi iptal etmek için.
   
    ![Cihaz yapılandırmasını silme](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Süresi dolan cihaz parolasını değiştirme
Bir StorSimple cihazı StorSimple Snapshot Manager ile kimlik doğrulaması için bir parola girmeniz gerekir. Cihazınızın kurulumunun yapılabilmesi için Windows PowerShell arabirimini kullandığınızda bu parola yapılandırın. Ancak, parola süresinin dolmasını sağlayabilir. Böyle bir durumda, parolasını değiştirmek için Klasik Azure Portalı'nı kullanabilirsiniz. Ardından, cihaz StorSimple Snapshot Manager'da yapılandırıldığından, parola süresi dolmadan önce cihaz StorSimple anlık görüntü Yöneticisi'nde yeniden doğrulaması gerekir.

#### <a name="to-change-the-expired-password"></a>Süresi dolan parolasını değiştirme
1. Klasik Azure portalında StorSimple Yöneticisi hizmeti başlatın.
2. Tıklayın **cihazları** > **yapılandırma** aygıt için.
3. StorSimple Snapshot Manager bölümüne inin. 14-15 karakter bir parola girin. Parola büyük harf, küçük harfler, sayısal ve özel karakterlerin bir karışımı içerdiğinden emin olun.
4. Onaylamak için parolayı yeniden girin.
5. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.

#### <a name="to-re-authenticate-the-device"></a>Cihazın yeniden kimlik doğrulaması
1. StorSimple Snapshot Manager'ı başlatın.
2. İçinde **kapsam** bölmesinde tıklayın **cihazları**. Yapılandırılan cihazların listesi görünür **sonuçları** bölmesi.
3. Cihaz, sağ tıklayın ve ardından seçin **doğrulaması**.
4. İçinde **doğrulaması** penceresinde, yeni bir parola girin.
5. Cihaz, sağ tıklatın ve seçin seçin **yenileme cihaz**. Bu cihaz StorSimple Snapshot Manager ile eşitler.

## <a name="replace-a-failed-device"></a>Başarısız bir cihazı Değiştir
Bir StorSimple cihazı başarısız olur ve bir bekleme (yük devretme) cihaz tarafından değiştirilmesi için yeni cihazı bağlayın ve ilişkili yedekleri görüntülemek için aşağıdaki adımları kullanın.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Yük devretmeden sonra yeni bir cihaza bağlanmak için
1. Yeni cihaz için iSCSI bağlantısını yeniden yapılandırın. Yönergeler için Git "7. adım: Bağlayın, başlatın ve bir birimde"içinde [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Yeni bir StorSimple cihazı eskisinin aynı IP adresi varsa, eski yapılandırmada bağlanmanız mümkün olabilir.


1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   3. Üzerinde **Hizmetleri** penceresinde **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **Hizmeti Durdur**.
2. Eski cihazla ilgili yapılandırma bilgilerini kaldırın:
   
   1. Dosya Gezgini'nde C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın.
   2. BACatalog klasöründeki dosyaları silin.
3. Microsoft StorSimple Yönetim hizmetini yeniden başlatın:
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   2. Üzerinde **Hizmetleri** penceresinde **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **hizmetini yeniden**.
4. StorSimple Snapshot Manager'ı başlatın.
5. Yeni bir StorSimple cihazı yapılandırmak için adım 2'ndaki adımları tamamlayın: Bir StorSimple cihazı bağlayın [StorSimple Snapshot Manager'ı Dağıtma](storsimple-snapshot-manager-deployment.md).
6. Sağ üst düzey düğüm **kapsam** bölmesi (Bu örnekte StorSimple Snapshot Manager) ve ardından **içeri aktarmalar ekranını Değiştir**. 
7. İçeri aktarılan birim gruplarını ve yedeklemeleri StorSimple Snapshot Manager'da görünür olduğunda, bir ileti görüntülenir. **Tamam** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [görüntülemek ve birimler yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-volumes.md).

