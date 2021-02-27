# C#

### Check null variable (easy way)

[https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/operators/null-coalescing-operator
](https://docs.microsoft.com/es-es/dotnet/csharp/language-reference/operators/null-coalescing-operator
)
El operador de uso combinado de NULL ?? devuelve el valor del operando izquierdo si no es null; 
en caso contrario, evalúa el operando derecho y devuelve su resultado. El operador ?? no evalúa su operando derecho si el operando izquierdo se evalúa como no NULL.

'''
this.server = server ?? throw new ArgumentNullException(nameof(server));

'''
