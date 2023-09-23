LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='apps')

allow_k8s_contexts('tap-sandbox')

k8s_custom_deploy(
    'node-express',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --namespace " + NAMESPACE +
               " --yes --output yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=["package.json", "server.js"],
    container_selector='workload',
    live_update=[
      sync('server.js', '/workspace/server.js')
    ]
)

k8s_resource('node-express', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'node-express', 'app.kubernetes.io/component': 'run'}])
