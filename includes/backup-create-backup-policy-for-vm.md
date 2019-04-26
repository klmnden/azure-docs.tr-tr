---
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 11/09/2018
ms.author: raynew
ms.openlocfilehash: 3631d2e9beaa7c0d9ee018a32981a278381a7d86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60406853"
---
## <a name="defining-a-backup-policy"></a>Yedekleme ilkesi tanımlama
Yedekleme ilkesi veri anlık görüntülerinin ne zaman alınacağının ve bu anlık görüntülerin ne kadar süreyle saklanacağının bir matrisini tanımlar. VM yedeklemesi için bir ilke tanımlandığında, yedekleme işini *günde bir kez* tetikleyebilirsiniz. Yeni bir ilke oluşturduğunuzda bu ilke kasaya uygulanır. Yedekleme İlkesi arabirimi şöyle görünür:

![Yedekleme ilkesi](./media/backup-create-policy-for-vms/backup-policy.png)

İlke oluşturmak için:

1. **İlke adı** için bir ad girin.
2. Verilerinizin anlık görüntüleri Günlük veya Haftalık aralıklarla alınabilir. Veri anlık görüntülerinin günlük mü, yoksa haftalık mı olacağını seçmek için **Yedekleme Sıklığı** açılan menüsünü kullanın.

   * Günlük aralığını seçerseniz, anlık görüntünün alınacağı günün saatini seçmek için vurgulanan denetimi kullanın. Saati değiştirmek için saat seçimini kaldırıp yeni saati seçin.

     ![Günlük yedekleme ilkesi](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Haftalık aralığını seçerseniz, anlık görüntü alınacak haftanın günlerini ve günün saatini seçmek için vurgulanan denetimleri kullanın. Gün menüsünde, bir veya birden çok gün seçin. Saat menüsünde bir saat seçin. Saati değiştirmek için seçili saatin seçimini kaldırıp yeni saati seçin.

     ![Haftalık yedekleme ilkesi](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Varsayılan olarak, tüm **Elde Tutma Aralığı** seçenekleri seçilidir. Kullanmak istemediğiniz elde tutma aralığı sınırının seçimini kaldırın. Ardından, kullanılacak aralıkları belirtin.

    Aylık ve Yıllık elde tutma aralıkları günlük veya haftalık artışı temel alan anlık görüntüleri belirtmenizi sağlar.

   > [!NOTE]
   > 
   > - VM korurken yedekleme işi günde bir kez çalıştırılır. Yedeklemenin çalıştırıldığı saat her elde tutma aralığı için olanla aynıdır.
   > - Kurtarma noktası tarih ve saatte yedekleme işini zamanlama olduğunda yedek anlık görüntüyü bakılmaksızın tamamlandığında oluşturulur.
   >   - Örn. Herhangi bir sorun nedeniyle, 12:01:00 anlık görüntü tamamlandıktan ve 11:30 PM zamanlanmış yedekleme sıklığı, kurtarma noktası sonraki tarih ve 12:01:00 ile oluşturulur.
   > - Aylık yedekleme olması durumunda yedekleme her ayın ilk gününde çalıştırmak üzere ayarlanmışsa ve sonraki günün bazı sorun nedeniyle anlık görüntü tamamlandıktan sonra aylık yedekleme için oluşturulan kurtarma noktası sonraki gün (yani etiketlenir İkincisi söz konusu ay).


4. İlkeyle ilgili tüm seçeneklerin ayarlanmasından sonra, dikey pencerenin en üstünde **Kaydet**’e tıklayın.

    Yeni ilke hemen kasaya uygulanır.
