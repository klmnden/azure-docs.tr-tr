---
title: StorSimple sanal dizinin yedekleme kopyalama | Microsoft Docs
description: Yedek kopya ve bir dosya, StorSimple sanal diziden kurtarmak öğrenin.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 768c9a1c906999f4690c9c8f7d075743ab1678ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875994"
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>StorSimple sanal dizinizi yedekten kopyalama

## <a name="overview"></a>Genel Bakış

Bu makalede, bir yedekleme kümesi paylaşımlar veya birimler, Microsoft Azure StorSimple sanal dizisindeki kopyalamak nasıl adım adım açıklanmaktadır. Kopyalanan yedekleme silinmiş veya kayıp dosyasını kurtarmak için kullanılır. Makale ayrıca, StorSimple sanal bir dosya sunucusu olarak yapılandırılmış dizi bir öğe düzeyinde kurtarma gerçekleştirmeye yönelik ayrıntılı adımlar içerir.

## <a name="clone-shares-from-a-backup-set"></a>Bir yedekleme kümesinden kopya paylaşımları

**Paylaşımlar kopyalama denemeden önce bu işlemi tamamlamak için cihazda yeterli alan olduğundan emin olun.** İçinde bir yedek kopyadan kopyalamak için [Azure portal](https://portal.azure.com/), aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-share"></a>Bir paylaşım kopyalamak için

1. Gözat **aygıtları** dikey. Seçin ve aygıtınızı tıklatın ve ardından **paylaşımları**. Kopyalama, bağlam menüsü çağrılacak paylaşımı sağ istediğiniz paylaşımı seçin. Seçin **kopya**.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. İçinde **kopya** dikey penceresinde tıklatın **yedekleme > seçin** ve ardından aşağıdakileri yapın: 
   
   a.    Zaman aralığı tabanlı bu aygıtta bir yedekleme filtreleyin. Aralarından seçim yapabileceğiniz **son 7 gün**, **son 30 gündeki**, ve **geçen yılda**.
   
   b.    Görüntülenen filtrelenmiş yedeklemeleri listesinde, bir yedekleme kopyası seçin.
   
   c.    **Tamam** düğmesine tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. İçinde **kopya** dikey penceresinde tıklatın **hedef ayarları** ve ardından aşağıdakileri yapın:
   
   a.    Bir paylaşım adı belirtin. Paylaşım adı 3-127 karakter içermelidir.
   
   b.    İsteğe bağlı olarak kopyalanan paylaşımı için bir açıklama sağlayın.
   
   c.    Geri yüklediğiniz paylaşımı türünü değiştiremezsiniz. Katmanlı bir paylaşımı, bir katmanlı ve yerel olarak sabitlenmiş yerel olarak sabitlenmiş bir paylaşım olarak kopyalandı.
   
   d.    Kapasite kopyalama paylaşımının boyutuna eşit olarak ayarlanır.
   
   e.    Bu paylaşım için Yöneticiler atayın. Kopyalama tamamlandıktan sonra dosya Gezgini aracılığıyla paylaşım özelliklerini değiştirmek mümkün olacaktır.
   
   f.    **Tamam** düğmesine tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Tıklatın **kopya** bir kopya işi başlatmak için. İş tamamlandıktan sonra kopyalama işlemi başlatır ve size bildirilir. Kopya ilerlemesini izlemek için Git **işleri** dikey ve iş ayrıntılarını görüntülemek için iş'i tıklatın.
5. Kopya başarıyla oluşturulduktan sonra geri gittiğinizde **paylaşımları** Cihazınızı dikey penceresinde.
6. Artık, yeni kopyalanan paylaşımı paylaşımlar listesinde aygıtınızda görüntüleyebilirsiniz. Katmanlı bir paylaşımı katmanlı gibi kopyalanan ve yerel olarak sabitlenmiş bir paylaşım olarak yerel olarak sabitlenmiş bir paylaşımı.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Bir yedekleme kümesi birimlerden kopyalama

Azure portalında bir yedekten kopyalamak için bir paylaşım kopyalarken ayarlara benzer adımları yapmanız gerekir. Kopyalama işlemi aynı sanal cihaz üzerinde yeni bir birim yedekleme klonlar; farklı bir cihaz kopyalayamıyor.

#### <a name="to-clone-a-volume"></a>Birim kopyalama

1. Gözat **aygıtları** dikey. Seçin ve aygıtınızı tıklatın ve ardından **birimleri**. Seç, kopyalamak istediğiniz birim bağlam menüsünü çağırmak için birimi sağ tıklatın. Seçin **kopya**.
   
   ![Bir birimi kopyalama](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. İçinde **kopya** dikey penceresinde tıklatın **yedekleme** ve ardından aşağıdakileri yapın: 
   
   a.    Zaman aralığı tabanlı bu aygıtta bir yedekleme filtreleyin. Aralarından seçim yapabileceğiniz **son 7 gün**, **son 30 gündeki**, ve **geçen yılda**. 
   
   b.    Görüntülenen filtrelenmiş yedeklemeleri listesinde, bir yedekleme kopyası seçin.
   
   c.    **Tamam** düğmesine tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. İçinde **kopya** dikey penceresinde tıklatın **hedef birim ayarlarını** ve ardından aşağıdakileri yapın::
   
   a. Aygıt adı otomatik olarak doldurulur.
   
   b. Bir birim için ad **birim klonlanmış**. Birim adı 3 ile 127 karakter içermelidir.
   
   c. Birim türü, özgün birimin otomatik olarak ayarlanır. Katmanlı birim katmanlı olarak kopyalanabilen ve yerel olarak sabitlenmiş bir birim yerel olarak sabitlenmiş.
   
   d. İçin **bağlı Konaklar**, tıklatın **seçin**.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. İçinde **bağlı Konaklar** dikey penceresinde, varolan bir ACR seçin veya yeni bir ACR ekleyin. Yeni bir ACR eklemek için bir ACR adı ve ana bilgisayar IQN sağlamanız gerekir. **Seç**'e tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Tıklatın **kopya** kopyalama işlemini başlatmak için.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Kopya işi oluşturulduktan sonra kopyalama başlatılır. Kopya oluşturulduktan sonra Cihazınızda birimleri dikey penceresinde görüntülenir. Katmanlı birim katmanlı olarak kopyalanabilen ve yerel olarak sabitlenmiş bir birim yerel olarak sabitlenmiş bir birim klonlanmış unutmayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Birim birimlerin listesini çevrimiçi göründükten sonra kullanılabilir bir birimdir. İSCSI Başlatıcı konakta iSCSI Başlatıcı Özellikleri penceresinde hedefleri listesini yenileyin. Kopyalanan birim adını içeren yeni bir hedef 'durum sütununun altında devre dışı olarak' görüntülenmesi gerekir.
8. Hedef seçin ve tıklatın **Bağlan**. Başlatıcı Hedef bağlandıktan sonra durum olarak değiştirilmesi gerekir **bağlı**.
9. İçinde **Disk Yönetimi** penceresinde bağlanan birimler görünür aşağıdaki çizimde gösterildiği gibi. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.

> [!IMPORTANT]
> Bir birim veya bir yedekleme paylaşımından kopyalamaya çalışırken ayarlayın, kopyalama işi başarısız olursa, bir hedef birim veya paylaşım hala portalda oluşturulabilir. Bu hedef birimde silin veya bu öğesinden doğan gelecekteki sorunları en aza indirmek için portalda paylaşmak önemlidir.
> 
> 

## <a name="item-level-recovery-ilr"></a>Öğe düzeyinde Kurtarma (ILR)

Bu sürüm, StorSimple sanal bir dosya sunucusu olarak yapılandırılmış dizisindeki öğe düzeyinde kurtarmayı (ILR) sunar. Bu özellik StorSimple cihazında tüm paylaşımlar, bir bulut yedeğinden dosya ve klasörlerin ayrıntılı kurtarma yapmanıza olanak sağlar. Silinen dosyaların bir Self Servis modelini kullanarak son yedeklerden geri alabilirsiniz.

Her paylaşımı var. bir *.backups* en son yedeklemeleri içeren klasör. İstenen yedekleme gidin, ilgili dosyaları ve klasörleri yedekten kopyalayın ve bunları geri yükleyin. Bu özellik, dosyaları yedeklerden geri yüklemek için administrators çağrıları ortadan kaldırır.

1. ILR gerçekleştirirken, dosya Gezgini üzerinden yedeklemeler görüntüleyebilirsiniz. Yedekleme için bakmak istediğiniz belirli Paylaş'ı tıklatın. Göreceğiniz bir *.backups* tüm yedeklemeler depolar paylaşımı altında oluşturulan klasör. Genişletme *.backups* yedeklemeleri görüntülemek için klasörü. Klasör tüm yedekleme hiyerarşi ayrılmış görünümünü gösterir. Bu görünüm, isteğe bağlı oluşturulur ve genellikle, yalnızca birkaç saniyelik oluşturmak için alır.
   
   Son beş yedeklemeler bu şekilde görüntülenir ve bir öğe düzeyinde kurtarma gerçekleştirmek için kullanılabilir. Beş son yedeklemelerini hem planlanmış varsayılan hem de el ile yedekleme içerir.
   
   * **Zamanlanmış yedeklemeler** olarak adlandırılan &lt;aygıt adı&gt;YYYYAAGG SSDDSS UTC DailySchedule.
   * **El ile Yedekleme** Ad-özel-YYYYAAGG-SSDDSS-UTC olarak adlı.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Silinen dosyanın en son sürümünü içeren yedekleme tanımlayın. Klasör adı her önceki örneklerinin bir UTC zaman damgası içeriyor ancak, klasörü oluşturulduğu zaman, yedekleme başladığında gerçek cihaz saattir. Klasör zaman damgası bulun ve yedeklemeleri belirlemek için kullanın.

3. Önceki adımda tanımlanan yedeklemesine geri yüklemek istediğiniz dosyayı veya klasörü bulun. Not yalnızca dosya veya izniniz klasörleri görüntüleyebilirsiniz. Belirli dosyaları veya klasörleri erişemiyorsanız, bir paylaşım yöneticisine başvurun. Yönetici, paylaşım izinlerini düzenleyin ve belirli dosya veya klasöre erişim vermek için dosya Gezgini'ni kullanabilirsiniz. Bu paylaşım yönetici bir kullanıcı grubu yerine tek bir kullanıcı olduğunu önerilen en iyi uygulamadır.

4. Dosya veya klasör, StorSimple dosya sunucusunda uygun paylaşımına kopyalayın.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır hakkında daha fazla bilgi [StorSimple sanal yerel web kullanıcı arabirimini kullanarak dizinizi yönetmek](storsimple-ova-web-ui-admin.md).

