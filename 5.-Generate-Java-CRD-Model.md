### Model Classes from Popular CRDs

The project also provides model classes generated from some frequently used open source projects as separate maven dependencies. Please refer to the following to see their respective documentation.
* [cert-manager](client-java-contrib/cert-manager)
* [prometheus operator](client-java-contrib/prometheus-operator)

It's also recommended to read [this documentation](https://github.com/kubernetes-client/java/blob/master/docs/generate-model-from-third-party-resources.md) to learn details about automatically generating custom Models that fits into this library. Alternatively, you can also write the models manually by implementing `io.kubernetes.client.common.KubernetesObject` for the singular resource types and `io.kubernetes.client.common.KubernetesListObject` for the list types, e.g.:

```java

public class Foo implements io.kubernetes.client.common.KubernetesObject {
    ...
}


public class FooList implements io.kubernetes.client.common.KubernetesListObject {
    ...
}
```