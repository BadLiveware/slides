# Udda tal i en lista
```csharp
List&lt;int> collection = new List&lt;int> { 1, 2, 3, 4, 5 };
```

## Imperativ
Instruktioner för jobbet som ska utföras

```csharp
List&lt;int> results = new List&lt;int>();
foreach(var num in collection)
{
    if (num % 2 != 0)
          results.Add(num);
}
```

## Deklarativ
Vad resultatet ska vara

```csharp
var results = collection.Where( num => num % 2 != 0);
```



# Vad händer om något går fel?
* Imperativ
* Deklarativ