---
title: Gözcü Azure önizlemesiyle uyarıları araştırma | Microsoft Docs
description: Azure Gözcü uyarıları araştırma hakkında bilgi edinmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: rkarlin
ms.openlocfilehash: 319ec5d09a6daddb5c1fc36f680ee6d0d856e337
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205421"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Öğretici: Azure Önizleme Gözcü ile tehditleri algılayın

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Çalıştırdıktan sonra [veri kaynaklarınıza bağlı](quickstart-onboard.md) şüpheli bir şey olduğunda size bildirilmesini istiyorsanız Azure Gözcü için. Bunu sağlamak için oluşturduğunuz Azure Gözcü atayabileceğiniz çalışmaları üreten uyarı kuralları ve kullanım anomalileri ve ortamınızdaki tehditlere derin bir şekilde araştırmak için Gelişmiş sağlar. 

Bu öğretici Azure Gözcü ile tehditleri algılamanıza yardımcı olur.
> [!div class="checklist"]
> * Algılama kuralları oluşturma
> * Tehditlere yanıt verme

## <a name="create-detection-rules"></a>Algılama kuralları oluşturma

Çalışmalarını araştırmak için öncelikle algılama kuralları oluşturmanız gerekir. 

> [!NOTE]
> Aracılığıyla Azure Gözcü içinde oluşturulan uyarılar kullanılabilir [Microsoft Graph güvenlik](https://aka.ms/securitygraphdocs). Başvurmak [Microsoft Graph güvenlik uyarıları belgeleri](https://aka.ms/graphsecurityreferencebetadocs) daha ayrıntılı bilgi ve tümleştirme iş ortakları için.

Algılama kuralları bunlar ortaya, araştırılması düzeltilebilir ve sağlama türleri tehditleri ve ortamınızda hemen hakkında bilmek istediğiniz şüpheli olabilecek anormallikleri dayanır. 

1. Azure portalında Azure Gözcü altında seçin **Analytics**.

   ![Analiz](./media/tutorial-detect-threats/alert-rules.png)

2. Üst menü çubuğunda, **+ Ekle**.  

   ![Uyarı kuralı oluştur](./media/tutorial-detect-threats/create-alert-rule.png)

3. Altında **uyarı kuralı oluştur**, açıklayıcı bir ad verin ve ayarlayın **önem derecesi** gerektiği şekilde. 

4. Log Analytics'te sorgu oluşturun ve içine yapıştırın **kümesi uyarı kuralı** alan. Aşağıda, anormal bir kaynak sayısı, Azure etkinlik oluşturulduğunda uyarı bir örnek sorgu verilmiştir.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

5. İçinde **varlık eşlemesi** bölümünde, altındaki alanları kullanın **varlık türü** Azure Gözcü tarafından tanınan bir varlığın alanları sorgunuzu sütunları eşlemek için. Her alan için Log Analytics'te ilgili varlık alanına oluşturulan sorgu ilgili sütunda eşleyin. İlgili sütun adı altında seçin **özelliği**. Her varlığın birden çok alan, örneğin SID, GUID, vb. içerir. Alanlar, yalnızca üst düzey varlığı göre varlık eşleyebilirsiniz.

6. Altında uyarı tetikleme koşullarını tanımlayın **uyarı tetikleme**. Bu, uyarıyı tetikleyen koşulları tanımlar. 

7. Ayarlama **sıklığı** için ne sıklıkla sorgu - olarak günde bir kez seyrek veya sık olarak 5 dakikada bir olarak çalıştırılır. 

8. Ayarlama **süresi** zaman penceresi için sorguyu çalıştırır - üzerinde veri miktarını denetlemek için örneğin, saatte 60 dakika arasında veri çalıştırabilirsiniz.

9. Ayrıca **gizleme**. Gizleme yinelenen uyarılar için aynı olayı tetiklenmekte gelen durdurmak istediğinizde kullanışlıdır. Bu şekilde, belirli bir dönemde tetiklenmekte gelen uyarılar durdurabilirsiniz. Bu, aynı olay için yinelenen uyarıları önlemek ve bir süre için ardışık uyarıları bastırmak izin yardımcı olabilir. Örneğin, varsa **zamanlama uyarı** **sıklığı** 60 dakika olarak ayarlanmıştır ve **zamanlama süresi uyarı** iki saat olarak ayarlanır ve sorgu sonuçları tanımlanan aşılan Eşik, tetikleyen bir uyarı iki kez olduğunda, öncelikle son 60 dakika boyunca algılandığında ve yeniden örnekleniyor veri 2 saatlik ilk 60 dakika içinde olduğunda. Bir uyarının, gizleme süre içinde uyarı süresini ayarlama olması gerektiğini öneririz. Böylece en son bir saat boyunca gerçekleşen olayları için uyarıları yalnızca tetiklenen Bizim örneğimizde, 60 dakika gizleme ayarlamak isteyebilirsiniz.

8. Sonra Query'ye yapıştırın **kümesi uyarı kuralı** alan, uyarının bir simülasyon hemen görebilirsiniz **mantıksal uyarı benzetimi** böylece anlamak ne kadar veriler elde edebilirsiniz belirli bir zaman aralığında oluşturduğunuz uyarı oluşturulur. Bu ne için ayarladığınız üzerinde bağlıdır **sıklığı** ve **eşiği**. Ortalama olarak, uyarıyı çok sık tetiklenip olduğunu görürseniz, ortalama temel olacak şekilde daha yüksek sonuç sayısı ayarlamak istersiniz.

9. Tıklayın **Oluştur** , uyarı kuralı başlatılamadı. Uyarı oluşturulduktan sonra bir durumda uyarıyı içeren oluşturulur. İçinde satır olarak tanımlanmış olan algılama kuralları görebilirsiniz **güvenlik analizleri** sekmesi. Her bir kural - eşleşmelerini tetiklenen uyarıların sayısını da görebilirsiniz. Bu listeden etkinleştirebilir, devre dışı bırakın veya her kuralını Sil. Siz de sağ tıklayarak düzenlemek, devre dışı bırakmak, kopyalama, eşleşmeleri göstermek veya bir kuralı silmek her bir uyarı için satırın sonundaki üç nokta (...) seçin. **Analytics** sayfasıdır tüm etkin uyarı kuralları, bir galeri şablonları dahil olmak üzere etkinleştirmeniz ve uyarı kuralları şablonlarına dayalı oluşturun.

1. Uyarı kuralları sonuçlarını görülebilir **çalışmaları** sayfası olduğu önceliklendirme, [çalışmalarını araştırmak](tutorial-investigate-cases.md), ve bu tehditleri ortadan kaldıracak.



## <a name="respond-to-threats"></a>Tehditlere yanıt verme

Azure Sentinel playbook'ları kullanarak tehditlerine yanıt verme için iki birincil seçenek sunar. Playbook bir uyarı tetiklenir ya da bir uyarıya yanıt olarak bir playbook el ile çalıştırabilirsiniz otomatik olarak çalışacak şekilde ayarlayabilirsiniz.

- Playbook playbook yapılandırdığınızda, bir uyarı tetiklendiğinde otomatik olarak çalışacak şekilde ayarlayın. 

- El ile gelen bir playbook uyarı içine tıklayarak çalıştırın **playbook'ları görüntüleme** ve sonra çalıştırılacak bir playbook seçerek.




## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Gözcü kullanarak tehditleri algılama başlama öğrendiniz. 

Yanıtlarınız tehditlere otomatikleştirme hakkında bilgi edinmek için [otomatik playbook'ları kullanarak tehditleri nasıl](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Tehditleri](tutorial-respond-threats-playbook.md) tehditleri verdiğiniz yanıtları otomatik hale getirmek için.

