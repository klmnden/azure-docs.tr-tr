---
title: Satın alma ve Azure - App Service SSL sertifikası yapılandırma | Microsoft Docs
description: Bir App Service sertifikası satın alma ve App Service uygulamanızı bağlama hakkında bilgi edinin
services: app-service
documentationcenter: .net
author: cephalin
manager: jpconnoc
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cephalin
ms.reviewer: apurvajo
ms.custom: seodec18
ms.openlocfilehash: e7768eb29caf66fd8f666a9475ac0787826a47e0
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618908"
---
# <a name="buy-and-configure-an-ssl-certificate-for-azure-app-service"></a>Satın alma ve Azure App Service için SSL sertifikası yapılandırma

Bu öğretici, güvenliğinin nasıl sağlanacağını gösterir, [App Service uygulaması](https://docs.microsoft.com/azure/app-service/) veya [işlev uygulaması](https://docs.microsoft.com/azure/azure-functions/) (satın) oluşturarak bir App Service sertifikası [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) ve kendisine bağlama bir App Service uygulaması.

> [!TIP]
> App Service sertifikaları, herhangi bir Azure veya Azure Hizmetleri için kullanılabilir ve uygulama hizmetleri için sınırlı değildir. Bunu yapmak için onu istediğiniz yere kullanabileceğiniz bir App Service sertifikasının yerel PFX kopyasını oluşturmak gerekir. Daha fazla bilgi için [bir App Service sertifikasının yerel PFX kopyasını oluşturma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/04/12/export-an-azure-app-service-certificate-pfx-powershell/).
>

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda izlemek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanız için bir etki alanı adını eşleme](app-service-web-tutorial-custom-domain.md) veya [satın alma ve Azure'da yapılandırma](manage-custom-dns-buy-domain.md)

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="start-certificate-order"></a>Sertifika siparişi Başlat

App Service sertifikası sipariş başlangıç <a href="https://portal.azure.com/#create/Microsoft.SSL" target="_blank">App Service sertifikası oluşturma sayfası</a>.

![Sertifika oluşturma](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Sertifika yapılandırmanıza yardımcı olması için aşağıdaki tabloyu kullanın. Tamamladığınızda **Oluştur**’a tıklayın.

| Ayar | Açıklama |
|-|-|
| Ad | App Service sertifikanız için bir kolay ad. |
| Ana bilgisayar adı çıplak etki | Burada kök etki alanı belirtirseniz, güvenlik altına alan bir sertifika almak *hem* kök etki alanı ve `www` alt etki alanı. İçin güvenli bir alt etki alanı yalnızca belirtin alt etki alanı burada tam etki alanı adını (örneğin, `mysubdomain.contoso.com`). |
| Subscription | Web uygulamasının barındırıldığı veri merkezi. |
| Resource group | Sertifikayı içeren kaynak grubu. App Service uygulamanızı, örneğin aynı kaynak grubunu seçin ya da yeni bir kaynak grubunu kullanın. |
| Sertifika SKU'su | Standart bir sertifika mı yoksa mı oluşturmak için sertifika türünü belirler [joker sertifikası](https://wikipedia.org/wiki/Wildcard_certificate). |
| Yasal koşullar | Yasal koşulları kabul onaylamak için tıklayın. Sertifikaları Godaddy'den elde edilir. |

## <a name="store-in-azure-key-vault"></a>Azure Key vault'ta Store

Sertifika satın alma işlemi tamamlandıktan sonra bu sertifikayı kullanarak başlamadan önce tamamlamanız gereken birkaç adım vardır. 

Sertifikayı seçin [App Service sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasında'a tıklayın **sertifika Yapılandırması** > **1. adım: Store**.

![içinde KV depolamak hazır görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) şifreleme anahtarlarını ve gizli bulut uygulamaları ve Hizmetleri tarafından kullanılan korunmasına yardımcı olan bir Azure hizmetidir. App Service sertifikaları için tercih ettiğiniz depolamadır.

İçinde **Key Vault durumu** sayfasında **Key Vault deposu** yeni bir kasa oluşturun veya mevcut bir kasayı seçin. Yeni bir kasa oluşturmak isterseniz, Oluştur'a tıklayın ve kasa yapılandırma yardımcı olması için aşağıdaki tabloyu kullanın. Yeni Key Vault içinde aynı abonelik ve kaynak grubu oluşturmak için bkz.

| Ayar | Açıklama |
|-|-|
| Ad | Alfasayısal karakterler ve kısa çizgiler için oluşan benzersiz bir ad. |
| Resource group | Bir öneri App Service sertifikanızı olarak aynı kaynak grubunu seçin. |
| Location | App Service uygulamanızı aynı konumu seçin. |
| Fiyatlandırma katmanı | Bilgi için [Azure anahtar kasası fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/key-vault/). |
| Erişim ilkeleri| Uygulamalar ve izin verilen erişimi kasa kaynaklara tanımlar. Daha sonra bu adımları uygulayarak yapılandırma [çeşitli uygulamalar bir anahtar kasası erişim](../key-vault/key-vault-group-permissions-for-apps.md). |
| Sanal ağ erişimi | Belirli bir Azure sanal ağları kasa erişimi kısıtlayın. Daha sonra bu adımları uygulayarak yapılandırma [Azure anahtar kasası yapılandırma güvenlik duvarları ve sanal ağlar](../key-vault/key-vault-network-security.md) |

Kasayı seçtikten sonra kapatmak **Key Vault deposu** sayfası. **Store** seçeneği başarı için yeşil bir onay işareti göstermelidir. Sayfa bir sonraki adımda açık tutun.

## <a name="verify-domain-ownership"></a>Etki alanı sahipliğini doğrulayın

Aynı **sertifika Yapılandırması** sayfasında son adımda kullanılan **2. adım: Doğrulama**.

![](./media/app-service-web-purchase-ssl-web-site/verify-domain.png)

Seçin **App Service doğrulaması**. Web uygulamanız için etki alanı zaten eşlenmiş bu yana (bkz [önkoşulları](#prerequisites)), zaten doğrulandı. Tıklamanız yeterli **doğrulama** bu adımı tamamlamak için. Tıklayın **Yenile** ileti kadar düğmesi **sertifikadır etki alanını doğruladıysanız** görünür.

> [!NOTE]
> Etki alanı doğrulama yöntemlerinin dört türleri desteklenir: 
> 
> - **App Service** -etki alanı aynı Abonelikteki bir App Service uygulamasına zaten eşlendiğinde en kullanışlı seçenektir. App Service uygulaması zaten etki alanı sahipliğini doğruladı olgu avantajlarından yararlanır.
> - **Etki alanı** -doğrulayın bir [satın aldığınız Azure App Service etki alanı](manage-custom-dns-buy-domain.md). Azure, otomatik olarak sizin için doğrulama TXT kaydı ekler ve işlemini tamamlar.
> - **Posta** -etki alanı için etki alanı yöneticisinin e-posta göndererek doğrulayın. Seçeneğini belirlediğinizde yönergeleri sağlanır.
> - **El ile** -HTML sayfasını kullanarak etki alanını doğrulayın (**standart** yalnızca sertifika) veya bir DNS TXT kaydı. Seçeneğini belirlediğinizde yönergeleri sağlanır.

## <a name="bind-certificate-to-app"></a>Sertifika uygulamasına bağlama

İçinde  **[Azure portalında](https://portal.azure.com/)** , soldaki menüden **uygulama hizmetleri** >  **\<your_ uygulama >** .

Uygulamanızın sol gezinti bölmesinden seçin **SSL ayarları** > **özel sertifikaları (.pfx)**  > **alma App Service sertifikası**.

![Sertifika İçeri Aktar görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

Yeni satın aldığınız sertifikayı seçin.

Sertifikayı içeri aktarıldığına göre bir eşleşen etki alanı adı uygulamanıza bağlamak gerekir. Seçin **bağlamaları** > **SSL bağlaması Ekle**. 

![Sertifika İçeri Aktar görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/AddBinding.png)

Bağlamasında yapılandırmanıza yardımcı olması için aşağıdaki tabloyu kullanın **SSL bağlamaları** iletişim kutusunda, ardından **bağlaması Ekle**.

| Ayar | Açıklama |
|-|-|
| Ana Bilgisayar Adı | SSL bağlaması için eklemek için etki alanı adı. |
| Özel sertifika parmak izi | Bağlama sertifikası. |
| SSL türü | <ul><li>**SNI SSL** -birden fazla SNI tabanlı SSL bağlaması eklenebilir. Bu seçenek, aynı IP adresi üzerinde birden fazla SSL sertifikası ile birden fazla etki alanının güvenliğini sağlamaya olanak tanır. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNI’yi destekler (daha kapsamlı tarayıcı desteği bilgilerini [Sunucu Adı Belirtimi](https://wikipedia.org/wiki/Server_Name_Indication) bölümünde bulabilirsiniz).</li><li>**IP tabanlı SSL** - Yalnızca bir adet IP tabanlı SSL bağlaması eklenebilir. Bu seçenek yalnızca bir SSL sertifikası ile ayrılmış bir genel IP adresinin güvenliğini sağlamaya olanak tanır. Bağlama yapılandırdıktan sonra adımları [kayıt yeniden eşlemek için IP SSL](app-service-web-tutorial-custom-ssl.md#remap-a-record-for-ip-ssl). </li></ul> |

## <a name="verify-https-access"></a>HTTPS erişimi doğrulama

Kullanıp uygulamanızın ziyaret `HTTPS://<domain_name>` yerine `HTTP://<domain_name>` sertifika doğru şekilde yapılandırıldığını doğrulayın.

## <a name="rekey-certificate"></a>Sertifikayı yeniden anahtarla

Sertifikanızı özel düşünüyorsanız anahtarının güvenliği, sertifikanız yeniden anahtarlama. Sertifikayı seçin [App Service sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasında ve ardından **yeniden anahtarla ve Eşitle** sol gezinti bölmesinden.

Tıklayın **yeniden anahtarlama** işlemini başlatmak için. Bu işlemin tamamlanması 1-10 dakika sürebilir.

![yeniden anahtarlama SSL görüntüsü Ekle](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Sertifikanız yeniden anahtarlama için izine sahip sertifika yetkilisinden verilen yeni bir sertifika yapar.

Yeniden anahtarlama işlemi tamamlandıktan sonra tıklayın **eşitleme**. Eşitleme işlemi, uygulamalarınıza kapalı kalma süresi neden olmadan konak adı bağlamaları App Service sertifika için otomatik olarak güncelleştirir.

> [!NOTE]
> Yoksa tıklarsanız **eşitleme**, App Service otomatik olarak 48 saat içinde sertifikanıza eşitler.

## <a name="renew-certificate"></a>Sertifikayı Yenile

Sertifikayı, sertifikanın otomatik yenilenmesini üzerinde istediğiniz zaman açmak için seçin [App Service sertifikaları](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) sayfasında'a tıklayın **otomatik yenileme ayarları** sol gezinti bölmesinde.

Seçin **üzerinde** tıklatıp **Kaydet**. Sertifika süresi dolmadan önce 60 gün açık olarak otomatik yenileme varsa otomatik olarak yenileme başlayabilirsiniz.

![Sertifika otomatik olarak Yenile](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Sertifikayı bunun yerine el ile yenilemek için tıklayın **el ile yenileme**. Sertifikanızın süresi dolmadan önce 60 gün el ile yenilemek için istekte bulunabilir.

Yenileme işlemi tamamlandıktan sonra tıklayın **eşitleme**. Eşitleme işlemi, uygulamalarınıza kapalı kalma süresi neden olmadan konak adı bağlamaları App Service sertifika için otomatik olarak güncelleştirir.

> [!NOTE]
> Yoksa tıklarsanız **eşitleme**, App Service otomatik olarak 48 saat içinde sertifikanıza eşitler.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="more-resources"></a>Daha fazla kaynak

* [HTTPS zorlama](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [TLS 1.1/1.2 zorlama](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions)
* [Azure App Service'teki uygulama kodunuzda SSL sertifikası kullanma](app-service-web-ssl-cert-load.md)
* [SSS: App Service sertifikaları](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
