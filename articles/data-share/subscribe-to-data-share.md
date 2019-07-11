---
title: Öğretici - kabul edin ve Azure veri paylaşımı Önizlemesi'ni kullanarak veri alma
description: Öğretici - kabul edin ve Azure veri paylaşımı Önizlemesi'ni kullanarak veri alma
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 2dc4994d88fc03c23a6d5722d6018c926e7d6b8c
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788167"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share-preview"></a>Öğretici: Kabul edin ve Azure veri paylaşımı Önizlemesi'ni kullanarak veri alma

Bu öğreticide, Azure veri paylaşma Preview kullanılarak bir veri Paylaşım daveti kabul öğreneceksiniz. Her zaman en son sizinle paylaşılan verilerin anlık görüntüsünü olmasını sağlamak normal yenileme aralığı etkinleştirme yanı sıra sizinle paylaşılan veri almak nasıl öğreneceksiniz. 

> [!div class="checklist"]
> * Azure veri paylaşımı Önizleme daveti kabul etme
> * Bir Azure veri paylaşımı Önizleme hesabı oluşturma
> * Verileriniz için bir hedef belirtin
> * Zamanlanmış yenileme, veri paylaşımı için bir abonelik oluşturun

## <a name="prerequisites"></a>Önkoşullar
Bir veri Paylaşım daveti kabul etmeden önce aşağıda listelenen Azure kaynaklarının sayısını hazırlamanız gerekir. 

Bir veri Paylaşım daveti kabul etmeden önce tüm ön koşullar tamamlandı olduğundan emin olun. 

* Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure depolama hesabı: Zaten yoksa, oluşturabileceğiniz bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Bir veri paylaşım Davetiyesi: Microsoft Azure tarafından sağlanan bir davet başlıklı bir konu ile birlikte "Azure veri paylaşımı davetini **<yourdataprovider@domain.com>** ".

> [!IMPORTANT]
> Kabul edin ve Azure veri paylaşımı almak için önce Microsoft.DataShare kaynak sağlayıcısını kaydetmeniz gerekir ve verileri kabul eden depolama hesabının sahibi olmanız gerekir. İçindeki yönergeleri izleyin [Azure veri paylaşma Önizleme sorun giderme](data-share-troubleshoot.md) yanı sıra veri paylaşımı kaynak sağlayıcısını kaydetmek için kendinizi bir depolama hesabı sahibi olarak ekleyin. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="open-invitation"></a>Açık davet

Davetiye veri sağlayıcınızdan için gelen kutunuzu kontrol edin. Davet başlıklı Microsoft Azure'dan olduğundan **Azure veri paylaşımı davetini <yourdataprovider@domain.com>** . Paylaşım adı birden çok davet varsa doğru paylaşım kabul etmiyoruz sağlamak için not alın. 

SELECT deyiminde **görüntüleyebilir davet** davetinizi azure'da görmek için. Bu sizi, alınan paylaşımları görünümünüzü götürür.

![Davet](./media/invitations.png "davetlerin listesi") 

Görüntülemek istediğiniz paylaşımı seçin. 

## <a name="accept-invitation"></a>Daveti kabul edin
Tüm alanları geçirilene, dahil olduğundan emin olun **kullanım**. Kullanım koşullarını kabul ediyorsanız, kabul belirtmek için onay kutusunu istenmeyecek. 

![Kullanım koşullarını](./media/terms-of-use.png "kullanım koşulları") 

Altında *hedef veri paylaşımı hesabı*, abonelik ve kaynak grubu, veri paylaşımınızda dağıtımı seçin. 

İçin **veri paylaşımı hesabı** alanın, Seç **Yeni Oluştur** veri paylaşımı hesabınız yoksa. Aksi halde, veri paylaşımınızda kabul etmek için istediğiniz mevcut bir veri paylaşımı hesabını seçin. 

İçin *paylaşım adı alınan* alan veri sağlayan tarafından belirtilen varsayılan değeri bırakın veya alınan paylaşımı için yeni bir ad belirtin. 

![Hedef veri paylaşımı hesabı](./media/target-data-share.png "hesabının hedef veri paylaşma") 

Kullanım koşullarını kabul ediyorum ve seçin, paylaşım için belirtilen bir konuma sonra *kabul et ve yapılandırma*. Bu seçeneği seçerseniz, bir paylaşım abonelik oluşturulacak ve sonraki ekranda içine kopyalanacak, verileriniz için bir hedef depolama hesabı seçmenizi ister. 

![Kabul seçenekleri](./media/accept-options.png "kabul seçenekleri") 

Artık daveti kabul edin, ancak daha sonra Seç depolamanızı yapılandırın tercih ederseniz *kabul edin ve daha sonra yapılandırma*. Bu seçenek, hedef depolama hesabı daha sonra yapılandırmanıza olanak sağlar. Depolama alanınızı daha sonra yapılandırmaya devam etmek için bkz: [, depolama hesabının nasıl yapılandırılacağı](how-to-configure-mapping.md) verilerinizi sürdürmek ayrıntılı adımları yapılandırmayı paylaşan sayfası. 

Daveti kabul etmek istemiyorsanız seçin *Reddet*. 

## <a name="configure-storage"></a>Depolama alanını yapılandırma
Altında *hedef depolama ayarlarını*kaynak grubunu ve istediğiniz depolama hesabı, verilerinizi almak, aboneliği seçin. 

![Hedef depolama ayarlarını](./media/target-storage-settings.png "hedef depolama") 

Düzenli yenilemeler verilerinizi almak, anlık görüntü ayarlarını etkinleştirdiğinizden emin olun. Veri sağlayıcısı, veri paylaşımı dahil bir anlık görüntü ayarını zamanlama yalnızca görüleceğini unutmayın. 

![Anlık görüntü ayarları](./media/snapshot-settings.png "anlık görüntü ayarları") 

*Kaydet*’i seçin. 

## <a name="trigger-a-snapshot"></a>Tetikleyici bir anlık görüntü

Alınan paylaşımlarında anlık görüntüsünü tetikleyebilirsiniz seçerek Ayrıntılar sekmesi -> **tetikleyici anlık görüntü**. Burada, verilerinizi tam veya artımlı anlık görüntüsünü tetikleyebilirsiniz. İlk kez veri almak, veri sağlayıcısı ise, tam kopya'yı seçin. 

![Tetikleyici anlık görüntü](./media/trigger-snapshot.png "tetikleyici anlık görüntü") 

Son çalıştırma durumunu olduğunda *başarılı*, alınan verileri görüntülemek için depolama hesabı açın. 

Kullandığınız depolama hesabını denetlenecek seçin **veri kümeleri**. 

![Tüketici veri kümeleri](./media/consumer-datasets.png "tüketici veri kümesi eşleme") 

## <a name="view-history"></a>Geçmişi görüntüleme
Anlık görüntülerin geçmişini görüntülemek için gidin alınan paylaşımları geçmişi ->. Son 60 gün için oluşturulan tüm anlık görüntülerin geçmişini burada bulabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure veri paylaşımı alıp kabul etmek nasıl modellerin. Azure veri paylaşımı kavramları hakkında daha fazla bilgi edinmek için devam [Kavramları: Azure veri paylaşımı terminolojisi](terminology.md).