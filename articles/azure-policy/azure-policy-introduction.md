---
title: "Azure ilkesine genel bakış | Microsoft Docs"
description: "Azure ilke oluşturmak, atamak ve ilke tanımları Azure ortamınızda yönetmek için kullandığınız Azure, bir hizmettir."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 10/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: 01b0915cdf37ddc00a4e6a38c99ce7f6f8c4f9c9
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-policy"></a>Azure ilke nedir?

BT yönetimini iş hedeflerine ve BT projeler arasında netlik oluşturur. İyi BT yönetimini, girişimleri planlama ve stratejik bir düzeyde öncelikleri ayarı içerir. Şirketiniz hiçbir zaman çözümlendi, ilkelerini uygulama daha iyi yardımcı göründüğü BT sorunlarını önemli sayıda karşılaşırsa yönetmek ve bunları engelleyebilirsiniz. Azure ilke nereden geldiğini budur.

Azure ilke oluşturmak, atamak ve ilke tanımları yönetmek için kullandığınız Azure bir hizmettir. Bu kaynakları Kurumsal standartları ve hizmet düzeyi sözleşmeleri ile uyumlu olmak için İlke tanımları farklı kurallar ve Eylemler kaynaklarınızı uygulayın. Değerlendirme için olanları yerinde sahip ilke tanımları ile uyumlu olmayan taranacak kaynaklarınızın çalıştırarak bunu yapar.

## <a name="policy-definition"></a>İlke tanımı

Her ilke tanımı çalıştırılacağı uygulanır ve alan eşlik eden eylem yerleştirin Koşullar karşılanıyorsa koşul içeriyor.

Azure İlkesi'nde, varsayılan olarak kullanabileceğiniz bazı yerleşik ilkeleri sunuyoruz. Örneğin:

- **SQL Server 12.0 gerektiren**: Bu ilke tanımı tüm SQL Server'lar sürüm 12.0 kullandığınızdan emin olmak için koşullar/kurallarına sahiptir. Bu ölçütleri karşılayan değil tüm sunucuları reddetmek için kendi eylemdir.
- **Depolama hesabı SKU'ları izin**: Bu ilke tanımı dağıtılmakta olan bir depolama hesabı SKU boyutları kümesi içinde olup olmadığını koşullar/kurallar kümesine sahiptir. Eylemi tanımlı SKU boyutları kümesine uymayan tüm sunucuları engellemektir.
- **Kaynak türü izin**: Bu ilke tanımı, kuruluşunuzun dağıtabilirsiniz kaynak türlerini belirtmek için koşullar/kurallar kümesine sahiptir. Eylemi bu tanımlı listeyi parçası olmayan tüm kaynaklar engellemektir.

İlke tanımları yapılar hakkında daha fazla bilgi için bu makalenin - Ara [ilke tanımı yapısını](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>İlke ataması
Bir ilke atamasını belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımı ' dir. Bu kapsam bir yönetim grubundan bir kaynak grubuna çeşitli işlemleri ifade edebilir.

Ayar ilke tanımları ve atamaları hakkında daha fazla bilgi için bkz: [kaynak ilkesine genel bakış](../azure-resource-manager/resource-manager-policy.md).

## <a name="policy-parameters"></a>İlke parametreleri
İlke parametreleri oluşturmanız gerekir ilke tanımları sayısını azaltarak ilke yönetimini basitleştirmeye yardımcı. Daha fazla genel yapmak için bir ilke tanımı oluşturulurken parametreleri tanımlayabilirsiniz. Bu ilke tanımı farklı senaryolar için (örneğin, bir dizi bir abonelik için konumları belirtme) farklı değerler geçirerek ne zaman yeniden kullanabileceğiniz sonra ilkeyi atarken.

Parametreleri tanımlı/bir ilke tanımı oluşturulurken oluşturulur. Bir parametre tanımlandığında, verilen bir ad ve isteğe bağlı olarak bir değer verilen. Örneğin, konumu olarak bir ilke için bir parametre tanımlayın ve farklı değerleri gibi verin *EastUS* veya *WestUS* bir ilkeyi atarken ne.

İlke parametreleri hakkında daha fazla bilgi için bkz: [kaynak ilkesine genel bakış - parametreleri](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Girişimi tanımı
Bir girişimi tanımı tekil değerlendiriyoruz elde doğrultusunda uyarlanabilir ilke tanımları koleksiyonudur. Örneğin, başlıklı bir girişimi oluşturabilirsiniz **Azure Güvenlik Merkezi'nde izlemesini etkinleştir**, Azure Güvenlik Merkezi tüm kullanılabilir güvenlik önerileri izlemek için bir hedefi.

Bu girişim altında ilke tanımları gibi gerekir:

1. **İzleyici şifrelenmemiş SQL veritabanı Güvenlik Merkezi'nde** – şifrelenmemiş SQL veritabanları ve sunucuları izleme.
2. **İşletim sistemi güvenlik açıkları Güvenlik Merkezi'ndeki izleme** – yapılandırılmış temel karşılamadığı sunucularını izlemek için.
3. **Eksik Endpoint Protection Güvenlik Merkezi'ndeki izleme** – yüklü endpoint protection Aracısı olmadan sunucular izleme.

## <a name="initiative-assignment"></a>Girişimi atama
Bir ilke atamasını gibi girişimi atama belirli bir kapsama atanmış bir girişimi bir tanımıdır. Girişimi atamaları her kapsam için birkaç girişimi tanımları yapmak için gereken azaltın. Bu kapsam için bir kaynak grubu yönetim grubundan değişiklik gösterebilir.

Önceki örnekten **Azure Güvenlik Merkezi'nde izlemesini etkinleştir** girişimi için farklı kapsamlar atanabilir. Örneğin, bir atama atanması **subscriptionA**başka atanabilir yaparken **subscriptionB**.

## <a name="initiative-parameters"></a>Girişimi parametreleri
İlke parametreleri gibi girişimi parametreleri artıklık azaltarak girişimi yönetimini basitleştirmeye yardımcı olması. Temelde Initiative içinde ilke tanımları tarafından kullanılan parametrelerin listesi girişimi parametreleridir.

Örneğin, bir girişimi tanımı - sahip olduğu bir senaryo ele **initiativeC**, iki ilke tanımları ile. Bir tanımlanan parametre sahip her ilke tanımı:

|İlke  |Parametrenin adı     |Parametrenin türü  |Not                                                                                                |
|--------|----------------------|-------|----------------------------------------------------------------------------------------------------------------|
|policyA |allowedLocations      |Dizi  |Parametre türü bir dizi olarak tanımlamış Bu parametre bir değer için dize listesi bekliyor |
|policyB |allowedSingleLocation |Dize |Parametre türü dize olarak tanımlamış Bu parametre bir değer için bir sözcük bekliyor          |

Bu senaryoda, girişimi parametrelerini tanımlarken **initiativeC**, üç seçeneğiniz vardır:

1. Bu girişim içinde ilke tanımları parametrelerinin yararlanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation* girişimi parametrelerini hale  **initiativeC**.
2. Bu girişimi tanımındaki ilke tanımları parametreleri için değerleri girin. Bu örnekte, konumlara listesini sağlayabilirsiniz **policyA'ın parametre – allowedLocations** ve **policyB'ın parametre – allowedSingleLocation**.
Bu girişim atarken değerleri de sağlayabilirsiniz.
3. Bir listesini sağlayın *değeri* bu Initiative atarken kullanılabilir seçenekleri. Bu bu Initiative atadığınızda, ilke tanımları Initiative içinde devralınan parametrelerinden yalnızca sağlanan bu listeden değerleri anlamına gelir.

Sağlanan değer listenize girişimi tanımı oluşturulurken seçenekleri, örneğin, sahipse – *EastUS*, *WestUS*, *CentralUS*, ve *WestEurope* , farklı bir değer gibi giriş kuramayacaktır *Güneydoğu Asya* girişimi atama sırasında listesinin bir parçası olmadığından.

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

Oluşturma ve ilke tanımları ve atamaları yönetme sırasında biz tutumunuzu izlemeniz gereken birkaç işaretçi şunlardır:

- İlke tanımları, ortamınızda oluşturuyorsanız, bir denetim tutmak için bir izin verme efekti aksine etkisi parça ile ilke tanımı etkisini ortamınızda başlangıç öneririz. Komut dosyaları için otomatik ölçeklendirme yerinde uygulamalarınızı zaten varsa, reddetme etkisi ayarı zaten yerinde olduğuna otomasyonlara görevleri aksatabilir.
- Kuruluş hiyerarşileri tanımlar ve atamaları oluşturulurken göz önünde bulundurmanız önemlidir. Bir yüksek düzeyde, örneğin yönetim grubu veya abonelik düzeyinde tanımları oluşturma ve sonraki alt düzeyde atama öneririz. Örneğin, yönetim grubu düzeyinde bir ilke tanımı oluşturursanız, o tanımının bir ilke atamasını abonelik düzeye kısıtlanabilir.
- Ortamınızı uyumluluk durumunu daha iyi anlamak için standart fiyatlandırma katmanı, kullanarak öneririz.
- Yalnızca bir ilke göz önünde olsa bile her zaman girişimi tanımları ilke tanımları yerine kullanmanızı öneririz. Örneğin, bir ilke tanımı – varsa *policyDefA* ve altında girişimi tanımı - oluşturmak *initiativeDefC*, başka bir ilke tanımı'nın gibi hedefleri oluşturmaya karar verirseniz *policyDefA*, yalnızca altında ekleyebilirsiniz *initiativeDefC* ve böylece bunları daha iyi izleyebilirsiniz.

   Tüm yeni ilke tanımları girişimi tanımına eklenen bir girişimi tanımından girişimi atama oluşturduktan sonra otomatik olarak girişimi tanımın altında girişimi atamaları altında döküm aklınızda bulundurun. Ancak, yeni ilke tanımı sunulan yeni bir parametre varsa, bu tanımı veya atama düzenleyerek yansıtacak şekilde atamaları ve girişimi tanımı güncelleştirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar:
Azure ilkesine genel bakış ve bazı biz Tanıtımı anahtar kavramlar sahip olduğunuza göre önerilen sonraki adımlar şunlardır:

- [Bir ilke tanımı atayın](./assign-policy-definition.md)
- [Azure CLI kullanarak bir ilke tanımı atayın](./assign-policy-definition-cli.md)
- [PowerShell kullanarak bir ilke tanımı atayın](./assign-policy-definition-ps.md)
