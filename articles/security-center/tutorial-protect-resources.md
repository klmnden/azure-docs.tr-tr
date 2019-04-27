---
title: Azure Güvenlik Merkezi Öğreticisi - Kaynaklarınızı Azure Güvenlik Merkezi ile koruma | Microsoft Docs
description: Bu öğreticide, tam zamanında VM erişimi ilkesinin ve uygulama denetim ilkesinin nasıl yapılandırılacağı gösterilmektedir.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: monhaber
ms.openlocfilehash: 6e8c10ecb85addf2ef6a995e3c0b8ac611343cfa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612445"
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Öğretici: Kaynaklarınızı Azure Güvenlik Merkezi ile koruma
Güvenlik Merkezi, kötü amaçlı etkinliği engellemek için erişim ve uygulama denetimlerini kullanarak tehditlere maruz kalma riskinizi sınırlar. Just-ın-Time (JIT) sanal makine (VM) erişimi, Vm'lere kalıcı erişimi engellemenize olanak sağlayarak saldırılarına maruz kalma riskinizi azaltır. Bunun yerine, VM'ler için yalnızca gerektiğinde denetimli ve denetlenen erişim sağlamış olursunuz. Uyarlamalı uygulama denetimleri hangi uygulamaların VM'leriniz üzerinde çalışabileceğini denetleyerek kötü amaçlı yazılımlara karşı VM'lerin sağlamlaştırılmasına yardımcı olur. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan işlemleri analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Tam zamanında VM erişimi denetimini yapılandırma
> * Uygulama denetim ilkesi yapılandırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticide ele alınan özellikleri adım adım görmek için Güvenlik Merkezi’nin Standart fiyatlandırma katmanında olmanız gerekir. Ücretsiz olarak Güvenlik Merkezi standart deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/). [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md) başlıklı hızlı başlangıçta Standart katmanına nasıl yükseltebileceğiniz adım adım açıklanmıştır.

## <a name="manage-vm-access"></a>VM erişimini yönetme
JIT VM erişimi, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır, Azure Vm'lere gelen trafiği kilitlemek için kullanılabilir.

Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Bunların yalnızca VM’ye bağlı olduğunuzda (örneğin, yönetim veya bakım görevleri gerçekleştirmek için) açık olması gerekir. Tam zamanında erişim etkinleştirildiğinde Güvenlik Merkezi, yönetim bağlantı noktalarının saldırganlar tarafından hedeflenememesi için bunlara erişimi kısıtlayan Ağ Güvenlik Grubu (NSG) kurallarını kullanır.

1. Güvenlik Merkezi ana menüsünde seçin **tam zamanında VM erişimi** altında **Gelişmiş bulut SAVUNMASI**.

   ![Tam zamanında VM erişimi][1]

   **Tam zamanında VM erişimi** , Vm'lerinizin durumu hakkında bilgi sağlar:

   - **Yapılandırılan** - Tam zamanında VM erişimini destekleyecek şekilde yapılandırılmış VM’lerdir.
   - **Önerilen** - Tam zamanında VM erişimini destekleyebilen ancak bunun için yapılandırılmamış VM’lerdir.
   - **Öneri olmayan** - Bir VM’nin önerilmemesinin olası nedenleri şunlardır:

     - NSG yok - Tam zamanında erişim çözümü için NSG’nin mevcut olması gerekir.
     - Klasik VM - Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager üzerinden dağıtılan VM’leri desteklemektedir.
     - Diğer - Aboneliğin veya kaynak grubunun güvenlik ilkesinde tam zamanında erişim çözümü kapatılmışsa VM bu kategoridedir ya da bu kategorideki bir VM’de genel IP adresi eksik olabilir ve bir NSG mevcut olmayabilir.

2. Önerilen bir VM’yi seçin ve bu VM için bir tam zamanında ilkesi yapılandırmak üzere **1 VM’de JIT’yi etkinleştir** seçeneğine tıklayın:

   Güvenlik Merkezi’nin önerdiği varsayılan bağlantı noktalarını kaydedebilir veya tam zamanında erişim çözümünü etkinleştirmek için kullanmak istediğiniz yeni bir bağlantı noktası ekleyip yapılandırabilirsiniz. Bu öğreticide **Ekle** seçeneğini belirleyerek bir bağlantı noktası ekleyeceğiz.

   ![Bağlantı noktası yapılandırması ekleme][2]

3. **Bağlantı noktası yapılandırması ekle** seçeneğinde şunları belirlersiniz:

   - Bağlantı noktası
   - Protokol türü
   - İzin verilen kaynak IP’leri (onaylanan bir isteğin ardından erişim elde etmesine izin verilen IP aralıkları)
   - İstek süresi üst sınırı (belirli bir bağlantı noktasının açılabileceği süre için üst sınır)

4. Kaydetmek için **Tamam**’ı seçin.

## <a name="harden-vms-against-malware"></a>VM’leri kötü amaçlı yazılımlara karşı sağlamlaştırma
Uyarlamalı uygulama denetimleri, diğer avantajlarının yanı sıra VM’lerinizin kötü amaçlı yazılımlara karşı sağlamlaştırılmasına yardımcı olan yapılandırılmış kaynak grupları üzerinde çalışmasına izin verilen uygulamalar tanımlamanıza yardımcı olur. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan işlemleri analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.

Bu özellik yalnızca Windows makinelerde kullanılabilir.

1. Güvenlik Merkezi ana menüsüne geri dönün. **GELİŞMİŞ BULUT SAVUNMASI** bölümünde **Uyarlamalı uygulama denetimleri** seçeneğini belirleyin.

   ![Uyarlamalı uygulama denetimleri][3]

   **Kaynak grupları** bölümünde üç sekme bulunur:

   - **Yapılandırılmış**: Uygulama denetimiyle yapılandırılan Vm'leri içeren kaynak gruplarının listesi.
   - **Önerilen**: Uygulama denetimi önerilir kaynak gruplarının listesi.
   - **Öneri olmayan**: Uygulama denetimi önerisi olmayan Vm'leri içeren kaynak gruplarının listesi. Örneğin, uygulamaların sürekli değiştiği ve kararlı bir duruma geçmediği VM'ler.

2. Uygulama denetimi önerilerinin bulunduğu kaynak gruplarının listesi için **Önerilen** sekmesini seçin.

   ![Uygulama denetimi önerileri][4]

3. **Uygulama denetimi kuralları oluştur** seçeneğini açmak için bir kaynak grubunu seçin. **VM'leri Seç** bölümünde önerilen VM'lerin listesini gözden geçirin ve uygulama denetimi gerçekleştirmek istemediklerinizin yanındaki onay işaretini kaldırın. **Beyaz listeye ekleme kuralları için işlemleri seçin** bölümünde önerilen uygulamaların listesini gözden geçirin ve uygulamak istemediklerinizin yanındaki onay işaretini kaldırın. Liste aşağıdakileri içerir:

   - **AD**: Tam uygulama yolu
   - **İŞLEMLER**: Her yolun içinde bulunan nasıl uygulama sayısı
   - **ORTAK**: "Evet seçeneği", bu işlemlerin bu kaynak grubundaki VM'lerin çoğunda yürütüldüğünü gösterir
   - **AÇIKLARDAN**: Bir uyarı simgesi, uygulamaların bir saldırgan tarafından uygulama beyaz listesini atlamak için kullanılabilir olmadığını gösterir. Bu uygulamaları onaylamadan önce gözden geçirmeniz önerilir.

4. Seçimlerinizi tamamladıktan sonra **Oluştur**’u seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız Standart katmanını çalıştırmaya devam edin ve otomatik sağlamayı etkinleştirilmiş halde tutun. Devam etmeyi planlamıyorsanız veya Ücretsiz katmanına dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **Güvenlik İlkesi**’ni seçin.
2. Ücretsiz katmanına döndürmek istediğiniz aboneliği veya ilkeyi seçin. **Güvenlik ilkesi** açılır.
3. **İLKE BİLEŞENLERİ** altında **Fiyatlandırma katmanı**’nı seçin.
4. Aboneliği Standart katmanından Ücretsiz katmanına geçirmek için **Ücretsiz**’i seçin.
5. **Kaydet**’i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **Güvenlik ilkesi**’ni seçin.
2. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin.
3. Otomatik sağlamayı kapatmak için **Güvenlik ilkesi – Veri Toplama** altındaki **Ekleme** bölümünden **Kapalı**’yı seçin.
4. **Kaydet**’i seçin.

>[!NOTE]
> Otomatik sağlama devre dışı bırakıldığında Microsoft Monitoring Agent’ın sağlandığı Azure VM’lerinden aracı kaldırılmaz. Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, aşağıdaki işlemleri gerçekleştirerek, tehditlere maruz kalma riskinizi nasıl sınırlayabileceğinizi öğrendiniz:

> [!div class="checklist"]
> * VM’lere yalnızca gerektiğinde denetimli ve denetlenen erişim sağlamak için bir tam zamanında VM erişimi ilkesi yapılandırma
> * VM’lerinizde hangi uygulamaların çalışabileceğini denetlemek için uyarlamalı uygulama denetimleri ilkesi yapılandırma

Güvenlik olaylarına yanıt verme hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik olaylarına yanıt verme](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
