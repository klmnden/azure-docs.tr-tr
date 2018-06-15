---
title: Azure DevOps Projesine Genel Bakış | Microsoft Docs
description: Azure DevOps Projesinin değerini anlama
services: devops-project
documentationcenter: ''
author: mlearned
manager: douge
editor: mlearned
ms.assetid: ''
ms.service: devops-project
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/03/2018
ms.author: mlearned
ms.openlocfilehash: b87cb14fc707d14638c2d6a52af6f295276a2279
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33779149"
---
# <a name="overview-of-azure-devops-project"></a>Azure DevOps Projesine Genel Bakış

Azure DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. DevOps projesi kaynağı, Azure portalından birkaç hızlı adımla seçtiğiniz Azure hizmetinde sık kullandığınız uygulama türünü başlatmanıza yardımcı olur. DevOps Projesi, uygulamanızı geliştirmek, dağıtmak ve izlemek için ihtiyaç duyduğunuz her şeyi ayarlar.
DevOps Projesi panosu, Azure portalında tek bir görünümden kod kaydetmelerini, derlemelerini ve dağıtımlarını izlemenize olanak sağlar.

## <a name="why-should-i-use-the-azure-devops-project"></a>Azure DevOps Projesini neden kullanmalıyım?

Azure DevOps Projesi, Azure’a Sürekli Tümleştirme (CI) ve Sürekli Teslim (CD) işlem hattının kurulumunu otomatikleştirir.  Mevcut kodunuzla başlayabilir veya sağlanan örnek uygulamalardan birini kullanabilir ve sonra bu uygulamayı Sanal Makineler, App Service, Azure Container Service, Azure SQL Veritabanı ve Azure Service Fabric gibi çeşitli Azure hizmetlerine dağıtabilirsiniz.  

Azure DevOps projesi, ilk Git deposunun ayarlanmasından, CI/CD işlem hattının yapılandırılmasına, izleme için bir Application Insights kaynağının oluşturulmasından Azure portalındaki bir Azure DevOps Projesi oluşturulmasıyla birlikte tüm çözümün tek bir görünümünün sağlanmasına kadar bir DevOps işlem hattının ilk yapılandırmasına ilişkin tüm işleri yapar.

Aşağıdakileri yapmak için Azure DevOps Projesini kullanabilirsiniz:

* Uygulamanızı Azure’a hızlı şekilde dağıtma
* Bir VSTS CI/CD işlem hattının kurulumunu otomatikleştirme
* VSTS ile Azure’a CI/CD’nin nasıl düzgün şekilde ayarlanacağını görüntülemek ve anlamak için şablon olarak DevOps projesini kullanma
* Azure’a CI/CD işlem hattını kullanmaya başlama ve özel senaryolarınıza göre yayın işlem hattını daha fazla özelleştirme

## <a name="how-do-i-use-the-azure-devops-project"></a>Azure DevOps Projesini nasıl kullanırım?

Azure DevOps Projesi, Azure portalından kullanılabilir.  Portaldan diğer Azure kaynakları gibi bir Azure DevOps projesi kaynağı oluşturursunuz.  DevOps projesi, çeşitli yapılandırma seçeneklerine yönelik adım adım sihirbaza benzer bir deneyim sağlar.  

İlk kurulumun parçası olarak birçok yapılandırma seçeneği arasından seçim yaparsınız.  Bu seçenekler şunlardır:

* Sağlanan örnek uygulamayı kullanma veya kendi kodunuzu getirme
* Uygulama dili seçme
* Dile dayalı bir Uygulama altyapısı seçme
* Bir Azure hizmeti seçme (dağıtım hedefi)
* VSTS hesabı (yeni veya mevcut)
* Azure aboneliğinizi seçme
* Azure hizmetlerinin konumunu seçme
* Azure hizmetleri için çeşitli fiyatlandırma katmanları arasından seçim yapma

Azure DevOps Projesini kullandıktan sonra, Azure portalındaki Azure DevOps Projesi panosunda tek bir yerden tüm kaynakları da silebilirsiniz.

## <a name="azure-devops-project-and-vsts-integration"></a>Azure DevOps Projesi ve VSTS tümleştirmesi

DevOps Projeleri, VSTS tarafından desteklenir.  DevOps projesi, Azure’a CI/CD’yi ayarlamak için VSTS’de gereken tüm işleri otomatikleştirir.  Yeni veya mevcut VSTS hesabında bir Git deposu oluşturulur.  DevOps projesi, örnek bir uygulamayı veya mevcut kodunuzu yeni bir Git deposuna kaydeder.  Otomasyon, her yeni kod kaydının bir derleme başlatması için derlemeye yönelik bir CI tetikleyicisi de oluşturur.  DevOps projesi ayrıca bir CD tetikleyicisi oluşturur ve her yeni başarılı derlemeyi seçtiğiniz Azure hizmetine dağıtır.  Derleme ve yayın tanımları, ek senaryolar için özelleştirilebilir.  Derleme ve yayın tanımlarını diğer projelerde kullanmak üzere kopyalayabilirsiniz de.

DevOps Projenizi oluşturduktan sonra aşağıdakileri yapabilirsiniz:

* Derleme ve yayın işlem hattınızı özelleştirme
* Kod akışınızı yönetmek ve kalitenizi yüksek tutmak için çekme isteklerini kullanma
* Kalite çıtasını yükseltmek için kodunuzu birleştirmeden önce her bir kaydı test edip derleme
* Uygulamanızla birlikte Proje kapsamınızı ve sorunlarınızı izleme

## <a name="how-do-i-start-using-the-azure-devops-project"></a>Azure DevOps Projesini nasıl kullanmaya başlarım?

* [Azure DevOps Projesini Kullanmaya Başlama](https://docs.microsoft.com/vsts/build-release/actions/azure-devops-project-github)

## <a name="azure-devops-project-videos"></a>Azure DevOps Projesi videoları

* [Azure DevOps Projesi ile CI/CD oluşturma](https://channel9.msdn.com/Events/Connect/2017/T174/player/)
