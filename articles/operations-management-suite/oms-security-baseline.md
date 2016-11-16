---
title: "Operations Management Suite Güvenlik ve Denetim Çözümü Temeli | Microsoft Belgeleri"
description: "Bu belgede; uyumluluk ve güvenlik amaçlarıyla, izlenen tüm bilgisayarlarda bir temel değerlendirmesinin gerçekleştirilmesi için OMS Güvenlik ve Denetim çözümünün nasıl kullanılacağı açıklanmaktadır."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1c3e2cf86a33f9bbe6b34f4f52b82a078b91661f


---
# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Operations Management Suite Güvenlik ve Denetim Çözümünde Temel Değerlendirmesi
Bu belge, izlenen kaynaklarınızın güvenlik durumuna erişmek için [Operations Management Suite (OMS) Güvenlik ve Denetim Çözümü](operations-management-suite-overview.md) temel değerlendirmesi özelliklerini kullanmanıza yardımcı olur.

## <a name="what-is-baseline-assessment"></a>Temel Değerlendirmesi nedir?
Dünya çapındaki sektör ve devlet kuruluşlarıyla birlikte Microsoft, yüksek güvenlikli sunucu dağıtımlarını temsil eden bir Windows yapılandırması tanımlar. Bu yapılandırma, kayıt defteri anahtarlarının, denetim ilkesi ayarlarının ve güvenlik ilkesi ayarlarının yanı sıra Microsoft'un bu ayarlar için önerilen değerlerinden oluşan bir kümedir. Bu kural kümesi, Güvenlik temeli olarak bilinir. OMS Güvenlik ve Denetim temeli değerlendirmesiyle tüm bilgisayarlarınız uyumluluk açısından sorunsuz bir biçimde taranır. 

Üç kural türü mevcuttur:

* **Kayıt defteri kuralları**: Kayıt defteri anahtarlarının doğru şekilde ayarlanıp ayarlanmadığını denetler.
* **Denetim ilkesi kuralları**: Denetim ilkenize ilişkin kurallardır.
* **Güvenlik ilkesi kuralları**: Kullanıcının makinedeki izinlerine ilişkin kurallardır.

> [!NOTE]
> Bu özelliğe ilişkin kısa bir genel bakış için [Güvenlik Yapılandırma Temelini değerlendirmek için OMS Güvenlik'i kullanma](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) bölümünü okuyun.
> 
> 

## <a name="security-baseline-assessment"></a>Güvenlik Temeli Değerlendirmesi
Panoyu kullanarak, OMS Güvenlik ve Denetim tarafından izlenen tüm bilgisayarlar için geçerli güvenlik temeli değerlendirmenizi gözden geçirebilirsiniz.  Güvenlik temeli değerlendirmesine ilişkin panoya erişmek için aşağıdaki adımları uygulayın:

1. **Microsoft Operations Management Suite** ana panosunda, **Güvenlik ve Denetim** kutucuğuna tıklayın.
2. **Güvenlik ve Denetim** panosunda, **Güvenlik Etki Alanları** bölümünde **Temel Değerlendirmesi**'ne tıklayın. **Güvenlik Temeli Değerlendirmesi** panosu aşağıdaki görüntüde gösterildiği gibi görünür:
   
    ![OMS Güvenlik ve Denetim Temeli Değerlendirmesi](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Bu pano üç ana bölüme ayrılır:

* **Bilgisayarların temel ile karşılaştırması**: Bu bölüm, erişilen bilgisayarların sayısının ve değerlendirmede başarılı olan bilgisayarların yüzdesinin bir özetini sağlar. Bu ayrıca en başarılı 10 bilgisayarı ve değerlendirmeye ilişkin yüzde sonucunu da belirtir.
* **Gerekli Kurallar Durumu**: Bu bölüm, başarısız kuralların önem derecesinin ve türünün bildirilmesi amacını taşır. İlk grafiğe baktığınızda başarısız kuralların birçoğunun kritik önemde olup olmadığını hızlı bir şekilde tespit edebilirsiniz. Bu ayrıca en başarısız 10 kuralın ve bunların önem derecesinin bir listesini de sağlar. İkinci grafik, değerlendirme sırasında başarısız olan kuralın türünü gösterir. 
* **Temel değerlendirmesi eksik bilgisayarlar**: Bu bölümde işletim sistemi uyumsuzluğu veya hatalar nedeniyle erişilmemiş bilgisayarların listesi sağlanır. 

### <a name="accessing-computers-compared-to-baseline"></a>Bilgisayarların temel ile karşılaştırmasına erişme
İdeal olarak tüm bilgisayarlarınızın, güvenlik temeli değerlendirmesiyle uyumlu olması gerekir. Ancak bazı durumlarda bunun olmaması olağandır. Güvenlik yönetimi işleminin bir parçası olarak, tüm güvenlik değerlendirmesi testlerinde başarısız olan bilgisayarların gözden geçirilmesi önemlidir. Bu yöntem, **Bilgisayarların temel ile karşılaştırması** bölümünde yer alan **Erişilen bilgisayarlar** seçeneği belirlendiğinde hızlıca görselleştirilebilir. Şu ekranda görüldüğü gibi, bilgisayarların listesini gösteren günlük arama sonucunu göreceksiniz:

![Erişilen bilgisayar sonuçları](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Arama sonucu bir tablo biçiminde görüntülenir; burada ilk sütun bilgisayar adını, ikinci sütun ise başarısız olmuş kuralların sayısını gösterir. Başarısız olmuş kural türü ile ilgili bilgi almak için bilgisayar adının yanındaki başarısız kural sayısına tıklayın. Aşağıdaki görüntüde gösterilene benzer bir sonuç göreceksiniz:

![Erişilen bilgisayar sonuçlarına ilişkin ayrıntılar](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Bu arama sonucunda, erişilen kuralların toplamına, başarısız olan kritik önemdeki kuralların sayısına, uyarı kurallarına ve başarısız kurallara ilişkin bilgilere ulaşırsınız.

### <a name="accessing-required-rules-status"></a>Gerekli kurallar durumuna erişme
Değerlendirmede başarılı olan bilgisayarların yüzde değeriyle ilgili bilgi aldıktan sonra öneme göre hangi kuralların başarısız olduğuna ilişkin daha fazla bilgi edinmek isteyebilirsiniz. Bu görselleştirme, sonraki değerlendirme için uyumlu olmalarını sağlamak üzere öncelikle hangi bilgisayarların ele alınacağını belirlemenize yardımcı olur. **Önem derecesine göre başarısız olan kurallar** kutucuğunda, **Gerekli kurallar durumu** altında yer alan grafiğin Kritik bölümü üzerine gelin ve tıklayın. Aşağıdaki ekranda gösterilene benzer bir sonuç göreceksiniz:

![Önem derecesine göre başarısız olan kurallara ilişkin ayrıntılar](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

Bu günlük sonucunda; başarısız olan temel kuralının türünü, bu kuralın açıklamasını ve bu güvenlik kuralının Common Configuration Enumeration (CCE) kimliğini görebilirsiniz. Bu öznitelikler, hedef bilgisayarda bu sorunun giderilmesine yönelik bir düzeltme eyleminin gerçekleştirilmesi için yeterli olacaktır.

> [!NOTE]
> CCE hakkında daha fazla bilgi için [National Vulnerability Database](https://nvd.nist.gov/cce/index.cfm)'e (Ulusal Güvenlik Açıkları Veritabanı) erişin.
> 
> 

### <a name="accessing-computers-missing-baseline-assessment"></a>Temel değerlendirmesi eksik bilgisayarlara erişim
OMS, Windows Server 2008 R2'den Windows Server 2012 R2'ye kadar etki alanı üyesi temel profilini destekler. Windows Server 2016 temeli, henüz tamamlanmamıştır ve yayımlandıktan hemen sonra eklenecektir. OMS Güvenlik ve Denetim temeli değerlendirmesiyle taranan tüm diğer işletim sistemleri **Temel değerlendirmesi eksik bilgisayarlar** bölümünde görünür.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede OMS Güvenlik ve Denetim temeli değerlendirmesi hakkında bilgi edindiniz. OMS Güvenlik hakkında daha fazla bilgi edinmek için şu makalelere göz atın:

* [Operations Management Suite'e (OMS) genel bakış](operations-management-suite-overview.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Güvenlik Uyarılarını İzleme ve Yanıtlama](oms-security-responding-alerts.md)
* [Operations Management Suite Güvenlik ve Denetim Çözümünde Kaynakları İzleme](oms-security-monitoring-resources.md)




<!--HONumber=Nov16_HO2-->


