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
