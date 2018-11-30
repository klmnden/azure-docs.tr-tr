---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 11/26/2018
ms.author: mabrigg
ms.openlocfilehash: be9b3b3a9cbb39386d13f76c595ce5e34688adac
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336088"
---
Doğrulama iş akışlarında **zamanlama** test iş akışı oluşturma işlemi sırasında belirtilen düzey iş akışı ortak parametreleri kullanır (bkz [hizmetolarakAzureStackdoğrulamaişakışıortakparametreleri](../azure-stack-vaas-parameters.md)). Herhangi bir test parametre değerleri geçersiz hale gelirse, bunları belirtildiği gibi resupply gerekir [iş akışı parametreleri değiştirmek](../azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Var olan bir örneği üzerinde bir doğrulama testi zamanlama portalda eski örneği yerine yeni bir örneğini oluşturur. Eski örneği için günlükleri korunur ancak Portalı'ndan erişilebilir değildir.  
Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Seçin **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

1. Test parametreleri gözden geçirin ve ardından **Gönder** test yürütme için zamanlamak için.