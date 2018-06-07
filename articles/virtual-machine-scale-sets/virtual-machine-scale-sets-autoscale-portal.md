---
title: Otomatik ölçeklendirme sanal makine ölçek Azure portalında ayarlar | Microsoft Docs
description: Sanal makine ölçek otomatik ölçeklendirme kurallar oluşturmak nasıl Azure portalında ayarlar
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: c9386f7dd0ba390a5f089be058c7f3edd6e33cf9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652381"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Bir sanal makineyi ölçeği Azure portalında Ayarla otomatik olarak ölçeklendirin
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz sanal makine örneği sayısını tanımlarsınız. Uygulamanızın talebi değiştikçe, sanal makine örneklerinin sayısını otomatik olarak artırabilir veya azaltabilirsiniz. Otomatik ölçeklendirme özelliği, uygulamanızın yaşam döngüsü boyunca uygulama performansındaki değişikliklere veya müşteri taleplerine ayak uydurmanıza olanak tanır.

Bu makalede, Ölçek kümesi VM örnekleri performansını izlemek Azure Portalı'nda otomatik ölçeklendirme kurallarını oluşturulacağını gösterir. Otomatik ölçeklendirme kurallar artırın veya bu performans ölçümleri yanıta VM örnekleri sayısını azaltın. Bu adımları tamamlayabilmeniz için [Azure PowerShell](tutorial-autoscale-powershell.md) veya [Azure CLI 2.0](tutorial-autoscale-cli.md).


## <a name="prerequisites"></a>Önkoşullar
Otomatik ölçeklendirme kuralları oluşturmak için mevcut bir sanal makine gereksinim ölçek kümesi. Bir ölçek kümesi oluşturabileceğiniz [Azure portal](quick-create-portal.md), [Azure PowerShell](quick-create-powershell.md), veya [Azure CLI 2.0](quick-create-cli.md).


## <a name="create-a-rule-to-automatically-scale-out"></a>Otomatik olarak genişletmek için kural oluşturma
Uygulamanızın talebi artarsa, ölçek kümenizdeki sanal makine örneklerinde üzerindeki yük de artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. CPU veya disk gibi hangi ölçümlerin izleneceğini, uygulama yükünün belirli bir eşiği ne kadar süre karşılaması gerektiği ve ölçek kümesine kaç sanal makine örneği ekleneceğini denetlersiniz.

1. Azure portal ve select açmak **kaynak grupları** panonun sol taraftaki menüden.
2. Ölçek kümesini içerir ve ardından, Ölçeği Ayarla kaynaklar listesinden kaynak grubunu seçin.
3. Seçin **ölçeklendirme** ölçek sol taraftaki menüden penceresi ayarlayın. Düğme seçin **etkinleştirmek otomatik ölçeklendirme**:

    ![Azure portalında otomatik ölçeklendirme etkinleştir](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Ayarlarınız için bir ad girin *otomatik ölçeklendirme*, ardından seçeneğini **bir kural eklemek**.

5. Ortalama CPU yükünü 10 dakikalık bir süre içinde % 70'den büyük olduğunda ayarlamak ölçek VM örnekleri sayısını artırır bir kural oluşturalım. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında artar. Ölçek kümesi VM örnekleri az sayıda'de, ayarladığınız **işlemi** için *artırmak sayısına göre* ve ardından belirtin *1* veya *2* için *örnek sayısını*. Çok sayıda VM örneği, % %10 20 veya bir artış ölçek kümesi VM örnekleri daha uygun olabilir.

    Kural için aşağıdaki ayarları belirtin:
    
    | Parametre              | Açıklama                                                                                                         | Değer          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Zaman toplama*     | Toplanan ölçümlerin analiz için nasıl bir araya getirileceğini tanımlar.                                                | Ortalama        |
    | *Ölçüm adı*          | İzlenecek ve ölçek kümesi eylemlerinin uygulanmasında temel alınacak performans ölçümü.                                                   | CPU yüzdesi |
    | *Zaman çizgisi İstatistiği* | Her zaman çizgisi, toplanan ölçümleri analiz için nasıl toplanması gerektiğini tanımlar.                             | Ortalama        |
    | *işleci*             | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                     | Şu değerden fazla:   |
    | *Eşik*            | Otomatik ölçeklendirme kuralın bir eylemi tetikleyen neden olan yüzdesi.                                                 | 70             |
    | *Süre*             | Ölçüm ve eşik değerleri karşılaştırılmadan önce izleme yapılacak süre.                                   | 10 dakika     |
    | *İşlem*            | Ölçek kümesini ve yukarı veya aşağı kuralın geçerli olduğunda ve hangi artış ölçeklendirmeniz gerekir tanımlar                        | Yüzdeyi şu kadar artır: |
    | *Örnek sayısı*       | Kural tetiklendiğinde değiştirilmesi gereken sanal makine örneklerinin yüzdesi.                                            | 20             |
    | *Seyrek erişimli (dakika)*  | Otomatik ölçeklendirme eylemlerinin geçerli olması için kural tekrar uygulanmadan önceki bekleme süresi. | 5 dakika      |

    Aşağıdaki örnekler, bu ayarları eşleşen Azure portalında oluşturulan bir kural gösterir:

    ![VM örneği sayısını artırmak için bir otomatik ölçeklendirme kuralı oluşturma](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Bir kural oluşturmak için seçin **Ekle**


## <a name="create-a-rule-to-automatically-scale-in"></a>İçinde otomatik olarak ölçeklendirmek için kural oluşturma
Bir akşam veya hafta sonu uygulama talebiniz azalabilir. Yük belirli bir süreye yayılarak tutarlı şekilde azalıyorsa, ölçek kümesindeki sanal makine örneği sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Mevcut talebi karşılamak için gerekli örnek sayısını yalnızca siz çalıştırdığınızdan, bu ölçeği daraltma eylemi ölçek kümenizi çalıştırma maliyetini azaltır.

1. Tercih **bir kural eklemek** yeniden.
2. Ortalama CPU yükünü 10 dakikalık bir süre içinde % 30 daha sonra düştüğünde ayarlamak ölçek VM örneği sayısı azalır bir kural oluşturun. Kural harekete geçirdiğinde VM örneklerinin sayısını 20 oranında azalır.

    Önceki kuralla gibi aynı yaklaşımı kullanın. Kural için aşağıdaki ayarları ayarlayın:
    
    | Parametre              | Açıklama                                                                                                          | Değer          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *işleci*             | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                      | Şu değerden az:   |
    | *Eşik*            | Otomatik ölçeklendirme kuralın bir eylemi tetikleyen neden olan yüzdesi.                                                 | 30             |
    | *İşlem*            | Ölçek kümesini ve yukarı veya aşağı kuralın geçerli olduğunda ve hangi artış ölçeklendirmeniz gerekir tanımlar                         | Yüzdeyi şu kadar azalt: |
    | *Örnek sayısı*       | Kural tetiklendiğinde değiştirilmesi gereken sanal makine örneklerinin yüzdesi.                                             | 20             |

3. Bir kural oluşturmak için seçin **Ekle**


## <a name="define-autoscale-instance-limits"></a>Otomatik ölçeklendirme örneği sınırları tanımlayın
Otomatik ölçeklendirme profilinizi minimum, maksimum ve VM örneği varsayılan sayısı tanımlamanız gerekir. Otomatik ölçeklendirme kurallarınızı uygulandığında, bu örneği sınırları, en yüksek sayısını örnekleri veya en düşük örneklerinin ötesinde ölçek ötesinde ölçeklendirmeyin olduğundan emin olun.

1. Aşağıdaki örneği sınırları ayarlayın:

    | Minimum | Maksimum | Varsayılan|
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

Otomatik ölçeklendirme kurallarınızı nasıl uygulandığını görmek için seçin **çalıştırma geçmişi** sayfanın üst kısmında **ölçeklendirme** penceresi. Otomatik ölçeklendirme kurallarını tetikleyici ve VM örneği, Ölçek sayısı artışları veya düşüşleri ayarladığınızda grafik ve olayları gösterir listeleyin.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yatay olarak ölçekleme ve artırmak veya azaltmak için otomatik ölçeklendirme kurallarını kullanmayı öğrendiniz *numarası* , Ölçek VM örnekleri kümesi. Ayrıca dikey olarak artırmak veya azaltmak VM örneği için ölçeklendirmek *boyutu*. Daha fazla bilgi için bkz: [dikey otomatik ölçeklendirme sanal makine ölçek kümeleri ile](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Üzerindeki VM örneklerinize yönetme hakkında daha fazla bilgi için bkz: [Yönet sanal makine ölçek ayarlar Azure PowerShell ile](virtual-machine-scale-sets-windows-manage.md).

Tetikleyici, otomatik ölçeklendirme kurallarını taktirde uyarı oluşturma konusunda bilgi almak için bkz: [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanmak](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Ayrıca [e-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için kullanım denetim günlüklerini](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
