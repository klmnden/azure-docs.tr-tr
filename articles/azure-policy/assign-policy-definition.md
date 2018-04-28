---
title: Azure ortamınızda uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma | Microsoft Docs
description: Bu makalede, uyumlu olmayan kaynakları belirlemek üzere bir ilke tanımı oluşturma adımlarında size yol gösterilir.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/18/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 6bb9eddb6a663e1f230c9c46835661ad20c02cfd
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Azure ortamınızda uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma
Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir. Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile *uyumsuzdur*.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* ilke tanımını atayacaksınız.

1. Azure İlkesi sayfasının sol bölmesinde **Atamalar**'ı seçin.
2. **Atamalar** bölmesinin üst kısmında **İlke Ata**'yı seçin.

   ![İlke tanımı atama](media/assign-policy-definition/select-assign-policy.png)

3. **İlke Ata** sayfasında, kullanılabilir tanımlar listesini açmak için **İlke** alanının yanındaki ![İlke tanımı düğmesine](media/assign-policy-definition/definitions-button.png) tıklayın.

   ![Kullanılabilir ilke tanımlarını açma](media/assign-policy-definition/open-policy-definitions.png)

   Azure İlkesi, kullanabileceğiniz yerleşik ilke tanımlarıyla birlikte gelir. Şunlara benzer yerleşik ilke tanımları görürsünüz:

   - Etiketi ve değerini zorla
   - Etiketi ve değerini uygula
   - SQL Server Sürüm 12.0 gerektir

    Kullanılabilir tüm yerleşik ilkelerin tam listesi için bkz. [İlke şablonları](json-samples.md).

4. *Yönetilen disk kullanmayan VM'leri denetle* tanımını bulmak için ilke tanımlarınızda arama yapın. Bu ilkeye tıklayın ve **Seç**'e tıklayın.

   ![Doğru ilke tanımını bulma](media/assign-policy-definition/select-available-definition.png)

5. İlke ataması için görünen **Ad** sağlayın. Bizim durumumuzda, *Yönetilen disk kullanmayan VM'leri denetleme* adını kullanalım. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklamada ilke atamasının yönetilen disk kullanmayan tüm sanal makineleri nasıl tanımladığına ilişkin ayrıntılar sağlanır.
6. İlkenin mevcut kaynaklara uygulanmasını güvence altına almak için fiyatlandırma katmanını **Standart** olarak değiştirin.

   Azure İlkesi içinde iki fiyatlandırma katmanı vardır: *Ücretsiz* ve *Standart*. Ücretsiz katmanıyla, ilkeleri yalnızca gelecek kaynaklarda zorunlu tutabilirsiniz; Standart katmanıyla ise, uyumluluk durumunuzu daha iyi anlayabilmek için ilkeleri mevcut kaynaklarda da zorunlu tutarsınız. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure İlkesi fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-policy/).

7. İlkenin uygulanmasını istediğiniz **Kapsam**'ı seçin.  Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir.
8. Önceden kaydolduğunuz aboneliği (veya kaynak grubunu) seçin. Bu örnekte **Azure Analytics Capacity Dev** aboneliği kullanılmaktadır, ancak sizin seçenekleriniz farklı olabilir. **Seç**'e tıklayın.

   ![Doğru ilke tanımını bulma](media/assign-policy-definition/assign-policy.png)

9. **Dışlamalar** alanını şimdilik boş bırakın ve sonra **Ata**’ya tıklayın.

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemeye hazırsınız.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Sol bölmede **Uyumluluk**’u seçin ve oluşturduğunuz ilke atamasını arayın.

![İlke uyumluluğu](media/assign-policy-definition/policy-compliance.png)

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa **Uyumlu olmayan kaynaklar** altında görünür.

Bir koşul mevcut kaynaklarınıza göre değerlendirilip true sonucunu verdiğinde, bu kaynaklar ilkeyle uyumlu değil olarak işaretlenir. Yukarıdaki örnek resim uyumlu olmayan kaynakları gösterir. Aşağıdaki tabloda, elde edilen uyumluluk durumu için farklı ilke eylemlerinin koşul değerlendirmesi ile nasıl çalıştığı gösterilmektedir. Azure portalında değerlendirme mantığı görünmese de, uyumluluk durumu sonuçları gösterilir. Uyumluluk durumu sonucu uyumlu veya uyumsuz şeklindedir.

| **Kaynak Durumu** | **Eylem** | **İlke Değerlendirmesi** | **Uyumluluk Durumu** |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu Değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu Değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist eylemleri IF deyiminin TRUE olmasını gerektirir. Eylemler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.
## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer kılavuzlar, bu hızlı başlangıcı temel alır. Sonraki kılavuzlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
1. Sol bölmede **Atamalar**'ı seçin.
2. Oluşturduğunuz atamayı arayın ve ardından sağ tıklayın.

   ![Atamayı silme](media/assign-policy-definition/delete-assignment.png)

3.  **Atamayı Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir kapsama ilke tanımı atadınız. İlke tanımı, kapsamdaki tüm kaynakların uyumlu olmasını sağlar ve hangilerinin uyumlu olmadığını belirler.

ilkeleri atama hakkında daha fazla bilgi edinmek ve **gelecekte** oluşturulacak kaynakların uyumlu olduğundan emin olmak için şu öğreticiyle devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./create-manage-policy.md)
