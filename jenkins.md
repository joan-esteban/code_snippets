# Jenkins

### 1. Groovy/Avoid serialize errors
Jenkins needs to serialize state of jobs, so all components must be serializable. There are a special case that have some incidence when you try to develop something using Groovy: iterate over a map/list

Example of backtrace
```
Caused: java.io.NotSerializableException: java.util.LinkedHashMap$Entry
	at org.jboss.marshalling.river.RiverMarshaller.doWriteObject(RiverMarshaller.java:926)
	at org.jboss.marshalling.river.BlockMarshaller.doWriteObject(BlockMarshaller.java:65)
```
* Apply fix proposed at: https://issues.jenkins-ci.org/browse/JENKINS-49732
