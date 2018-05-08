---
title: Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimlerini kullanarak Azure VM'lerinde çalışan uygulamaları beyaz listeye eklemenize yardımcı olur.
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/15/2018
ms.author: yurid
ms.openlocfilehash: 841dbbf97a7fa25aa001636ba92cc2a966be4908
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri (Önizleme)
Bu kılavuzu kullanarak Azure Güvenlik Merkezi'ndeki uygulama denetimi özelliklerini yapılandırmayı öğrenebilirsiniz.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimleri nelerdir?
Uyarlamalı uygulama denetimleri, Azure'da yer alan VM'lerinizde çalışabilecek uygulamaları denetlemenize ve bu sayede VM'lerinizi kötü amaçlı yazılımlara karşı korumanıza yardımcı olur. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan uygulamaları analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur. Bu özellik, uygulama beyaz listelerini yapılandırma ve bakımını yapma sürecini önemli ölçüde kolaylaştırarak şunları yapmanızı sağlar:

- Kötü amaçlı yazılımdan koruma yazılımlarının tespit edemeyecekleri de dahil olmak üzere kötü amaçlı uygulamaları çalıştırma girişimlerini engelleme veya bu durumlarda uyarı düzenleme
- Kuruluşunuzun yalnızca lisanslı yazılım kullanımını gerektiren kuruluş güvenlik ilkelerine uygun hareket etme.
- Ortamınızda istenmeyen yazılımların kullanılmasını önleme.
- Eski ve desteklenmeyen uygulamaların çalışmasını önleme.
- Kuruluşunuzda kullanılmasına izin verilmeyen belirli yazılım araçlarını engelleme.
- BT ekibinin uygulama üzerinden gizli verilere erişimi denetlemesini mümkün kılma.

## <a name="how-to-enable-adaptive-application-controls"></a>Uyarlamalı uygulama denetimleri nasıl etkinleştirilir?
Uyarlamalı uygulama denetimleri, yapılandırılmış kaynak gruplarında çalıştırılmasına izin verilen bir uygulama kümesi tanımlamanıza yardımcı olur. Bu özellik yalnızca Windows makinelerde kullanılabilir (tüm sürümler, klasik veya Azure Resource Manager). Güvenlik Merkezi'nde uygulama beyaz listesini yapılandırmak için aşağıdaki adımları kullanabilirsiniz:

1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmeden **Gelişmiş bulut savunması** altında bulunan **Uyarlamalı uygulama denetimlerini** seçin.

    ![Savunma](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

**Uyarlamalı uygulama denetimleri** sayfası açılır.

![denetimler](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

**VM grupları** bölümünde üç sekme bulunur:

* **Yapılandırılan**: Uygulama denetimiyle yapılandırılan VM’leri içeren grupların listesidir.
* **Önerilen**: Uygulama denetiminin önerildiği grupların listesidir. Güvenlik Merkezi makine öğrenimi özelliklerini kullanarak VM'lerin tutarlı bir şekilde aynı uygulamaları çalıştırıp çalıştırmadığına bakar ve uygulama denetimi için uygun olan VM'leri tanımlar.
* **Öneri olmayan**: Uygulama denetimi önerisi olmayan VM’leri içeren grupların listesidir. Örneğin, uygulamaların sürekli değiştiği ve kararlı bir duruma geçmediği VM'ler.

> [!NOTE]
> Güvenlik Merkezi benzer VM’lerin önerilen en iyi uygulama denetimi ilkesini alması için VM grupları oluşturan özel bir kümeleme algoritması kullanır.
>
>

### <a name="configure-a-new-application-control-policy"></a>Yeni bir uygulama denetim ilkesi yapılandırma
1. Uygulama denetimi önerileri bulunan grupların listesi için **Önerilen** sekmesine tıklayın:

  ![Önerilen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

  Liste aşağıdakileri içerir:

  - **AD**: Aboneliğin ve grubun adı
  - **VM'ler**: Grup içindeki sanal makine sayısı
  - **DURUM**: Önerilerin durumu, çoğu durumda açık olacaktır
  - **ÖNEM DERECESİ**: Önerilerin önem derecesi

2. **Uygulama denetimi kuralları oluştur** seçeneğini açmak için bir grup seçin.

  ![Uygulama denetimi kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. **VM'leri Seç** bölümünde önerilen VM'lerin listesini gözden geçirin ve uygulama denetimi gerçekleştirmek istemediklerinizin yanındaki onay işaretini kaldırın. Daha sonra iki liste görürsünüz:

  - **Önerilen uygulamalar**: Bu gruptaki VM’lerde yaygın olan ve bu nedenle Güvenlik Merkezi tarafından uygulama denetim kuralları için önerilen uygulamaların bir listesi.
  - **Daha fazla uygulama**: Bu gruptaki VM’lerde daha seyrek olan veya Açıklardan Yararlanılabilir olarak bilinen (daha fazla bilgi aşağıdadır) ve kurallar uygulanmadan önce gözden geçirilmesi önerilen uygulamaların bir listesi.

4. Her bir listedeki uygulamaları gözden geçirin ve uygulamak istemediklerinizin işaretini kaldırın. Her liste aşağıdakileri içerir:

  - **AD**: Bir uygulamanın sertifika bilgileri veya tam uygulama yolu
  - **DOSYA TÜRLERİ**: Uygulama dosya türü. Bu EXE, Script veya MSI olabilir.
  - **AÇIKLARDAN YARARLANABİLİR**: Uygulamaların, uygulama beyaz listesini atlamak için bir saldırgan tarafından kullanılma ihtimali olması halinde bir uyarı simgesi görünür. Bu uygulamaları onaylamadan önce gözden geçirmeniz önerilir.
  - **KULLANICILAR**: Bir uygulama çalıştırmasına izin verilmesi önerilen kullanıcılar

5. Seçimlerinizi tamamladıktan sonra **Oluştur**’u seçin.

Güvenlik Merkezi uygulama denetimini her zaman varsayılan olarak *Denetim* modunda çalıştırır. Beyaz listenin iş yükünüzü olumsuz etkilemeyeceği doğrulandıktan sonra *Zorunlu kıl* modunu seçebilirsiniz.

Güvenlik Merkezi, temel yapılandırma oluşturmak ve VM gruplarına benzersiz öneri sunmak için en az iki haftalık veri kullanmaktadır. Güvenlik Merkezi standart katmanının yeni müşterileri başlangıçta VM gruplarının *öneri yok* sekmesi altında olduğunu görebilir.

> [!NOTE]
> Güvenlik Merkezi, en iyi güvenlik deneyimini sunmak üzere beyaz listeye alınması gereken uygulamalar için her zaman bir yayımcı kuralı oluşturmaya çalışır ve yalnızca yayımcı bilgisi olmayan (imzalanmış olmayan) uygulamalara ait EXE dosyalarının tam yolu için bir yol kuralı oluşturulur.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Uygulama denetimiyle yapılandırılmış bir grubu düzenleme ve izleme

1. Uygulama denetimiyle yapılandırılmış grubu düzenleyip izlemek için **Uyarlamalı uygulama denetimleri** sayfasına geri dönüp **VM Grupları** altından **YAPILANDIRILMIŞ** seçeneğini belirleyin:

  ![Gruplar](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  Liste aşağıdakileri içerir:

  - **AD**: Aboneliğin ve grubun adı
  - **VM'ler**: Grup içindeki sanal makine sayısı
  - **MOD**: Denetim modu beyaz listeye alınmamış uygulamaların çalıştırma girişimlerini günlüğe kaydeder; Zorla modu ise beyaz listeye alınmamış uygulamaların çalışmasına izin vermez
  - **SORUNLAR**: Herhangi bir geçerli ihlal

2. Bir grubu seçtikten sonra **Uygulama denetim ilkesini düzenle** sayfasında değişiklik yapabilirsiniz.

  ![Koruma](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. **Koruma modu** altında aşağıdakilerden birini seçebilirsiniz:

  - **Denetim**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılmaz ve yalnızca korumalı VM'lerdeki etkinliği denetler. Bu, bir uygulamanın hedef VM'de çalışmasını engellemeden önce genel davranışını gözlemlemek istediğiniz senaryolar için önerilir.
  - **Zorunlu kıl**: Bu modda uygulama denetimi çözümü kuralları zorunlu kılar ve çalışmasına izin verilmeyen uygulamaların engellenmesini sağlar.

  Yukarıda belirtildiği gibi yeni uygulama denetimi ilkeleri her zaman *Denetim* modunda yapılandırılır. **İlke uzantısı** bölümünde beyaz listeye eklemek istediğiniz uygulamaların yollarını ekleyebilirsiniz. Bu yolları eklediğinizde Güvenlik Merkezi ilgili uygulamalar için uygun kuralları oluşturur ve mevcut kurallara ekler.

  **Son Sorunlar** bölümünde geçerli ihlaller listelenir.

  ![Sorunlar](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

  Bu liste aşağıdakileri içerir:
  - **SORUNLAR**: Günlüğe kaydedilmiş tüm ihlaller burada yer alır ve bunlar aşağıdakiler olabilir:

      - **ViolationsBlocked**: Çözüm, Zorunlu kıl modunda çalıştırıldığında ve beyaz listede yer almayan bir uygulama yürütülmeye çalıştığında.
      - **ViolationsAudited**: Çözüm Denetim modunda çalıştırıldığında ve beyaz listede yer almayan bir uygulama yürütüldüğünde.

 - **VM SAYISI**: Bu sorun türündeki sanal makinelerin sayısı.

  Bu satırlardan her birine tıkladığınızda açılan [Azure Etkinlik Günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) sayfasında bu ihlalin bulunduğu tüm VM'ler hakkında bilgi alabilirsiniz. Satır sonundaki üç noktaya tıklayarak ilgili girişi silebilirsiniz. **Yapılandırılmış sanal makineler** bölümünde bu kuralların geçerli olduğu VM'lerin listesi yer alır.

  ![Yapılandırılmış sanal makineler](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

  **Beyaz listeye yayımcı ekleme kuralları** altındaki liste şunları içerir:

  - **KURAL**: Her bir uygulama için bulunan sertifika bilgileri kullanılarak yayımcı kuralı oluşturulan uygulamalar listelenir
  - **DOSYA TÜRÜ**: Belirli bir yayıncı kuralı tarafından kapsanan dosya türleri. Bu, şunlardan herhangi biri olabilir: EXE, Betik veya MSI.
  - **KULLANICILAR**: Her uygulamayı çalıştırma izni verilen kullanıcıların sayısı

  Daha fazla bilgi için bkz. [Applocker'daki Yayımcı Kurallarını Kavrama](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker).

  ![Beyaz listeye ekleme kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

  Satır sonundaki üç noktaya tıklayarak ilgili kuralı silebilir veya izin verilen kullanıcıları düzenleyebilirsiniz.

  **Beyaz listeye yol ekleme kuralları** bölümünde, bir dijital sertifika ile imzalanmamış ancak beyaz listeye ekleme kurallarında yer alan uygulamalar için uygulama yolunun tamamı (belirli dosya türü dahil) listelenir.

  > [!NOTE]
  > Varsayılan olarak Güvenlik Merkezi, en iyi güvenlik deneyimini sunmak üzere beyaz listeye alınması gereken EXE dosyaları için her zaman bir yayımcı kuralı oluşturmaya çalışır ve yalnızca yayımcı bilgisi olmayan (imzalanmış olmayan) EXE dosyalarının tam yolu için bir yol kuralı oluşturulur.

  ![Beyaz listeye yol ekleme kuralları](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

  Liste aşağıdakileri içerir:
  - **AD**: Yürütülebilir dosyanın tam yolu
  - **DOSYA TÜRÜ**: Belirli bir yol kuralı tarafından kapsanan dosya türleri. Bu, şunlardan herhangi biri olabilir: EXE, Betik veya MSI.
  - **KULLANICILAR**: Her uygulamayı çalıştırma izni verilen kullanıcıların sayısı

  Satır sonundaki üç noktaya tıklayarak ilgili kuralı silebilir veya izin verilen kullanıcıları düzenleyebilirsiniz.

4. **Uyarlamalı uygulama denetimleri** sayfasında değişiklik yaptıktan sonra **Kaydet** düğmesine tıklayın. Değişiklikleri uygulamamaya karar verirseniz, **At** seçeneğine tıklayın.

### <a name="not-recommended-list"></a>Önerilmeyenler listesi

Güvenlik Merkezi uygulama beyaz listeye ekleme özelliğini yalnızca kararlı bir uygulama kümesi çalıştıran sanal makineler için önerir. Bağlantılı VM'lerdeki uygulamalar sürekli değişiyorsa öneriler oluşturulmayacaktır.

![Öneri](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Liste aşağıdakileri içerir:
- **AD**: Aboneliğin ve grubun adı
- **VM'ler**: Grup içindeki sanal makine sayısı

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Güvenlik Merkezi'ndeki uyarlamalı uygulama denetimlerini kullanarak Azure VM'lerinde çalışan uygulamaları beyaz listeye eklemeyi öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
