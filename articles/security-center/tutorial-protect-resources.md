---
title: "Azure Güvenlik Merkezi Öğreticisi - kaynaklarınızı Azure Güvenlik Merkezi ile koruma | Microsoft Docs"
description: "Bu öğreticide, yalnızca bir yapılandırma gösterilir zaman VM erişim ilke ve uygulama Denetim İlkesi."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: f0a32f90e68101f805a52427fab2d5bb29b94939
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Öğretici: Azure Güvenlik Merkezi ile kaynaklarınızı korumak
Güvenlik Merkezi, kötü amaçlı etkinliği engellemek için erişim ve uygulama denetimlerini kullanarak, tehditlere maruz sınırlar. Yalnızca zaman sanal makine (VM), sanal makineleri için sürekli erişime olanak tanıyarak, saldırılara maruz kalma erişim azaltır. Bunun yerine, yalnızca gerektiğinde VM'ler için denetimli ve denetlenen erişim sağlar. Uyarlamalı uygulama denetimler, hangi uygulamaların Vm'leriniz çalıştırabilirsiniz denetleyerek kötü amaçlı yazılımlara karşı VM'ler sağlamlaştırmak yardımcı olur. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan işlemleri analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yalnızca bir yapılandırma VM zamanında erişim ilkesi
> * Bir uygulama Denetim İlkesi yapılandırma

Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) başlamadan önce.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ele özellik adım adım için Güvenlik Merkezi'nin standart fiyatlandırma katmanı olmalıdır. Güvenlik Merkezi standart hiçbir ücret ilk 60 gün süreyle deneyebilirsiniz. Hızlı Başlangıç [Onboard Azure aboneliğinize Güvenlik Merkezi standart](security-center-get-started.md) standart yükseltme size yol göstermektedir.

## <a name="manage-vm-access"></a>VM erişimini yönetme
Tam zamanında VM erişim gerektiğinde VM'ler bağlamak için kolay erişim sağlarken saldırılara maruz kalma azaltma, Azure vm'lerine gelen trafik kilitlemek için kullanılabilir.

Tam zamanında VM erişim önizlemede değil.

Yönetim bağlantı noktalarını her zaman açık olması gerekmez. Bunlar yalnızca VM Yönetimi veya bakım görevleri gerçekleştirmek örnek için bağlıyken açık olması gerekir. Tam zamanında etkinleştirilmişse, Güvenlik Merkezi saldırganlar tarafından hedeflenemez, yönetim bağlantı noktalarına erişimi sınırlayan ağ güvenlik grubu (NSG) kurallarını kullanır.

1. Güvenlik Merkezi ana menü seçin **saat VM erişimi hemen** altında **Gelişmiş bulut SAVUNMA**.

  ![Anlık VM erişimi][1]

  **Zaman VM Access'te yalnızca** Vm'leriniz durumu hakkında bilgi sağlar:

  - **Yapılandırılmış** -yalnızca zaman VM erişimini desteklemek üzere yapılandırılmış VM'ler.
  - **Önerilen** -Vm'lerini yalnızca süresi VM erişimi destekler ancak için yapılandırılmamış.
  - **Öneri yok** -önerilmez için bir VM neden nedenleri şunlardır:

    - NSG - yalnızca eksik zamanında çözüm bir NSG yerinde olmasını gerektirir.
    - Klasik VM - Güvenlik Merkezi'nde yalnızca zaman VM erişim şu anda yalnızca Azure Resource Manager aracılığıyla dağıtılan VM'ler destekler.
    - Bu kategorideki diğer - bir VM olan ise yalnızca çözüm kapalı abonelik veya kaynak grubu ya da, güvenlik ilkesi zaman VM, bir ortak IP eksik ve bir NSG yerinde sahip değil.

2. Önerilen VM seçin ve'ı tıklatın **etkinleştirmek JIT 1 VM'de** bir yalnızca yapılandırmak için bu VM zaman ilkesinde:

  Güvenlik Merkezi önerir veya eklemek ve yapılandırmak istediğiniz yalnızca etkinleştirmek yeni bir bağlantı noktası bağlantı noktalarını varsayılan kaydedebilirsiniz zaman çözümde. Bu öğreticide, bir bağlantı noktası seçerek ekleyelim **Ekle**.

  ![Bağlantı noktası yapılandırması Ekle][2]

3. Altında **Ekle bağlantı noktası yapılandırmasını**, tanımlarsınız:

  - Bağlantı noktası
  - Protokol türü
  - Kaynak IP - IP aralıkları onaylanmış bir istek üzerine erişmek için izin verilen izin verilen
  - En fazla istek süresi - belirli bir bağlantı noktasını açılabilir en büyük zaman penceresi

4. Seçin **Tamam** kaydetmek için.

## <a name="harden-vms-against-malware"></a>Kötü amaçlı yazılımlardan sağlamlaştırmak VM'ler
Uyarlamalı uygulama denetimler Vm'leriniz kötü amaçlı yazılımlardan sağlamlaştırmak yardımcı olan diğer avantajlar arasında yapılandırılmış kaynak gruplarında çalışmasına izin verilen uygulamalar kümesi tanımlamanıza yardımcı olur. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan işlemleri analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.

Uyarlamalı uygulama denetimleri önizlemededir. Bu özellik yalnızca Windows makineler için kullanılabilir.

1. Güvenlik Merkezi ana menüye dönmek. Altında **Gelişmiş bulut SAVUNMA**seçin **Uyarlamalı uygulama denetimleri**.

   ![Uyarlamalı uygulama denetimleri][3]

  **Kaynak grupları** bölüm üç sekme içerir:

  - **Yapılandırılmış**: kaynak listesi grupları uygulama denetimi ile yapılandırılmış olan sanal makineleri içeren.
  - **Önerilen**: kaynak grupları için hangi uygulama denetim önerilir listesi.
  - **Öneri yok**: kaynak listesi grupları VMs herhangi bir uygulama denetim önerimiz içeren. Örneğin, uygulamaların sürekli değiştiği ve kararlı bir duruma geçmediği VM'ler.

2. Seçin **önerilen** sekmesini uygulama denetim önerileri kaynak gruplarıyla listesi.

  ![Uygulama denetimi önerileri][4]

3. Açmak için bir kaynak grubu seçin **uygulama denetim kuralları oluşturma** seçeneği. **VM'leri Seç** bölümünde önerilen VM'lerin listesini gözden geçirin ve uygulama denetimi gerçekleştirmek istemediklerinizin yanındaki onay işaretini kaldırın. **Beyaz listeye ekleme kuralları için işlemleri seçin** bölümünde önerilen uygulamaların listesini gözden geçirin ve uygulamak istemediklerinizin yanındaki onay işaretini kaldırın. Liste aşağıdakileri içerir:

  - **AD**: tam uygulama yolu
  - **İşlemler**: kaç uygulama her yol içinde yer
  - **Ortak**: "Evet" Bu işlemler bu kaynak grubundaki çoğu vm'lerinde çalıştırılıncaya gösterir
  - **ETKİLENME**: uygulamalar bir saldırgan tarafından uygulamaları güvenilir listesine ekleme atlamak için kullanılabilir değilse bir uyarı simgesi gösterir. Bu uygulamaları onaylamadan önce gözden geçirmeniz önerilir.

4. Seçimleri tamamladıktan sonra seçin **oluşturma**.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Diğer quickstarts ve öğreticiler bu koleksiyonda Bu hızlı başlangıç oluşturun. Sonraki quickstarts ve öğreticiler ile çalışmaya devam etmek planlıyorsanız, standart katmanı çalıştırmaya devam etmek ve etkin otomatik sağlamayı tutun. Devam etmek için ücretsiz katmanı döndürülmesini istediğiniz düşünmüyorsanız:

1. Güvenlik Merkezi ana menüye dönmek ve seçmek **Güvenlik İlkesi**.
2. Abonelik veya serbest döndürmek istediğiniz ilkeyi seçin. **Güvenlik İlkesi** açar.
3. Altında **İlkesi BİLEŞENLERİ**seçin **fiyatlandırma katmanı**.
4. Seçin **serbest** Standart abonelik değiştirmek için ücretsiz katmanına katmanı.
5. **Kaydet**’i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüye dönmek ve seçmek **Güvenlik İlkesi**.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz aboneliği seçin.
3. Altında **güvenlik ilkesi – veri toplama**seçin **kapalı** altında **Onboarding** otomatik sağlamayı devre dışı bırakmak için.
4. **Kaydet**’i seçin.

>[!NOTE]
> Otomatik sağlamayı devre dışı bırakma, Microsoft Monitoring Agent aracı bir yere sağlanan Azure VM'lerin kaldırmaz. Otomatik sağlama sınırları güvenlik kaynaklarınız için izleme devre dışı bırakılıyor.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, tarafından tehditlere maruz sınırlamak öğrendiniz:

> [!div class="checklist"]
> * Yalnızca bir yapılandırma zamanı VM erişimi sağlamak için ilke, denetlenen ve yalnızca gerektiğinde VM'ler erişimi denetlenen
> * Hangi uygulamaların Vm'leriniz çalıştırabilirsiniz denetlemek Uyarlamalı uygulama Denetim İlkesi yapılandırma

Güvenlik olaylarını yanıtlama hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: güvenlik olaylarını yanıtlama](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
