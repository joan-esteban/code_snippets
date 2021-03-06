# Jenkins
- https://groovy-lang.org/style-guide.html
- https://groovyconsole.appspot.com/
- http://groovy-lang.org/operators.html

***

### Get Version from file
<sup><sup>Written: 2019/10/27  Jenkins version: 2.155</sup></sup> 
```groovy
def getVersion(){
    String version = new File(env.WORKSPACE+ '/version.txt').getText('UTF-8')
    return version.trim() + "." + env.BUILD_NUMBER 
}
```

***

### Check if current run is a Replay
<sup><sup>Written: 2019/10/27  Jenkins version: 2.155</sup></sup> 
* Found at: https://stackoverflow.com/questions/51555910/how-to-know-inside-jenkinsfile-script-that-current-build-is-a-replay/52302879#52302879
```groovy
def isBuildAReplay() {
  // https://stackoverflow.com/questions/51555910/how-to-know-inside-jenkinsfile-script-that-current-build-is-an-replay/52302879#52302879
  def replyClassName = "org.jenkinsci.plugins.workflow.cps.replay.ReplayCause"
  currentBuild.rawBuild.getCauses().any{ cause -> cause.toString().contains(replyClassName) }
}
```

***

### Load a Json 
<sup><sup>Written: 2019/10/27  Jenkins version: 2.155</sup></sup> 
```groovy
// https://stackoverflow.com/questions/37864542/jenkins-pipeline-notserializableexception-groovy-json-internal-lazymap
@NonCPS
def loadComponentsFile(filename){
    //def filename = "components.json"
    println("Loading JSON file: $filename")
    def inputFile = new File(env.WORKSPACE + ".//" + filename)
    def inputJSON = new JsonSlurper().parse(inputFile)
    return new HashMap<>(inputJSON)
}
(...)
def useJson(loadedJson){
   def component_keys = loadedJson['components'].keySet() as List
   for (String item_key: items_keys){
   	def component =  componentsJson['components'][item_key] 
	switch(component['type']){ 	
		case "KEY1":
			def description = new String(component['description'])
			(...)
	}
   }
}

```

***

### Groovy: Get class name
```groovy
def x = "It is currently ${ new Date() }"
println x.getClass().getName()
```

***

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


##### References
- Applied fix proposed at: https://issues.jenkins-ci.org/browse/JENKINS-49732
- Interesting explanation on NonCPS: https://github.com/jenkinsci/workflow-cps-plugin


***


### Variable substitution from already existing String.
<sup><sup>Written: 2019/10/26  Jenkins version: 2.155</sup></sup> 

I had a variable comming from a file where I want to act as declared en code, so I want that ${whatever} would be replace but this variable.

Groovy use `groovy.lang.GString` that it have built-in template, that are a different from String of Java.

On code a string is type `org.codehaus.groovy.runtime.GStringImpl`

When you run a groovy script this point to class `WorkflowScript`

:ok: Solution:
```groovy
@NonCPS
def stringInterpolationWithEnv(str){
    println("--- stringInterpolationWithEnv($str)")
    def template = new SimpleTemplateEngine()
    def variables = new HashMap<>(env.getEnvironment())
    def t = template.createTemplate(str).make(variables)
    return new String(t.toString())
}
```

### Trying to use env object to string subtitution
- Object `env` on a scripted pipeline is type class org.jenkinsci.plugins.workflow.cps.EnvActionImpl.

```groovy
import groovy.text.SimpleTemplateEngine
import groovy.lang.Binding

@NonCPS
def transform(){
    def javaString = 'This is a Java String because simple quote and $BRANCH_NAME is not interpolated'
    println("--- ste")
    def template = new SimpleTemplateEngine()
    println("--- ge")
    def mapEnv = env.getEnvironment()
    println(mapEnv)
    println("--- tc")
    def s = template.createTemplate(javaString).make(mapEnv)
    println("--- render")
    println(s)
    println(s.toString())
}

def myvar = "1234"

transform()
```
- Another try usign JsonSlurper:
   - Item inside map are `java.lang.String`
```
@NonCPS
def transform(filename){
    def inputFile = new File(env.WORKSPACE + ".//" + filename)
    def inputJSON = new JsonSlurper().parse(inputFile)
    def m =  new HashMap<>(inputJSON)
    println(m['publish']['nas_prefix_path'])
    println(m['publish']['nas_prefix_path'].getClass().getName())
    def variables = [
            INSTALL_NAME: 'name'
        ]
    def template = new SimpleTemplateEngine()
    def t = template.createTemplate(javaString).make(variables)
    println(t.toString())
}
node{
    println(env.WORKSPACE)
    transform('jenkins_settings.json')
}

```
- note:
``` 
getBinding().hasVariable("MY_PARAM")
```
### Check if a params is defined
```groovy
params.containsKey(key)
```
:warning: Important: to create 'key' dont use string interpolation (ex: "MY_VAR_${name}") better "MY_VAR_"+name 

### Clone a String List
- I don't known why, but iterating over a json I lose data, so a create strings (`new String(json['mykey'])` ) and also clone `List`
```groovy
@NonCPS
def cloneStringList(list){
    println("--- cloneStringList($list)")
    assert list instanceof List
    def result = []
    for (String item : list){
        result << new String(item)
    }
    
    return result
}
```

##### References
- https://stackoverflow.com/questions/30512887/variable-substitution-in-jenkins-plugin
- https://stackoverflow.com/questions/56101952/string-interpolation-in-groovy-with-jenkins-pipeline-file-not-working
- https://issues.jenkins-ci.org/browse/JENKINS-16660
- https://javadoc.jenkins-ci.org/hudson/EnvVars.html#expand(java.lang.String 
- http://docs.groovy-lang.org/latest/html/documentation/index.html#_double_quoted_string
- http://docs.groovy-lang.org/latest/html/api/groovy/lang/GString.html
- Error `java.lang.ClassCastException: java.io.PrintWriter cannot be cast to java.lang.String`: https://issues.jenkins-ci.org/browse/JENKINS-38432
- https://stackoverflow.com/questions/28572080/how-to-access-parameters-in-a-parameterized-build
***

# Fast scriptAprroval
- Go to <jenkins>/scriptAprroval
- Fill *Signatures already aprroved:*

```
method groovy.json.JsonSlurper parse java.io.File
method groovy.lang.GroovyObject invokeMethod java.lang.String java.lang.Object
method groovy.text.TemplateEngine createTemplate java.lang.String
method hudson.model.Run getCauses
method hudson.model.Run getEnvironment
method java.io.File createNewFile
method java.io.File exists
method org.jenkinsci.plugins.workflow.support.actions.EnvironmentAction getEnvironment
method org.jenkinsci.plugins.workflow.support.steps.build.RunWrapper getRawBuild
new groovy.text.SimpleTemplateEngine
new java.io.File java.lang.String
new java.lang.String java.lang.String
new java.util.HashMap java.util.Map
staticMethod jenkins.model.Jenkins getInstance
staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods append java.io.File java.lang.Object
staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods getAt java.util.Collection java.lang.String
staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods getText java.io.File java.lang.String
staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods putAt java.lang.Object java.lang.String java.lang.Object
staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods write java.io.File java.lang.String
staticMethod org.jenkinsci.plugins.pipeline.modeldefinition.Utils markStageSkippedForConditional java.lang.String
```

