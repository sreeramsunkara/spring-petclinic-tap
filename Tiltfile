SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='tapdemoregistry.azurecr.io/spring-petclinic-app-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
allow_k8s_contexts('tap')
k8s_custom_deploy(
    'spring-petclinic',
    apply_cmd="tanzu apps workload apply -f kubernetes/tap/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null " +
              "&& kubectl get workload spring-petclinic --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f kubernetes/tap/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    image_selector='tapdemoregistry.azurecr.io/spring-petclinic-' + NAMESPACE,
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('spring-petclinic', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'spring-petclinic'}])