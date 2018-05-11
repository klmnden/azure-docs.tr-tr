---
title: Azure automation'da ilk Python runbook Uygulamam
description: Test ve basit bir Python runbook yayımlama oluşturma adımlarını anlatan öğretici.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 3b5e3d7f2cfb9d5f93d03c75cdbfa69a16bdc63d
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="my-first-python-runbook"></a>İlk Python runbook Uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 
> 

Bu öğreticide, oluşturulmasını açıklanmaktadır bir [Python runbook](automation-runbook-types.md#python-runbooks) Azure Automation. Sınama ve yayımlama basit bir runbook ile başlatın. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, daha sağlam runbook parametreleri ekleyerek runbook'u hale.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="create-a-new-runbook"></a>Yeni bir runbook oluşturma

Metnini veren basit bir runbook oluşturarak başlayın *Hello World*.

1. Azure portalında, Otomasyon hesabınızı açın.

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.<br>

1. Runbook’ların listesini açmak için **İŞLEM YÖNETİMİ** altında **Runbooklar**’ı seçin.
2. Seçin **+ runbook Ekle** yeni bir runbook oluşturmak için.
3. Runbook adını verin *MyFirstRunbook-Python*.
4. Bu durumda, oluşturacağız bir [Python runbook](automation-runbook-types.md#python-runbooks) şekilde select **Python 2** için **Runbook türü**.
5. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="add-code-to-the-runbook"></a>Runbook'a kod ekleme

Şimdi "Hello World" metninin yazdırmak için basit bir komut ekleyin:

```python
print("Hello World!")
```

Tıklatın **kaydetmek** runbook'u kaydetmek için.

## <a name="test-the-runbook"></a>Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
3. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.
   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Bu taşınır *başlangıç* bir çalışan işi talep ettiğinde ve ardından *çalıştıran* runbook gerçekten çalışmaya başladığında.
4. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, görmelisiniz *Hello World*.
5. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="publish-and-start-the-runbook"></a>Yayımlama ve runbook'u Başlat

Oluşturduğunuz runbook hala taslak modunda değil. Üretimde çalıştırılabilmesi için önce yayımlanması gerekir.
Bir runbook yayımladığınızda, Taslak sürümü mevcut yayımlanmış sürümün üzerine.
Bu durumda, runbook oluşturduğunuz çünkü yayımlanmış bir sürüm henüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
2. Runbook'ta görüntülemek için sola kaydırırsanız, **Runbook'lar** Bölmesi şimdi gösterir bir **yazma durumu** , **yayımlanan**.
1. Bölmeyi görüntülemek üzere geri sağa kaydırın **MyFirstRunbook-Python**.
   Üst kısımdaki seçenekler runbook’u başlatmamıza, görüntülememize, gelecek bir zamanda başlatılmak üzere zamanlamamıza ya da bir HTTP çağrısıyla başlatılabilmesi için [web kancası](automation-webhooks.md) oluşturmamıza olanak tanır.
2. İstediğiniz runbook'u başlatmak için bu nedenle tıklatın **Başlat** ve ardından **Tamam** zaman Runbook'u Başlat dikey pencere açılır.
3. Oluşturduğunuz runbook işi için bir iş bölmesi açıldı. Bu bölme kapatılabilir, ancak işin ilerleme durumunu izleyebilmek için bu durumda, bu açık bırakın.
4. İş durumu gösterilen **iş özeti** ve runbook test zaman gördüğümüz durumların aynısıdır.
5. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. Çıktı bölmesi açılır ve görebilirsiniz, *Hello World*.
6. Çıktı bölmesini kapatın.
7. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Çıktı akışında yalnızca *Merhaba Dünya* metnini görmelisiniz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.
8. Akışlar bölmesini ve Python MyFirstRunbook bölmesine dönmek için iş bölmesini kapatın.
9. Bu runbook’a ait İşler bölmesini açmak için **İşler**’e tıklayın. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.
10. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## <a name="add-authentication-to-manage-azure-resources"></a>Azure kaynaklarınızı yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz.
Azure kaynaklarınızı yönetmek için kimlik bilgilerini kullanarak kimlik doğrulaması yapmak betik sahiptir, [Otomasyon hesabı](automation-offering-get-started.md).

> [!NOTE]
> Otomasyon hesabı için runas sertifikası olacak şekilde orada hizmet asıl özelliğiyle oluşturulmuş olması gerekir.
> Automation hesabınız ile hizmet sorumlusu oluşturulmamışsa konumunda açıklanan yöntemi kullanarak doğrulayabilir [Python için Azure yönetim kitaplıkları ile kimlik doğrulama](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).

1. Tıklayarak metin düzenleyicisini açın **Düzenle** MyFirstRunbook-Python bölmesi.
1. Azure için kimlik doğrulaması yapmak için aşağıdaki kodu ekleyin:
   ```python
   import os
   from azure.mgmt.compute import ComputeManagementClient
   import azure.mgmt.resource
   import automationassets

   def get_automation_runas_credential(runas_connection):
       from OpenSSL import crypto
       import binascii
       from msrestazure import azure_active_directory
       import adal

       # Get the Azure Automation RunAs service principal certificate
       cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
       pks12_cert = crypto.load_pkcs12(cert)
       pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())

       # Get run as connection information for the Azure Automation service principal
       application_id = runas_connection["ApplicationId"]
       thumbprint = runas_connection["CertificateThumbprint"]
       tenant_id = runas_connection["TenantId"]

       # Authenticate with service principal certificate
       resource ="https://management.core.windows.net/"
       authority_url = ("https://login.microsoftonline.com/"+tenant_id)
       context = adal.AuthenticationContext(authority_url)
       return azure_active_directory.AdalAuthentication(
       lambda: context.acquire_token_with_client_certificate(
               resource,
               application_id,
               pem_pkey,
               thumbprint)
       )

   # Authenticate to Azure using the Azure Automation RunAs service principal
   runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
   azure_credential = get_automation_runas_credential(runas_connection)
   ```

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Python işlem istemci oluşturun ve VM başlatmak için kod ekleme

Azure VM ile birlikte çalışmak için bir örneğini oluşturmak [Python için Azure işlem istemci](https://docs.microsoft.com/python/api/azure.mgmt.compute.computemanagementclient?view=azure-python).

VM başlatmak için işlem İstemcisi'ni kullanın. Aşağıdaki kodu runbook'a ekleyin:

```python
# Initialize the compute management client with the RunAs credential and specify the subscription to work against.
compute_client = ComputeManagementClient(
azure_credential,
  str(runas_connection["SubscriptionId"])
)


print('\nStart VM')
async_vm_start = compute_client.virtual_machines.start("MyResourceGroup", "TestVM")
async_vm_start.wait()
```

Burada _MyResourceGroup_ VM içeren kaynak grubu adı ve _TestVM_ başlatmak istediğiniz VM adıdır. 

Test etmek ve VM başlatır yeniden görmek için runbook çalıştırın.

## <a name="use-input-parameters"></a>Giriş parametreleri kullanın

Runbook şu anda kaynak grubu ve VM adları için sabit kodlanmış değerler kullanır.
Şimdi bu değerleri giriş parametrelerini alır kod ekleyelim.

Kullandığınız `sys.argv` parametre değerlerini almak için değişkeni.
Aşağıdaki kod runbook'a hemen art arda ekleme `import` deyimleri:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Bu içeri aktarmalar `sys` modül ve kaynak grubu ve VM adları tutacak iki değişken oluşturur.
Dikkat bağımsız değişken listesi öğesi `sys.argv[0]`, komut dosyası adı ve kullanıcı tarafından giriş değil.

Şimdi giriş parametre değerlerini sabit kodlanmış değerler kullanmak yerine runbook'un son iki satırlık değiştirebilirsiniz:

```python
async_vm_start = compute_client.virtual_machines.start(resource_group_name, vm_name)
async_vm_start.wait()
```

Python runbook başlattığınızda (üzerinde **Test** sayfa veya yayımlanan bir runbook olarak), parametreler için değerler girebilirsiniz **Runbook'u Başlat** altında sayfa **parametreleri** .

İlk kutusunda bir değer girmeniz başlattıktan sonra ikinci bir görünür ve, böylece gereken sayıda parametre değerlerini girebilirsiniz benzeri.

Komut dosyasına olarak değerler kullanılabilir `sys.argv` eklediğiniz kod olduğu gibi dizi.

İlk parametre için değer olarak, kaynak grubunuzun adını ve ikinci parametre değeri olarak başlatmak için VM adını girin.

![Parametre değerlerini girin](media/automation-first-runbook-textual-python/runbook-python-params.png)

Tıklatın **Tamam** runbook'u başlatın. Runbook çalıştırır ve belirttiğiniz VM başlatır.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
* Python ile Azure için geliştirme hakkında bilgi edinmek için [Python geliştiricileri için Azure](https://docs.microsoft.com/python/azure/?view=azure-python).
* Örnek Python 2 runbook'ları görüntülemek için bkz: [Azure Otomasyon GitHub](https://docs.microsoft.com/python/azure/?view=azure-python).
