---
title: Azure Güvenlik Merkezi'nde SQL bilgi koruması İlkesi özelleştirme | Microsoft Docs
description: Azure Güvenlik Merkezi'nde bilgi koruma ilkelerine özelleştirmeyi öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 2ebf2bc7-232a-45c4-a06a-b3d32aaf2500
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: 9b63fb963408b8f22453c7ea78e36a49402273a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60906165"
---
# <a name="customize-the-sql-information-protection-policy-in-azure-security-center-preview"></a>Azure Güvenlik Merkezi (Önizleme) SQL bilgi koruması ilkesinde özelleştirme
 
Bir SQL bilgi koruması ilkesi tanımlı ve Azure Güvenlik Merkezi'nde tüm Azure kiracınız için özelleştirilebilir.

Information protection, bulma, Sınıflandırma, etiketleme ve hassas verileri Azure veri kaynaklarınızı korumak için bir Gelişmiş bir güvenlik özelliğidir. Bulma ve sınıflandırma en hassas verileriniz (iş, Finans, sağlık, PII, vb.), kuruluş bilgilerini koruma stature içinde rol yürütebilirsiniz. Altyapı olarak hizmet eder:
- Veri gizliliği standartlarını ve yasal uyumluluk gereksinimlerini karşılamak yardımcı olma
- (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimi hakkında uyarı
- Son derece hassas verileri içeren erişimini denetleme ve verilerinin güvenliğini artırma depolar
 
[SQL bilgi koruması](../sql-database/sql-database-data-discovery-and-classification.md) şu anda Azure SQL veritabanı için desteklenen SQL veri depoları için bu paradigma uygular. SQL bilgi koruması otomatik olarak bulur ve hassas olabilecek verileri sınıflandırır, sınıflandırma özniteliklere sahip gizli verileri kalıcı olarak etiketleme için etiketleme bir mekanizma sağlar ve ayrıntılı bir panoya gösteren sağlar Sınıflandırma veritabanının durumu. Ayrıca, böylece hassas verileri ayıklamak sorguları açıkça denetlenebilir SQL sorguları duyarlılığına sonuç kümesi ve veri korumalı hesaplar. SQL bilgi koruması hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı veri bulma ve sınıflandırma](../sql-database/sql-database-data-discovery-and-classification.md).
 
Sınıflandırma mekanizması, sınıflandırma sınıflandırma - olun iki birincil yapılar dayalı **etiketleri** ve **bilgi türleri**.
- **Etiketleri** – sütunda depolanan verilerin duyarlılık düzeyi tanımlamak için kullanılan ana sınıflandırma öznitelikleri. 
- **Bilgi türleri** – sütunda depolanan verilerin türünü içine ek ayrıntı düzeyi sağlar.
 
Information Protection etiketleri ve varsayılan olarak kullanılan bilgi türleri, yerleşik bir dizi ile birlikte gelir. Bunlar özelleştirmek için Azure Güvenlik Merkezi'nde Information protection ilkesinin özelleştirebilirsiniz.
 
## <a name="customize-the-information-protection-policy"></a>Bilgi koruma ilkesini özelleştirme
Kiracınızda Azure Information protection ilkesinin özelleştirmek için ihtiyacınız [kiracının kök yönetim grubunda yönetim ayrıcalıkları](security-center-management-groups.md). 
 
1. Güvenlik Merkezi ana menüsünde seçin **Güvenlik İlkesi**.
2. Seçin **hiyerarşik görünümü (Önizleme)** ve ardından altındaki **Kiracı kök grubu**, tıklayın **ayarlarını Düzenle**.
 
   ![Information protection ilkesini yapılandırma](./media/security-center-info-protection-policy/security-policy.png) 
 
3. Altında **ilke bileşenleri**, tıklayın **Information protection**. İçinde **bilgi Koruması ayarlarını** sayfasında etiketleri geçerli kümesini görüntüleyebilirsiniz. Hassasiyet düzeyini verilerinizi kategorilere ayırmak için kullanılan ana sınıflandırma öznitelikleri şunlardır. Buradan, yapılandırabileceğiniz **Information protection etiketlerini** ve **bilgi türleri** Kiracı. 
 
### <a name="customizing-labels"></a>Etiketleri özelleştirme
 
1. Düzenleyebilir veya var olan bir etiketi silme veya yeni bir etiket ekleyebilirsiniz. Var olan bir etiketi düzenlemek için bu etiketi seçin ve ardından **yapılandırma**, üst veya sağ taraftaki bağlam menüsü. Yeni bir etiket eklemek için tıklatın **Oluştur etiket** üst menü çubuğundaki veya etiketleri tablonun alt kısmındaki.
2. İçinde **yapılandırma duyarlılık etiketi** ekranı oluşturabilir veya etiket adını ve açıklamasını değiştirin. Etiket getirerek etkin veya devre dışı olup olmadığını da ayarlayabilirsiniz **etkin** Aç veya kapat. Son olarak, ekleme veya etiketle ilişkili bilgi türlerini kaldırın. Sınıflandırma önerileri ilişkili duyarlılık etiketi, bilgi türünü otomatik olarak içerecektir eşleşen herhangi bir veri bulundu.
3. **Tamam** düğmesine tıklayın.
 
   ![Duyarlılık etiketini yapılandırın](./media/security-center-info-protection-policy/config-sensitivity-label.png)
 
4. Etiketleri duyarlılık artan, sırasına göre listelenir. Etiketler arasında sıralamasını değiştirmek için tablodaki yeniden sıralamak veya etiketlerin sürükleyin **Yukarı Taşı** ve **Aşağı Taşı** sırasını değiştirmek için düğmeler. 
 
    ![Information protection ilkesini yapılandırma](./media/security-center-info-protection-policy/move-up.png)
 
5. Tıkladığınızdan emin olun **Kaydet** işiniz bittiğinde ekranın üstünde.
 
 
## <a name="adding-and-customizing-information-types"></a>Ekleme ve bilgi türlerini özelleştirme
 
1. Tıklayarak bilgi türlerini özelleştirme ve yönetme **bilgi türlerini yönetme**.
2. Yeni bir **bilgi türü**seçin **bilgi türünü oluşturma** üst menüdeki. Ad, açıklama, yapılandırma ve desen dizeleri için arama **bilgi türü**. Arama deseni dizesi anahtar sütunların meta verileri temel alarak veritabanlarınızda hassas verileri tanımlamak için otomatik bulma altyapısı kullanan joker karakterler ('%' karakter kullanarak), isteğe bağlı olarak kullanabilirsiniz.
 
    ![Information protection ilkesini yapılandırma](./media/security-center-info-protection-policy/info-types.png)
 
3. Ayrıca yerleşik yapılandırabilirsiniz **bilgi türleri** bazı mevcut dizeleri devre dışı bırakma ek arama deseni dizeleri ekleyerek veya açıklamayı değiştirerek. Yerleşik nelze odstranit **bilgi türleri** veya adlarını düzenleyin. 
4. **Bilgi türlerini** , bulma derecelendirme türleri listede daha önce Eşleştir dener anlamına gelir, artan sıraya göre listelenmiştir. Derecelendirme bilgi türleri arasında değiştirmek için türleri tablo doğru yere sürükleyin veya kullanın **Yukarı Taşı** ve **Aşağı Taşı** sırasını değiştirmek için düğmeler. 
5. Tıklayın **Tamam** işiniz bittiğinde.
6. Bilgi türlerinizi yönetme tamamladıktan sonra tıklayarak ilgili etiketlerle ilgili türleri ilişkilendirdiğinizden emin olun **yapılandırma** belirli bir etiket ve uygun olarak bilgi türleri ekleme veya silme.
7. Tıkladığınızdan emin olun **Kaydet** ana **etiketleri** dikey penceresinde yaptığınız değişiklikleri uygulamak için.
 
Information protection ilkenizi tam olarak tanımlanan kaydedilir ve sonra kiracınızdaki tüm Azure SQL veritabanlarında veri sınıflandırmasını uygulanır.
 
 
## <a name="next-steps"></a>Sonraki adımlar
 
Bu makalede, Azure Güvenlik Merkezi'nde bir SQL bilgi koruması ilkesi tanımlama hakkında bilgi edindiniz. Sınıflandırmak ve SQL veritabanlarınızı hassas verileri korumak için SQL bilgi koruması kullanma hakkında daha fazla bilgi edinmek için [Azure SQL veritabanı veri bulma ve sınıflandırma](../sql-database/sql-database-data-discovery-and-classification.md). 

Güvenlik ilkeleri ve Azure Güvenlik Merkezi veri güvenliği hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
 
- [Güvenlik ilkelerine genel bakış](security-center-policies-overview.md): Güvenlik Merkezi'nde güvenlik ilkelerini genel bir bakış edinin
- [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi nasıl yönetir ve verileri koruyan öğrenin


