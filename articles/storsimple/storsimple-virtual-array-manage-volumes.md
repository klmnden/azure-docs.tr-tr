---
title: StorSimple sanal dizisindeki birimleri yönetme | Microsoft Docs
description: StorSimple cihaz Yöneticisi'ni açıklar ve StorSimple sanal dizinizi birimlerde yönetmek için kullanımı açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: a507bf1866952cb79fa6334fed80c88cd207cd0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876057"
---
# <a name="use-storsimple-device-manager-service-to-manage-volumes-on-the-storsimple-virtual-array"></a>StorSimple sanal dizinin birimlerde yönetmek için Aygıt Yöneticisi'ni StorSimple hizmeti

## <a name="overview"></a>Genel Bakış

Bu öğretici StorSimple cihaz Yöneticisi hizmeti oluşturmak ve StorSimple sanal dizinizi birimlerde yönetmek için nasıl kullanılacağını açıklar.

StorSimple cihaz Yöneticisi hizmeti, StorSimple çözümünüzün bir tek web arabiriminden yönetmenizi sağlayan Azure portalında bir uzantısıdır. Paylaşımları ve birimler yönetmeye ek olarak, StorSimple cihaz Yöneticisi hizmeti görüntülemek ve cihazları yönetmek, uyarıları görüntüleyin ve görüntülemek ve yedekleme ilkeleri ve yedekleme kataloğu yönetmek için kullanabilirsiniz.

## <a name="volume-types"></a>Birim türleri

StorSimple birimlerini olabilir:

* **Yerel olarak sabitlenmiş**: Bu birimlerdeki veriler her zaman dizi kalmasını ve buluta dağıtılmamasını.
* **Katmanlı**: Bu birimlerdeki verileri buluta sığdırmaya. Katmanlı birim oluşturduğunuzda, alanın % 10'yaklaşık yerel katmanında sağlanır ve alan % 90'ını bulutta sağlanır. Örneğin, 1 TB birim sağlanan, 100 GB yerel alanında bulunması ve 900 GB bulutta kullanılabilir zaman veri katmanları. Bu, sırayla cihazda yerel alana çalıştırırsanız, (% 10 yerel katmanda gereken kullanılamaz çünkü), katmanlı birim sağlayamazsınız olduğunu gösterir.

### <a name="provisioned-capacity"></a>Sağlanan kapasite
Her bir birim türü için maksimum sağlanan kapasite için aşağıdaki tabloya bakın.

| **Sınır tanımlayıcısı**                                       | **Sınırı**     |
|------------------------------------------------------------|---------------|
| Katmanlı birim en küçük boyut                            | 500 GB        |
| Katmanlı birim en büyük boyutu                            | 5 TB          |
| Yerel olarak sabitlenmiş bir birim en küçük boyut                    | 50 GB         |
| Yerel olarak sabitlenmiş bir birim en büyük boyutu                    | 500 GB        |

## <a name="the-volumes-blade"></a>Birimleri dikey penceresi
**Birimleri** StorSimple hizmeti Özet dikey menüsünde belirli bir StorSimple dizi depolama birimlerin listesini görüntüler ve bunları yönetmenize olanak sağlar.

![Birimleri dikey penceresi](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

Bir birim öznitelikleri bir dizi oluşur:

* **Birim adı** – benzersiz olmalı ve birim tanımlamanıza yardımcı olan açıklayıcı bir ad.
* **Durum** – çevrimiçi veya çevrimdışı olabilir. Bir birim çevrimdışıysa, birimi kullanmak erişim izni verilen başlatıcılarına (Sunucuları) görünür değil.
* **Tür** – birim olup olmadığını belirten **katmanlı** (varsayılan) veya **yerel olarak sabitlenmiş**.
* **Kapasite** – toplam Başlatıcı (sunucu) tarafından depolanan veri miktarını karşılaştırıldığında kullanılan veri miktarını belirtir.
* **Yedekleme** – durumda StorSimple sanal dizisi tüm birimleri otomatik olarak yedekleme için etkinleştirilir.
* **Bağlı Konaklar** – bu birimi erişmesine izin başlatıcıları (Sunucuları) belirtir.

![Birimleri ayrıntıları](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Aşağıdaki görevleri gerçekleştirmek için Bu öğreticide yönergeleri kullanın:

* Birim Ekle
* Bir birim değiştirme
* Bir birim çevrimdışı duruma getirin
* Bir birim Sil

## <a name="add-a-volume"></a>Birim Ekle

1. StorSimple hizmeti Özet dikey penceresinden tıklayın **+ Add birim** komut çubuğundan. Bu açılır **birim ekleme** dikey.
   
    ![Birim ekle](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. İçinde **birim ekleme** dikey penceresinde aşağıdakileri yapın:
   
   * İçinde **birim adı** alanında, biriminiz için benzersiz bir ad girin. Adı 3 ile 127 karakter içeren bir dize olmalıdır.
   * İçinde **türü** açılır listesinde, oluşturulup oluşturulmayacağını belirtin bir **katmanlı** veya **yerel olarak sabitlenmiş** birim. Yerel GARANTİLERİN, düşük gecikme ve yüksek performansın gerektiği iş yükleri için seçin **birim'yerel olarak sabitlenmiş**. Diğer tüm veriler için seçin **katmanlı** birim.
   * İçinde **kapasite** alanında, birimin boyutunu belirtin. Katmanlı birim 500 GB ile 5 TB arasında olmalıdır ve yerel olarak sabitlenmiş bir birim 50 GB ve 500 GB arasında olması gerekir.
   * * Tıklatın **bağlı Konaklar**, bu birime bağlanmak ve ardından istediğiniz iSCSI başlatıcısı karşılık gelen bir erişim denetimi kaydı (ACR) seçin **seçin**.
3. Yeni bir bağlı ana bilgisayar eklemek için tıklatın **yeni Ekle**, bir ana bilgisayar ve onun iSCSI adı tam adını (IQN) ve ardından **Ekle**.
   
    ![Birim ekle](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Biriminiz yapılandırma tamamladığınızda tıklatın **oluşturma**. Belirtilen ayarlarla bir birim oluşturulur ve aynı başarılı üzerinde oluşturulmasını bir bildirim görürsünüz. Varsayılan olarak yedekleme birimi için etkin.
5. Birim başarıyla oluşturulduğunu doğrulamak için Git **birimleri** dikey. Listelenen birimleri görmeniz gerekir.
   
    ![Birim oluşturma başarılı](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Bir birim değiştirme

Birim erişim konakları değiştirmeniz gerektiğinde bir birim değiştirin. Diğer öznitelikleri biriminin birim oluşturulduktan sonra değiştirilemez.

#### <a name="to-modify-a-volume"></a>Bir birim değiştirmek için

1. Gelen **birimleri** StorSimple hizmeti Özet dikey ayarı değiştirmek için istediğiniz birimin bulunduğu sanal dizinin seçin.
2. **Seçin** tıklatın ve birim **bağlı Konaklar** şu anda bağlanılan ana makine görüntülemek ve farklı bir sunucuya değiştirmek için.
   
    ![Toplu düzenleme](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Tıklayarak yaptığınız değişiklikleri kaydetmek **kaydetmek** komut çubuğu. Belirtilen ayarlarınızı uygulanır ve bir bildirim görürsünüz.

## <a name="take-a-volume-offline"></a>Bir birim çevrimdışı duruma getirin

Değiştirmek veya silmek planlama yaparken bir birim çevrimdışına gerekebilir. Bir birim çevrimdışı olduğunda okuma-yazma erişimi için kullanılabilir değildir. Cihaz yanı sıra konak birimi çevrimdışı olması gerekir.

#### <a name="to-take-a-volume-offline"></a>Bir birim çevrimdışına almak için

1. Söz konusu birim çevrimdışı duruma getirmeden önce kullanımda olmadığından emin olun.
2. Birimin çevrimdışı konakta ilk alın. Bu, olası tüm birimdeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için işletim sistemi için yönergelerine bakın.
3. Konak biriminde çevrimdışı olduğundan, birim çevrimdışı dizi aşağıdaki adımları gerçekleştirerek başlatıldıktan sonra:
   
   * Gelen **birimleri** StorSimple hizmeti Özet dikey ayarı, çevrimdışı duruma getirmenizi istediğiniz birimin bulunduğu sanal dizinin seçin.
   * **Seçin** tıklatın ve birim **...**  (Alternatif olarak bu satırı sağ) ve bağlam menüsünden seçin **çevrimdışına**.
     
        ![Çevrimdışı birim](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Bilgileri gözden **çevrimdışına** dikey ve işlemin onaylamış olursunuz. Tıklatın **çevrimdışına** birimi çevrimdışı duruma getirmek için. İşlem devam ettiğinden, bir bildirim görürsünüz.
   * Birim başarıyla çevrimdışı duruma olduğunu doğrulamak için Git **birimleri** dikey. Birimin durumu çevrimdışı olarak görmeniz gerekir.
     
       ![Çevrimdışı birim onayı](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Bir birim Sil

> [!IMPORTANT]
> Yalnızca çevrimdışı ise, bir birim silebilirsiniz.
> 
> 

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için

1. Gelen **birimleri** , silmek istediğiniz birimin bulunduğu sanal dizinin StorSimple hizmeti Özet dikey ayarı seçin.
2. **Seçin** tıklatın ve birim **...**  (Alternatif olarak bu satırı sağ) ve bağlam menüsünden seçin **silmek**.
   
    ![Birim Sil](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Silmek istediğiniz birime durumunu denetleyin. Silmek istediğiniz birim çevrimdışı durumda değilse, bunu ilk olarak, aşağıdaki adımları çevrimdışına [bir birim çevrimdışına](#take-a-volume-offline).
4. İçinde onaylamanız istendiğinde **silmek** dikey penceresinde, onay kabul etmek ve tıklatın **silmek**. Birim artık silinecek ve **birimleri** dikey birimleri sanal dizi güncelleştirilmiş listesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [bir StorSimple biriminin kopyalama](storsimple-virtual-array-clone.md).

