---
title: Otomatik ölçeklendirme sanal makine ölçek kümeleri Azure Portalı'nda | Microsoft Docs
description: Azure Portalı'nda nasıl sanal makine ölçek için otomatik ölçeklendirme kuralları oluşturmak ayarlar
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 648bc0295cd5435e9c3e44f33b7ae80522fa8e0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60618889"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Sanal makine ölçek kümesi Azure Portalı'nda otomatik olarak ölçeklendirme
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz sanal makine örneği sayısını tanımlarsınız. Uygulamanızın talebi değiştikçe, sanal makine örneklerinin sayısını otomatik olarak artırabilir veya azaltabilirsiniz. Otomatik ölçeklendirme özelliği, uygulamanızın yaşam döngüsü boyunca uygulama performansındaki değişikliklere veya müşteri taleplerine ayak uydurmanıza olanak tanır.

Bu makalede Azure portalında ölçek kümenizdeki VM örneği performansını izleme, otomatik ölçeklendirme kuralları oluşturma işlemini gösterir. Bu otomatik ölçeklendirme kuralları, artırın veya bu performans ölçümlerine yanıt olarak sanal makine örneği sayısını azaltın. Bu adımlar da tamamlayabilirsiniz [Azure PowerShell](tutorial-autoscale-powershell.md) veya [Azure CLI](tutorial-autoscale-cli.md).


## <a name="prerequisites"></a>Önkoşullar
Otomatik ölçeklendirme kuralları oluşturmak için mevcut bir sanal makine gerekir. ölçek kümesi. Bir ölçek kümesi oluşturabilirsiniz [Azure portalında](quick-create-portal.md), [Azure PowerShell](quick-create-powershell.md), veya [Azure CLI](quick-create-cli.md).


## <a name="create-a-rule-to-automatically-scale-out"></a>Otomatik olarak ölçeğini genişletmek için kural oluşturma
Uygulamanızın talebi artarsa, ölçek kümenizdeki sanal makine örneklerinde üzerindeki yük de artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. CPU veya disk gibi hangi ölçümlerin izleneceğini, uygulama yükünün belirli bir eşiği ne kadar süre karşılaması gerektiği ve ölçek kümesine kaç sanal makine örneği ekleneceğini denetlersiniz.

1. Azure portal ve select açın **kaynak grupları** panosunun sol taraftaki menüden.
2. Ölçek kümenizi içeren ve ardından kaynak listesinden ölçek kümenizi seçin kaynak grubunu seçin.
3. Seçin **ölçeklendirme** ölçeğin sol taraftaki menüden penceresi ayarlayın. Düğmeyi seçin **etkinleştirmek otomatik ölçeklendirme**:

    ![Azure portalında otomatik ölçeklendirmeyi etkinleştir](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Gibi ayarlarınız için bir ad girin *otomatik ölçeklendirme*, ardından seçeneğini **alınabilecek**.

5. Ortalama CPU yükü bir 10 dakika boyunca % 70'in çıktığında bir ölçek kümesindeki sanal makine örneği sayısını artıran bir kural oluşturalım. Kural tetiklendiğinde, sanal makine örneği sayısını 20 oranında artar. Ölçek kümeleri küçük bir sanal makine örneği sayısını'de, ayarladığınız **işlemi** için *sayıyı şu kadar Artır* belirtin *1* veya *2* için *örnek sayısı*. Çok sayıda sanal makine örnekleri, % %10 20 veya bir artış ile ölçek kümeleri, VM örnekleri daha uygun olabilir.

    Kural için aşağıdaki ayarları belirtin:
    
    | Parametre              | Açıklama                                                                                                         | Değer          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Zaman toplama*     | Toplanan ölçümlerin analiz için nasıl bir araya getirileceğini tanımlar.                                                | Ortalama        |
    | *Ölçüm adı*          | İzlenecek ve ölçek kümesi eylemlerinin uygulanmasında temel alınacak performans ölçümü.                                                   | CPU yüzdesi |
    | *Zaman dilimi İstatistiği* | Her zaman dilimi içinde toplanan ölçümlerin analiz için nasıl araya getirileceğini tanımlar.                             | Ortalama        |
    | *İşleci*             | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                     | Büyüktür   |
    | *Eşik*            | Otomatik ölçeklendirme kuralının bir eylemi tetiklemesine neden olan yüzdesi.                                                 | 70             |
    | *Süresi*             | Ölçüm ve eşik değerleri karşılaştırılmadan önce izleme yapılacak süre.                                   | 10 dakika     |
    | *İşlem*            | Yukarı veya aşağı kural geçerli olduğunda ve hangi artış ölçek kümesinin ölçeğinin büyütüleceğini tanımlar                        | Yüzdeyi şu kadar artır: |
    | *Örnek sayısı*       | Kural tetiklendiğinde değiştirilmesi gereken sanal makine örneklerinin yüzdesi.                                            | 20             |
    | *Seyrek erişimli (dakika)*  | Otomatik ölçeklendirme eylemlerinin geçerli olması için kural tekrar uygulanmadan önceki bekleme süresi. | 5 dakika      |

    Aşağıdaki örnekler, bu ayarları eşleşen Azure portalında oluşturulan bir kural gösterir:

    ![Sanal makine örneği sayısını artırmak için bir otomatik ölçeklendirme kuralı oluşturma](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Bir kural oluşturmak için Seç **Ekle**


## <a name="create-a-rule-to-automatically-scale-in"></a>Otomatik ölçeklendirme için kural oluşturma
Bir akşam veya hafta sonu uygulama talebiniz azalabilir. Yük belirli bir süreye yayılarak tutarlı şekilde azalıyorsa, ölçek kümesindeki sanal makine örneği sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Mevcut talebi karşılamak için gerekli örnek sayısını yalnızca siz çalıştırdığınızdan, bu ölçeği daraltma eylemi ölçek kümenizi çalıştırma maliyetini azaltır.

1. Tercih **alınabilecek** yeniden.
2. Ortalama CPU yükü ardından 10 dakikalık dönem boyunca % 30 'un altına düşünceye çıktığında bir ölçek kümesindeki sanal makine örneği sayısını azaltır bir kural oluşturun. Kural tetiklendiğinde, sanal makine örneği sayısını 20 oranında azalır.

    Önceki bir kural olarak aynı yaklaşımı kullanın. Kural için aşağıdaki ayarları ayarlayın:
    
    | Parametre              | Açıklama                                                                                                          | Değer          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *İşleci*             | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                      | Şu değerden az:   |
    | *Eşik*            | Otomatik ölçeklendirme kuralının bir eylemi tetiklemesine neden olan yüzdesi.                                                 | 30             |
    | *İşlem*            | Yukarı veya aşağı kural geçerli olduğunda ve hangi artış ölçek kümesinin ölçeğinin büyütüleceğini tanımlar                         | Yüzdeyi şu kadar azalt: |
    | *Örnek sayısı*       | Kural tetiklendiğinde değiştirilmesi gereken sanal makine örneklerinin yüzdesi.                                             | 20             |

3. Bir kural oluşturmak için Seç **Ekle**


## <a name="define-autoscale-instance-limits"></a>Otomatik ölçeklendirme örneği sınırlarını tanımlama
Otomatik ölçeklendirme profilinizi, minimum, maksimum ve varsayılan sanal makine örneği sayısını tanımlamanız gerekir. Otomatik ölçeklendirme kurallarınızı uygulandığında, bu örneği sınırları ötesinde örnekleri veya minimum örneği ötesinde ölçek kümesinde en fazla sayısını ölçeği değil, emin olun.

1. Aşağıdaki örnek limitleri ayarlayın:

    | Minimum | Maksimum | Varsayılan|
    |---------|---------|--------|
    | 2       | 10      | 2      |

2. Örnek limitleri ve otomatik ölçeklendirme kurallarını uygulamak için seçin **Kaydet**.


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Bir ölçek kümesindeki örneklerin sayısı İzleyicisi
Sayısı ve sanal makine örneklerinin durumunu görmek için seçin **örnekleri** ölçeğin sol taraftaki menüden penceresi ayarlayın. Durum, sanal makine örneği olup olmadığını gösterir *oluşturma* ölçek kümesini otomatik olarak Ölçeklendirmesi eşitlenene veya devre dışı olduğundan *silme* içinde ölçeği otomatik olarak ölçeklenen gibi.

![Bir ölçek kümesi sanal makine örneklerinin listesini görüntülemek](media/virtual-machine-scale-sets-autoscale-portal/view-instances.png)


## <a name="autoscale-based-on-a-schedule"></a>Bir zamanlamaya göre otomatik ölçeklendirme
Önceki örneklerde, bir ölçek daraltma veya genişletme CPU kullanımı gibi temel konak ölçümleri kümesi otomatik olarak Ölçeklendirildi. Zamanlamalara göre temel otomatik ölçeklendirme kuralları oluşturabilirsiniz. Zamanlama tabanlı kurallar çekirdek çalışma saatleri gibi uygulama talep beklenen bir artış önceden sanal makine örneği sayısını otomatik olarak ölçeği ve örnek sayısını daha az tahmin anda otomatik ölçeklendirme için izin ver hafta sonu gibi talep.

1. Seçin **ölçeklendirme** ölçeğin sol taraftaki menüden penceresi ayarlayın. Önceki örneklerde oluşturulan mevcut otomatik ölçeklendirme kurallarını silmek için Çöp Kutusu simgesini seçin.

    ![Mevcut otomatik ölçeklendirme kurallarını Sil](media/virtual-machine-scale-sets-autoscale-portal/delete-rules.png)

2. Tercih **ölçeklendirme koşulu Ekle**. Kural adı yanındaki kalem simgesini seçin ve gibi bir ad verin *her iş günü ölçeğini genişletme*.

    ![Varsayılan otomatik ölçeklendirme kuralı yeniden adlandırma](media/virtual-machine-scale-sets-autoscale-portal/rename-rule.png)

3. Radyo düğmesini seçin **belirli bir örnek sayısına ölçeklendirin**.
4. Örnek sayısını ölçeklendirmek için girin *10* örnek sayısı olarak.
5. Seçin **belirli günlerde Yinele** için **zamanlama** türü.
6. Tüm iş günlerini, Pazartesi ile Cuma günleri seçin.
7. Uygun bir saat dilimi seçin, ardından belirtin bir **başlangıç zamanı** , *09:00*.
8. Tercih **ölçeklendirme koşulu Ekle** yeniden. Adlı bir zamanlama oluşturmak için işlemi tekrarlayın *ölçeğini Akşam sırasında* ölçekler için *3* örnekler, hafta içi her gün yinelenen ve başlar *18:00*.
9. Zamanlama tabanlı otomatik ölçeklendirme kurallarınızı uygulamak için seçin **Kaydet**.

    ![Bir zamanlamaya göre ölçeğini otomatik ölçeklendirme kuralları oluşturma](media/virtual-machine-scale-sets-autoscale-portal/schedule-autoscale.PNG)

Otomatik ölçeklendirme kurallarınızı nasıl uygulandığını görmek için seçin **çalıştırma geçmişi** üstündeki **ölçeklendirme** penceresi. Otomatik ölçeklendirme kurallarını tetikleme ve ölçek kümenizdeki VM örneği sayısını artışları veya düşüşleri ayarladığınızda graf ve olayları gösterir listeleyin.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yatay olarak genişletmek ve artırmak veya azaltmak için otomatik ölçeklendirme kuralları kullanmayı öğrendiniz *numarası* ölçek VM örnekleri kümesi. Ayrıca dikey olarak artırabilir veya azaltabilirsiniz VM örneğine ölçeklendirebilirsiniz *boyutu*. Daha fazla bilgi için [sanal makine ölçek kümeleri ile dikey otomatik ölçeklendirme](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Sanal makine örnekleriniz yönetme hakkında daha fazla bilgi için bkz: [Yönet sanal makine ölçek kümeleri Azure PowerShell ile](virtual-machine-scale-sets-windows-manage.md).

Tetikleyici, otomatik ölçeklendirme kuralları taktirde uyarı oluşturma konusunda bilgi almak için bkz: [e-posta ve Web kancası, Azure İzleyici'de uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın](../azure-monitor/platform/autoscale-webhook-email.md). Ayrıca [e-posta ve Web kancası, Azure İzleyici'de uyarı bildirimleri göndermek için Denetim günlükleri kullanın](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
