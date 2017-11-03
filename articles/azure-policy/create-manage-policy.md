---
title: "Azure ilke oluşturmak ve kuruluş uyumluluğu zorlamak üzere ilkelerini yönetmek için kullanın | Microsoft Docs"
description: "Standartları zorlamak, yasal uyumluluk ve denetim gereksinimlerini karşılamak, maliyetleri denetim, güvenlik ve performans tutarlılık sağlamak ve kurumsal geniş tasarım ilkeleri zorunlu tuttukları için Azure ilkesini kullanın."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 10/06/2017
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 55e5a60294fc5ccb2a55b1e572af2fd27c68f462
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>Uyumluluğu zorlamak üzere ilkeleri oluşturun ve yönetin

Azure'da ilkeleri oluşturun ve yönetin nasıl giderildiğini anlamak, Kurumsal standartları ve hizmet düzeyi sözleşmeleri ile uyumlu sağlama için önemlidir. Bu öğreticide, bazı oluşturma, atama ve ilkelerini yönetme, kuruluşunuz genelindeki gibi ilgili daha yaygın görevleri yapmak için Azure ilke kullanmayı öğreneceksiniz:

> [!div class="checklist"]
> * Gelecekte oluşturacağınız kaynakları için bir koşul uygulamak için bir ilke atama
> * Oluşturma ve birden çok kaynak için uyumluluğu izlemek amacıyla bir girişimi tanımı atama
> * Uyumlu olmayan veya reddedilen kaynak çözümleyin
> * Bir kuruluş genelinde yeni bir ilke uygulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="opt-in-to-azure-policy"></a>Azure ilke kabul

Erişim isteğinde bulunmak için kaydetmeniz gerekir böylece azure ilke sınırlı Önizleme'de kullanıma sunulmuştur.

1. Git Azure ilke https://aka.ms/getpolicy ve select **kaydolun** sol bölmede.

   ![İlke Ara](media/assign-policy-definition/sign-up.png)

2. Katılımı Azure aboneliklerini seçerek İlkesi **abonelik** çalışmak istediğiniz listesi. Ardından **kaydetmek**.

   Abonelik listeniz tüm Azure abonelikleri içerir.

   ![Azure ilke kullanmayı kabulü](media/assign-policy-definition/preview-opt-in.png)

   İsteğe bağlı olarak, birkaç gün için bize kayıt İsteğiniz kabul etmek için kadar sürebilir. İsteğiniz kabul sonra size e-posta aracılığıyla hizmet kullanmaya başlayabilirsiniz bildirilecek.

## <a name="assign-a-policy"></a>Bir ilke atama

Azure ilkesiyle zorlamayı ilk adımı, bir ilke tanımı atamaktır. Bir ilke tanımı hangi koşul bir ilke uygulanır ve hangi eylemin yapılacağını altında tanımlar. Bu örnekte, biz adlı yerleşik ilke tanımı atamak *gerektiren SQL Server sürümü 12.0*, tüm SQL Server veritabanlarını uyumlu olmasını v12.0 olmalıdır koşul zorlamak için.

1. Arama ve seçerek Azure portalında Azure İlkesi Hizmeti başlatma **İlkesi** sol bölmede.

   ![İlke Ara](media/assign-policy-definition/search-policy.png)

2. Seçin **atamaları** Azure İlkesi sayfasının sol bölmede. Atama, belirli bir kapsamda gerçekleşmesi için atanan bir ilkedir.
3. Seçin **atama İlkesi** üstünden **atamaları** bölmesi.

   ![Bir ilke tanımı atayın](media/create-manage-policy/select-assign-policy.png)

4. Üzerinde **atama İlkesi** sayfasında, ![ilke tanımı düğmesi](media/assign-policy-definition/definitions-button.png) yanına **İlkesi** alan kullanılabilir tanımları listesini açın.

   ![Açık mevcut ilke tanımları](media/create-manage-policy/open-policy-definitions.png)

5. Seçin **SQL Server sürümü 12.0 gerektiren**.
   
   ![Bir ilke bulun](media/create-manage-policy/select-available-definition.png)

6. Bir görüntü sağlamak **adı** ilke ataması için. Bu durumda, kullanalım *gerektiren SQL Server sürümü 12.0*. Ayrıca, isteğe bağlı bir ekleyebilirsiniz **açıklama**. Bu ilke ataması bu ortamda oluşturulan tüm SQL sunucuları nasıl sağlar hakkında ayrıntılar sürüm 12.0 olan açıklama sağlar.
7. Fiyatlandırma katmanını değiştirmek **standart** ilkesi var olan kaynaklara uygulanma emin olmak için.

   Azure ilke içinde– iki fiyatlandırma katmanı vardır *serbest* ve *standart*. Ücretsiz katmanı ile ilkeleri yalnızca uygulayabilirsiniz gelecekteki kaynaklardaki standart sırada, ayrıca bunları uyumluluk durumu daha iyi anlamak için var olan kaynakların zorunlu kılabilir. Biz sınırlı önizlemede olduğundan seçmek için bir fatura almaz için henüz bir fiyatlandırma modelini yayımlandı değil *standart*. Daha fazla bilgi için fiyatlandırma hakkında bakın: [fiyatlandırma Azure İlkesi](https://acom-milestone-ignite.azurewebsites.net/pricing/details/azure-policy/).

8. Seçin **kapsam** -Azure ilkesine tercih ettiğinizde abonelik (veya kaynak grubu), önceden kayıtlı. Kaynakları gruplandırma ilke ataması üzerinde zorlanan ya da hangi kaynakların bir kapsamı belirler. Bir abonelik için kaynak gruplarını aralığında.

   Bu abonelik - kullanarak Biz bu örneğin **Azure Analytics kapasite geliştirme**. Aboneliğinizi farklılık gösterir.

10. Seçin **atamak**.

## <a name="implement-a-new-custom-policy"></a>Yeni bir özel ilke uygulama

Biz ilke tanımı atadığınız, ortamınızda çoğaltmanın oluşturulan VM'ler G serisinde olamaz sağlayarak maliyetleri kaydetmek için yeni bir ilke oluşturmak için yapacağız. Kurumsal bir kullanıcı VM G serisinde oluşturmayı dener her zaman bu şekilde isteğini reddetti.

1. Seçin **tanımı** altında **yazma** sol bölmede.

   ![Yazma altında tanımı](media/create-manage-policy/definition-under-authoring.png)

2. Seçin **+ ilke tanımı**.
3. Aşağıdakileri girin:

   - İlke tanımı - adını *VM SKU G serisi küçük gerektirir*
   - İlke tanımı yapmak için – yöneliktir açıklaması Bu ilke tanımı bu kapsamda oluşturulan tüm sanal makineleri SKU'ları maliyetini azaltmak G serisi daha küçük olmasını zorlar.
   - İlke tanımı Canlı – bu durumda, bizim ilke tanımı Live'da abonelik **Danışmanı Analytics kapasite geliştirme**. Abonelik listeniz farklılık gösterir.
   - Json koduyla yazma:
      - İlke parametreleri.
      - İlke kuralları /, bu durumda – VM SKU boyutunu G seriye eşit koşulları
      - Bu durumda – ilke etkili **reddetme**.
   
   İşte json aşağıdaki gibi görünmelidir

```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "like": "Standard_G*"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
}
```

   Json kodunu örneklerini görüntülemek için bu makalenin - Ara [kaynak ilkesine genel bakış](../azure-resource-manager/resource-manager-policy.md)
   
4. **Kaydet**’i seçin.

## <a name="create-and-assign-an-initiative-definition"></a>Oluşturun ve girişimi tanımı atayın

Bir girişimi tanımıyla bir değerlendiriyoruz elde etmek için birkaç ilke tanımları gruplandırabilirsiniz. Kaynakları tanımı kapsamında girişimi tanımını oluşturan ilke tanımları uyumlu kalmasını sağlamak için bir girişimi tanımı oluşturun.  Bkz: [Azure ilkesine genel bakış](./azure-policy-introduction.md) girişimi tanımları hakkında daha fazla bilgi için.

### <a name="create-an-initiative-definition"></a>Girişimi tanımı oluşturun

1. Seçin **tanımları** altında **yazma** sol bölmede.

   ![Tanımları seçin](media/create-manage-policy/select-definitions.png)

2. Seçin **Initiative tanımı** bu seçim, sayfanın en üstünde alır **Initiative tanımı** formu.
3. Ad ve açıklama girişiminin girin.

   Kaynaklardır güvenli alma hakkında ilke tanımları uyumlu, Initiative adı olacaktır olmak istiyoruz Bu örnekte, **alma güvenli**, açıklama olacaktır: **Bu girişim yapıldı kaynakların güvenliğini sağlama ile ilişkili tüm ilke tanımları işlemek için oluşturulan**.

   ![Girişimi tanımı](media/create-manage-policy/initiative-definition.png)

4. Listesine göz **kullanılabilir tanımları** ve o Initiative eklemek istediğiniz ilke tanımlarını seçin. İçin bizim **güvenli alma** girişimi, ilke tanımları yerleşik aşağıdakileri ekleyin:
   - SQL Server sürüm 12.0 gerektirir
   - Güvenlik Merkezi'nde korumasız web uygulamalarını izleyin.
   - Güvenlik Merkezi'nde izin veren ağ üzerinden izleyin.
   - Olası uygulama uygulamaları güvenilir listeye almayı Güvenlik Merkezi'nde izleyin.
   - Güvenlik Merkezi'nde şifrelenmemiş VM diskleri izleyin.

   ![Girişimi tanımları](media/create-manage-policy/initiative-definition-2.png)

   İlke tanımları listeden seçtikten sonra altında görürsünüz **ilkeleri ve parametreleri**, yukarıda gösterildiği gibi.

5. **Oluştur**’u seçin.

### <a name="assign-an-initiative-definition"></a>Bir girişimi tanımı atayın

1. Git **tanımları** altında sekmesinde **yazma**.
2. Arama **güvenli alma** oluşturduğunuz girişimi tanımı.
3. Girişimi tanımı'nı seçin ve ardından **atamak**.

   ![Bir tanımı atayın](media/create-manage-policy/assign-definition.png)

4. Doldurmak **atama** girerek form:
   - Ad: Get güvenli atama
   - Açıklama: Bu Grup İlkesi tanımlarında zorlamayı doğrultusunda bu girişimi atama uyarlanmış **Azure Danışmanı kapasite geliştirme** abonelik
   - Fiyatlandırma katmanı: standart
   - uygulanan bu atama gibi kapsamı: **Azure Danışmanı kapasite geliştirme**

5. Seçin **atamak**. 

## <a name="resolve-a-non-compliant-or-denied-resource"></a>Uyumlu olmayan veya reddedilen kaynak çözümleyin

SQL server sürümü 12.0 gerektirecek şekilde ilke tanımı atama sonra yukarıdaki örnekte,, farklı bir sürümü ile oluşturulmuş bir SQL server reddedildi. Bu bölümde, biz oluşturulan bir SQL server farklı sürümün bulunduğu bir reddedilen girişimi çözümünde size taramasını.

1. Seçin **atamaları** sol bölmede.
2. Tüm ilke atamalarını göz atın ve başlatma *gerektiren SQL Server sürümü 12.0* atama.
3. SQL server oluşturmaya çalıştığınız kaynak grupları için bir dışlama isteyin. Bu durumda, biz Microsoft.Sql/servers/databases hariç: *baconandbeer/Cheetos* ve *baconandbeer/Chorizo*.

   ![İstek dışlama](media/create-manage-policy/request-exclusion.png)

   Reddedilen bir kaynak çözmek diğer yolları içerir: SQL server tarafından oluşturulan gerek ve erişimi varsa ilkeyi doğrudan düzenlemek için güçlü bir gerekçe varsa, ilkeyle ilişkili kişiye ulaşmasını.

4. **Kaydet**’i seçin.

Bu bölümde, bir SQL server sürümüyle 12.0, dışlama kaynaklara isteyerek oluşturma girişimi, reddi çözümlendi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilerde ile çalışmaya devam etmeyi planlıyorsanız, temiz bu Kılavuzu'nda oluşturulan kaynakları yukarı değil. Devam etmek düşünmüyorsanız atamaları ya da yukarıda oluşturduğunuz tanımları silmek için aşağıdaki adımları kullanın:

1. Seçin **tanımları** (veya **atamaları** atama silmeyi denediğiniz varsa) sol bölmedeki.
2. Yeni Initiative veya ilke tanımı (veya atama) yeni oluşturulan arayın.
3. Üç nokta tanımı veya atama ucundaki ve seçin **silmek tanımı** (veya **silmek atama**).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, başarılı bir şekilde aşağıdaki gerçekleştirdiniz:

> [!div class="checklist"]
> * Gelecekte oluşturacağınız kaynakları için bir koşul uygulamak için bir ilke atanan
> * Oluşturulan ve birden çok kaynak için uyumluluğu izlemek amacıyla bir girişimi tanımı atayın
> * Uyumlu olmayan veya reddedilen kaynak çözümlendi
> * Yeni bir ilke bir kuruluş genelinde uygulanmadı

İlke tanımları yapılar hakkında daha fazla bilgi için bu makalenin bakın:

> [!div class="nextstepaction"]
> [İlke tanımı yapısı](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure)
