---
title: "Otomatik ölçeklendirme sanal makine ölçek Azure portalında ayarlar | Microsoft Docs"
description: "Sanal makine ölçek otomatik ölçeklendirme kurallar oluşturmak nasıl Azure portalında ayarlar"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 3714a4feb14bc47132e501629fc339bc7d0e40a1
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Bir sanal makineyi ölçeği Azure portalında Ayarla otomatik olarak ölçeklendirin
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz VM örneği sayısını tanımlayın. Uygulama talep değiştikçe otomatik olarak artırın veya VM örneği sayısını azaltın. Otomatik ölçeklendirme özelliği ile isteğe bağlı müşteri takip edin veya uygulamanızın yaşam döngüsü boyunca uygulama performans değişikliklerine yanıt verme olanak sağlar.

Bu makalede, Ölçek kümesi VM örnekleri performansını izlemek Azure Portalı'nda otomatik ölçeklendirme kurallarını oluşturulacağını gösterir. Otomatik ölçeklendirme kurallar artırın veya bu performans ölçümleri yanıta VM örnekleri sayısını azaltın. Bu adımları tamamlayabilmeniz için [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) veya [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md).


## <a name="prerequisites"></a>Ön koşullar
Otomatik ölçeklendirme kuralları oluşturmak için mevcut bir sanal makine gereksinim ölçek kümesi. Bir ölçek kümesi oluşturabileceğiniz [Azure portal](virtual-machine-scale-sets-portal-create.md), [Azure PowerShell](virtual-machine-scale-sets-create.md#create-from-powershell), veya [Azure CLI 2.0](virtual-machine-scale-sets-create.md#create-from-azure-cli).


## <a name="create-a-rule-to-automatically-scale-out"></a>Otomatik olarak genişletmek için kural oluşturma
Uygulama talep artarsa, Ölçek VM örnekleri üzerindeki yük artar ayarlayın. Bu artan yükün tutarlı, ise yalnızca kısa isteğe bağlı yerine, Ölçek kümesi VM örnekleri sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu VM örnekleri oluşturulur ve uygulamalarınızı dağıtılan bunlara trafiğini yük dengeleyici üzerinden dağıtmak ölçek kümesini başlar. CPU veya disk, ne kadar süreyle uygulama yük belirli bir eşiği karşılamalı ve ölçek eklemek için kaç VM örnekleri kümesi gibi izlemek için hangi ölçümleri denetler.

1. Azure portal ve select açmak **kaynak grupları** panonun sol taraftaki menüden.
2. Ölçek kümesini içerir ve ardından, Ölçeği Ayarla kaynaklar listesinden kaynak grubunu seçin.
3. Seçin **ölçeklendirme** ölçek sol taraftaki menüden penceresi ayarlayın. Düğme seçin **etkinleştirmek otomatik ölçeklendirme**:

    ![Azure portalında otomatik ölçeklendirme etkinleştir](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Ayarlarınız için bir ad girin *otomatik ölçeklendirme*, ardından seçeneğini **bir kural eklemek**.

5. Ortalama CPU yükünü 10 dakikalık bir süre içinde % 70'den büyük olduğunda ayarlamak ölçek VM örnekleri sayısını artırır bir kural oluşturalım. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında artar. Ölçek kümesi VM örnekleri az sayıda'de, ayarladığınız **işlemi** için *artırmak sayısına göre* ve ardından belirtin *1* veya *2* için *örnek sayısını*. Çok sayıda VM örneği, % %10 20 veya bir artış ölçek kümesi VM örnekleri daha uygun olabilir.

    Kural için aşağıdaki ayarları belirtin:
    
    | Parametre              | Açıklama                                                                                                         | Değer          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Zaman toplama*     | Toplanan ölçümleri analiz için nasıl toplanması gerektiğini tanımlar.                                                | Ortalama        |
    | *Ölçüm adı*          | İzleme ve ölçek uygulamak için performans ölçüm Eylemler ayarlayın.                                                   | CPU Yüzdesi |
    | *Zaman çizgisi İstatistiği* | Her zaman çizgisi, toplanan ölçümleri analiz için nasıl toplanması gerektiğini tanımlar.                             | Ortalama        |
    | *İşleci*             | Ölçüm verilerinin eşikle karşılaştırmak için kullanılan işleci.                                                     | Şu değerden fazla:   |
    | *Eşik*            | Otomatik ölçeklendirme kuralın bir eylemi tetikleyen neden olan yüzdesi.                                                 | 70             |
    | *Süre*             | Ölçüm ve eşik değerlerini karşılaştırılır önce izlenen süre miktarı.                                   | 10 dakika     |
    | *İşlem*            | Ölçek kümesini ve yukarı veya aşağı kuralın geçerli olduğunda ve hangi artış ölçeklendirmeniz gerekir tanımlar                        | Yüzdeyi şu kadar artır: |
    | *Örnek sayısı*       | Kural harekete geçirdiğinde VM örnekleri yüzdesi değiştirilmelidir.                                            | 20             |
    | *Seyrek erişimli (dakika)*  | Otomatik ölçeklendirme eylemleri etkili olması için zamanı sağlayacak şekilde kural önce beklenecek süreyi yeniden uygulanır. | 5 dakika      |

    Aşağıdaki örnekler, bu ayarları eşleşen Azure portalında oluşturulan bir kural gösterir:    

    ![VM örneği sayısını artırmak için bir otomatik ölçeklendirme kuralı oluşturma](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Bir kural oluşturmak için seçin **Ekle**


## <a name="create-a-rule-to-automatically-scale-in"></a>İçinde otomatik olarak ölçeklendirmek için kural oluşturma
Bir akşam veya hafta sonu, uygulamayı isteğe bağlı azaltabilir. Bu azalmasına yükü bir süre boyunca tutarlı ise, Ölçek kümesi VM örnekleri sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu ölçek eylemi geçerli talebi karşılamak için gerekli örnek sayısı yalnızca çalışacak şekilde ayarlayın, Ölçek çalıştırmak için maliyeti azaltır.

1. Tercih **bir kural eklemek** yeniden.
2. Ortalama CPU yükünü 10 dakikalık bir süre içinde % 30 daha sonra düştüğünde ayarlamak ölçek VM örneği sayısı azalır bir kural oluşturun. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında azalır.

    Önceki kuralla gibi aynı yaklaşımı kullanın. Kural için aşağıdaki ayarları ayarlayın:
    
    | Parametre              | Açıklama                                                                                                          | Değer          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *İşleci*             | Ölçüm verilerinin eşikle karşılaştırmak için kullanılan işleci.                                                      | Şu değerden az:   |
    | *Eşik*            | Otomatik ölçeklendirme kuralın bir eylemi tetikleyen neden olan yüzdesi.                                                 | 30             |
    | *İşlem*            | Ölçek kümesini ve yukarı veya aşağı kuralın geçerli olduğunda ve hangi artış ölçeklendirmeniz gerekir tanımlar                         | Yüzdeyi şu kadar azalt: |
    | *Örnek sayısı*       | Kural harekete geçirdiğinde VM örnekleri yüzdesi değiştirilmelidir.                                             | 20             |

3. Bir kural oluşturmak için seçin **Ekle**


## <a name="define-autoscale-instance-limits"></a>Otomatik ölçeklendirme örneği sınırları tanımlayın
Otomatik ölçeklendirme profilinizi minimum, maksimum ve VM örneği varsayılan sayısı tanımlamanız gerekir. Otomatik ölçeklendirme kurallarınızı uygulandığında, bu örneği sınırları, en yüksek sayısını örnekleri veya en düşük örneklerinin ötesinde ölçek ötesinde ölçeklendirmeyin olduğundan emin olun.

1. Aşağıdaki örneği sınırları ayarlayın:

    | Minimum | En fazla | Varsayılan|
    |---------|---------|--------|
    | 2       | 10      | 2      |

2. Otomatik ölçeklendirme kurallarını ve örneği sınırları uygulamak için seçin **kaydetmek**.


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Ölçek kümesi örneği monitör sayısı
Sayısı ve VM örneği durumunu görmek için seçin **örnekleri** ölçek sol taraftaki menüden penceresi ayarlayın. Durum VM örneği olup olmadığını gösteren *oluşturma* ölçeği otomatik olarak ayarla çıkışı ölçeklendirir veya olduğundan *silme* ölçek içinde otomatik olarak ölçeklendirir gibi.

![Ölçek kümesi VM örneklerinin bir listesini görüntüleyin](media/virtual-machine-scale-sets-autoscale-portal/view-instances.png)


## <a name="autoscale-based-on-a-schedule"></a>Bir zamanlamaya göre otomatik ölçeklendirme
Önceki örneklerde otomatik olarak içeri veya dışarı temel ana ölçümleri CPU kullanımı gibi kümesiyle ölçeği genişletilmiş. Zamanlamada göre otomatik ölçeklendirme kuralları da oluşturabilirsiniz. Zamanlama tabanlı kurallar çekirdek çalışma saatleri gibi uygulama talep beklenen bir artış öncesinde VM örneklerinin sayısını otomatik olarak ölçeğini ve örnek sayısı daha az düşündüğünüz bir anda otomatik olarak ölçekleme izin ver hafta sonu gibi talep.

1. Seçin **ölçeklendirme** ölçek sol taraftaki menüden penceresi ayarlayın. Önceki örneklerde oluşturulan varolan otomatik ölçeklendirme kurallarını silmek için çöp kutusu simgesine'ı seçin.

    ![Varolan otomatik ölçeklendirme kurallarını Sil](media/virtual-machine-scale-sets-autoscale-portal/delete-rules.png)

2. Tercih **ölçek koşul Ekle**. Kuralı adının yanındaki kalem simgesini seçin ve bir ad sağlayın *her iş günü sırasında ölçek genişletme*.

    ![Varsayılan otomatik ölçeklendirme kuralı yeniden adlandırma](media/virtual-machine-scale-sets-autoscale-portal/rename-rule.png)

3. Radyo düğmesini seçin **belirli örnek sayısı için ölçek**.
4. Örnek sayısını ölçeklendirmek için girin *10* örnek sayısı olarak.
5. Seçin **yineleyin belirli günleri** için **zamanlama** türü.
6. Tüm iş günlerini, Pazartesi-Cuma günleri seçin.
7. Uygun saat dilimi seçin ve ardından belirtin bir **başlangıç zamanı** , *09:00*.
8. Tercih **ölçek koşul Ekle** yeniden. Adlı bir zamanlama oluşturmak için bu işlemi yinelemeniz *sırasında Akşam ölçeklendirme* , ölçeklenir *3* örnekler, her hafta içi günü yinelenen ve başlar *18:00*.
9. Zamanlama tabanlı otomatik ölçeklendirme kurallarınızı uygulamak için seçin **kaydetmek**.

    ![Bir zamanlamaya göre ölçeği otomatik ölçeklendirme kuralları oluşturma](media/virtual-machine-scale-sets-autoscale-portal/schedule-autoscale.PNG)

Otomatik ölçeklendirme kurallarınızı nasıl uygulandığını görmek için seçin **çalıştırma geçmişi** sayfanın üst kısmında **ölçeklendirme** penceresi. Grafik ve olayları gösterir otomatik ölçeklendirme kurallarını tetiklemek ve VM örneği, Ölçek sayısı artırır veya azaltır listeleyin.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yatay olarak ölçekleme ve artırmak veya azaltmak için otomatik ölçeklendirme kurallarını kullanmayı öğrendiniz *numarası* , Ölçek VM örnekleri kümesi. Ayrıca dikey olarak artırmak veya azaltmak VM örneği için ölçeklendirmek *boyutu*. Daha fazla bilgi için bkz: [dikey otomatik ölçeklendirme sanal makine ölçek kümeleri ile](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Üzerindeki VM örneklerinize yönetme hakkında daha fazla bilgi için bkz: [Yönet sanal makine ölçek ayarlar Azure PowerShell ile](virtual-machine-scale-sets-windows-manage.md).

Tetikleyici, otomatik ölçeklendirme kurallarını taktirde uyarı oluşturma konusunda bilgi almak için bkz: [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanmak](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Ayrıca [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için kullanım denetim günlüklerini](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
