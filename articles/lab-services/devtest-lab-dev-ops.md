---
title: Azure DevTest Labs'i DevOps ve tümleştirme | Microsoft Docs
description: Azure DevTest Labs Laboratuvar bir sürekli tümleştirme (CI) içinde kullanma hakkında bilgi edinin / sürekli teslim (CD) işlem hatları bir kuruluş ortamında.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: 62c44bfea28d47d7c32aa7ef440a40d45c314683
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078930"
---
# <a name="integration-of-azure-devtest-labs-and-azure-devops"></a>Azure DevTest Labs ve Azure DevOps tümleştirmesi
DevOps, yazılım geliştirme (Geliştirme) bir sistem için işlemler (Ops) ile tümleşen bir yazılım geliştirme metodolojisinin birleşimidir. Bu sistem iş hedefleri ile uyumlu yeni özellikler, güncelleştirmeleri ve düzeltmeleri teslim edebilir. Bu Metodoloji hedefleri, kullanım biçimlerini ve müşteri geri bildirimlerine göre yeni özellik tasarlamak kadar her şeyi kapsar; düzeltme, kurtarma ve sorunları ortaya çıktığında sistem sağlamlaştırma. Bir kolayca tanımlanan bu metodolojide sürekli tümleştirme (CI) bileşenidir / sürekli teslim (CD) işlem hattı. Bir CI/CD işlem hattı, bilgi, kod ve yapı, test ve dağıtım sistemi üretmek için içeren bir dizi aracılığıyla bir işleme kaynaklarına alır. Bu makalede bir işlem hattı içindeki labs bir kuruluş ortamında etkili bir şekilde kullanmak için farklı yollar ele alınmaktadır. 

## <a name="benefits-of-using-labs-in-devops-workflow"></a>Labs DevOps iş akışında kullanmanın avantajları 

### <a name="focused-access"></a>Odaklanmış erişim 
Bir laboratuvar bir bileşen olarak kullanmak, sınırlı bir kişi grubu ile ilişkilendirmek belirli bir ekosistem sağlar. Genellikle, bir ekip veya bir ortak alan veya belirli bir özelliği çalışma grubuna atanmış bir laboratuvar olacaktır.   

### <a name="infrastructure-replication-in-the-cloud"></a>Bulut altyapısı çoğaltma 
Bir geliştirici, bir kaynak kodu ve araçlarla geliştirme kutusunda içeren bir geliştirme ekosistemine hızlı bir şekilde ayarlayabilirsiniz. Geliştirici üretim yapılandırmaya neredeyse aynı olan bir ortam oluşturabilirsiniz. Bu daha hızlı bir şekilde iç döngü geliştirmeye yardımcı olur. 

### <a name="pre-production"></a>Üretim öncesi 
CI/CD işlem hattında bir laboratuvarına sahip olmak, birden çok farklı üretim öncesi ortamlar veya zaman uyumsuz test etmek için aynı anda çalışan makineler kolaylaştırır. Yapı aracıları gibi farklı destek altyapıları, dağıtılan ve bir laboratuvar yönetilir. 

## <a name="devops-with-devtest-labs"></a>DevTest Labs ile DevOps 

### <a name="development--operation"></a>Geliştirme / işlem 
Bir laboratuvar odaklanan bir özellik alanında çalışan bir takımın. Alana özgü kaynakları, araçları, betikleri veya Resource Manager şablonları gibi paylaşmak için bu ortak odağı sağlar. Daha hızlı değişiklikler, daha küçük bir grupla olumsuz etkileri sınırlandırırken sağlar. Bu paylaşılan kaynakların tüm gerekli kodu, araçları ve yapılandırma ile geliştirme VM'ler oluşturmak Geliştirici izin verin. Bunlar, ya da dinamik olarak oluşturulabilir veya temel görüntüleri özelleştirmelerle oluşturan bir sistem vardır. Yalnızca Geliştirici VM'ler oluşturabilirsiniz, ancak laboratuar ortamında uygun Azure kaynaklarını oluşturmak için gerekli şablonları temel DevTest Labs ortamlar da oluşturabilirsiniz. Herhangi bir değişiklik veya zararlı iş laboratuvar ortamında başka hiç kimse etkilemeden gerçekleştirilebilir. Ürünün bir tek başına system müşterinin makinede yüklü olduğu bir senaryoyu ele alalım. Bu senaryoda, DevTest Labs yapıları kullanarak ve iç döngü hızlı test etmek için müşteri yapılandırmalarının kodu önceden derleme ek yazılım yüklemeniz içeren VM oluşturma geliştirdi. 
  
## <a name="cicd-pipeline"></a>CI/CD işlem hattı 
CI/CD işlem hattı bir çekme isteği, geliştiricinin koddan varolan kodu ile tümleştirir ve üretim ekosistemine dağıtır taşıma DevOps içindeki önemli bileşenlerinden biridir. Tüm kaynaklar bir laboratuvar olması gerekmez. Örneğin, bir Jenkins ana Laboratuvar dışında daha kalıcı bir kaynak olarak ayarlanmış olması. Ardışık düzende labs tümleştirme belirli bazı örnekleri aşağıda verilmiştir. 

### <a name="build"></a>Oluşturma 
Derleme işlem hattı, yayın işlem hattında kapalı verilmesini birlikte sınanacak bileşenlerinin paket oluşturma üzerinde odaklanmıştır. Laboratuvarlar, yapı aracılarını ve diğer destek kaynakları için konumu olarak derleme işlem hattı bir parçası olabilir. Dinamik olarak altyapı oluşturma imkanı daha fazla denetim sağlar. Bir laboratuar ortamında birden çok ortama sahip özelliği sayesinde, özel derleme kaynaklarına benzersiz olarak tanımlanabilmesi için ortam bilgileri bir parçası olarak yapı kimliği kullanırken her derleme zaman uyumsuz olarak çalıştırılabilir.   

Laboratuvar erişim kısıtlama olanağı, derleme aracıları için güvenliği artırır ve yanlışlıkla bozulma olasılığını azaltır.  

### <a name="test"></a>Test etme 
DevTest Labs, Azure için otomatik ve el ile test etmek için kullanılan kaynak (VM'ler, ortamları) oluşturulmasını otomatik hale getirmek bir CI/CD işlem hattı sağlar. Yapılar veya farklı özel yapılandırmaları test etmek için gerekli oluşturmak için yapı işlemi bilgileri kullanmak formülleri kullanarak VM'lerin oluşturulması.   

### <a name="release"></a>Yayınla 
Kod dağıtılmadan önce DevTest Labs doğrulama bölümündeki sürüm için yaygın olarak kullanılır. Test yapı bölümünde benzerdir. DevTest Labs içinde dağıtılan üretim kaynakları olmamalıdır. 

### <a name="customization"></a>Özelleştirme 
Azure DevOps Vm'leri işlenmesini ve belirli Laboratuvar ortamlarında izin var olan görevleri vardır. Azure DevOps Hizmetleri CI/CD işlem hattı yönetmenin bir yolu olsa da, REST API'leri çağırmak için PowerShell betiklerini yürütme veya Azure CLI'yı kullanma olanağı destekleyen herhangi bir sisteme Laboratuvar tümleştirebilirsiniz. 

Bazı CI/CD işlem hattı yöneticileri DevTest Labs ile Azure içindeki kaynakları yönetebilir, var olan açık kaynaklı eklentileri varken. Bazı özel komut dosyası işlem hattının belirli gereksinimlere uyacak şekilde kullanmanız gerekebilir.  Laboratuvar erişim elde etmek için bir görev yürütülürken unutmayın, bir hizmet sorumlusu ile uygun rolü kullanılır. Hizmet sorumlusu, genellikle Laboratuvar katkıda bulunan rolü erişmesi gerekir. Daha fazla bilgi için [tümleştirme Azure DevTests Labs, Azure DevOps sürekli tümleştirme ve teslim ardışık içine](devtest-lab-integrate-ci-cd-vsts.md). 
 