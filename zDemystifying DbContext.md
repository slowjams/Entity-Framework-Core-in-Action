Demystifying DbContext
==============================

```C#
//-----------------------------------------------------------------------------------------------------------------V
public class DbContext : IInfrastructure<IServiceProvider>, IDbContextDependencies, IDbSetCache, IDbContextPoolable   // namespace Microsoft.EntityFrameworkCore;
{
   private readonly DbContextOptions _options;

   private IDictionary<(Type Type, string Name), object>? _sets;
   private IDbContextServices? _contextServices;
   private IDbContextDependencies? _dbContextDependencies;
   private DatabaseFacade _database;
   private ChangeTracker _changeTracker;

   private IServiceScope _serviceScope;
   private DbContextLease _lease = DbContextLease.InactiveLease;
   private DbContextPoolConfigurationSnapshot? _configurationSnapshot;
   private List<IResettableService> _cachedResettableServices;
   private bool _initializing;
   private bool _disposed;

   private readonly Guid _contextId = Guid.NewGuid();
   private int _leaseCount;

   protected DbContext() : this(new DbContextOptions<DbContext>()) { }

   public DbContext(DbContextOptions options)
   {
      _options = options;

      ServiceProviderCache.Instance.GetOrAdd(options, providerRequired: false)
         .GetRequiredService<IDbSetInitializer>()
         .InitializeSets(this);
   }

}
//-----------------------------------------------------------------------------------------------------------------Ʌ

//--------------------------------------------------------V
public abstract class DbContextOptions : IDbContextOptions
{

}
//--------------------------------------------------------Ʌ
```