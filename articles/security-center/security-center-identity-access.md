---
title: "Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme | Microsoft Docs"
description: "Kullanıcılarınızın erişim etkinliğini ve kimlikle ilgili sorunları izleme amacıyla Azure Güvenlik Merkezi'ndeki tanımlama ve erişim özelliklerini nasıl kullanacağınızı öğrenin."
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
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: 842045fbcb5b4f661cc006a207f4087370a7b4ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-identity-and-access-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme
Bu makale kullanıcılarınızın kimliğini ve erişim etkinliğini izleme amacıyla Azure Güvenlik Merkezi'ni kullanmanıza yardımcı olur.

## <a name="why-monitor-identity-and-access"></a>Kimlik ve erişim neden izlenir?
Kimlik, kuruluşunuz için denetim düzlemi olmalıdır, kimliğinizi korumak ise en yüksek önceliğiniz olmalıdır. Geçmişte kuruluşların etrafında duvarlar vardı ve bu duvarlar birincil savunma hattının bir parçasıydı. Günümüzde buluta taşınan veri ve uygulama miktarı arttıkça kimlik yeni savunma hattı haline geliyor.

Kimlik etkinliklerini izleyerek bir olay gerçekleşmeden önce öngörülü eylemlerde veya bir saldırı girişimini durdurmak için reaktif eylemlerde bulunabilirsiniz. Kimlik ve Erişim panosu aşağıdakiler dahil olmak üzere kimlik durumunuza genel bakış sağlar:

* Başarısız oturum açma girişimi sayısı. 
* Bu girişimler sırasında kullanılan kullanıcı hesapları.
* Kilitlenmiş hesaplar.
* Parolası değiştirilmiş veya sıfırlanmış hesaplar. 
* Oturum açmış olan hesap sayısı.

## <a name="monitor-identity-and-access-activities"></a>Kimlik ve erişim etkinliklerini izleme
Kimlikle ve erişimle ilgili geçerli etkinlikleri görmek için **Kimlik ve Erişim** panosuna erişmeniz gerekir.

1. **Güvenlik Merkezi** panosunu açın.

2. Sol taraftaki bölmede, **Önleme**'nin altında yer alan **Kimlik ve Erişim**'i seçin. Birden fazla çalışma alanınız varsa çalışma alanı seçici açılır.

    ![Çalışma alanı seçimi](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > En sağdaki sütunda **PLANI YÜKSELT** görünüyorsa çalışma alanı ücretsiz aboneliği kullanmaktadır. Bu özelliği kullanmak için Standart aboneliğe yükseltin. En sağdaki sütunda **YÜKSELTME GEREKİYOR** görünüyorsa bu özelliği kullanmak için [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)'i yükseltin. Fiyatlandırma planı hakkında daha fazla bilgi için Güvenlik Merkezi fiyatlandırma sayfasını inceleyin. 
    > 
3. Araştırmanız gereken birden fazla çalışma alanı varsa **BAŞARISIZ OTURUM AÇMA SAYISI** sütununa göre araştırmada öncelik belirleyebilirsiniz. Bu sayı, bu çalışma alanındaki geçerli başarısız oturum açma girişimi sayısını gösterir. Kullanmak istediğiniz çalışma alanını seçtiğinizde **Kimlik ve Erişim** panosu açılır.

    ![Kimlik ve Erişim](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. Bu panoda mevcut olan bilgiler, olası bir şüpheli etkinliğini tanımlamanız için size hemen yardımcı olabilir. Pano üç ana bölüme ayrılır:

    a. **Kimlik duruşu**. Bu çalışma alanında gerçekleşen kimlikle ilgili etkinliklerin özetini gösterir.

    b. **Başarısız oturum açma sayısı**. Başarısız oturum açma girişimlerinin temel nedenini hızlı bir şekilde tanımlamanıza yardımcı olur. Başarısız oturum açma girişimi sayısı en fazla olan 10 hesabı gösterir.

    c. **Zaman içinde oturum açma işlemleri**. Zaman içindeki oturum açma işlemi sayısını hızlı bir şekilde tanımlamanıza yardımcı olur. En fazla sayıda oturum açma girişiminin yapıldığı bilgisayarların listesini gösterir.
    
Belirlediğiniz kutucuktan bağımsız olarak görünen pano Günlük Araması sorgusuna bağlıdır. Tek fark, sorgu türü ve sonuçtur. Yine de bilgisayar gibi bir öğe seçebilir ve ilgili verileri görebilirsiniz. 

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede Güvenlik Merkezi'nde kimlik ve erişim izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi'nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi sorun giderme kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin. 
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını bulun.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

