---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: d322aa27c7c9dcf4ad365ec22f8074aeacc85d3b
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502855"
---
Form tanıyıcı kullanmak için erişim verildiğinde, bir karşılama iletisi alırsınız birkaç bağlantı ve kaynakları içeren e-posta. "Azure portalında" bağlantıyı Azure portalını açın ve bir Form tanıyıcı kaynak oluşturmak için bu iletiyi kullanın. İçinde **Oluştur** bölmesinde aşağıdaki bilgileri sağlayın:

|    |    |
|--|--|
| **Name** | Kaynağınız için açıklayıcı bir ad. Örneğin, açıklayıcı bir ad kullanmanızı öneririz *MyNameFormRecognizer*. |
| **Abonelik** | Erişim izni verilen Azure aboneliği seçin. |
| **Location** | Bilişsel hizmet örneğinizin konumu. Farklı konumlara gecikmelere neden ancak kaynağınızı çalışma zamanı kullanılabilirliğini etkilemez sahip. |
| **Fiyatlandırma katmanı** | Kaynak maliyeti, seçtiğiniz fiyatlandırma katmanı ve kullanım bağlıdır. Daha fazla bilgi için bkz. API [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Kaynak grubu** | [Azure kaynak grubu](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) kaynağınızı içerecek. Yeni bir grup oluşturmak veya önceden mevcut olan bir gruba ekleyin. |

> [!IMPORTANT]
> Normalde Azure portalında bir Bilişsel Hizmet kaynağı oluşturduğunuzda, (bilişsel çok hizmette kullanılan) bir hizmet birden çok abonelik anahtarı veya bir tek hizmet abonelik anahtarı (yalnızca belirli bir bilişsel hizmet ile kullanılır) oluşturma seçeneğine sahip olursunuz. Ancak, Form tanıyıcı Önizleme sürümü olduğundan, çoklu hizmet abonelikte dahil değildir ve Hoş Geldiniz e-postasındaki bağlantıya kullanılmadıkça tek hizmet aboneliği oluşturulamıyor.

Form tanıyıcı kaynağınızı dağıtımı tamamlandığında bulun ve seçim **tüm kaynakları** portalında listesi. Ardından **anahtarları** abonelik anahtarlarınızı görüntülemek için sekmesinde. İki anahtarı kaynağa erişim sunar. Değerini kopyalayın **anahtar 1**. Sonraki bölümde kullanır.