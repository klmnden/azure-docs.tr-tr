---
title: Azure Güvenlik Merkezi standart Gelişmiş güvenlik için hazırlanma | Microsoft Docs
description: " Nasıl için yerleşik Azure Güvenlik Merkezi standardı için Gelişmiş bilgi güvenliği. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: d83beecfc5a8f6b8a01c64e809bc84c6fd0238bf
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="onboarding-to-azure-security-center-standard-for-enhanced-security"></a>Azure Güvenlik Merkezi standart Gelişmiş güvenlik için hazırlanma
Gelişmiş güvenlik yönetimi ve tehdit koruması, karma bulut iş yükleri için yararlanmak için Güvenlik Merkezi standart yükseltin.  Standart 60 gün süreyle ücretsiz deneyebilirsiniz. Güvenlik Merkezi bkz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/) daha fazla bilgi için.

Güvenlik Merkezi standart içerir:

- **Karma güvenlik** – tüm şirket içi güvenlik birleşik bir görünümünü almak ve bulut iş yükleri. Güvenlik ilkeleri uygulanır ve güvenlik standartlarıyla uyumluluğu sağlamak için karma bulut iş yükleri güvenlik sürekli olarak değerlendirin. Güvenlik duvarları ve diğer iş ortaklarının çözümleri gibi farklı kaynaklardan güvenlik verileri toplayın, bunlar üzerinde arama ve analiz gerçekleştirin.
- **Tehdit algılama Gelişmiş** -gelişmiş analizler ve Microsoft akıllı güvenlik kenar siber saldırıları gelişen üzerinden almak için grafik kullanın.  Yerleşik davranış analizi ve makine öğrenimi özelliklerinden yararlanarak saldırıları ve sıfır gün saldırılarına yol açabilecek güvenlik açıklarını tespit edin. Ağları, makineleri ve bulut hizmetlerini gelen saldırılara veya güvenlik ihlali sonrası etkinliklere karşı izleyin. Etkileşimli araçlar ve bağlama dayalı tehdit zekası ile araştırmaları kolaylaştırın.
- **Erişim ve uygulama denetimleri** -bloğu kötü amaçlı yazılım ve diğer istenmeyebilecek uygulamaları uygulamaları güvenilir listeye almayı önerileri uygulayarak, belirli iş yükleri için uyarlanmış ve makine öğrenme tarafından sağlanmıştır. Ağ saldırı yüzeyini yalnızca zaman, denetlenen erişimi olan Yönetim noktalarına Azure vm'lerinde deneme yanılma ve diğer ağ saldırılarına maruz büyük ölçüde azaltır, azaltın.

## <a name="detecting-unprotected-resources"></a>Korumasız kaynakları algılama     
Güvenlik Merkezi, Güvenlik Merkezi Standart sürümü için etkinleştirilmemiş herhangi bir Azure aboneliğini veya çalışma alanını otomatik olarak algılar. Buna, Güvenlik Merkezi Ücretsiz sürümünü kullanan Azure abonelikleri ve etkin bir Güvenlik çözümü olmayan çalışma alanları dahildir.

Tüm Azure aboneliği abonelik içindeki tüm kaynaklar tarafından devralınan bir standart katmanı yükseltebilir veya yalnızca belirli bir kaynak grubunu yükseltmek için benzersiz bir ilke tanımlayabilirsiniz. Kaynak Grup İlkesi ayarlarının benzersiz olduğundan, Güvenlik Merkezi, abonelik için standart katmana yükselttiğinizde fiyatlandırma ilkeleri üzerine yazılmaz. Standart uygulama katmanı için bir abonelik yalnızca uygulanır abonelik içindeki Güvenlik Merkezi tarafından oluşturulan çalışma alanlarına raporlama VM'ler. Standart uygulama katmanı çalışma alanına raporlama çalışma alanı için tüm kaynakların uygulanır.

> [!NOTE]
> Maliyetlerinizi yönetmek ve yönelik bir çözüm aracıların belirli bir dizi sınırlama tarafından toplanan veri miktarını sınırlamak isteyebilirsiniz. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) kapsam çözümü uygulamak ve çalışma alanındaki bilgisayarların alt ağlarından hedef sağlar.  Hedefleme çözümü kullanıyorsanız, Güvenlik Merkezi bir çözüm olmamasından olarak çalışma listeler.
>
>

## <a name="upgrade-an-azure-subscription"></a>Bir Azure aboneliğine yükseltme
Tüm abonelikler için standart yükseltmek için:
1. Güvenlik Merkezi ana menüsündeki seçin **Onboarding**.
2. Altında **Onboarding Gelişmiş Güvenlik**, Güvenlik Merkezi hazırlanması için uygun abonelikleri listeler. Tüm listelenen abonelikleri seçerek yükseltme **uygulamak standart planı**.

  ![Tüm abonelikleri yükseltme][1]

Standart olarak tek bir abonelik yükseltmek için: bir abonelikten yükseltebilirsiniz **Onboarding** seçerek **uygulamak standart katmanı**. Abonelik altında bir kaynak grubu için standart yükseltmek için aboneliği seçin:
1. Bir abonelik seçin.  **Güvenlik İlkesi** abonelikte bulunan kaynak grubu hakkında bilgi sağlar.
2. Abonelik veya kaynak grubu seçin.

  ![Tüm abonelikleri yükseltme][2]

3. Seçin **standart** ücretsiz standart olarak yükseltmek için.
4. **Kaydet**’i seçin.

> [!NOTE]
> Bir abonelik için standart yükseltme'ı Aç [otomatik sağlamayı](security-center-enable-data-collection.md) , daha önce devre dışı bırakılmışsa. İzleme aracıları otomatik sağlamayı öneririz.
>
>

## <a name="upgrade-a-workspace"></a>Bir çalışma alanı yükseltme
Standart çalışma alanına uygulama raporlama çalışma alanı için tüm kaynaklar için geçerlidir.

1. Geri dönüp **Onboarding** dikey.
2. Bir çalışma alanı seçin.

  ![Bir çalışma alanı yükseltme][8]

3. Seçin **standart** yükseltmek için.  
4. **Kaydet**’i seçin.

   > [!NOTE]
   > Burada serbest olmayabilir veya standart çalışma alanınızı uygulanan bir senaryo vardır. Serbest seçerseniz, Güvenlik Merkezi'nin ücretsiz özellikleri, Azure Vm'leri uygulanır. Ücretsiz özellikleri, Azure olmayan bilgisayarlara uygulanmaz. Standart seçerseniz, standart yetenekleri tüm Azure Vm'leri ve raporlama çalışma alanı için Azure olmayan bilgisayarlara uygulanır. Azure ve Azure dışı kaynaklar için Gelişmiş güvenlik sağlamak için standart uygulamanızı öneririz.
   >
   >

## <a name="onboard-non-azure-computers"></a>Yerleşik Azure olmayan bilgisayarlar
Güvenlik Merkezi, Azure dışı bilgisayarların güvenlik durumunu izleyebilir ancak öncelikle bu kaynakları eklemeniz gerekir. Azure olmayan bilgisayarlar ekleyebilirsiniz **Onboarding** dikey veya **işlem** dikey. Biz, her iki yöntem adım geçireceğiz.

### <a name="add-new-non-azure-computers-from-onboarding"></a>Hazırlama yeni Azure olmayan bilgisayarlar Ekle

1. Geri dönüp **Onboarding**.   
2. Seçin **yeni Azure olmayan bilgisayarlar eklemek istiyor musunuz**.

  ![Azure olmayan bilgisayar ekleme][3]

Varolan çalışma varsa, bunlar altında listelenen **yeni Azure olmayan bilgisayarlar eklemek**. Mevcut bir çalışma alanına bilgisayar eklemek veya yeni bir çalışma alanı oluşturun. Yeni bir çalışma alanı oluşturmak için bağlantıyı seçin **yeni bir çalışma alanı Ekle**.

Biz, her iki yöntem yol:

- Yeni bir çalışma alanı oluşturun ve bilgisayar ekleyin
- Varolan bir çalışma alanını seçin ve bilgisayar ekleyin

**Yeni bir çalışma alanı oluşturun ve bilgisayar ekleyin**

1. Altında **yeni Azure olmayan bilgisayarlar eklemek**seçin **yeni bir çalışma alanı Ekle**.

   ![Yeni bir çalışma alanı Ekle][4]

2. Altında **güvenlik ve Denetim**seçin **OMS çalışma** yeni bir çalışma alanı oluşturmak için.
3. Altında **OMS çalışma**, çalışma alanınız için bilgileri girin.
4. Altında **OMS çalışma**seçin **Tamam**.  Tamam'ı seçin, sonra bir Windows veya Linux aracısı ve anahtarlarını yüklemek için bir bağlantı alırsınız aracı yapılandırmada kullanmak, çalışma alanı kimliği için.
5. Altında **güvenlik ve Denetim**seçin **Tamam**.

**Varolan bir çalışma alanını seçin ve bilgisayar ekleyin**

İş akışını izleyerek bir bilgisayar ekleyebilmeniz için **Onboarding**, yukarıda gösterildiği gibi. İş akışını izleyerek bir bilgisayarı da ekleyebilirsiniz **işlem**. Bu örnekte, kullandığımız **işlem**.

1. Güvenlik Merkezi'nin ana menüye dönmek ve **genel bakış** Pano.

   ![Genel Bakış][5]

2. Seçin **işlem** döşeme.
3. Altında **işlem**seçin **bilgisayarları eklemek**.

   ![Bilgi İşlem dikey penceresi][6]

4. Altında **yeni Azure olmayan bilgisayarlar eklemek**, bilgisayarınıza bağlayın ve'ı tıklatın, bir çalışma alanı seç **bilgisayar Ekle**.

   ![Bilgisayarları ekleme][7]

 **Doğrudan Aracısı** dikey anahtarları aracı yapılandırmada kullanmak, çalışma alanı kimliği için ve bir Windows veya Linux aracısını yüklemek için bir bağlantı sağlar.   

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, öğrenilen yerleşik Azure ve Güvenlik Merkezi'nin Gelişmiş Güvenlik ile yararlanmak için Azure olmayan kaynakları.  Daha fazla ile yapmak için edildi kaynaklarınızı bakın

- [Veri toplamayı etkinleştirme](security-center-enable-data-collection.md)
- [Tehdit zekası raporu](security-center-threat-report.md)
- [Tam zamanında VM erişim](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-onboarding/onboard.png
[2]: ./media/security-center-onboarding/onboard-subscription.png
[3]: ./media/security-center-onboarding/add-non-azure-resource.png
[4]: ./media/security-center-onboarding/create-workspace.png
[5]: ./media/security-center-onboarding/overview.png
[6]: ./media/security-center-onboarding/compute-blade.png
[7]: ./media/security-center-onboarding/add-non-azure-computer.png
[8]: ./media/security-center-onboarding/onboard-workspace.png
