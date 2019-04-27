---
title: StorSimple sanal dizisi yedekleme kopyalama | Microsoft Docs
description: Bir yedek kopya ve dosya kurtarma, StorSimple sanal dizisi hakkında bilgi edinin.
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
ms.openlocfilehash: feffbb634af62d70a840febcf2a04afb7bdeeddd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60580937"
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>StorSimple Virtual Array'iniz yedekten kopyalama

## <a name="overview"></a>Genel Bakış

Bu makalede, bir yedekleme kümesi paylaşımları veya Microsoft Azure StorSimple Virtual Array'iniz birimlerde kopyalamak nasıl adım adım açıklanır. Kopyalanan yedekleme silinmiş veya kayıp bir dosyayı kurtarmak için kullanılır. Bu makalede, StorSimple sanal bir dosya sunucusu olarak yapılandırılmış dizi öğe düzeyinde kurtarma gerçekleştirmek için ayrıntılı adımları da içerir.

## <a name="clone-shares-from-a-backup-set"></a>Bir yedekleme kümesinden kopya paylaşımları

**Paylaşımları kopyalamak denemeden önce bu işlemi tamamlamak için cihazda yeterli alan olduğundan emin olun.** İçinde bir yedek kopyadan kopyalamak için [Azure portalında](https://portal.azure.com/), aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-share"></a>Bir paylaşımı kopyalamak için

1. Gözat **cihazları** dikey penceresi. Seçin ve Cihazınızı tıklayın ve ardından **paylaşımları**. Kopyalamak için bağlam menüsünü açmak için paylaşıma sağ tıklayın, istediğiniz paylaşımı seçin. Seçin **kopya**.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. İçinde **kopya** dikey penceresinde tıklayın **yedekleme > seçin** ve ardından aşağıdakileri yapın: 
   
   a.    Zaman aralığı tabanlı bu cihaz üzerinde bir yedek filtreleyin. Aralarından seçim yapabileceğiniz **son 7 gün**, **son 30 gündeki**, ve **son yıl**.
   
   b.    Filtrelenmiş yedeklemeleri görüntülenen listede kopyalama yapılacak bir yedekleme seçin.
   
   c.    **Tamam** düğmesine tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. İçinde **kopya** dikey penceresinde tıklayın **hedef ayarları** ve ardından aşağıdakileri yapın:
   
   a.    Bir paylaşım adı belirtin. Paylaşım adı 3-127 karakter içermelidir.
   
   b.    İsteğe bağlı olarak kopyalanan paylaşım için bir açıklama sağlayın.
   
   c.    Geri yüklediğiniz paylaşım türünü değiştiremezsiniz. Katmanlı bir paylaşım, bir katmanlı ve yerel olarak sabitlenmiş bir yerel olarak sabitlenmiş paylaşımı kopyalanmış olan.
   
   d.    Kapasite kopyalama paylaşımın boyutuna eşit olarak ayarlanır.
   
   e.    Bu paylaşım için Yöneticiler atayın. Kopyalama tamamlandıktan sonra dosya Gezgini aracılığıyla paylaşım özelliklerini değiştirmek mümkün olacaktır.
   
   f.    **Tamam** düğmesine tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Tıklayın **kopya** bir kopyalama işi başlatılamadı. İş tamamlandıktan sonra kopyalama işlemi başlar ve size bildirilir. Kopya ilerlemesini izlemek için Git **işleri** dikey penceresini ve iş ayrıntılarını görüntülemek için iş'e tıklayın.
5. Kopyalama başarıyla oluşturulduktan sonra geri gidin **paylaşımları** Cihazınızda dikey.
6. Artık Cihazınızda paylaşımlarını listesinde yeni kopyalanan paylaşım görüntüleyebilirsiniz. Katmanlı bir paylaşım katmanlı olarak kopyalanan ve yerel olarak sabitlenmiş bir paylaşım olarak yerel olarak sabitlenmiş bir paylaşımı.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Bir yedekleme kümesi birimlerden kopyalama

Azure portalında bir yedeği kopyalamak için bir paylaşım kopyalarken ayarlara benzer adımları yapmanız gerekir. Kopyalama işlemi, aynı sanal cihazda yeni bir birime yedekleme klonlar; farklı bir cihaza kopyalanamıyor.

#### <a name="to-clone-a-volume"></a>Bir birimi kopyalama

1. Gözat **cihazları** dikey penceresi. Seçin ve Cihazınızı tıklayın ve ardından **birimleri**. Bağlam menüsünü açmak için birime sağ tıklayın, kopyalamak istediğiniz bir birimi seçin. Seçin **kopya**.
   
   ![Bir birimi kopyalama](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. İçinde **kopya** dikey penceresinde tıklayın **yedekleme** ve ardından aşağıdakileri yapın: 
   
   a.    Zaman aralığı tabanlı bu cihaz üzerinde bir yedek filtreleyin. Aralarından seçim yapabileceğiniz **son 7 gün**, **son 30 gündeki**, ve **son yıl**. 
   
   b.    Filtrelenmiş yedeklemeleri görüntülenen listede kopyalama yapılacak bir yedekleme seçin.
   
   c.    **Tamam** düğmesine tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. İçinde **kopya** dikey penceresinde tıklayın **hedef birim ayarları** ve ardından aşağıdakileri yapın::
   
   a. Cihaz adını otomatik olarak doldurulur.
   
   b. Bir birim adı için sağlamak **birim kopyalanan**. Birim adı 3 ile 127 karakter içermelidir.
   
   c. Birim türünü, özgün birimin otomatik olarak ayarlanır. Katmanlı birim katmanlı olarak kopyalanan ve yerel olarak sabitlenmiş bir birim olarak yerel olarak sabitlenmiş.
   
   d. İçin **bağlı Konaklar**, tıklayın **seçin**.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. İçinde **bağlı Konaklar** dikey penceresinde mevcut bir ACR'yi seçin veya yeni ACR ekleyin. Yeni ACR eklemek için bir ACR adı ve konak IQN sağlamanız gerekir. **Seç**'e tıklayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Tıklayın **kopya** bir kopyalama işi başlatmak için.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Kopyalama, kopyalama işiniz oluşturulduktan sonra başlar. Kopya oluşturulduktan sonra Cihazınızda birimleri dikey penceresinde görüntülenir. Katmanlı birim katmanlı olarak kopyalanan ve yerel olarak sabitlenmiş bir birim, yerel olarak sabitlenmiş bir birim kopyalanmış unutmayın.
   
   ![Bir yedekleme kopyalama](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Birimin çevrimiçi birimler listesinde göründükten sonra kullanılabilir bir birimdir. İSCSI Başlatıcı konakta iSCSI Başlatıcı Özellikleri penceresinde hedeflerin listesini yenileyin. Kopyalanan birim adını içeren yeni bir hedef 'olarak durum sütununun altında inactive' görüntülenmesi gerekir.
8. Hedef seçin ve tıklayın **Connect**. Durumu değiştirmek başlatıcının hedef bağlandıktan sonra **bağlı**.
9. İçinde **Disk Yönetimi** penceresinde bağlı birimleri görünür aşağıdaki çizimde gösterildiği gibi. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.

> [!IMPORTANT]
> Zaman birimi veya paylaşımı bir yedekleme kümesinden kopyalamak kopyalama işi başarısız olursa çalışırken, bir hedef birimi veya paylaşım hala portalda oluşturulabilir. Bu hedef birimde veya bu öğeden doğan gelecekteki sorunları en aza indirmek için portaldaki paylaşıma önemlidir.
> 
> 

## <a name="item-level-recovery-ilr"></a>Öğe düzeyinde kurtarmayı (ILR)

Bu sürüm, StorSimple sanal bir dosya sunucusu olarak yapılandırılmış dizi öğe düzeyinde kurtarmayı (ILR) sunar. Özellik StorSimple cihazında tüm paylaşımlar bulut yedeğinden dosya ve klasörlerin parçalı kurtarma gerçekleştirmenize izin verir. Bir Self Servis modeli kullanarak son yedeklerden silinen dosyaları geri alabilirsiniz.

Her paylaşımına sahip bir *.backups* en son yedeklemelerin içeren klasör. İstenen yedekleme gidin, ilgili dosyaları ve klasörleri yedekten kopyalayın ve bunları geri yükleyin. Bu özellik, yedeklerden dosyaları geri yüklemek için administrators çağrıları ortadan kaldırır.

1. ILR gerçekleştirirken, dosya Gezgini yedeklemelerini görüntüleyebilirsiniz. Yedekleme için bakmak için istediğiniz belirli paylaşımına tıklayın. Göreceğiniz bir *.backups* oluşturulan tüm yedeklemeleri depolayan bir paylaşım kapsamında klasörü. Genişletin *.backups* yedekleri görüntülemek için klasör. Klasör tüm yedekleme hiyerarşi ayrılmış görünümünü gösterir. Bu görünüm, isteğe bağlı oluşturulan ve genellikle yalnızca birkaç oluşturmak için saniye sürer.
   
   Son beş yedeklemeler bu şekilde görüntülenir ve bir öğe düzeyinde kurtarma gerçekleştirmek için kullanılabilir. Beş son yedeklemelerin hem planlanmış varsayılan hem de el ile yedeklemeler içerir.
   
   * **Zamanlanmış yedeklemeler** olarak adlı &lt;cihaz adı&gt;Longtermretentionpolicy YYYYMMDD SSDDSS UTC.
   * **El ile yedeklemeler** Ad-özel-YYYYMMDD-SSDDSS-UTC adlandırılmış.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Silinen dosyanın en son sürümünü içeren bir yedekleme belirleyin. Klasör adı önceki örneklerin her bir UTC zaman damgası içeriyor ancak, klasör oluşturulduğu zamanı, yedekleme başlatıldığında gerçek cihaz zamandır. Klasör zaman damgası bulun ve yedeklemeleri tanımlamak için kullanın.

3. Önceki adımda tanımladığınız yedeklemesine geri yüklemek istediğiniz dosyayı veya klasörü bulun. Not izinlerine sahip olduğunuz klasörleri ve dosyaları yalnızca görüntüleyebilir. Belirli dosyaları veya klasörleri erişemiyorsanız, bir paylaşım yöneticisine başvurun. Yönetici, paylaşım izinleri düzenleyebilir ve belirli dosya veya klasöre erişim vermek için dosya Gezgini'ni kullanabilirsiniz. Paylaşım yönetici bir kullanıcı grubu yerine tek bir kullanıcı olduğunu bir önerilen en iyi yöntemdir.

4. Dosya veya klasör uygun StorSimple dosya sunucusu paylaşımına kopyalayın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [StorSimple sanal yerel web UI aracılığıyla dizininiz yönetmek](storsimple-ova-web-ui-admin.md).

