---
title: Azure depolama alanına erişmek için bir Windows VM yönetilen kimliğini kullan
description: Azure Storage erişmek için yönetilen bir Windows VM kimliği kullanma sürecinde anlatan öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: 9ccc94727a18fbcd77f00000531934be57b8e132
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801373"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Öğretici: Azure Storage erişmek için yönetilen bir Windows VM kimliğini kullanın.

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici yönetilen kimlik bir Windows sanal makine için etkinleştirme ve Azure Storage erişmek için kimliğini kullanma gösterilmektedir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yeni bir kaynak grubunda bir Windows sanal makine oluşturma 
> * Bir Windows sanal makine (VM) üzerinde yönetilen kimlik etkinleştir
> * Bir depolama hesabında blob kapsayıcı oluşturun
> * Windows VM kimlik yönetilen bir depolama hesabı erişim 
> * Bir erişmek ve Azure Storage çağırmak için kullanın 

> [!NOTE]
> Azure depolama için Azure Active Directory kimlik doğrulaması genel önizlemede değil.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturma

Bu bölümde daha sonra yönetilen bir kimlik verilir bir Windows VM oluşturun.

1.  Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgileri İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makine açılır.
5.  Yeni bir seçmek için **kaynak grubu** oluşturulması, seçmek için sanal makine için istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alt görüntü metin](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-identity-on-your-vm"></a>VM üzerinde yönetilen kimliğini etkinleştir

Yönetilen bir sanal makine kimliğini, kodunuza kimlik bilgileri almaya gerek kalmadan Azure AD'den erişim belirteçleri almak sağlar. Perde arkasında yönetilen kimlik Azure portal aracılığıyla bir sanal makinede etkinleştirilmesi iki işlemi yapar: yönetilen bir kimlik oluşturmak üzere Azure AD ile VM kaydeder ve VM kimliğini yapılandırır. 

1. Yeni bir sanal makine kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. Altında **ayarları** kategorisini tıklatın **yapılandırma**.
3. Yönetilen kimlik etkinleştirmek için seçin **Evet**.
4. Tıklatın **kaydetmek** yapılandırmayı uygulamak için. 

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Bu bölümde, bir depolama hesabı oluşturun. 

1. Tıklatın **+ kaynak oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. Tıklatın **depolama**, ardından **depolama hesabı - blob, dosya, tablo, kuyruk**.
3. Altında **adı**, depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **tür hesap** ayarlanmalı **Resource manager** ve **depolama (genel amaçlı v1)**. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Bir blob kapsayıcı oluşturun ve depolama hesabına bir dosyayı karşıya yüklemek

Dosya blob depolama gerektirdiğinden dosyasının depolanacağı bir blob kapsayıcısı oluşturmanız gerekir. Yeni depolama hesabındaki blob kapsayıcısı için bir dosya yükleyin.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Altında **Blob hizmeti**, tıklatın **kapsayıcıları**.
3. Tıklatın **+ kapsayıcı** sayfanın üst kısmında.
4. Altında **yeni kapsayıcı**, altında ve kapsayıcı için bir ad girin **genel erişim düzeyi** varsayılan değer tutun.

    ![Depolama kapsayıcısı oluşturma](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Tercih ettiğiniz bir Düzenleyicisi'ni kullanarak başlıklı bir dosya oluşturun *world.txt hello* yerel makinenizde.  Dosyasını açın ve "Hello world! (tırnak işaretleri olmadan) metin ekleme :) "ve kaydedin. 
6. Kapsayıcı adına sonra tıklatarak yeni oluşturulan kapsayıcıya dosyayı karşıya yüklemeyi **karşıya yükle**
7. İçinde **karşıya yükleme blob** bölmesi altında **dosyaları**, klasör simgesine tıklayın ve dosyaya göz atın **hello_world.txt** yerel makinenizde dosyayı seçin ve ardından **Karşıya**.
    ![Metin dosyasını karşıya yükle](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Bir Azure depolama kapsayıcısı, VM erişim 

VM'ın yönetilen kimliği, Azure depolama blobunu verileri almak için kullanabilirsiniz.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.  
2. Tıklatın **erişim denetimi (IAM)** sol panelinde bağlantı.  
3. Tıklatın **+ Ekle** VM için yeni bir rol ataması eklemek için sayfanın en üstünde.
4. Altında **rol**, açılan listeden seçin **depolama Blob veri Okuyucu (Önizleme)**. 
5. Sonraki açılır altında **atamak için erişim**, seçin **sanal makine**.  
6. Ardından, uygun abonelik listelenir olun **abonelik** açılır ve ardından **kaynak grubu** için **tüm kaynak gruplarının**.  
7. Altında **seçin**, VM'yi seçin ve ardından **kaydetmek**. 

    ![İzin ata](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Erişim belirteci almak ve Azure Storage çağırmak için kullanın 

Azure depolama yönetilen kimliğini kullanarak doğrudan erişim belirteçleri kabul edebilir destekler Azure AD kimlik doğrulama, yerel olarak alınır. Bu Azure AD ile tümleştirme Azure Storage'nın parçası olan ve bağlantı dizesini kimlik bilgileri sağladığını farklıdır.

.Net kodu örneği, bir bağlantı açarak bir İşte Azure bir erişim belirteci kullanarak ve ardından daha önce oluşturduğunuz dosyasının içeriğini okuma depolama. Bu kod, VM'ın yönetilen kimlik uç noktasına erişmek için VM'de çalıştırması gerekir. .NET framework 4.6 veya daha yüksek erişim belirteci yöntemi kullanmak için gereklidir. Değerini `<URI to blob file>` uygun şekilde. Bu değer oluşturulur ve blob depolama ve kopyalama karşıya dosya giderek edinebilirsiniz **URL** altında **özellikleri** **genel bakış** sayfası.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;

namespace StorageOAuthToken
{
    class Program
    {
        static void Main(string[] args)
        {
            //get token
            string accessToken = GetMSIToken("https://storage.azure.com/");
           
            //create token credential
            TokenCredential tokenCredential = new TokenCredential(accessToken);

            //create storage credentials
            StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

            Uri blobAddress = new Uri("<URI to blob file>");

            //create block blob using storage credentials
            CloudBlockBlob blob = new CloudBlockBlob(blobAddress, storageCredentials);
        
            //retrieve blob contents
            Console.WriteLine(blob.DownloadText());
            Console.ReadLine();
        }

        static string GetMSIToken(string resourceID)
        {
            string accessToken = string.Empty;
            // Build request to acquire MSI token
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=" + resourceID);
            request.Headers["Metadata"] = "true";
            request.Method = "GET";

            try
            {
                // Call /token endpoint
                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Pipe response Stream to a StreamReader, and extract access token
                StreamReader streamResponse = new StreamReader(response.GetResponseStream());
                string stringResponse = streamResponse.ReadToEnd();
                JavaScriptSerializer j = new JavaScriptSerializer();
                Dictionary<string, string> list = (Dictionary<string, string>)j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
                accessToken = list["access_token"];
                return accessToken;
            }
            catch (Exception e)
            {
                string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                return accessToken;
            }
        }            
    }
}
```

Yanıt dosyasının içeriğini içerir:

`Hello world! :)`

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, öğrenilen nasıl Windows sanal makinesi Azure depolama alanına erişmek için yönetilen kimlik etkinleştirin.  Bilgi edinmek için Azure Storage hakkında daha fazla bakın:

> [!div class="nextstepaction"]
> [Azure Depolama](/azure/storage/common/storage-introduction)



