---
title: "Azure Güvenlik Merkezi güvenlik açıkları OS düzeltme | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir ** düzeltmek OS güvenlik açıkları **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 39879c22278a55f841e294cda5a89bec2bdf6988
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>İşletim sistemi güvenlik açıkları Azure Güvenlik Merkezi'nde Düzelt
Azure Güvenlik Merkezi günlük sanal makineleri (VM'ler) ve sanal makineleri hale getirebilecek bir yapılandırma için ve bilgisayarları daha savunmasız işletim sistemi (OS) çözümler. Güvenlik Merkezi, işletim sistemi yapılandırması önerilen yapılandırma kuralları eşleşmiyor ve bu güvenlik açıklarına değinen yapılandırma değişiklikleri önerir güvenlik açıkları gidermek önerir.

> [!NOTE]
> İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için bkz: [önerilen yapılandırma kuralları listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
İşletim sistemi düzeltme güvenlik açıkları, Güvenlik Merkezi'nde bir öneri olarak sunulur. Bu öneri altında görüntülenen **önerileri** ve altında **işlem**.

Bu örnekte ele alacağız **düzeltmek OS güvenlik açıklarından (Microsoft)** önerinin altında **işlem**.
1. Seçin **işlem** Güvenlik Merkezi ana menü altında.

   ![İşletim sistemi güvenlik açıklarını düzeltin][1]

2. Altında **işlem**seçin **düzeltmek OS güvenlik açıklarından (Microsoft)**. **İşletim sistemi güvenlik açıklarından (Microsoft) uyuşmazlığı** panosu açılır.

   ![İşletim sistemi güvenlik açıklarını düzeltin][2]

  Pano üstündeki sağlar:

  - VM'in ve bilgisayarın arasında işletim sistemi yapılandırması başarısız oldu önem kurallarıyla toplam sayısı.
  - VM'in ve bilgisayarın arasında işletim sistemi yapılandırması başarısız oldu türü kurallarıyla toplam sayısı.
  - Windows işletim sistemi yapılandırmalarınızı ve Linux işletim sistemi yapılandırmalarınızı tarafından başarısız kuralları toplam sayısı.

  Panonun alt Vm'leriniz ve bilgisayarlar ve eksik güncelleştirmenin önem tüm başarısız kuralları listeler. Liste aşağıdakileri içerir:

  - **CCEID**: CCE kuralı için benzersiz tanımlayıcı. Güvenlik Merkezi, yapılandırma kuralları için benzersiz tanımlayıcı atamak için Common Configuration Enumeration (CCE) kullanır.
  - **AD**: başarısız kural adı
  - **KURAL türü**: kayıt defteri anahtarı, güvenlik ilkesi ya da Denetim İlkesi
  - **VM Sanal makineleri &AMP; bilgisayarları**: toplam sayısı VM'ler ve başarısız çizgili bilgisayarlar için geçerlidir
  - **Kural önem**: kritik, önemli ya da uyarı CCE önem derecesi değeri
  - **DURUM**: Önerinin geçerli durumu:

    - **Açık**: Öneri henüz ele alınmadı
    - **Devam eden**: öneri şu anda bu kaynaklara uygulanıyor ve sizin tarafınızdan herhangi bir eylem gerekli
    - **Çözüldü**: Öneri daha önce tamamlandı. (Sorunu Çözümlendi, giriş soluk görüntülenir)

3. Başarısız bir kural ayrıntılarını görüntülemek için listeden seçin.

   ![Başarısız olan yapılandırma kuralları][3]

  Aşağıdaki bilgiler bu dikey pencerede sağlanır:

  - ADI--Kural adı
  - CCIED--CCE kuralı için benzersiz bir tanımlayıcı
  - İşletim sistemi sürümü-VM veya bilgisayar işletim sistemi sürümü
  - Kuralı önem--Kritik, önemli ya da uyarı CCE önem derecesi değeri
  - TAM açıklama--Kuralı açıklaması
  - Güvenlik AÇIĞI - Açıklama güvenlik açığı veya kural uygulanmamış durumunda riski
  - OLASI etkisini--kuralı uygulandığında iş etkisi
  - Karşı önlem – düzeltme adımları
  - BEKLENEN değer--Güvenlik Merkezi kural karşı VM OS yapılandırmanızı inceler, beklenen değer
  - Gerçek değer--VM OS yapılandırmanızı kural karşı analizini sonra değer döndürdü.
  - Kural işlemi--VM OS yapılandırmanızın kural karşı Çözümleme sırasında Güvenlik Merkezi tarafından kullanılan kuralı işlemi

4. Seçin **arama** üst Şerit simgesi. Arama VM'ler ve seçili işletim sistemi güvenlik açığı bilgisayarlarla sahip listeleme çalışma alanları açar. Bu çalışma alanı seçimi dikey yalnızca seçilen kuralı farklı çalışma alanlarına bağlanan birden çok VM için geçerli olup olmadığını gösterilir.

  ![Listelenen çalışma alanları][4]

5. Bir çalışma alanı seçin. Günlük analizi arama sorgusu çalışma alanına filtrelenmiş ile işletim sistemi Güvenlik Açığı'nı açar.

  ![İşletim sistemi güvenlik açığı ile çalışma][5]

6. Daha fazla bilgi için listeden bir bilgisayar seçin. Bu bilgisayar için yalnızca filtrelenen bilgilerle başka bir arama sonucu açar.

  ![Bu bilgisayar için filtrelenmiş][6]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede "Düzelt işletim sistemi güvenlik açıkları." Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir Yapılandırma kuralları gözden geçirebilirsiniz [burada](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Güvenlik Merkezi, yapılandırma kuralları için benzersiz tanımlayıcı atamak için CCE (ortak yapılandırma sıralaması) kullanır. Ziyaret [CCE](https://nvd.nist.gov/cce/index.cfm) daha fazla bilgi için site.

Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Güvenlik Merkezi'nde desteklenen platformlar](security-center-os-coverage.md) -desteklenen Windows ve Linux VM'ler listesini sağlar.
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemek öğrenin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) -yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) -iş ortağı çözümlerinizin sistem durumunu öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) -Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
