---
title: "Azure Service Fabric güvenilir koleksiyon nesnesi serileştirmede | Microsoft Docs"
description: "Azure Service Fabric güvenilir koleksiyonları nesne seri hale getirme"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: c14794b71ce7340d9e90a56d781c712e247ded06
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Azure Service Fabric güvenilir koleksiyon nesnesi serileştirmede
Güvenilir koleksiyonları çoğaltmak ve makine hataları ve güç kesintileri arasında dayanıklı olduklarından emin olmak için kendi öğeleri kalır.
Çoğaltmak için hem öğeleri kalıcı hale getirmek için bunları seri hale getirmek güvenilir koleksiyonları gerekir.

Güvenilir koleksiyonları uygun seri hale getirici için belirli bir türde güvenilir durum Yöneticisi'nden alın.
Güvenilir durum Yöneticisi yerleşik serileştiricileri içerir ve verilen bir tür için kayıtlı olması özel serileştiricileri sağlar.

## <a name="built-in-serializers"></a>Yerleşik serileştiricileri

Varsayılan olarak verimli bir şekilde serileştirilebilir böylece güvenilir durum Yöneticisi bazı ortak türleri için yerleşik seri hale getirici içerir. Diğer türleri için güvenilir durum Yöneticisi geri kullanılacak döner [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Yerleşik serileştiricileri türlerini değiştiremezsiniz ve türü adı gibi türü hakkındaki bilgileri içerecek şekilde gerekmez bildikleri olduğundan daha verimlidir.

Güvenilir durum Yöneticisi aşağıdaki türleri için yerleşik seri hale getirici sahiptir: 
- GUID
- bool
- Bayt
- sbyte
- Byte]
- char
- Dize
- Ondalık
- Çift
- Kayan nokta
- Int
- uint
- uzun
- ulong
- kısa
- ushort

## <a name="custom-serialization"></a>Özel seri hale getirme

Özel serileştiricileri, performansı artırmak için veya kablo üzerinden ve diskteki verileri şifrelemek için yaygın olarak kullanılır. Türü hakkında bilgi serileştirmek gerekmeyen beri diğer nedenlerin yanı sıra özel serileştiricileri genel seri hale getirici yaygın olarak daha verimlidir. 

[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) T. belirtilen tür için özel bir seri hale getirici kaydetmek için kullanılır Bu kayıt kurtarma başlamadan önce tüm güvenilir koleksiyonları kalıcı verilerini okumak için ilgili seri hale getirici erişiminiz olduğundan emin olmak için StatefulServiceBase yapımı gerçekleşmelidir.

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed to set OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> Özel serileştiricileri yerleşik serileştiricileri öncelik verilir. İnt özel seri hale getirici kaydedildiğinde, örneğin, bunu int için yerleşik serileştirici yerine tamsayılar serileştirmek için kullanılır

### <a name="how-to-implement-a-custom-serializer"></a>Özel bir seri hale getirici gerçekleştirme

Özel bir seri hale getirici uygulamak gereken [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) arabirimi.

> [!NOTE]
> IStateSerializer<T> aşırı yazma ve alan okuma için taban değeri olarak adlandırılan bir ek T içerir. Değişiklikleri seri hale getirme için API'dir. Şu anda değişiklikleri seri hale getirme özellik gösterilmez. Değişiklikleri seri hale getirme gösterilir ve etkin kadar bu nedenle, bu iki aşırı denir değil.

Aşağıdaki dört özellikler içeren OrderKey adlı bir örnek özel türüdür

```C#
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

Aşağıdadır IStateSerializer örnek uygulaması<OrderKey>.
Okuma ve yazma içinde baseValue alın, ileten uyumluluk için kendi ilgili aşırı yüklemesini çağırın aşırı unutmayın.

```C#
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a>Upgradability
İçinde bir [uygulama yükseltmesinde](service-fabric-application-upgrade.md), alt düğümler, aynı anda bir yükseltme etki alanı yükseltme uygulanır. Bu işlem sırasında bazı yükseltme etki alanları, uygulamanızın sürümü olacaktır ve bazı yükseltme etki alanları, uygulamanızın eski sürümünde olacaktır. Piyasaya çıkma sırasında uygulamanın yeni sürümü verilerinizin eski sürümü okuyabilir ve uygulamanızın eski sürümünü verilerinizin yeni sürümü okuyabilir. Veri biçimi ileriye ve geriye dönük olarak uyumlu değilse, yükseltme başarısız olabilir ya da kötüsü, veriler kaybolmuş veya bozulmuş.

Yerleşik seri hale getirici kullanıyorsanız, uyumluluğu hakkında endişelenmeniz gerekmez.
Özel bir seri hale getirici veya DataContractSerializer kullanıyorsanız, ancak verileri sonsuz İleri ve geri girmek uyumlu olması gerekir.
Diğer bir deyişle, her sürümü seri hale getirici, serileştirme ve seri durumdan türü herhangi bir sürümü gerekiyor.

Veri sözleşmesi kullanıcılar ekleme, kaldırma ve alanları değiştirme için iyi tanımlanmış sürüm kuralları başvurmalıdır. Veri sözleşmesi bilinmeyen alanlarla ilgilenme, seri hale getirme ve seri durumdan çıkarma işlemine takma ve sınıf devralma ile ilgilenen için destek de vardır. Daha fazla bilgi için bkz: [kullanarak veri sözleşmesi](https://msdn.microsoft.com/library/ms733127.aspx).

Özel seri hale getirici kullanıcılar geriye doğru olduğundan ve iletir uyumlu hale getirmek için kullandıkları seri hale getirici yönergeleriyle uyumlu.
Tüm sürümler destekleyen en yaygın yolu başında boyutu bilgileri eklemek ve yalnızca isteğe bağlı özellikler ekleme.
Çok olabilir ve akış kalan kısmını atlama olarak bu şekilde her sürümü okuyabilir.

## <a name="next-steps"></a>Sonraki adımlar
  * [Seri hale getirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.
  * [Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.
  * Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).
  * Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).
  * Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [sorun giderme uygulama yükseltmelerini](service-fabric-application-upgrade-troubleshooting.md).
