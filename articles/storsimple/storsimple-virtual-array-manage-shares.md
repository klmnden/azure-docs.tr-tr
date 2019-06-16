---
title: StorSimple sanal dizisi paylaşımlarını yönetin | Microsoft Docs
description: StorSimple cihaz Yöneticisi'ni ve StorSimple Virtual Array'iniz paylaşımlarında yönetmek için kullanma işlemini açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 82a6cdb6c9a39a0d196049a7ba662681ea06b36a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62116875"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-shares-on-the-storsimple-virtual-array"></a>StorSimple sanal dizisi paylaşımlarında yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple Virtual Array'iniz üzerinde paylaşımlarını oluşturmak ve yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanmayı açıklar.

StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzü tek bir web arabiriminden yönetmenizi sağlayan Azure portalında bir uzantısıdır. Paylaşımları ve birimler yönetmenin yanı sıra görüntülemek ve cihazları yönetmek, uyarıları görüntülemek, yedekleme ilkelerini yönetme ve yedekleme kataloğunu yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanabilirsiniz.

## <a name="share-types"></a>Paylaşım türü

StorSimple paylaşımları olabilir:

* **Yerel olarak sabitlenmiş**: Bu paylaşımlar verilerinde dizide her zaman kalır ve buluta dağıtılmamasını.
* **Katmanlı**: Bu paylaşımlar verileri buluta sığdırmaya. Katmanlı bir paylaşım oluşturduğunuzda, yerel katmanında yaklaşık % 10 alanı sağlandığında ve bulutta % 90 ' alanı sağlanır. Örneğin, 1 TB paylaşımı sağladıysanız, 100 GB yerel alanı bulunması ve 900 GB bulutta kullanılabilir olduğunda veri katmanları. Bu sırayla cihazda yerel alanın tümünü dışında çalıştırırsanız, (% 10 yerel katmanına gerekli kullanılamaz çünkü), katmanlı bir paylaşım sağlayamayacağınızı anlamına gelir.

### <a name="provisioned-capacity"></a>Sağlanan kapasite

Her bir paylaşım türü için sağlanan kapasite üst sınırı için aşağıdaki tabloya bakın.

| **Sınır tanımlayıcı** | **Sınırı** |
| --- | --- |
| En düşük katmanlı bir paylaşım boyutu |500 GB |
| En çok katmanlı bir paylaşım boyutu |20 TB |
| Yerel olarak sabitlenmiş bir paylaşım boyutu için alt |50 GB |
| Yerel olarak sabitlenmiş bir paylaşımı en büyük boyutu |2 TB |

## <a name="the-shares-blade"></a>Paylaşımları dikey penceresi

**Paylaşımları** StorSimple hizmeti Özet dikey menüsünde verilen bir StorSimple dizi üzerinde depolama paylaşımlarının listesini görüntüler ve bunları yönetmenize olanak sağlar.

![Paylaşımları dikey penceresi](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

Bir paylaşımı öznitelikleri bir dizi oluşur:

* **Paylaşım adı** – benzersiz olmalıdır ve Paylaşım belirlemeye yardımcı olur ve açıklayıcı bir ad.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir paylaşımı çevrimdışı olduğunda, paylaşımın kullanıcılarına erişmek mümkün olmayacaktır.
* **Tür** – paylaşım olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.
* **Kapasite** – paylaşımında depolanan verilerin toplam miktarı karşılaştırıldığında kullanılan veri miktarını belirtir.
* **Açıklama** – paylaşım açıklayan yardımcı olan isteğe bağlı ayar.
* **İzinleri** -Windows Gezgini yönetilebilir paylaşımı için NTFS izinleri.
* **Yedekleme** – StorSimple sanal dizisi, tüm paylaşımları için Yedekleme otomatik olarak etkin durumda.

![Paylaşımları ayrıntıları](./media/storsimple-virtual-array-manage-shares/share-details.png)

Yönergeleri Bu öğreticide aşağıdaki görevleri gerçekleştirmek için kullanın:

* Paylaşım ekleme
* Bir paylaşımı değiştirme
* Bir paylaşımı çevrimdışı duruma getirin
* Paylaşımı silme

## <a name="add-a-share"></a>Paylaşım ekleme

1. StorSimple hizmet özeti dikey penceresinden tıklayın **+ Ekle paylaşımı** komut çubuğundan. Bu açılır **Ekle paylaşımı** dikey penceresi.

    ![Paylaşım Ekle](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. İçinde **Ekle paylaşımı** dikey penceresinde aşağıdakileri yapın:
   
   1. İçinde **paylaşım adı** paylaşımınız için benzersiz bir ad girin. Ad 3 ile 127 karakter içeren bir dize olmalıdır.

   2. İsteğe bağlı **açıklama** paylaşım için. Açıklama, paylaşım sahiplerini belirlemeye yardımcı olur.

   3. İçinde **türü** açılan listesinde, oluşturulup oluşturulmayacağını belirtin bir **katmanlı** veya **yerel olarak sabitlenmiş** paylaşın. Yerel GARANTİLERİN, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için seçin **Paylaşımı'yerel olarak sabitlenmiş**. Diğer tüm veriler için seçin **katmanlı** paylaşın.

   4. İçinde **kapasite** alanında, paylaşım boyutu belirtin. Katmanlı bir paylaşım 500 GB ile 20 TB arasında olmalıdır ve yerel olarak sabitlenmiş bir paylaşımı, 50 GB ile 2 TB arasında olmalıdır.

   5. İçinde **ayarlanmış varsayılan tam izinleri** alanında, kullanıcının veya grubun bu paylaşıma erişim izinleri atayın. Kullanıcı veya kullanıcı grubunun adını _john@contoso.com_ biçimi. Bu paylaşımlara erişmek yönetici ayrıcalıkları izin vermek için bir kullanıcı grubuna (yerine tek bir kullanıcı) kullanmanızı öneririz. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.
3. Paylaşımınızın yapılandırma işlemini tamamladığınızda tıklayın **Oluştur**. Belirtilen ayarlarla bir paylaşımı oluşturulur ve bir bildirim görürsünüz. Varsayılan olarak, paylaşımı için yedekleme etkinleştirilecektir.
4. Paylaşımı başarıyla oluşturulduğunu doğrulamak için şu adrese gidin **paylaşımları** dikey penceresi. Listelenen paylaşımı görmeniz gerekir.
   
    ![Paylaşımı oluşturma başarılı](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Bir paylaşımı değiştirme

Paylaşımın açıklaması değiştirmeniz gerektiğinde bir paylaşımı değiştirin. Paylaşım oluşturulduktan sonra başka bir paylaşım özellikleri değiştirilebilir.

#### <a name="to-modify-a-share"></a>Bir paylaşımı değiştirmek için

1. Gelen **paylaşımları** , değiştirmek istediğiniz paylaşımı bulunduğu sanal diziyi StorSimple hizmeti Özet dikey penceresinde ayarını seçin.
2. **Seçin** geçerli açıklamasını görüntülemek ve değiştirmek için paylaşımı.
3. Değişiklikleri Kaydet'e tıklayarak **Kaydet** komut çubuğu. Belirtilen ayarlar uygulanmaz ve bir bildirim görürsünüz.
   
    ![ Paylaşım Düzenle](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Bir paylaşımı çevrimdışı duruma getirin

Değiştirmek veya silmek planlama yaparken bir paylaşımı çevrimdışı duruma gerekebilir. Bir paylaşımı çevrimdışı olduğunda, okuma-yazma erişimi için kullanılabilir değil. Cihaz yanı sıra konak paylaşımı çevrimdışı duruma gerekecektir.

#### <a name="to-take-a-share-offline"></a>Bir paylaşımı çevrimdışı duruma getirmek için

1. Söz konusu paylaşımı çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Paylaşımı, aşağıdaki adımları uygulayarak dizide çevrimdışı duruma:
   
    1. Gelen **paylaşımları** çevrimdışına size istediğiniz paylaşımı bulunduğu sanal diziyi StorSimple hizmeti Özet dikey penceresinde ayarını seçin.

    2. **Seçin** tıklatın ve Paylaşım **...**  (Alternatif olarak bu satıra sağ tıklayın) ve bağlam menüsünden **çevrimdışına**.
     
        ![Çevrimdışı paylaşımı](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Bilgileri gözden **çevrimdışına** dikey penceresinde ve işlemin onaylamış olursunuz. Tıklayın **çevrimdışına** paylaşımı çevrimdışı duruma getirmek için. Devam eden işlemin bir bildirim görürsünüz.

    4. Paylaşımı başarıyla çevrimdışı bırakıldığını olduğunu onaylamak için Git **paylaşımları** dikey penceresi. Paylaşım durumu çevrimdışı olarak görmeniz gerekir.

## <a name="delete-a-share"></a>Paylaşımı silme

> [!IMPORTANT]
> Yalnızca çevrimdışı olduğunda, bir paylaşım silebilirsiniz.


Bir paylaşımı silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-share"></a>Bir paylaşımı silmek için

1. Gelen **paylaşımları** silmek istediğiniz paylaşım bulunduğu sanal diziyi StorSimple hizmeti Özet dikey penceresinde ayarını seçin.
2. **Seçin** tıklatın ve Paylaşım **...**  (Alternatif olarak bu satıra sağ tıklayın) ve bağlam menüsünden **Sil**.
   
    ![Paylaşımı Sil](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Silmek istediğiniz paylaşım durumunu denetleyin. Silmek istediğiniz paylaşım çevrimdışı durumda değilse, önce çevrimdışına. Bağlantısındaki [bir paylaşımı çevrimdışı duruma](#take-a-share-offline).
4. İçinde onaylamanız istendiğinde **Sil** dikey penceresinde, onayı kabul edin ve tıklayın **Sil**. Paylaşım artık silinecek ve **paylaşımları** dikey penceresi sanal diziden paylaşımları güncelleştirilmiş listesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [StorSimple paylaşımı kopyalama](storsimple-virtual-array-clone.md).

