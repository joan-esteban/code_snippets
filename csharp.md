# C#
### Get class name for log
[https://stackoverflow.com/questions/2113069/c-sharp-getting-its-own-class-name](https://stackoverflow.com/questions/2113069/c-sharp-getting-its-own-class-name)

```
Logger?.Log(LogVerbosity.DEBUG, $"{this.GetType().Name}: Hello!);
```


### Check null variable (easy way)

[https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/operators/null-coalescing-operator
](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/operators/null-coalescing-operator
)
El operador de uso combinado de NULL ?? devuelve el valor del operando izquierdo si no es null; 
en caso contrario, evalúa el operando derecho y devuelve su resultado. El operador ?? no evalúa su operando derecho si el operando izquierdo se evalúa como no NULL.

'''
this.server = server ?? throw new ArgumentNullException(nameof(server));

'''

## Unittest (Nunit3)

- Using NUnit3 and Moq

### Mock: Return something from a Mock class

```
MockAuth.Setup(x => x.ValidateCredentials(It.IsAny<ClientSourceContext>(), It.IsAny<string>(), It.IsAny<string>(), out credentials)).Returns(new ResultBasic<ValidationResultEnum>(ValidationResultEnum.OK));
            
```

### Check function have been called

```
MockAuditStore.Verify(v => v.Write(It.IsAny<AuditTransaction>()), Times.Exactly(1));
```
