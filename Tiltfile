SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='dev.local/mysql-demo-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
allow_k8s_contexts('cl1-admin@cl1')

k8s_custom_deploy(
    'mysql-demo',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null " +
              "&& kubectl get workload mysql-demo --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    image_selector='dev.local/mysql-demo-' + NAMESPACE,
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('mysql-demo', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'mysql-demo'}])
