---
title: "Node.js sürümünü belirtme"
description: "Azure Web siteleri ve bulut Hizmetleri tarafından kullanılan Node.js sürümünü belirleme konusunda bilgi edinin"
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d0e15278-2ab4-4ec8-8256-913839c6d5ef
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: a20179c72b227deb14df442bea7b80cf31728aa7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Bir Azure uygulamasında Node.js sürümünü belirtme
Bir Node.js uygulaması barındırdığında, uygulamanızın belirli bir Node.js sürümünü kullandığından emin olmak isteyebilirsiniz. Bunu Azure üzerinde barındırılan uygulamalar için yapmanın birkaç yolu vardır.

## <a name="default-versions"></a>Varsayılan sürümleri
Azure tarafından sağlanan Node.js sürümleri sürekli olarak güncelleştirilir. Aksi belirtilmediği sürece, belirtilen varsayılan sürüm `WEBSITE_NODE_DEFAULT_VERSION` ortam değişkeni kullanılır. Bu varsayılan değeri geçersiz kılmak için bu makalenin aşağıdaki bölümlerdeki adımları izleyin.

> [!NOTE]
> Azure bulut hizmeti (web veya çalışan rolü) uygulamanızı barındırma ve uygulama dağıttığınız ilk kez ise, Azure Azure üzerinde kullanılabilir varsayılan sürümlerini biriyle eşleşiyorsa, geliştirme ortamınızı yüklediğiniz gibi Node.js aynı sürümünü kullanmayı dener.
>
>

## <a name="versioning-with-packagejson"></a>Package.json sürüm oluşturma
Aşağıdakileri ekleyerek kullanılacak Node.js sürümünü belirtebilirsiniz, **package.json** dosyası:

    "engines":{"node":version}

Burada *sürüm* kullanılacak belirli bir sürüm numarası. Sürüm, daha karmaşık koşulları belirtebilirsiniz:

    "engines":{"node": "0.6.22 || 0.8.x"}

0.6.22 sürümlerinden barındırma ortamında kullanılabilir olmadığından, yüksek kullanılabilir serisi olacaktır 0,8 yerine - 0.8.4 kullanılan sürümü.

## <a name="versioning-websites-with-app-settings"></a>Uygulama ayarları ile Web siteleri sürüm oluşturma
Bir Web uygulamasında barındırıyorsanız, ortam değişkeni ayarlayabilirsiniz **WEBSITE_NODE_DEFAULT_VERSION** istediğiniz sürümü için.

## <a name="versioning-cloud-services-with-powershell"></a>PowerShell ile sürüm bulut Hizmetleri
Bir bulut hizmet uygulamasında barındırma ve Azure PowerShell kullanarak uygulama dağıtıyorsanız, kullanarak varsayılan Node.js sürümünü kılabilirsiniz **kümesi AzureServiceProjectRole** PowerShell cmdlet'i. Örneğin:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Yukarıdaki deyimindeki parametreleri büyük/küçük harfe duyarlıdır unutmayın.  Node.js doğru sürümü seçilmiştir denetleyerek doğrulayabilirsiniz **motorları** , rolün özelliğinde **package.json**.

Aynı zamanda **Get-AzureServiceProjectRoleRuntime** bulut hizmet olarak barındırılan uygulamalar için kullanılabilir Node.js sürümlerinin bir listesi alınamadı.  Her zaman bu listede projenizi bağlıdır Node.js sürümü olduğunu doğrulayın.

## <a name="using-a-custom-version-with-azure-websites"></a>Özel bir sürüm ile Azure Web siteleri kullanma
Azure Node.js birkaç varsayılan sürümü sağlarken, varsayılan olarak sağlanmayan bir sürümünü kullanmak isteyebilirsiniz. Uygulamanız bir Azure Web sitesi olarak barındırılıyorsa, bunu kullanarak gerçekleştirebilirsiniz **iisnode.yml** dosya. Aşağıdaki adımlar, bir Azure Web sitesi özel bir Node.Js sürümünü kullanarak sürecinde yol:

1. Yeni bir dizin oluşturun ve ardından oluşturun bir **server.js** dizin içindeki dosya. **Server.js** dosyası şunları içermelidir:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Bu Web sitesine göz atarken kullanılan Node.js sürümünü görüntüler.
2. Yeni bir Web sitesi oluşturun ve site adını not edin. Örneğin, aşağıdaki amaçlarla [Azure komut satırı araçları] adlı yeni bir Azure Web oluşturmak için **numaralı**ve Web sitesi için Git deposu etkinleştirin.

        azure site create mywebsite --git
3. Adlı yeni bir dizin oluşturun **bin** dizin içeren bir alt olarak **server.js** dosya.
4. Belirli sürümünü indirin **node.exe** (Windows sürümü), uygulamanız ile kullanmak istediğiniz. Örneğin, aşağıdaki amaçlarla **curl** 0.8.1 sürümü karşıdan yüklemek için:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Kaydet **node.exe** içine dosya **bin** daha önce oluşturduğunuz klasör.
5. Oluşturma bir **iisnode.yml** aynı dizinde dosya **server.js** dosya ve ardından aşağıdaki içeriği ekleyin **iisnode.yml** dosyası:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Bu yol yerdir **node.exe** Azure Web uygulamanıza yayımladıktan sonra proje dosyasında bulunan olacaktır.
6. Uygulamanızı yayımlayın. Örneğin, yeni bir Web sitesi--git parametresiyle önceki oluşturduğum olduğundan, aşağıdaki komutları uygulama dosyalarını my yerel Git deposu ekleyin ve bunları Web sitesi depoya gönderme:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Uygulama yayımladıktan sonra Web sitesini bir tarayıcıda açın. Belirten iletiyi görmelisiniz "Merhaba Azure çalışan düğümü sürümünden: v0.8.1".

## <a name="next-steps"></a>Sonraki Adımlar
Uygulamanız tarafından kullanılan Node.js sürümünü belirtin, öğrenme yapacağınızı anladığınıza göre nasıl [modüllerle çalışma], [bir Node.js Web sitenizi oluşturup dağıtacak](app-service/app-service-web-get-started-nodejs.md), ve [Mac ve Linux için Azure komut satırı araçları kullanmak üzere nasıl].

Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/).

[Mac ve Linux için Azure komut satırı araçları kullanmak üzere nasıl]:cli-install-nodejs.md
[Azure komut satırı araçları]:cli-install-nodejs.md
[modüllerle çalışma]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: app-service/app-service-web-get-started-nodejs.md
