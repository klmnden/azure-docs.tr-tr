Bu adımda, kullanılabilirlik grubu dinleyicisi aynı ağ üzerinde çalışan bir istemci uygulaması kullanarak sınayın.

İstemci bağlantısı aşağıdaki gereksinimlere sahiptir:

* İstemci bağlantıları dinleyicisi olandan farklı bir bulut hizmeti barındıran Always On kullanılabilirlik çoğaltmalarının bulunan makinelerden gelmelidir.
* İstemciler her zaman açık çoğaltmaları farklı alt ağlarda olup olmadığını belirtmeniz *MultisubnetFailover = True* bağlantı dizesinde. Bu durum çeşitli alt çoğaltmaları için paralel bağlantı denemelerinde sonuçlanır. Bu senaryo bir çapraz bölge Always On kullanılabilirlik grubu dağıtımı içerir.

Sanal makineleri aynı Azure sanal ağı (ancak bir çoğaltmasını barındıran bir değil) birinden dinleyiciye bağlanmak bir örnektir. Kullanılabilirlik grubu dinleyicisi için SQL Server Management Studio bağlanmayı denemek için bu test tamamlamak için kolay bir yol değil. Başka bir basit yöntem çalıştırmaktır [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)aşağıdaki gibi:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [!NOTE]
> EndpointPort değer ise *1433*, çağrıda belirtmeniz gerekmez. Önceki ayrıca istemci makine aynı etki alanına katılmışsa ve Windows kimlik doğrulaması kullanarak arayan veritabanı izinleri verildi varsayar.
> 
> 

Dinleyici test ettiğinizde istemciler arasında yük devretmeler dinleyiciye bağlanabildiğinden emin olmak için kullanılabilirlik grubu yük devri emin olun.

