---
title: Azure automation'da ilk Python runbook'um
description: Test ve basit bir Python runbook yayımlama oluşturma adımlarını anlatan öğretici.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e79f4b58582ab6643a7a13ffee25503060a2208c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60929278"
---
# <a name="my-first-python-runbook"></a>İlk Python runbook'um

> [!div class="op_single_selector"]
> - [Grafik](automation-first-runbook-graphical.md)
> - [PowerShell](automation-first-runbook-textual-powershell.md)
> - [PowerShell İş Akışı](automation-first-runbook-textual.md)
> - [Python](automation-first-runbook-textual-python2.md)

Bu öğreticide oluşturulmasını adım adım göstermektedir bir [Python runbook](automation-runbook-types.md#python-runbooks) Azure automation'da. Test edin ve yayımlayın, basit bir runbook ile başlayacağız. Ardından, bir Azure sanal makinesini başlatmayı içeren bir örnekle, bu runbook’u gerçekten Azure kaynaklarını yönetmek üzere değiştirin. Son olarak, runbook'u daha sağlam runbook parametreleri ekleyerek yapmanızı ister.

> [!NOTE]
> Python runbook'u başlatmak için bir Web kancası kullanma desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
- Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
- Azure sanal makinesi. Bu makineyi durdurup başlatacağınız için makinenin üretime yönelik bir VM olmaması gerekir.

## <a name="create-a-new-runbook"></a>Yeni runbook oluştur

Metnini veren basit bir runbook oluşturmaya başlayın *Hello World*.

1. Azure portalında, Otomasyon hesabınızı açın.

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, yeni bir Otomasyon hesabına otomatik olarak dahil edilen modüllerdir. Burada ayrıca [önkoşullarda](#prerequisites) belirtilen Kimlik Bilgileri varlığınız da bulunmalıdır.<br>

1. Runbook’ların listesini açmak için **İŞLEM YÖNETİMİ** altında **Runbooklar**’ı seçin.
1. Seçin **+ runbook Ekle** yeni bir runbook oluşturmak için.
1. Runbook adını verin *MyFirstRunbook-Python*.
1. Bu durumda, oluşturacağız bir [Python runbook](automation-runbook-types.md#python-runbooks) seçin **Python 2** için **Runbook türü**.
1. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="add-code-to-the-runbook"></a>Runbook'a kod ekleme

Şimdi metin "Merhaba Dünya" yazdırmak için basit bir komut ekleyin:

```python
print("Hello World!")
```

Tıklayın **Kaydet** runbook'u kaydetmek.

## <a name="test-the-runbook"></a>Runbook'u test etme

Runbook’u üretimde kullanılabilir hale getirmek üzere yayımlamadan önce düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook'u test ettiğinizde, bunun **Taslak** sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülersiniz.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
1. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
1. Bir [runbook işi](automation-runbook-execution.md) oluşturulur ve durumu görüntülenir.
   İş durumu, bulutta bir runbook çalışanının kullanılabilir hale gelmesinin beklendiğini gösteren şekilde *Sırada* olarak başlar. Bu taşınır *başlangıç* bir çalışan işi talep ettiğinde ve ardından *çalıştıran* runbook gerçekten çalışmaya başladığında.
1. Runbook işi tamamlandığında çıktısı görüntülenir. Bu durumda, görmelisiniz *Hello World*.
1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="publish-and-start-the-runbook"></a>Yayımlama ve runbook'u Başlat

Oluşturduğunuz runbook hala taslak modundadır. Üretimde çalıştırılabilmesi için önce yayımlanması gerekir.
Bir runbook yayımladığınızda Taslak sürümü mevcut yayımlanmış sürümün üzerine yazın.
Bu durumda, runbook oluşturduğunuz çünkü bir yayımlanan sürümü henüz mevcut değil.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. Runbook'ta görüntülemek için sola kaydırırsanız **runbook'ları** bölmesi artık gösterir bir **yazma durumu** , **yayımlanan**.
1. Bölmeyi görüntülemek üzere geri sağa kaydırın **MyFirstRunbook-Python**.
   Üst kısımdaki seçenekler runbook'u başlatmamıza ya da gelecekte bir zamanda başlatmak üzere olanak tanır.
2. Runbook'u başlatmak için bu nedenle tıklatın istediğiniz **Başlat** ve ardından **Tamam** Runbook'u Başlat dikey penceresi açıldığında.
3. Oluşturduğunuz runbook işi için bir iş bölmesi açılır. Bu bölme kapatılabilir, ancak işin ilerleme durumunu izleyebilmek için bu durumda, açık bırakın.
1. İş durumu gösterilen **iş özeti** ve ne zaman, runbook test gördüğümüz durumların aynısıdır.
2. Runbook durumu olarak *Tamamlandı* gösterilince **Çıktı**’ya tıklayın. Çıktı bölmesi açılır ve gördüğünüz, *Hello World*.
3. Çıktı bölmesini kapatın.
4. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Çıktı akışında yalnızca *Merhaba Dünya* metnini görmelisiniz, ancak bu bölmede, runbook bunlara yazıyorsa Ayrıntılı ve Hata gibi runbook işine yönelik diğer akışlar da gösterilebilir.
5. Akışlar bölmesini ve Python MyFirstRunbook bölmesine dönmek için iş bölmesini kapatın.
6. Bu runbook’a ait İşler bölmesini açmak için **İşler**’e tıklayın. Bu bölmede, bu runbook tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için sadece bir işin listelendiğini görmeniz gerekir.
7. Runbook’u başlattığınızda, görüntülediğiniz iş bölmesini açmak için bu işe tıklayabilirsiniz. Böylece zaman içinde geri dönerek, belirli bir runbook için oluşturulan herhangi bir işin ayrıntılarını görüntüleyebilirsiniz.

## <a name="add-authentication-to-manage-azure-resources"></a>Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz.
Azure kaynaklarını yönetmek için Otomasyon hesabınızdan kimlik bilgilerini kullanarak kimlik doğrulaması yapmak betik vardır. Yardımcı olmak için kullanabileceğiniz [Azure Otomasyonu yardımcı programı paket](https://github.com/azureautomation/azure_automation_utility) kimlik doğrulaması ve Azure kaynaklarıyla etkileşim kurmak daha kolay.

> [!NOTE]
> Otomasyon hesabının hizmet asıl özellik için bir çalıştırma olarak sertifika olması burada ile oluşturulmuş olmalıdır.
> Otomasyon hesabınız ile hizmet sorumlusu oluşturulmamışsa açıklanan yöntemi kullanarak doğrulayabilir [Python için Azure yönetim kitaplıkları ile kimlik doğrulama](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).

1. Tıklayarak metin düzenleyicisini açın **Düzenle** MyFirstRunbook-Python bölmesinde.

2. Azure'da kimlik doğrulaması yapmak için aşağıdaki kodu ekleyin:

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

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>İşlem Python istemcisi oluşturma ve sanal Makineyi başlatmak için kod ekleyin

Azure Vm'lerle çalışmak için bir örneğini oluşturmak [Python için Azure işlem istemci](https://docs.microsoft.com/python/api/azure.mgmt.compute.computemanagementclient?view=azure-python).

İşlem istemci VM'yi başlatmak için kullanın. Runbook için aşağıdaki kodu ekleyin:

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

Burada _MyResourceGroup_ VM içeren kaynak grubunun adıdır ve _TestVM_ başlatmak istediğiniz VM'nin adıdır.

Test ve sanal makine başlatılmadan yeniden görmek için runbook'u çalıştırın.

## <a name="use-input-parameters"></a>Giriş parametreleri kullanma

Runbook, şu anda sabit kodlanmış değerler için kaynak grubunu ve VM adlarını kullanır.
Artık bu değerleri giriş parametrelerini alır kod ekleyelim.

Kullandığınız `sys.argv` parametre değerlerini almak için değişkeni.
Aşağıdaki kodu runbook'a hemen art arda ekleme `import` ifadeleri:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Bu aktarır `sys` modülü ve kaynak grubu ve VM adlarını tutmak için iki değişkeni oluşturur.
Dikkat edin bağımsız değişken listesinin öğe `sys.argv[0]`, komut dosyası adı ve kullanıcı tarafından giriş değil.

Artık runbook giriş parametresi değerleri, sabit kodlanmış değerler yerine kullanmak için son iki satırlık değiştirebilirsiniz:

```python
async_vm_start = compute_client.virtual_machines.start(resource_group_name, vm_name)
async_vm_start.wait()
```

Python runbook'u başlattığınızda (üzerinde **Test** sayfası veya yayımlanan bir runbook olarak), parametreler için değerleri girin **Runbook'u Başlat** altındaki **parametreleri** .

İlk kutusunda bir değer girildiğinde başlattıktan sonra ikinci bir görünür ve devam şekilde, böylece gerekli sayıda parametre değerlerini girin.

Komut dosyası kullanılabilir değerler `sys.argv` eklediğiniz kod olduğu gibi bir dizi.

İlk parametresi için değer olarak, kaynak grubu adını ve ikinci parametre değeri olarak başlatmak için VM adını girin.

![Parametre değerlerini girin](media/automation-first-runbook-textual-python/runbook-python-params.png)

Tıklayın **Tamam** runbook'u başlatın. Runbook'u çalıştırır ve belirttiğiniz VM'yi başlatır.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook uygulamam](automation-first-runbook-textual-powershell.md)
- Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
- PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
- Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
- Python ile Azure geliştirme hakkında bilgi edinmek için [Python geliştiricileri için Azure](https://docs.microsoft.com/python/azure/?view=azure-python)
- Örnek Python 2 runbook'ları görüntülemek için bkz: [Azure Otomasyon GitHub](https://github.com/azureautomation/runbooks/tree/master/Utility/Python)
