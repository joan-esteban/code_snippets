# Jenkins

1. Avoid serialize errors
Jenkins needs to serialize state of jobs, so all components must be serializable. There are a special case that have some incidence when you try to develop something using Groovy: iterate over a map/list
