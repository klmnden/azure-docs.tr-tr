---
title: "Azure Güvenlik Merkezi'nde Kimliği ve Erişimi İzleme | Microsoft Docs"
description: "Bu belge kullanıcılarınızın erişim etkinliğini ve kimlikle ilgili sorunları izleme amacıyla Azure Güvenlik Merkezi'ndeki tanımlama ve erişim özelliklerini kullanmanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 2e351cd38e7baefc09fa36ceabffec92de22433b
ms.contentlocale: tr-tr
ms.lasthandoff: 09/14/2017

---
# <a name="monitoring-identity-and-access-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Kimliği ve Erişimi İzleme
Bu belge kullanıcılarınızın kimliğini ve erişim etkinliğini izleme amacıyla Azure Güvenlik Merkezi'ni kullanmanıza yardımcı olur.

## <a name="why-monitor-identity-and-access"></a>Kimlik ve erişim neden izlenir?
Kimlik, kuruluşunuz için denetim düzlemi olmalıdır, kimliğinizi korumak ise en yüksek önceliğiniz olmalıdır. Geçmişte kuruluşlar etrafında çevresel alanlar vardı ve bu alanlar birincil savunma sınırları olarak görev yapardı, ancak günümüzde giderek daha fazla veri ve uygulamanın buluta taşınmasıyla birlikte, kimlik yeni çevresel alan haline geldi.

Kimlik etkinliklerinizi izleyerek bir olay gerçekleşmeden önce öngörülü eylemlerde veya bir saldırı girişimini durdurmak için reaktif eylemlerde bulunabilirsiniz. Kimlik ve Erişim panosu, size hatalı oturum açma girişimlerinin sayısını, bu girişimler sırasında kullanılan kullanıcı hesabını, kilitlenmiş hesapları, parolaları değiştirilen veya sıfırlanan hesapları ve şu anda oturum açmış olan hesap sayısını içerecek şekilde, kimlik durumunuza ilişkin bir genel bakış sağlar.

## <a name="how-to-monitor-identity-and-access-activities"></a>Kimlik ve erişim etkinlikleri nasıl izlenir?
Kimlikle ve erişimle ilgili geçerli etkinlikleri görselleştirmek için aşağıdaki adımları izleyin. **Kimlik ve Erişim** panosuna erişmeniz gerekir:

1.  **Güvenlik Merkezi** panosunu açın.
2.  Sol taraftaki bölmede, **Önleme**'nin altında yer alan **Kimlik ve Erişim**'e tıklayın. Birden fazla çalışma alanınız varsa çalışma alanı seçici açılır.

    ![çalışma alanı seçimi](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Son sütunda **PLANI YÜKSELT** ifadesinin görüntülenmesi bu çalışma alanının ücretsiz abonelik kullandığını ve bu özelliği kullanmak için standart sürüme yükseltmeniz gerektiğini gösterir. YÜKSELTME GEREKİYOR ifadesinin görüntülenmesi bu özelliği kullanmak için [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)'i güncelleştirmeniz gerektiğini gösterir. Fiyatlandırma planı hakkında daha fazla bilgi için Azure Güvenlik Merkezi fiyatlandırma sayfasını inceleyin. 
    > 
3. Araştırmanız gereken birden fazla çalışma alanınız varsa araştırmada ilgili çalışma alanındaki başarısız oturum açma sayısını gösteren **BAŞARISIZ OTURUM AÇMA SAYISI** sütununa göre öncelik belirleyebilirsiniz. Kullanmak istediğiniz çalışma alanını seçtiğinizde **Kimlik ve Erişim** panosu açılır.

    ![kimlik ve erişim](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. Bu panoda mevcut olan bilgiler, olası bir şüpheli etkinliğini tanımlamanız için size hemen yardımcı olabilir. Bu pano üç ana bölüme ayrılır:
    * **Kimlik duruşu**: Bu çalışma alanında gerçekleşen kimlikle ilgili etkinliklerin özetini gösterir.
    * **Başarısız oturum açma sayısı**: Başarısız oturum açma girişiminin ana nedenini hızlıca tanımlamanıza yardımcı olur ve başarısız oturum açma sayısı en fazla olan on hesap gösterilir.
    * **Zaman içindeki oturum açma sayısı**: Zaman içindeki oturum açma sayısını hızlıca tanımlamanıza yardımcı olur ve en fazla oturum açma girişiminde bulunan bilgisayar hesaplarının listesi görüntülenir.
    
Seçtiğiniz kutucuktan bağımsız olarak açılan pano [Günlük Araması](https://docs.microsoft.com/azure/security-center/security-center-search) sorgusunu temel alır ve tek fark, sorgu türü ile sonuç olacaktır. Yine de bilgisayar gibi bir öğe seçebilir, üzerine tıklayabilir ve ilgili verileri görebilirsiniz. 

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde kimlik ve erişim izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin. 
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.


