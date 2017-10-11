---
title: "StorSimple anlık görüntü Yöneticisi ile cihazları yönetme | Microsoft Docs"
description: "StorSimple Snapshot Manager MMC ek bileşenini bağlanmak ve StorSimple cihazları yönetmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 966ecbe3-a7fa-4752-825f-6694dd949946
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f5e3186a4271e0be781f367fa75ada195c58c960
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple Snapshot Manager bağlanmak ve StorSimple cihazları yönetmek için kullanın
## <a name="overview"></a>Genel Bakış
StorSimple anlık görüntü Yöneticisi'nde düğümlerini kullanabilirsiniz **kapsam** alınan StorSimple cihaz verileri doğrulayın ve bağlı depolama cihazları yenilemek için bölmesi. Ek olarak, tıkladığınızda, **aygıtları** düğümü, bağlı cihazlarınız ve ilgili durum bilgileri listesini görebilirsiniz **sonuçları** bölmesi.

![Bağlı aygıtlar](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Şekil 1: Cihaz StorSimple Snapshot Manager bağlı** 

Bağlı olarak, **Görünüm** seçimleri **sonuçları** bölmesinde her bir cihaz hakkında aşağıdaki bilgileri gösterir. (Bir görünüm yapılandırma hakkında daha fazla bilgi için Git [Görünüm menüsünde](storsimple-use-snapshot-manager.md#view-menu).

| Sonuçları sütun | Açıklama |
|:--- |:--- |
| Ad |Azure Klasik Portalı'nda yapılandırılan aygıt adı |
| modeli |Cihazın model numarası |
| Sürüm |Cihazda yüklü olan yazılım sürümü |
| Durum |Cihazın kullanılabilir olup olmadığı |
| Son eşitlendi |Tarih ve saat aygıt en son ne zaman eşitlendiği |
| Seri No |Cihaz seri numarası |

Sağ tıklattığınızda, **aygıtları** düğümünde **kapsam** bölmesinde, aşağıdaki eylemler seçebilirsiniz:

* Ekleme veya bir aygıt değiştirme
* Bir aygıtı bağlayın ve içeri aktarmalar doğrulayın
* Bağlı aygıtlar Yenile

Tıklatırsanız **aygıtları** düğümünü ve ardından sağ tıklayarak bir aygıt adı **sonuçları** bölmesinde, aşağıdaki eylemler seçebilirsiniz:

* Bir cihaz kimlik doğrulaması
* Cihaz ayrıntıları görüntüle
* Bir cihaz yenileme
* Cihaz yapılandırmasını Sil
* Aygıt parola değiştirme

> [!NOTE]
> Tüm bu eylemleri mevcuttur **Eylemler** bölmesi.


Bu öğretici StorSimple Snapshot Manager bağlanmak ve cihazları yönetmek ve aşağıdaki görevleri gerçekleştirmek için nasıl kullanılacağını açıklamaktadır:

* Ekleme veya bir aygıt değiştirme
* Bir aygıtı bağlayın ve içeri aktarmalar doğrulayın
* Bağlı aygıtlar Yenile
* Bir cihaz kimlik doğrulaması
* Cihaz ayrıntıları görüntüle
* Tek bir cihaza Yenile
* Cihaz yapılandırmasını Sil
* Süresi dolan cihaz parolasını değiştirme
* Başarısız aygıt değiştirin

> [!NOTE]
> StorSimple Snapshot Manager arabirimini kullanma hakkında genel bilgi için Git [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Ekleme veya bir aygıt değiştirme
Eklemek veya bir StorSimple cihazı değiştirmek için aşağıdaki yordamı kullanın.

#### <a name="to-add-or-replace-a-device"></a>Eklemek veya bir aygıt değiştirmek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **aygıtları** düğümünü ve ardından **bir aygıt yapılandırma**. **Bir aygıt yapılandırma** iletişim kutusu görüntülenir.
   
    ![Bir StorSimple cihazını Yapılandır](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 
3. İçinde **aygıt** açılır kutusunda, cihaz veya sanal aygıt IP adresi seçin. 
4. İçinde **parola** metin kutusuna, Klasik Azure portalı cihazı için oluşturduğunuz StorSimple Snapshot Manager parolasını yazın. **Tamam** düğmesine tıklayın. StorSimple Snapshot Manager tanımladığınız cihaz için arar. 
   
   * Cihaz kullanılabilir değilse, StorSimple Snapshot Manager bağlantı ekler.
   * Aygıt için herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. Tıklatın **Tamam** hata iletisini kapatın ve ardından **iptal** kapatmak için **bir aygıt yapılandırma** iletişim kutusu.

## <a name="connect-a-device-and-verify-imports"></a>Bir aygıtı bağlayın ve içeri aktarmalar doğrulayın
StorSimple cihazı bağlayın ve yedeklemeleri ilişkilendirdiğiniz varolan birim gruplarda alınır doğrulamak için aşağıdaki yordamı kullanın.

#### <a name="to-connect-a-device-and-verify-imports"></a>Bir aygıtı bağlayın ve içeri aktarmalar doğrulamak için
1. StorSimple Snapshot Manager bir aygıtı bağlamak için Ekle'ndaki yönergeleri izleyin veya bir aygıt değiştirin. Bir aygıta bağlandığında, StorSimple Snapshot Manager şu şekilde yanıt verir:
   
   * Aygıt için herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. 
   
   * Cihaz kullanılabilir değilse, StorSimple Snapshot Manager bağlantı ekler. Cihaz seçtiğinizde görünür **sonuçları** bölmesinde ve durum alanı cihaz olduğunu gösterir **kullanılabilir**. Birim grupları yedeklemeleri ilişkili sahip olması koşuluyla StorSimple Snapshot Manager aygıt için yapılandırılmış herhangi bir birim grubu içeri aktarır. Yedekleme ilkeleri içeri aktarılmadı. İlişkili yedeklemelerinizi olmayan birim grupları içeri aktarılmadı.
2. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
3. En üst düğüme sağ **kapsam** bölmesi ve ardından **içeri aktarmalar ekranını Değiştir**.
   
    ![İçeri aktarmalar ekranını Değiştir'i seçin](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 
4. **İçeri aktarmalar ekranını Değiştir** iletişim kutusu görüntülenirse, içeri aktarılan birim grupları ve yedeklemelerin durumunu gösteren. **Tamam** düğmesine tıklayın.

Birim grupları ve yedeklemeleri başarıyla içeri aktarıldı sonra birim grupları ve oluşturulan ve StorSimple Snapshot Manager ile yapılandırılmış yedeklemeler yalnızca yönetmek gibi StorSimple Snapshot Manager bunları yönetmek için kullanabilirsiniz. 

## <a name="refresh-connected-devices"></a>Bağlı aygıtlar Yenile
StorSimple Snapshot Manager ile bağlı StorSimple cihazları eşitlemek için aşağıdaki yordamı kullanın.

#### <a name="to-refresh-connected-devices"></a>Bağlı aygıtlar yenilemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde, sağ **aygıtları**ve ardından **yenileme aygıtları**. Birim grupları ve tüm sınıflarla da içeren yedeklemeler görüntüleyebilmek bu StorSimple Snapshot Manager bağlı aygıtları eşitler. 
   
    ![StorSimple cihazlarını Yenile](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)

**Yenileme aygıtları** eylem bağlı aygıtlardan yeni birim grupları ve tüm ilişkili yedeklemelerinin alır. Farklı **birimleri yeniden tara** eylemi için kullanılabilir **birimleri** düğümü, **yenileme aygıtları** yedekleme kayıt defterini geri yüklemeyin.

## <a name="authenticate-a-device"></a>Bir cihaz kimlik doğrulaması
StorSimple cihazı ile StorSimple Snapshot Manager kimliğini doğrulamak için aşağıdaki yordamı kullanın.

#### <a name="to-authenticate-a-device"></a>Bir cihazın kimliğini doğrulamak için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklatın **aygıtları**.
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **kimlik doğrulama**.
4. **Kimlik doğrulama** iletişim kutusu görüntülenir. Cihaz parolasını yazın ve ardından **Tamam**.
   
    ![Kimlik doğrulaması iletişim kutusu](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 

## <a name="view-device-details"></a>Cihaz ayrıntıları görüntüle
StorSimple cihazı ayrıntılarını görüntüleyebilir ve gerekirse, cihaz StorSimple Snapshot Manager ile eşitlemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-and-resynchronize-device-details"></a>Ayrıntıları görüntülemek ve aygıt yeniden eşitlemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklatın **aygıtları**.
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **ayrıntıları**.

4 **cihaz ayrıntıları** iletişim kutusu görüntülenir. Bu kutu gösterir adı, model, sürüm, seri numarası, durum, hedef iSCSI tam adını (IQN) ve son eşitleme tarihi ve saati.

* Tıklatın **yeniden eşitleme** cihaz eşitlenecek.
* Tıklatın **Tamam** veya **iptal** iletişim kutusunu kapatın.
  
  ![Cihaz ayrıntıları](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 

## <a name="refresh-an-individual-device"></a>Tek bir cihaza Yenile
Tek bir StorSimple cihazı ile StorSimple Snapshot Manager eşitlemek için aşağıdaki yordamı kullanın.

#### <a name="to-refresh-a-device"></a>Bir aygıt yenilemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesinde tıklatın **aygıtları**. 
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **yenileme aygıt**. Bu cihaz StorSimple Snapshot Manager ile eşitler.

## <a name="delete-a-device-configuration"></a>Cihaz yapılandırmasını Sil
StorSimple anlık görüntü Yöneticisi'nden bir bireysel StorSimple cihaz yapılandırma silmek için aşağıdaki yordamı kullanın.

#### <a name="to-delete-a-device-configuration"></a>Bir aygıt yapılandırmasını silmek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklatın **aygıtları**. 
3. İçinde **sonuçları** bölmesinde, cihaz adına sağ tıklayın ve ardından **silmek**. 
4. Aşağıdaki ileti görüntülenir. Tıklatın **Evet** tıklayın veya yapılandırmasını silmek **Hayır** silmeyi iptal etmek için.
   
    ![Cihaz yapılandırmasını Sil](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Süresi dolan cihaz parolasını değiştirme
StorSimple cihazı ile StorSimple Snapshot Manager kimlik doğrulaması için bir parola girmeniz gerekir. Aygıtı ayarlamak için Windows PowerShell arabirimini kullandığınızda bu parola yapılandırın. Ancak, parola süresinin dolmasını sağlayabilir. Bu durumda, parolasını değiştirmek için Klasik Azure Portalı'nı kullanabilirsiniz. Ardından, cihaz StorSimple anlık görüntü Yöneticisi'nde yapılandırıldığından, parola süresi dolmadan önce StorSimple anlık görüntü Yöneticisi'nde aygıt yeniden doğrulaması gerekir.

#### <a name="to-change-the-expired-password"></a>Süresi dolan parolayı değiştirmek için
1. Klasik Azure portalında StorSimple Yöneticisi hizmetini başlatın.
2. Tıklatın **aygıtları** > **yapılandırma** cihaz için.
3. StorSimple Snapshot Manager bölümüne gidin. 14-15 karakter olan bir parola girin. Parola büyük harf, küçük harfler, sayısal ve özel karakterlerin bir karışımı içerdiğinden emin olun.
4. Onaylamak için parolayı yeniden girin.
5. Sayfanın alt kısmındaki **Kaydet**’e tıklayın.

#### <a name="to-re-authenticate-the-device"></a>Cihaz yeniden doğrulamak için
1. StorSimple anlık görüntü Yöneticisi'ni başlatın.
2. İçinde **kapsam** bölmesinde tıklatın **aygıtları**. Yapılandırılmış aygıtların listesini görünür **sonuçları** bölmesi.
3. Aygıt, sağ tıklatın ve ardından seçin **kimlik doğrulama**.
4. İçinde **kimlik doğrulama** penceresinde, yeni bir parola girin.
5. Aygıt, sağ tıklatın ve seçin seçin **yenileme aygıt**. Bu cihaz StorSimple Snapshot Manager ile eşitler.

## <a name="replace-a-failed-device"></a>Başarısız aygıt değiştirin
StorSimple cihazı başarısız olur ve bekleme (yük devretme) aygıt tarafından değiştirildi, yeni cihaza bağlanın ve ilişkili yedeklemelerinizi görüntülemek için aşağıdaki adımları kullanın.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Yük devretme sonrasında yeni bir aygıta bağlanmayı
1. Yeni cihaz iSCSI bağlantısı yeniden yapılandırın. Yönergeler için Git "7. adım: bağlama, başlatma ve format bir birim" olarak [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

> [!NOTE]
> Yeni StorSimple cihazı eskisiyle aynı IP adresi varsa, eski yapılandırmada bağlanabiliyor olabilir.


1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   3. Üzerinde **Hizmetleri** penceresinde, seçin **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmetini durdurun**.
2. Eski aygıtla ilgili yapılandırma bilgilerini kaldırın:
   
   1. Dosya Gezgini'nde, C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın.
   2. BACatalog klasöründeki dosyaları silin.
3. Microsoft StorSimple Yönetim hizmetini yeniden başlatın:
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   2. Üzerinde **Hizmetleri** penceresinde, seçin **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmeti yeniden**.
4. StorSimple anlık görüntü Yöneticisi'ni başlatın.
5. Yeni StorSimple cihazı yapılandırma için adım 2'ndaki adımları tamamlayın: bir StorSimple cihazı bağlanmak [dağıtmak StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
6. Üst düzey düğümünde sağ **kapsam** bölmesinde (örnekte StorSimple Snapshot Manager) ve ardından **içeri aktarmalar ekranını Değiştir**. 
7. İçeri aktarılan birim grupları ve yedeklemeleri StorSimple anlık görüntü Yöneticisi'nde göründüğünden olmadığında bir ileti görüntülenir. **Tamam** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzün yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [görüntülemek ve birimleri yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-volumes.md).

