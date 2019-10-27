# Jenkins
- https://groovy-lang.org/style-guide.html
- https://groovyconsole.appspot.com/
- http://groovy-lang.org/operators.html

###   Groovy/Avoid serialize errors   
<sup><sup>Written: 2019/10/26  Jenkins version: 2.155</sup></sup> 

Jenkins needs to serialize state of jobs, so all components must be serializable. There are a special case that have some incidence when you try to develop something using Groovy: iterate over a map/list.

:ok: Good way to iterate over a `Map`:
```groovy
job_data.each{ e->
    infoFile.append("${key_prefix}${e.key}=${comma}${e.value}${comma}\n")
  }
```
:ok: Another way is to prefixed function with `@NonCPS`, example:
```groovy
@NonCPS
def cloneStringList(list){
    assert list instanceof List
    def result = []
    for (String item : list){
        result << new String(item)
    }
    return result
}
```
⚠️ Example of **bad code**:
```groovy
  for ( e in job_data ) {
    infoFile.append("${key_prefix}${e.key}=${comma}${e.value}${comma}\n")
  }
```
⚠️This code produce next **backtrace**:
```
Caused: java.io.NotSerializableException: java.util.LinkedHashMap$Entry
	at org.jboss.marshalling.river.RiverMarshaller.doWriteObject(RiverMarshaller.java:926)
	at org.jboss.marshalling.river.BlockMarshaller.doWriteObject(BlockMarshaller.java:65)
```
* Applied fix proposed at: https://issues.jenkins-ci.org/browse/JENKINS-49732

***

### Variable substitution from already existing String.
<sup><sup>Written: 2019/10/26  Jenkins version: 2.155</sup></sup> 

I had a variable comming from a file where I want to act as declared en code, so I want that ${whatever} would be replace but this variable.

Groovy use GString that it have built-in template, that are a different from String of Java.

##### References
- https://stackoverflow.com/questions/30512887/variable-substitution-in-jenkins-plugin
- https://stackoverflow.com/questions/56101952/string-interpolation-in-groovy-with-jenkins-pipeline-file-not-working
- https://issues.jenkins-ci.org/browse/JENKINS-16660
- https://javadoc.jenkins-ci.org/hudson/EnvVars.html#expand(java.lang.String 
- http://docs.groovy-lang.org/latest/html/documentation/index.html#_double_quoted_string
