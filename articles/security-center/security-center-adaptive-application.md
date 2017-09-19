---
title: "Azure Güvenlik Merkezi'ndeki Uyarlamalı Uygulama Denetimleri | Microsoft Docs"
description: "Bu belge, Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimlerini kullanarak Azure VM'lerinde çalışan uygulamaları beyaz listeye eklemenize yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 18ae6a970455646b7a25170f5abefa52a98b0ba2
ms.contentlocale: tr-tr
ms.lasthandoff: 09/14/2017

---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Azure Güvenlik Merkezi'ndeki Uyarlamalı Uygulama Denetimleri (Önizleme)
Bu kılavuzu kullanarak Azure Güvenlik Merkezi'ndeki uygulama denetimi özelliklerini yapılandırmayı öğrenebilirsiniz.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri nelerdir?
Uyarlamalı uygulama denetimleri, Azure'da yer alan VM'lerinizde çalışabilecek uygulamaları denetlemenize ve bu sayede VM'lerinizi kötü amaçlı yazılımlara karşı korumanıza yardımcı olur. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan işlemleri analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur. Bu özellik, uygulama beyaz listelerini yapılandırma ve bakımını yapma sürecini önemli ölçüde kolaylaştırarak şunları yapmanızı sağlar:

- Kötü amaçlı yazılımdan koruma yazılımlarının tespit edemeyecekleri de dahil olmak üzere kötü amaçlı uygulamaları çalıştırma girişimlerini engelleme veya bu durumlarda uyarı düzenleme
- Kuruluşunuzun yalnızca lisanslı yazılım kullanımını gerektiren kuruluş güvenlik ilkelerine uygun hareket etme.
- Ortamınızda istenmeyen yazılımların kullanılmasını önleme.
- Eski ve desteklenmeyen uygulamaların çalışmasını önleme.
- Kuruluşunuzda kullanılmasına izin verilmeyen belirli yazılım araçlarını engelleme.
- BT ekibinin uygulama üzerinden gizli verilere erişimi denetlemesini mümkün kılma.

## <a name="how-to-enable-adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri nasıl etkinleştirilir?
Uyarlamalı uygulama denetimleri, yapılandırılmış kaynak gruplarında çalıştırılmasına izin verilen bir uygulama kümesi tanımlamanıza yardımcı olur. Bu özellik yalnızca Windows makinelerde kullanılabilir (tüm sürümler, klasik veya Azure Resource Manager). Güvenlik Merkezi'nde uygulama beyaz listesini yapılandırmak için aşağıdaki adımları izleyin:

1.  **Güvenlik Merkezi** panosunu açın ve **Genel Bakış**'a tıklayın.
2.  **Gelişmiş bulut savunması**'nın altındaki **Uyarlamalı uygulama denetimleri** kutucuğunda toplam VM sayısı ve denetim altında olan VM sayısı gösterilir. Kutucukta ayrıca son bir hafta içinde bulunan sorun sayısı da görüntülenir: 

    ![Uyarlamalı uygulama denetimleri](./media/security-center-adaptive-application\security-center-adaptive-application-fig1.png)

3. Daha fazla seçenek için **Uyarlamalı uygulama denetimleri** kutucuğuna tıklayın.

    ![denetimler](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. Kaynak Grupları bölümünde üç sekme bulunur:
    * **Önerilen**:  Uygulama denetiminin önerildiği kaynak grubu listesi. Güvenlik Merkezi makine öğrenimi özelliklerini kullanarak VM'lerin tutarlı bir şekilde aynı uygulamaları çalıştırıp çalıştırmadığına bakar ve uygulama denetimi için uygun olan VM'leri tanımlar.
    * **Yapılandırılmış**: Uygulama denetimi ile yapılandırılmış olan VM'leri içeren kaynak gruplarının listesi. 
    * **Öneri yok**: Uygulama denetimi önerisi bulunmayan VM'leri içeren kaynak gruplarının listesi. Örneğin, uygulamaların sürekli değiştiği ve kararlı bir duruma geçmediği VM'ler.

Aşağıdaki bölümlerde seçenekler ve kullanım yöntemleri ayrıntılı olarak ele alınmıştır.

### <a name="configure-a-new-application-control-policy"></a>Yeni bir uygulama denetim ilkesi yapılandırma
Uygulama denetimi önerileri bulunan kaynak gruplarının listesi için **Önerilen** sekmesine tıklayın:

![Önerilen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

Liste aşağıdakileri içerir:
- **AD**: Aboneliğin ve kaynak grubunun adı
- **VM'ler**: Kaynak grubu içindeki sanal makine sayısı
- **DURUM**: Önerilerin durumu, çoğu durumda açık olacaktır
- **ÖNEM DERECESİ**: Önerilerin önem derecesi

**Uygulama denetimi kuralları oluştur** seçeneğini açmak için bir kaynak grubunu seçin:

![Uygulama denetimi kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

**VM'leri Seç** bölümünde önerilen VM'lerin listesini gözden geçirin ve uygulama denetimi gerçekleştirmek istemediklerinizin yanındaki onay işaretini kaldırın. **Beyaz listeye ekleme kuralları için işlemleri seçin** bölümünde önerilen uygulamaların listesini gözden geçirin ve uygulamak istemediklerinizin yanındaki onay işaretini kaldırın. Liste aşağıdakileri içerir:

- **AD**: Uygulamanın tam yolu
- **İŞLEMLER**: Her yolda bulunan uygulama sayısı
- **ORTAK**: Doğru değeri, bu işlemlerin bu kaynak grubundaki VM'lerin çoğunda yürütüldüğünü gösterir.
- **AÇIKLARDAN YARARLANABİLİR**: Uygulamaların, uygulama beyaz listesini atlamak için bir saldırgan tarafından kullanılma ihtimali olması halinde bir uyarı simgesi görünecektir. Bu uygulamaların onay verilmeden önce mutlaka gözden geçirilmesi önerilir. 

Seçimlerinizi tamamladıktan sonra **Oluştur** düğmesine tıklayın. Güvenlik Merkezi uygulama denetimini her zaman varsayılan olarak *Denetim* modunda çalıştırır. Beyaz listenin iş yükünüzü olumsuz etkilemeyeceği doğrulandıktan sonra *Zorunlu kıl* modunu seçebilirsiniz.

> [!NOTE]
> Güvenlik Merkezi en iyi deneyim olarak beyaz listeye alınması gereken uygulamalar için bir yayımcı kuralı oluşturmaya çalışacak ve yalnızca yayımcı bilgisi olmayan (imzalanmış olmayan) uygulamalara ait EXE dosyalarının tam yolu için bir yol kuralı oluşturulacaktır.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Uygulama denetimiyle yapılandırılmış bir grubu düzenleme ve izleme

Uygulama denetimiyle yapılandırılmış bir grubu düzenlemek ve izlemek için **Kaynak Grupları**'nın altındaki **YAPILANDIRILMIŞ** seçeneğine tıklayın:

![Kaynak grupları](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

Liste aşağıdakileri içerir:

- **AD**: Aboneliğin ve kaynak grubunun adı
- **VM'ler**: Kaynak grubu içindeki sanal makine sayısı
- **MOD**: Denetim modu beyaz listeye alınmamış uygulamaların çalıştırma girişimlerini günlüğe kaydeder; Engelle modu ise beyaz listeye alınmamış uygulamaların çalışmasına izin vermez
- **ÖNEM DERECESİ**: Önerilerin önem derecesi

Bir kaynak grubunu seçtikten sonra **Uygulama denetim ilkesini düzenle** sayfasında değişiklik yapabilirsiniz.

![Koruma](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

**Koruma Modu** altında aşağıdaki seçeneklerden birini belirleyebilirsiniz:
- **Denetim**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılmaz ve yalnızca korumalı VM'lerdeki etkinliği denetler. Bu, bir uygulamanın hedef VM'de çalışmasını engellemeden önce genel davranışını gözlemlemek istediğiniz senaryolar için önerilir.
- **Zorunlu kıl**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılar ve çalışmasına izin verilmeyen uygulamaların engellenmesini sağlar. 

Yukarıda belirtildiği gibi yeni uygulama denetimi ilkeleri her zaman *Denetim* modunda yapılandırılır. **İlke uzantısı** bölümünde beyaz listeye eklemek istediğiniz uygulamaların yollarını ekleyebilirsiniz. Bu yolları eklediğinizde Güvenlik Merkezi ilgili uygulamalar için uygun kuralları oluşturur ve mevcut kurallara ekler. **Sorunlar** bölümünde geçerli ihlaller listelenir.

![Sorunlar](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Bu liste aşağıdakileri içerir:

- **SORUNLAR**: Kaydedilmiş tüm ihlaller burada yer alır ve bunlar aşağıdakiler olabilir:
    - **ViolationsBlocked**: Çözüm, Zorunlu kıl modunda çalıştırıldığında ve beyaz listede yer almayan bir uygulama yürütülmeye çalıştığında.
    - **ViolationsAudited**: Çözüm Denetim modunda çalıştırıldığında ve beyaz listede yer almayan bir uygulama yürütüldüğünde.
    - **RulesViolatedManually**: Bir kullanıcı ASC yönetim portalı yerine VM'lerde el ile kural yapılandırmayı denediğinde.
- **VM’LER SAYISI**: Bu sorun türündeki sanal makinelerin sayısı.

Bu satırlardan birine tıkladığınızda açılacak [Azure Etkinlik Günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) sayfasında bu ihlalin bulunduğu tüm VM'ler hakkında bilgi alabilirsiniz. Satır sonundaki üç noktaya tıklayarak ilgili girişi silebilirsiniz. **Yapılandırılmış sanal makineler** bölümünde bu kuralların geçerli olduğu VM'lerin listesi yer alır. 

![Yapılandırılmış sanal makineler](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

**Beyaz listeye yayımcı ekleme kuralları** bölümünde, her bir uygulama için bulunan sertifika bilgileri kullanılarak yayımcı kuralı oluşturulan uygulamalar listelenir. Daha fazla bilgi için bkz. [Applocker'daki Yayımcı Kurallarını Kavrama](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker).

![Beyaz listeye ekleme kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Satır sonundaki üç noktaya tıklayarak ilgili kuralı silebilirsiniz. **Beyaz listeye yol ekleme kuralları** bölümünde, bir dijital sertifika ile imzalanmamış ancak beyaz listeye ekleme kurallarında yer alan uygulamalar için uygulama yolunun tamamı (yürütülebilir dahil) listelenir. 

> [!NOTE]
> Varsayılan ayarlarda Güvenlik Merkezi en iyi deneyim olarak beyaz listeye alınması gereken EXE dosyaları için bir yayımcı kuralı oluşturmaya çalışacak ve yalnızca yayımcı bilgisi olmayan (imzalanmış olmayan) EXE dosyalarının tam yolu için bir yol kuralı oluşturulacaktır.

![Beyaz listeye yol ekleme kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

Liste aşağıdakileri içerir:
- **AD**: Yürütülebilir dosyanın tam yolu
- **AÇIKLARDAN YARARLANABİLİR**: Uygulamaların, uygulama beyaz listesini atlamak için bir saldırgan tarafından kullanılma ihtimali olması halinde doğru ifadesi görünecektir.  

Satır sonundaki üç noktaya tıklayarak ilgili kuralı silebilirsiniz. Değişiklikleri yaptıktan sonra **Kaydet** düğmesine veya değişiklikleri uygulamak istemezseniz **At**'a tıklayabilirsiniz.

### <a name="not-recommended-list"></a>Önerilmeyenler listesi

Güvenlik Merkezi uygulama beyaz listeye ekleme özelliğini yalnızca kararlı bir uygulama kümesi çalıştıran sanal makineler için önerir. Bağlantılı VM'lerdeki uygulamalar sürekli değişiyorsa öneriler oluşturulmayacaktır. 

![Öneri](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Liste aşağıdakileri içerir:
- **AD**: Aboneliğin ve kaynak grubunun adı.
- **VM'ler**: Kaynak grubu içindeki sanal makine sayısı.

## <a name="preview-registration"></a>Önizleme kaydı

Uyarlamalı uygulama denetimleri Azure Güvenlik Merkezi Standart müşterilerine sınırlı genel önizleme olarak sunulmuştur. Önizlemeye katılmak için lütfen abonelik kimliklerinizi [bize](mailto:ASC_appcontrol@microsoft.com) e-posta ile gönderin.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimlerini kullanarak Azure VM'lerinde çalışan uygulamaları beyaz listeye eklemeyi öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin. 
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.


