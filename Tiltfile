# You will need to modify this file to enable Tilt live debugging
SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='dev.local/pacman-source') # CHANGEME - replace `source-image-location` with your writable repository
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
K8S_TEST_CONTEXT = os.getenv("K8S_TEST_CONTEXT", default='')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

allow_k8s_contexts(K8S_TEST_CONTEXT)

k8s_custom_deploy(
    'pacman',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
        " --local-path " + LOCAL_PATH +
        " --source-image " + SOURCE_IMAGE +
        " --namespace " + NAMESPACE +
        " --yes " +
        OUTPUT_TO_NULL_COMMAND +
        " && kubectl get workload pacman --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes" ,
    deps=['.'],
    container_selector='workload',
    live_update=[
        fall_back_on(['package.json']),
        sync('.', '/workspace')
]
)

k8s_resource('pacman', port_forwards=["8080:8080"],
    extra_pod_selectors=[{'carto.run/workload-name': 'pacman', 'app.kubernetes.io/component': 'run'}])
allow_k8s_contexts('aks-tap-build')    

