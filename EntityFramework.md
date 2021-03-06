Entity Framework
=================
## 定义 Domain Class

```C#
class Father
{
  public int Id { get; set; }
  public string Name { get; set; }
  public List<Child> Children { get; set; }
}

class Child
{
  public int Id { get; set; }
  public string Name { get; set; }
  public Father Father { get; set; }
}

static void Main(string[] args)
{
  using (var context = new FamilyContext())
  {
    var papa = new Father
    {
      Name = "Good PaPa",
      Children = new List<Child>
      {
        new Child() { Name = "Beautiful Boy" },
        new Child() { Name = "Snow white" }
      }
    };

    context.FatherSet.Add(papa);
    context.SaveChanges();
  }
}
```
## EntityFramework 约定
### Keys
命名为 Id，或者 [Class name]Id，转换为 PK, not null
### String
nvarchar(max)
### byte[]
varbinary(max)
### bool
bit, not null

## Configuration
### Data Annotations
```c#
[required]
[MaxLength(500)]
[MinLength(5)]
public string Name { get; set; }
[Column(TypeName="image")]
public byte[] Photo { get; set; }

[Table("Child")]
class Child
```
### Fluent API
```c#
class FamilyContext : DbContext
{
  public DbSet<Father> FatherSet { get; set; }
  public DbSet<Child> ChildSet { get; set; }

  protected override void OnModelCreating(DbModelBuilder modelBuilder)
  {
    modelBuilder.Entity<Father>()
      .ToTable("Father")
      .Property(p => p.Name)
        .IsRequired()
        .HasMaxLength(500)
        .HasColumnType("nvarchar(1024)");
  }
}
```
#### Organizing Fluent Configurations
```c#
class FatherConfiguration : EntityTypeConfiguration<Father>
{ 
  public FatherConfiguration()
  {
    ToTable("Father");
    Property(p => p.Name)
      .IsRequired()
      .HasMaxLength(500)
      .HasColumnType("nvarchar(1024)");
  }
}

modelBuilder.Configurations.Add(new FatherConfiguration());
```

### Property Attributes
#### Length
- Convention
    - max (type specified by database)  
- Data Annotation 
    - MinLength(nn) 
    - MaxLength(nn) 
    - StringLength(nn)  
- Fluent 
    - Entity<T>.Property(t=>t.PropertyName).HasMaxLength(nn)  

#### Data Type
- Convention 
    - The default column data type is determined by the database provider you are using. For SQL Server some example default data types are: 
        - String : nvarchar(max) 
        - Integer : int 
        - Byte Array : varbinary(max) 
        - Boolean : bit  
- Data Annotation 
    - Column(TypeName=“xxx”)  
- Fluent 
    - Entity<T>.Property(t=>t.PropertyName).HasColumnType (“xxx”)

#### Nullability and the Required Configuration
- Convention 
    - Key Properties : not null in database
    - Reference Types (String, arrays): null in the database
    - Value Types (all numeric types, DateTime, bool, char) : not null in database 
    - Nullable<T> Value Types : null in database
- Data Annotation
    - Required
- Fluent
    - Entity<T>.Property(t=>t.PropertyName).IsRequired

#### Mapping Keys
- Convention
    - Properties named Id
    - Properties named [TypeName] + Id
- Data Annotation
    - Key
- Fluent
    - Entity<T>.HasKey(t=>t.PropertyName)

#### Configuring Database-Generated Properties
- Convention
    - Integer keys: Identity
- Data Annotation
    - DatabaseGenerated(DatabaseGeneratedOption)
- Fluent
    - Entity<T>.Property(t=>t.PropertyName).HasDatabaseGeneratedOption(DatabaseGeneratedOption)

#### Configuring TimeStamp/RowVersion Fields
- Convention
    - None
- Data Annotation
    - TimeStamp
- Fluent
    - Entity<T>.Property(t=>t.PropertyName).IsRowVersion()
```c#
[Timestamp]
public byte[] RowVersion { get; set; }
```

#### Configuring Non-Timestamp Fields for Concurrency
- Convention
    - None
- Data Annotation
    - ConcurrencyCheck
- Fluent
    - Entity<T>.Property(t=>t.PropertyName).IsConcurrencyToken()
```c#
[ConcurrencyCheck]
public int SocialSecurityNumber { get; set; }
...
Property(p => p.SocialSecurityNumber).IsConcurrencyToken();
```

#### Mapping to Non-Unicode Database Types
- Convention
    - All strings map to Unicode-encoded database types
- Data Annotation
    unavailable
- Fluent
    - Entity<T>.Property(t=>t.PropertyName).IsUnicode(boolean)
```c#
Property(l => l.Owner).IsUnicode(false);
```

#### Affecting the Precision and Scale of Decimals
- Convention
    - Decimals are 18, 2
- Data Annotation
    - unavailable
- Fluent
    - Entity<T>.Property(t=>t.PropertyName).HasPrecision(n,n)

## Database Initialization
```c#
static void Main(string[] args)
{
  Database.SetInitializer(new DropCreateDatabaseIfModelChanges<FamilyContext>());
  ...
}
      


