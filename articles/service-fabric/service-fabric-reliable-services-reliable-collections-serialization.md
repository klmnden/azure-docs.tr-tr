---
title: Azure Service fabric'te güvenilir koleksiyon nesne serileştirme | Microsoft Docs
description: Azure Service Fabric Reliable Collections nesne seri hale getirme
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: aljo
ms.openlocfilehash: 2445b37e8152d8f55dad6eff057d273851dc2209
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340686"
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Azure Service fabric'te güvenilir koleksiyon nesne serileştirme
Güvenilir koleksiyonlar, çoğaltma ve makine hataları ve bölgesel elektrik kesintileriyle arasında dayanıklı olduklarından emin olmak için öğeleri kalıcı.
Çoğaltmak için hem öğeleri kalıcı hale getirmek için bunları seri hale getirmek güvenilir koleksiyonlar gerekir.

Güvenilir koleksiyonlar, güvenilir durum Yöneticisi'nden verilen tür için uygun serileştiriciyi almak.
Güvenilir durum Yöneticisi yerleşik seri hale getiricileri genişletme içerir ve verilen tür için kaydedilecek özel serileştiricileri sağlar.

## <a name="built-in-serializers"></a>Yerleşik seri hale getiricileri genişletme

Bunlar verimli bir şekilde varsayılan olarak seri hale getirilebilir, böylece bazı genel türleri için yerleşik seri hale getirici güvenilir durum Yöneticisi'ni içerir. Diğer türleri için güvenilir durum Yöneticisi geri kullanmaya döner [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Yerleşik seri hale getiricileri genişletme türlerini değiştiremezsiniz ve türü, tür adını gibi hakkındaki bilgileri içerecek şekilde gerekmez bildiğiniz daha verimlidir.

Güvenilir durum Yöneticisi aşağıdaki türleri için yerleşik seri hale getirici sahiptir: 
- Guid
- bool
- byte
- sbyte
- byte[]
- char
- string
- decimal
- double
- float
- int
- uint
- long
- ulong
- kısa
- ushort

## <a name="custom-serialization"></a>Özel serileştirme

Özel seri hale getiricileri genişletme, performansı artırmak veya aktarımda ve diskteki verileri şifrelemek için yaygın olarak kullanılır. Türü hakkında bilgi seri hale gerekmez bu yana diğer nedenlerin yanı sıra özel seri hale getiricileri genişletme genel seri hale getirici yaygın olarak daha verimlidir. 

[IReliableStateManager.TryAddStateSerializer\<T >](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer) T. verilen tür için özel bir serileştirici kaydetmek için kullanılır Bu kayıt, oluşumunu StatefulServiceBase kurtarma başlamadan önce tüm güvenilir koleksiyonlar, kalıcı verileri okumak için ilgili seri hale getirici erişimi olmasını sağlamak için gerçekleşmelidir.

```csharp
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
> Özel seri hale getiricileri genişletme yerleşik seri hale getiricileri genişletme üzerinde öncelik verilir. İnt için özel bir serileştirici kaydedildiğinde, bu örneğin, int için yerleşik seri hale getirici yerine tamsayılar serileştirmek için kullanılır

### <a name="how-to-implement-a-custom-serializer"></a>Özel bir serileştirici gerçekleştirme

Özel bir serileştirici uygulamak gereken [IStateSerializer\<T >](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) arabirimi.

> [!NOTE]
> IStateSerializer\<T > temel değer olarak adlandırılan bir T yazma ve okuma alan için bir aşırı yükleme içerir. Fark seri hale getirme için API'dir. Şu anda fark seri hale getirme özelliğini gösterilmez. Bu nedenle, fark serileştirme kullanıma sunulan ve etkin kadar bu iki aşırı yüklemeler çağrılır değil.

Aşağıdaki dört özellikleri içeren OrderKey adlı bir örnek özel türüdür

```csharp
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

Aşağıdadır IStateSerializer örnek uygulaması\<OrderKey >.
Alan aşırı yüklemeler baseValue içinde okuyup unutmayın ileten uyumluluk için kendi ilgili aşırı yüklemesini çağırın.

```csharp
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
İçinde bir [sıralı yükseltme uygulama](service-fabric-application-upgrade.md), düğümlerinin bir alt kümesi, aynı anda bir yükseltme etki alanı yükseltme uygulanır. Bu işlem sırasında bazı yükseltme etki alanları, uygulamanızın daha yeni sürümü üzerinde olacaktır ve bazı yükseltme etki alanlarında, uygulamanın eski sürümünü temel olacaktır. Dağıtım sırasında uygulamanızın yeni sürümünü verilerinizi eski sürümü okuyabilir ve uygulamanızın eski sürümünü verilerinizi yeni sürümünü okuyabilir olmalıdır. Veri biçimi ileriye ve geriye dönük olarak uyumlu değilse, yükseltme başarısız olabilir veya daha da kötüsü, veriler kaybolursa veya bozulursa.

Yerleşik seri hale getirici kullanıyorsanız, Uyumluluğu konusunda endişelenmeniz gerekmez.
Özel bir serileştirici veya DataContractSerializer kullanıyorsanız, ancak veri sonsuz İleri ve uyumlu olması gerekir.
Diğer bir deyişle, seri hale getirici'nün her sürümü seri hale getirmek ve seri durumdan çıkarılamıyor türü herhangi bir sürümü gerekir.

Veri sözleşmesi kullanıcılar ekleme, kaldırma ve alanları değiştirmek için iyi tanımlanmış sürüm oluşturma kuralları izlemelidir. Veri anlaşması bilinmeyen alanlarla ilgili, serileştirme ve seri durumundan çıkarma işlemine takma ve sınıf devralma ile ilgili destek de vardır. Daha fazla bilgi için [kullanarak veri anlaşması](https://msdn.microsoft.com/library/ms733127.aspx).

Özel seri hale getirici kullanıcılar, geriye doğru olduğundan ve iletir uyumlu hale getirmek için kullanarak seri hale getirici Kılavuzu uymanız gerekir.
Tüm sürümlerini destekleyen en yaygın yolu başında boyutu bilgiler ekleyerek ve isteğe bağlı özellikler yalnızca ekleme.
Çok olabilir ve akış kalan bölümünü atlama gibi her bir sürümü bu şekilde okuyabilir.

## <a name="next-steps"></a>Sonraki adımlar
  * [Seri hale getirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.
  * [Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.
  * Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).
  * Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).
  * Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).
