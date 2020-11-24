Here's a few code snippets showing how to access Kubernetes cluster using the library:

__list all pods__:

```java
import io.kubernetes.client.openapi.ApiClient;
import io.kubernetes.client.openapi.ApiException;
import io.kubernetes.client.openapi.Configuration;
import io.kubernetes.client.openapi.apis.CoreV1Api;
import io.kubernetes.client.openapi.models.V1Pod;
import io.kubernetes.client.openapi.models.V1PodList;
import io.kubernetes.client.util.Config;

import java.io.IOException;

public class Example {
    public static void main(String[] args) throws IOException, ApiException{
        ApiClient client = Config.defaultClient();
        Configuration.setDefaultApiClient(client);

        CoreV1Api api = new CoreV1Api();
        V1PodList list = api.listPodForAllNamespaces(null, null, null, null, null, null, null, null, null);
        for (V1Pod item : list.getItems()) {
            System.out.println(item.getMetadata().getName());
        }
    }
}
```

__watch on namespace object__:

```java
import com.google.gson.reflect.TypeToken;
import io.kubernetes.client.openapi.ApiClient;
import io.kubernetes.client.openapi.ApiException;
import io.kubernetes.client.openapi.Configuration;
import io.kubernetes.client.openapi.apis.CoreV1Api;
import io.kubernetes.client.openapi.models.V1Namespace;
import io.kubernetes.client.util.Config;
import io.kubernetes.client.util.Watch;

import java.io.IOException;

public class WatchExample {
    public static void main(String[] args) throws IOException, ApiException{
        ApiClient client = Config.defaultClient();
        Configuration.setDefaultApiClient(client);

        CoreV1Api api = new CoreV1Api();

        Watch<V1Namespace> watch = Watch.createWatch(
                client,
                api.listNamespaceCall(null, null, null, null, null, 5, null, null, Boolean.TRUE, null, null),
                new TypeToken<Watch.Response<V1Namespace>>(){}.getType());

        for (Watch.Response<V1Namespace> item : watch) {
            System.out.printf("%s : %s%n", item.type, item.object.getMetadata().getName());
        }
    }
}
```

More examples can be found in [examples](examples) folder. To run examples, run this command:

```shell
mvn exec:java -Dexec.mainClass="io.kubernetes.client.examples.Example"
```


Additionally, we prepared a few examples for common use-cases which are shown below:
- __Configuration__:
  - [InClusterClientExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/InClusterClientExample.java): 
  Configure a client while running inside the Kubernetes cluster.
  - [KubeConfigFileClientExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/KubeConfigFileClientExample.java): 
  Configure a client to access a Kubernetes cluster from outside.
- __Basics__:
  - [SimpleExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/Example.java):
  Simple minimum example of how to use the client.
  - [ProtoExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/ProtoExample.java): 
  Request/receive payloads in protobuf serialization protocol.
  - ([5.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-5.0.0)) [PatchExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/PatchExample.java): 
  Patch resource objects in various supported patch formats, equal to `kubectl patch`.
  - [FluentExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/FluentExample.java): 
  Construct arbitrary resource in a fluent builder style.
  - [YamlExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/YamlExample.java): 
  Suggested way to load or dump resource in Yaml.
- __Streaming__:
  - [WatchExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/WatchExample.java): 
  Subscribe watch events from certain resources, equal to `kubectl get <resource> -w`.
  - [LogsExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/LogsExample.java): 
  Fetch logs from running containers, equal to `kubectl logs`.
  - [ExecExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/ExecExample.java): 
  Establish an "exec" session with running containers, equal to `kubectl exec`.
  - [PortForwardExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/PortForwardExample.java): 
  Maps local port to a port on the pod, equal to `kubectl port-forward`.
  - [AttachExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/AttachExample.java): 
  Attach to a process that is already running inside an existing container, equal to `kubectl attach`.
  - [CopyExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/CopyExample.java): 
  Copy files and directories to and from containers, equal to `kubectl cp`.
  - [WebSocketsExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/WebSocketsExample.java): 
  Establish an arbitrary web-socket session to certain resources.
- __Advanced__: (NOTE: The following example requires `client-java-extended` module）
  - ([5.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-5.0.0)) [InformerExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/InformerExample.java): 
  Build an informer which list-watches resources and reflects the notifications to a local cache.
  - ([5.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-5.0.0)) [PagerExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/PagerExample.java): 
  Support Pagination (only for the list request) to ease server-side loads/network congestion.
  - ([6.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-6.0.0)) [ControllerExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/ControllerExample.java): 
  Build a controller reconciling the state of world by list-watching one or multiple resources.
  - ([6.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-6.0.0)) [LeaderElectionExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/LeaderElectionExample.java): 
  Leader election utilities to help implement HA controllers.
  - ([9.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-9.0.0)) [SpringIntegrationControllerExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/SpringControllerExample.java): 
  Building a kubernetes controller based on spring framework's bean injection, see additional documentation [here](./docs/java-controller-tutorial-rewrite-rs-controller.md).
  - ([9.0.0+](https://github.com/kubernetes-client/java/tree/client-java-parent-9.0.0)) [GenericKubernetesClientExample](./examples/examples-release-10/src/main/java/io/kubernetes/client/examples/GenericClientExample.java): 
  Construct a generic client interface for any kubernetes types, including CRDs.

