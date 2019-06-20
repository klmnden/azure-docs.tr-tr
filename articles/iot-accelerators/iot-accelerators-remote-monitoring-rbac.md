---
title: Uzaktan izleme erişim denetimi - Azure | Microsoft Docs
description: Bu makalede, Uzaktan izleme çözüm hızlandırıcısının rol tabanlı erişim denetimlerine (RBAC) nasıl yapılandıracağınız hakkında bilgi sağlar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: b0c9699bccbb539c9617fac2f3296483139e7188
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203139"
---
# <a name="configure-role-based-access-controls-in-the-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm hızlandırıcısının rol tabanlı erişim denetimlerini yapılandırın

Bu makalede Uzaktan izleme çözüm hızlandırıcısının rol tabanlı erişim denetimleri yapılandırma hakkında bilgi sağlar. Çözümdeki belirli özelliklere bireysel kullanıcılar için erişimi kısıtlamak rol tabanlı erişim denetimleri sağlar.

## <a name="default-settings"></a>Varsayılan ayarları

Uzaktan izleme çözümü ilk kez dağıttığınızda, iki rol vardır: **Yönetici** ve **salt okunur**.

Herhangi bir kullanıcının **yönetici** rolü aşağıdaki aşağıdaki izinleri de dahil olmak üzere, çözüm tam erişime sahiptir. Bir kullanıcı **salt okunur** rolü yalnızca çözüm görüntüleme erişimi olacaktır.

| İzin            | Yönetici | Salt Okunur |
|----------------       |-------|-----------|
| Çözümü görüntüle         | Evet   | Evet       |
| Güncelleştirme uyarıları         | Evet   | Hayır        |
| Uyarıları Sil         | Evet   | Hayır        |
| Cihazları oluşturun        | Evet   | Hayır        |
| Güncelleştirme cihazlar        | Evet   | Hayır        |
| Cihazları silme        | Evet   | Hayır        |
| Cihaz grupları oluşturma  | Evet   | Hayır        |
| Cihaz grupları güncelleştir  | Evet   | Hayır        |
| Cihaz grupları Sil  | Evet   | Hayır        |
| Kuralları oluşturma          | Evet   | Hayır        |
| Güncelleştirme kuralları          | Evet   | Hayır        |
| Kurallarını Sil          | Evet   | Hayır        |
| İş oluşturma           | Evet   | Hayır        |
| Güncelleştirme SIM Yönetimi | Evet   | Hayır        |

Varsayılan olarak, dağıtılan çözümü kullanıcıya otomatik olarak atanır **yönetici** rolü ve bir Azure Active Directory Uygulama sahibi. Bir uygulamanın sahibi olarak, Azure portalı üzerinden diğer kullanıcılara roller atayabilirsiniz. Çözümdeki rol atamak için başka bir kullanıcının isterseniz, Azure portalında bir uygulamanın sahibi olarak da ayarlanmalıdır.

> [!NOTE]
> Çözümü dağıtan kullanıcı **yalnızca kişi** kimler görüntüleyebilir, hemen sonra kendi edilmiş oluşturuldu. Vermek için uygulamayı salt okunur, yönetici veya bkz. aşağıdaki yönergeler aşağıda üzerinde kullanıcı eklemek veya kaldırmak bir özel rol olarak görüntülemek üzere başkalarının erişebileceği.

## <a name="add-or-remove-users"></a>Kullanıcı eklemek veya kaldırmak

Bir Azure Active Directory Uygulama sahibi ekleyip bir kullanıcı bir role Uzaktan izleme çözümü için Azure portalını kullanabilirsiniz. Aşağıdaki adımları kullanın [Azure Active Directory kuruluş uygulaması](../active-directory/manage-apps/add-application-portal.md#find-your-azure-ad-tenant-application) Uzaktan izleme çözümünü dağıttığınızda oluşturuldu.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Denetleme [kullanıcıdır dizinde](../active-directory/fundamentals/add-users-azure-active-directory.md) kullanmakta olduğunuz. İçin oturum açarken kullanılacak dizini seçtiğiniz [Microsoft Azure IOT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators) site. Dizin adı sağ üst köşesinde görülebilir [sayfa](https://www.azureiotsolutions.com/Accelerators).

1. Bulma **Kurumsal uygulama** Azure portalında, çözümünüz için. Bir kez vardır, listeye ayarlayarak filtre **uygulama türü** için **tüm uygulamaları**. Uygulama tarafından uygulama adınızı arayın. Uzaktan izleme çözümünüzü adı uygulama adıdır. Aşağıdaki ekran görüntüsünde, çözüm ve uygulamanın görünen adları olan **contoso rm4**.

    ![Kurumsal uygulama](media/iot-accelerators-remote-monitoring-rbac/appregistration.png)

1. Uygulamayı tıklayarak ve ardından uygulama sahibi olduğunuz denetleyin **sahipleri**. Aşağıdaki ekran görüntüsünde **Contoso yönetici** sahiplerinden biri olan **contoso rm4** uygulama:

    ![Sahipler](media/iot-accelerators-remote-monitoring-rbac/owners.png)

    Sahibi değilseniz, listeye eklemek için var olan bir sahip istemeniz gerekir. Sahipleri uygulama rolleri gibi atayabilirsiniz yalnızca **yönetici** veya **salt okunur** diğer kullanıcılara.

1. Uygulama rollerine atanmış kullanıcıların listesini görmek için tıklayın **kullanıcılar ve gruplar**.

1. Bir kullanıcı eklemek için tıklatın **+ Ekle kullanıcı**ve ardından **kullanıcılar ve gruplar, hiçbiri seçili** dizinden kullanıcı seçin.

1. Kullanıcının bir rol atamak için tıklayın **Select rolü, hiçbiri seçili** ve seçmeniz **yönetici** veya **salt okunur** kullanıcı rolü. Tıklayın **seçin**ve ardından **atama**.

    ![Rol seç](media/iot-accelerators-remote-monitoring-rbac/selectrole.png)

1. Kullanıcı rolü tarafından tanımlanan izinlere sahip Uzaktan izleme çözümü artık erişebilirsiniz.

1. Kullanıcıların uygulamadaki silebilirsiniz **kullanıcılar ve gruplar** portalında sayfası.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Uzaktan izleme çözümü içeren **yönetici** ve **salt okunur** ilk dağıtıldığında rolleri. Farklı izin kümeleri ile özel roller ekleyebilirsiniz. Özel rol tanımlamak için gerekir:

- Azure portalında uygulama için yeni bir rolü ekleyin.
- Yeni rol için bir ilke, kimlik doğrulama ve yetkilendirme mikro hizmet tanımlayın.
- Çözümün web kullanıcı Arabirimi güncelleştirin.

### <a name="define-a-custom-role-in-the-azure-portal"></a>Azure portalında bir özel rol tanımlayın

Aşağıdaki adımları, Azure Active Directory'de bir uygulamaya bir rol ekleme işlemi açıklanmaktadır. Bu örnekte, oluşturma, güncelleştirme ve silme cihazları Uzaktan izleme çözümüne üyelerinin izin veren yeni bir rol oluşturun.

1. Bulma **uygulama kaydı** Azure portalında, çözümünüz için. Uzaktan izleme çözümünüzü adı uygulama adıdır. Aşağıdaki ekran görüntüsünde, çözüm ve uygulamanın görünen adları olan **contoso rm4**.

    ![Uygulama kaydı](media/iot-accelerators-remote-monitoring-rbac/app-registration-2.png)

1. Uygulamanızı seçin ve ardından **bildirim**. Varolan iki gördüğünüz [uygulama rolleri](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) uygulama için tanımlanmış:

    ![Görünüm bildirimi](media/iot-accelerators-remote-monitoring-rbac/view-manifest.png)

1. Adlı bir rol eklemek için bildirimi düzenleyin **ManageDevices** aşağıdaki kod parçacığında gösterildiği gibi. Bir GUID gibi benzersiz bir dize için yeni rol kimliği gerekir. Bir hizmet gibi kullanarak yeni bir GUID oluşturabileceğiniz [çevrimiçi GUID Oluşturucu](https://www.guidgenerator.com/):

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Admin",
        "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "isEnabled": true,
        "description": "Administrator access to the application",
        "value": "Admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "Read Only",
        "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "isEnabled": true,
        "description": "Read only access to device information",
        "value": "ReadOnly"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "displayName": "ManageDevices",
        "id": "ADD A NEW GUID HERE",
        "isEnabled": true,
        "description": "A new role for the application.",
        "value": "ManageDevices"
      }
    ],
    ```

    Değişiklikleri kaydedin.

### <a name="define-a-policy-for-the-new-role"></a>Yeni rol için ilke tanımlama

Azure Portalı'nda uygulama rolü eklemek için bir ilke tanımlamanız gerekir sonra [roles.json](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/auth/Services/data/policies/roles.json) cihazları yönetmek için gereken izinleri atar rolü için.

1. Kopya [Uzaktan izleme mikro Hizmetler](https://github.com/Azure/remote-monitoring-services-dotnet) github deposunu yerel makinenize.

1. Düzen **auth/Services/data/policies/roles.json** ilkesi eklemek için dosya **ManageDevices** aşağıdaki kod parçacığında gösterildiği gibi rol. **Kimliği** ve **rol** değerlerini önceki bölümde uygulama bildirimindeki rol tanımı eşleşmesi gerekir. Biri izin verilen eylemlerin listesini sağlayan **ManageDevices** oluşturmak, güncelleştirmek ve çözüme bağlı cihazları silmek için rolü:

    ```json
    {
    "Items": [
      {
        "Id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
        "Role": "admin",
        "AllowedActions": [
          "UpdateAlarms",
          "DeleteAlarms",
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices",
          "CreateDeviceGroups",
          "UpdateDeviceGroups",
          "DeleteDeviceGroups",
          "CreateRules",
          "UpdateRules",
          "DeleteRules",
          "CreateJobs",
          "UpdateSimManagement"
        ]
      },
      {
        "Id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
        "Role": "readOnly",
        "AllowedActions": []
      },
      {
        "Id": "GUID FROM APP MANIFEST",
        "Role": "ManageDevices",
        "AllowedActions": [
          "CreateDevices",
          "UpdateDevices",
          "DeleteDevices"
        ]
      }
    ]
    }
    ```

1. Tamamladığınızda düzenleme **Services/data/policies/roles.json** dosya, yeniden oluşturun ve kimlik doğrulama ve yetkilendirme mikro hizmet için çözüm hızlandırıcınız yeniden dağıtın.

### <a name="how-the-web-ui-enforces-permissions"></a>Web kullanıcı Arabirimi izinleri nasıl zorunlu kılar

Kullanıcı Arabirimi kullanan web [kimlik doğrulama ve yetkilendirme mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) hangi eylemleri belirlemek için bir kullanıcının Al ve hangi denetimlerin kullanıcı Arabiriminde görünür izin verilir. Örneğin, çözümünüz çağrılırsa **contoso rm4**, web kullanıcı Arabirimi aşağıdaki isteği göndererek geçerli kullanıcı için izin verilen eylemlerin bir listesini alır:

```http
http://contoso-rm4.azurewebsites.net/v1/users/current
headers:
X-Source: true
Authorization: Bearer <JWT Token from ADAL>
```

Bir kullanıcı için adlı **cihaz Yöneticisi** içinde **ManageDevices** rolü, yanıt gövdesinde şu JSON içerir:

```json
{
  "Id": "userIdExample",
  "Email": "devicemanager@contoso.com",
  "Name": "Device Manager",
  "AllowedActions": [
    "CreateDevices",
    "UpdateDevices",
    "DeleteDevices"
  ]
}
```

Alınan aşağıdaki kod [deviceDelete.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/pages/devices/flyouts/deviceDelete/deviceDelete.js) içinde [web kullanıcı Arabirimi](https://github.com/Azure/pcs-remote-monitoring-webui/) izinleri bildirimli olarak nasıl zorlanır gösterir:

```json
<FlyoutContent>
  <Protected permission={permissions.deleteDevices}>
    <form className="device-delete-container" onSubmit={this.deleteDevices}>
      ...
    </form>
  </Protected>
</FlyoutContent>
```

Daha fazla bilgi için [korumalı bileşenleri](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/components/shared/protected/README.md). Ek izinler tanımlayabilirsiniz [authModel.js](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/services/models/authModels.js) dosya.

### <a name="how-the-microservices-enforce-permissions"></a>Mikro hizmetler izinleri nasıl zorla

Mikro hizmetler, ayrıca yetkisiz API istekleri karşı korumak için izinleri denetleyin. Bir mikro hizmet, bir API isteği aldığında, kodunu çözer ve kullanıcı kimliği ve kullanıcı rolüyle ilişkilendirilen izinleri almak için JWT belirtecini doğrular.

Alınan aşağıdaki kod [DevicesController.cs](https://github.com/Azure/remote-monitoring-services-dotnet/blob/master/iothub-manager/WebService/v1/Controllers/DevicesController.cs) dosyası [IoTHub Yöneticisi mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager), izinleri nasıl zorlanır gösterir:

```csharp
[HttpDelete("{id}")]
[Authorize("DeleteDevices")]
public async Task DeleteAsync(string id)
{
    await this.devices.DeleteAsync(id);
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl rol tabanlı erişim denetimleri Uzaktan izleme çözüm hızlandırıcısının uygulanan öğrendiniz.

Bkz: [Time Series Insights Gezgini için erişim denetimleri yapılandırma](iot-accelerators-remote-monitoring-rbac-tsi.md) Uzaktan izleme çözüm Hızlandırıcısını Time Series Insights Gezgininde erişimi yönetme hakkında bilgi.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz. [özelleştirme ve yeniden dağıtma bir mikro hizmet](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->