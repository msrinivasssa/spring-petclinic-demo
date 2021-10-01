custom_build('gcr.io/advanced-editions-engineering/spring-petclinic',
  'tanzu apps workload apply -f config/workload.yaml --local-path=. --yes && \
    while [[ $(kubectl get ksvc spring-petclinic -o \'jsonpath={..status.conditions[?(@.type=="Ready")].status}\') != "True" ]]; do echo "waiting for ksvc" && sleep 10; done',
  ['pom.xml', './target/classes'],
  live_update = [
    sync('./target/classes', '/workspace/BOOT-INF/classes')
  ],
  skips_local_docker=True
)
 
k8s_yaml('./config/workload.yaml')
k8s_kind('Workload', image_json_path='{.spec.params[?(@.name=="run-image")].value}')
k8s_resource(workload='spring-petclinic', extra_pod_selectors=[{'serving.knative.dev/service':'spring-petclinic'}])

