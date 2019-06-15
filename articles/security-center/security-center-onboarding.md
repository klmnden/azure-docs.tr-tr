---
title: Azure Güvenlik Merkezi standart Gelişmiş güvenlikten yararlanmaya başlamak için Onboarding | Microsoft Docs
description: " Bilgi nasıl için Azure Güvenlik Merkezi standart yerleşik Gelişmiş Güvenlik. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 19/02/2019
ms.author: v-mohabe
ms.openlocfilehash: fe8ceb8c196f7329027502847fba481169458d86
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65966794"
---
# <a name="onboarding-to-azure-security-center-standard-for-enhanced-security"></a>Gelişmiş güvenlikten yararlanmaya başlamak için Azure Güvenlik Merkezi standart ekleme
Gelişmiş güvenlik yönetimi ve tehdit koruması için hibrit bulut iş yüklerinizi yararlanmak için Güvenlik Merkezi standart yükseltin.  Standart ücretsiz deneyebilirsiniz. Güvenlik Merkezi'ni [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/) daha fazla bilgi için.

Güvenlik Merkezi standart içerir:

- **Karma güvenlik** – tüm şirket içi güvenlik birleşik bir görünümünü elde ve bulut iş yükleri. Güvenlik ilkeleri uygulayın ve güvenlik standartlarıyla uyumluluğu sağlamak için karma bulut iş yüklerinizin güvenliğini sürekli değerlendirin. Güvenlik duvarları ve diğer iş ortaklarının çözümleri gibi farklı kaynaklardan güvenlik verileri toplayın, bunlar üzerinde arama ve analiz gerçekleştirin.
- **Gelişmiş tehdit algılama** -Gelişmiş analiz ve Microsoft Intelligent Security gelişen siber saldırılardan üzerinden kenar almak için Graph kullanın.  Yerleşik davranış analizi ve makine öğrenimi özelliklerinden yararlanarak saldırıları ve sıfır gün saldırılarına yol açabilecek güvenlik açıklarını tespit edin. Ağları, makineleri ve bulut hizmetlerini gelen saldırılara veya güvenlik ihlali sonrası etkinliklere karşı izleyin. Etkileşimli araçlar ve bağlama dayalı tehdit zekası ile araştırmaları kolaylaştırın.
- **Erişim ve uygulama denetimlerini** -kendi iş yüklerinize göre uyarlanmış ve makine öğrenimi tarafından desteklenen blok kötü amaçlı yazılım ve diğer istenmeyen uygulamaları beyaz listeye alma önerilerini uygulayarak. Ağ saldırı yüzeyini tam zamanında, denetimli erişimi ile yönetim bağlantı noktalarına önemli ölçüde deneme yanılma ve diğer ağ saldırılarına maruz kalma riskinizi azaltır, Azure sanal makinelerinde azaltın.

## <a name="detecting-unprotected-resources"></a>Korumasız kaynaklara algılama     
Güvenlik Merkezi, Güvenlik Merkezi Standart sürümü için etkinleştirilmemiş herhangi bir Azure aboneliğini veya çalışma alanını otomatik olarak algılar. Buna, Güvenlik Merkezi Ücretsiz sürümünü kullanan Azure abonelikleri ve etkin bir Güvenlik çözümü olmayan çalışma alanları dahildir.

Tüm Azure aboneliğinin, abonelik içindeki tüm desteklenen kaynaklar tarafından devralınır standart katmana yükseltebilirsiniz. Standart uygulama katmanında bir çalışma alanı için çalışma alanınıza raporlayan tüm kaynakları uygular.

> [!NOTE]
> Maliyetlerinizi yönetin ve belirli bir aracılar kümesi için sınırlayarak bir çözüm için toplanan veri miktarını sınırlamak isteyebilirsiniz. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) çözüm için bir kapsam geçerli ve hedef çalışma alanındaki bilgisayarların alt ağlarından olanak tanır.  Çözüm hedefleme kullanıyorsanız, Güvenlik Merkezi çalışma alanına sahip olmayan bir çözüm olarak listeler.
>
>

## <a name="upgrade-an-azure-subscription-or-workspace"></a>Bir Azure aboneliğini veya çalışma alanını yükseltme
Standart abonelik veya çalışma alanını yükseltmek için:
1. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
  ![Başlarken](./media/security-center-onboarding/get-started.png)
2. **Yükselt** altında, Güvenlik Merkezi, ekleme işlemi için uygun abonelikleri ve çalışma alanlarını listeler. 
   - Tüm abonelikleri ve çalışma alanlarını deneme sürümü uygunluk durumlarıyla birlikte listelemek için, genişletilebilir **Deneme sürümünüzü uygulayın**’a tıklayabilirsiniz.
   -    Deneme sürümü için uygun olmayan abonelikler ve çalışma alanlarını yükseltebilirsiniz.
   -    Deneme sürümünüzü başlatmak için uygun çalışma alanlarını ve abonelikleri seçebilirsiniz.
3.  Seçili aboneliklerde deneme sürümünüzü başlatmak için **Deneme sürümünü başlat**’a tıklayın.
  ![Abonelik seçin](./media/security-center-onboarding/select-subscription.png)


   > [!NOTE]
   > Güvenlik Merkezi'nin ücretsiz özellikleri yalnızca Azure Vm'lerinizden ve VMSS için uygulanır. Ücretsiz özellikleri, Azure dışı bilgisayarlarınızı uygulanmaz. Standart'ı seçerseniz, tüm Azure Vm'leri, VM ölçek kümeleri ve Azure olmayan bilgisayarlar çalışma alanına raporlama standart yetenekleri uygulanır. Azure ve Azure dışı kaynaklar için Gelişmiş güvenliği sağlamak için standart uygulamanızı öneririz.
   >
   >

## <a name="onboard-non-azure-computers"></a>Azure dışı bilgisayarları ekleme
Güvenlik Merkezi, Azure dışı bilgisayarların güvenlik durumunu izleyebilir ancak öncelikle bu kaynakları eklemeniz gerekir. Azure dışı bilgisayarlar ekleyebilirsiniz **Başlarken** dikey veya **işlem** dikey penceresi. Her iki yöntem alacağız.

### <a name="add-new-non-azure-computers-from-getting-started"></a>Yeni Azure olmayan bilgisayarlardan ekleme **kullanmaya başlama**

1. Geri dönüp **Başlarken**.   
2. **Başlangıç** sekmesini seçin.

   ![Azure dışı](./media/security-center-onboarding/non-azure.png)

3. **Yeni Azure dışı bilgisayarlar ekle** altında, **Yapılandır**’a tıklayın. Log Analytics çalışma alanlarınızın bir listesi gösterilir. Listede, varsa, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulan varsayılan çalışma alanı bulunur. Bu çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.

   ![Azure olmayan bilgisayar ekleme][7]

Mevcut çalışma alanınız varsa bunlar altında listelenen **yeni Azure olmayan bilgisayar ekleme**. Mevcut bir çalışma alanına bilgisayar eklemek veya yeni bir çalışma alanı oluşturun. Yeni bir çalışma alanı oluşturmak için bağlantıyı seçin **yeni bir çalışma alanı Ekle**.

### <a name="add-new-non-azure-computers-from-compute"></a>Yeni Azure olmayan bilgisayarlardan ekleme **işlem**

**Yeni bir çalışma alanı oluşturun ve bilgisayar ekleme**

1. Altında **yeni Azure olmayan bilgisayar ekleme**seçin **yeni bir çalışma alanı Ekle**.

   ![Yeni bir çalışma alanı Ekle][4]

2. Altında **güvenlik ve Denetim**seçin **OMS çalışma alanı** yeni bir çalışma alanı oluşturmak için.
   > [!NOTE]
   > OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.
3. Altında **OMS çalışma alanı**, çalışma alanınız için bilgi girin.
4. Altında **OMS çalışma alanı**seçin **Tamam**.  Tamam'ı seçin, sonra bir Windows veya Linux aracısını ve anahtarlarını yüklemek için bir bağlantı alırsınız aracının yapılandırılmasında kullanılacak çalışma alanı Kimliğiniz için.
5. Altında **güvenlik ve Denetim**seçin **Tamam**.

**Mevcut bir çalışma alanını seçin ve bilgisayar ekleme**

İş akışını izleyerek bir bilgisayar ekleyebilirsiniz **ekleme**, yukarıda gösterildiği gibi. İş akışını izleyerek bir bilgisayar ekleyebilirsiniz **işlem**. Bu örnekte **işlem**.

1. Güvenlik Merkezi'nin ana menüye dönmek ve **genel bakış** Pano.

   ![Genel Bakış][5]

2. Seçin **işlem ve uygulamalar**.
3. Altında **işlem ve uygulamalar**seçin **bilgisayarlar eklemek**.

   ![Bilgi İşlem dikey penceresi][6]

4. Altında **yeni Azure olmayan bilgisayar ekleme**, bilgisayarınıza bağlayın ve bir çalışma alanı seçin **bilgisayar Ekle**.

   ![Bilgisayar ekleme][7]

   **Doğrudan aracı** dikey penceresinde çalışma alanı kimliği yanı sıra, Windows veya Linux aracı yükleme için bir bağlantı sağlar ve anahtarları aracının yapılandırılmasında kullanılacak.   

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede öğrendiğiniz yerleşik Azure Güvenlik Merkezi'nin Gelişmiş güvenlik için Azure dışı kaynakları nasıl.  Daha fazlasını yapmak için eklenen kaynaklarınızı bakın.

- [Veri toplamayı etkinleştirme](security-center-enable-data-collection.md)
- [Tehdit zekası raporu](security-center-threat-report.md)
- [Tam zamanında VM erişimi](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-onboarding/onboard.png
[2]: ./media/security-center-onboarding/onboard-subscription.png
[3]: ./media/security-center-onboarding/get-started.png
[4]: ./media/security-center-onboarding/create-workspace.png
[5]: ./media/security-center-onboarding/overview.png
[6]: ./media/security-center-onboarding/compute-blade.png
[7]: ./media/security-center-onboarding/add-computer.png
[8]: ./media/security-center-onboarding/onboard-workspace.png
