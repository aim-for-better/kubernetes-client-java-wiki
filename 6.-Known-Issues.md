### Known Issues (not yet resolved)

##### 1. Exception on deleting resources: "java.lang.IllegalStateException: Expected a string but was BEGIN_OBJECT..."

This is happening because openapi schema from kubernetes upstream doesn't match its implementation due to 
the limitation of openapi v2 schema expression [#86](https://github.com/kubernetes-client/java/issues/86). 
Consider either catch and ignore the JsonSyntaxException or do the deletion in the following form:

- Use Kubectl equivalence, see examples [here](https://github.com/kubernetes-client/java/blob/6fa3525189d9e50d9b07016155642ddf59990905/e2e/src/test/groovy/io/kubernetes/client/e2e/kubectl/KubectlNamespaceTest.groovy#L69-L72)
- Use generic kubernetes api, see examples [here](https://github.com/kubernetes-client/java/blob/6fa3525189d9e50d9b07016155642ddf59990905/examples/src/main/java/io/kubernetes/client/examples/GenericClientExample.java#L56)
