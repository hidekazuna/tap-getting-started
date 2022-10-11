SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.hidekazun.jp/tap/supply-chain/tanzu-java-web-app3-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'tanzu-java-web-app3',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --registry-ca-cert " + "/etc/docker/certs.d/harbor.hidekazun.jp/ca.crt" +
               " --registry-username " + "admin" +
               " --registry-password " + "VMware1!" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload tanzu-java-web-app3 --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tanzu-java-web-app3', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tanzu-java-web-app3'}])

allow_k8s_contexts('workload1-admin@workload1')